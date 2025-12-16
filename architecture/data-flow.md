# Data Flow Diagrams

This document illustrates the key data flows in Forever Message, showing how information moves through the system for various operations.

## Table of Contents
1. [Bottle Creation Flow](#bottle-creation-flow)
2. [Like/Unlike Flow](#likeunlike-flow)
3. [Comment Creation Flow](#comment-creation-flow)
4. [Authentication Flow](#authentication-flow)
5. [Bottle Sync Flow](#bottle-sync-flow)

---

## Bottle Creation Flow

This is the most complex flow in the system, involving queuing, async processing, IPFS upload, and blockchain interaction.

```mermaid
sequenceDiagram
    actor User
    participant UI as Frontend<br/>(CreateBottleModal)
    participant Auth as Auth Context<br/>(Supabase Session)
    participant API as API Route<br/>/api/bottles/create
    participant Queue as bottles_queue<br/>(Supabase)
    participant Webhook as DB Webhook
    participant Edge as Edge Function<br/>process-bottle
    participant Process as API Route<br/>/api/bottles/process
    participant IPFS as Storacha<br/>(IPFS)
    participant RPC as Alchemy RPC<br/>(Base Sepolia)
    participant Contract as ForeverMessage<br/>Contract
    participant DB as bottles<br/>(Supabase)
    participant Toast as Toast System<br/>(Sonner + NotificationStore)

    User->>UI: Enter message, click "Cast"

    activate UI
    UI->>Auth: getSession()
    Auth-->>UI: JWT token

    UI->>API: POST /api/bottles/create<br/>{message, token}
    activate API
    API->>API: Validate JWT<br/>(middleware)
    API->>API: Extract wallet address

    API->>Queue: INSERT {message, user_id, status:'queued'}
    Queue-->>API: Queue ID
    API-->>UI: 200 {queueId, status:'queued'}
    deactivate API

    UI-->>User: Show progress (Queued)
    deactivate UI

    Note over Queue,Webhook: Webhook triggers on INSERT

    activate Webhook
    Webhook->>Webhook: Detect new row<br/>(status='queued')
    Webhook->>Edge: Trigger edge function
    activate Edge

    Edge->>Process: POST /api/bottles/process<br/>{queueId, message, userId}
    activate Process

    Process->>Queue: UPDATE {status:'uploading', progress:10}

    Process->>IPFS: Upload message JSON
    activate IPFS
    IPFS-->>Process: IPFS CID
    deactivate IPFS

    Process->>Queue: UPDATE {status:'minting', ipfs_cid, progress:40}

    Process->>RPC: Send transaction<br/>createBottle(cid, creator)
    activate RPC
    RPC->>Contract: Execute createBottle()
    activate Contract
    Contract->>Contract: Mint NFT<br/>bottleId++
    Contract->>Contract: Emit BottleCreated event
    Contract-->>RPC: Transaction hash
    deactivate Contract
    RPC-->>Process: Bottle ID
    deactivate RPC

    Process->>Queue: UPDATE {status:'confirming', blockchain_id, progress:80}

    Process->>Process: Wait 5s for confirmation

    Process->>DB: INSERT INTO bottles<br/>{id, creator, ipfs_hash, message, ...}
    DB-->>Process: Success

    Process->>Queue: UPDATE {status:'completed', progress:100}

    Process-->>Edge: 200 {success, bottleId, cid}
    deactivate Process
    Edge-->>Webhook: Success
    deactivate Edge
    deactivate Webhook

    Queue->>Toast: Status update (queued)
    Toast-->>User: "Sealing your message..."

    Queue->>Toast: Status update (uploading)
    Toast-->>User: "Sealing your message..."

    Queue->>Toast: Status update (minting)
    Toast-->>User: "Casting your bottle..."

    Queue->>Toast: Status update (confirming)
    Toast-->>User: "Almost there..."

    Queue->>Toast: Status update (completed)
    Toast-->>User: "Your bottle has been cast!" (success)

    Note over UI: SWR auto-refetch

    activate UI
    UI->>API: GET /api/bottles
    API->>DB: SELECT * FROM bottles
    DB-->>API: Bottle list
    API-->>UI: Bottles (including new one)
    UI->>UI: Add bottle to ocean
    UI-->>User: Show new bottle
    deactivate UI
```

### Key Points
- **Instant Response**: User gets response in <1s
- **Async Processing**: Heavy work happens in background (60s timeout)
- **Progress Tracking**: `bottles_queue` status updated throughout
- **Reliability**: If processing fails, error stored in queue for debugging

---

## Like/Unlike Flow

Simpler flow that requires authentication and updates both database and blockchain.

```mermaid
sequenceDiagram
    actor User
    participant UI as Frontend<br/>(LikeButton)
    participant Hook as useLikes Hook
    participant Auth as Supabase<br/>Session
    participant API as API Route<br/>/api/bottles/[id]/like
    participant DB as likes<br/>(Supabase)
    participant RPC as Alchemy RPC
    participant Contract as ForeverMessage<br/>Contract

    User->>UI: Click like button

    activate UI
    UI->>Hook: toggleLike()
    activate Hook

    Hook->>Auth: getSession()
    Auth-->>Hook: JWT token

    Hook->>Hook: Optimistic update<br/>(local state)
    UI-->>User: Show liked state

    Hook->>API: POST /api/bottles/{id}/like<br/>{token}
    activate API
    API->>API: Validate JWT<br/>Extract wallet address

    API->>DB: SELECT FROM likes<br/>WHERE bottle_id AND user_id
    DB-->>API: Existing like or null

    alt Like exists (Unlike)
        API->>DB: DELETE FROM likes
        DB-->>API: Success

        API->>RPC: Send transaction<br/>unlikeBottle(bottleId, userAddress)
        activate RPC
        RPC->>Contract: Execute unlikeBottle()
        Contract->>Contract: Emit BottleUnliked event
        Contract-->>RPC: Success
        deactivate RPC
        RPC-->>API: Transaction hash

        API-->>Hook: {liked: false, likeCount}
    else Like doesn't exist (Like)
        API->>DB: INSERT INTO likes
        DB-->>API: Success

        API->>RPC: Send transaction<br/>likeBottle(bottleId, userAddress)
        activate RPC
        RPC->>Contract: Execute likeBottle()
        Contract->>Contract: Emit BottleLiked event
        Contract-->>RPC: Success
        deactivate RPC
        RPC-->>API: Transaction hash

        API-->>Hook: {liked: true, likeCount}
    end

    deactivate API

    Hook->>Hook: Update cache<br/>(SWR mutate)
    Hook-->>UI: Updated state
    deactivate Hook
    UI-->>User: Confirmed like state
    deactivate UI
```

### Key Points
- **Optimistic Updates**: UI updates immediately for better UX
- **Dual Write**: Database + blockchain for redundancy
- **Idempotent**: Toggle behavior (like → unlike → like)
- **Auth Required**: Must be signed in to like

---

## Comment Creation Flow

Similar to bottle creation but simpler (no queuing needed).

```mermaid
sequenceDiagram
    actor User
    participant UI as Frontend<br/>(AddCommentForm)
    participant Auth as Supabase<br/>Session
    participant API as API Route<br/>/api/bottles/[id]/comments
    participant IPFS as Storacha
    participant RPC as Alchemy RPC
    participant Contract as ForeverMessage<br/>Contract
    participant DB as comments<br/>(Supabase)

    User->>UI: Enter comment, submit

    activate UI
    UI->>Auth: getSession()
    Auth-->>UI: JWT token

    UI-->>User: Show "Uploading..." toast

    UI->>API: POST /api/bottles/{id}/comments<br/>{message, token}
    activate API
    API->>API: Validate JWT
    API->>API: Check bottle exists<br/>and not expired

    API->>IPFS: Upload comment JSON
    activate IPFS
    IPFS-->>API: IPFS CID
    deactivate IPFS

    UI-->>User: Show "Posting to blockchain..." toast

    API->>RPC: Send transaction<br/>addComment(bottleId, cid, userAddress)
    activate RPC
    RPC->>Contract: Execute addComment()
    activate Contract
    Contract->>Contract: commentId++
    Contract->>Contract: Emit CommentAdded event
    Contract-->>RPC: Comment ID
    deactivate Contract
    RPC-->>API: Transaction hash
    deactivate RPC

    API->>DB: INSERT INTO comments<br/>{bottle_id, ipfs_hash, user_id, blockchain_id}
    DB-->>API: Success

    API-->>UI: 200 {commentId, cid}
    deactivate API

    UI-->>User: Show "Comment posted!" toast
    UI->>UI: Refresh comments list
    deactivate UI
```

### Key Points
- **Synchronous**: Comment posted immediately (within timeout)
- **IPFS First**: Upload to IPFS before blockchain
- **Toast Feedback**: Clear progress indicators for user
- **No Queue**: Simpler than bottles, completes in ~3-5s

---

## Authentication Flow

SIWE (Sign-In With Ethereum) flow for wallet-based authentication.

```mermaid
sequenceDiagram
    actor User
    participant UI as Frontend<br/>(Header)
    participant RainbowKit as RainbowKit<br/>ConnectButton
    participant Wallet as User's Wallet
    participant AuthCtx as Auth Context
    participant NonceAPI as API Route<br/>/api/auth/nonce
    participant VerifyAPI as API Route<br/>/api/auth/verify
    participant Supabase as Supabase Auth

    User->>UI: Click "Connect Wallet"

    activate UI
    UI->>RainbowKit: Open modal
    activate RainbowKit
    RainbowKit-->>User: Show wallet options
    User->>RainbowKit: Select wallet (e.g., MetaMask)
    RainbowKit->>Wallet: Request connection
    activate Wallet
    Wallet-->>User: Approval modal
    User->>Wallet: Approve
    Wallet-->>RainbowKit: Connected {address, chainId}
    deactivate Wallet
    RainbowKit-->>UI: Connection success
    deactivate RainbowKit

    UI->>AuthCtx: Detect isConnected=true
    activate AuthCtx
    AuthCtx->>AuthCtx: Check isAuthenticated
    AuthCtx->>AuthCtx: Not authenticated, trigger signIn()

    AuthCtx->>NonceAPI: GET /api/auth/nonce
    activate NonceAPI
    NonceAPI->>NonceAPI: generateNonce()
    NonceAPI-->>AuthCtx: {nonce}
    deactivate NonceAPI

    AuthCtx->>AuthCtx: Create SIWE message<br/>{domain, address, nonce, ...}

    AuthCtx->>Wallet: Request signature<br/>(signMessageAsync)
    activate Wallet
    Wallet-->>User: Show signature request
    User->>Wallet: Sign message
    Wallet-->>AuthCtx: Signature
    deactivate Wallet

    AuthCtx->>VerifyAPI: POST /api/auth/verify<br/>{message, signature}
    activate VerifyAPI
    VerifyAPI->>VerifyAPI: Parse SIWE message
    VerifyAPI->>VerifyAPI: Verify signature

    alt Signature valid
        VerifyAPI->>Supabase: Create/get user<br/>{email: address@wallet.local}
        activate Supabase
        Supabase-->>VerifyAPI: User + session tokens
        deactivate Supabase

        VerifyAPI-->>AuthCtx: {session: {access_token, refresh_token}}
    else Signature invalid
        VerifyAPI-->>AuthCtx: 401 Error
    end
    deactivate VerifyAPI

    AuthCtx->>Supabase: setSession({access_token, refresh_token})
    Supabase-->>AuthCtx: Success

    AuthCtx->>AuthCtx: Set isAuthenticated=true
    AuthCtx-->>UI: Authentication complete
    deactivate AuthCtx

    UI-->>User: Show wallet address
    deactivate UI
```

### Key Points
- **SIWE Standard**: Industry-standard wallet authentication
- **No Password**: Only wallet signature required
- **JWT Session**: Supabase manages session tokens
- **Middleware Protection**: All protected routes validate JWT

---

## Bottle Sync Flow

Background process that syncs blockchain events to the database.

```mermaid
sequenceDiagram
    participant Cron as Cron Job<br/>(Scheduled)
    participant EdgeFn as Edge Function<br/>sync-bottles
    participant RPC as Alchemy RPC
    participant Contract as ForeverMessage<br/>Contract
    participant DB as Supabase DB
    participant IPFS as IPFS Gateway

    Note over Cron: Runs every 5 minutes

    Cron->>EdgeFn: Trigger sync
    activate EdgeFn

    EdgeFn->>DB: SELECT MAX(id) FROM bottles
    DB-->>EdgeFn: Last synced bottle ID (e.g., 42)

    EdgeFn->>RPC: Get contract instance
    EdgeFn->>Contract: Filter BottleCreated events<br/>WHERE bottleId > 42
    activate Contract
    Contract-->>EdgeFn: Event logs array
    deactivate Contract

    loop For each event
        EdgeFn->>EdgeFn: Parse event<br/>{bottleId, creator, ipfsHash}

        EdgeFn->>IPFS: Fetch ipfsHash
        activate IPFS
        IPFS-->>EdgeFn: JSON {message, userId, timestamp}
        deactivate IPFS

        EdgeFn->>DB: Check if bottle exists<br/>SELECT * WHERE id=bottleId
        DB-->>EdgeFn: Exists or null

        alt Bottle doesn't exist
            EdgeFn->>DB: INSERT INTO bottles<br/>{id, creator, ipfs_hash, message, ...}
            DB-->>EdgeFn: Success
        else Bottle exists
            EdgeFn->>EdgeFn: Skip (already synced)
        end
    end

    EdgeFn-->>Cron: Sync complete<br/>{newBottles: count}
    deactivate EdgeFn
```

### Key Points
- **Scheduled Sync**: Runs every 5 minutes
- **Incremental**: Only fetches new events
- **Idempotent**: Safe to run multiple times
- **Recovery**: Syncs missed bottles if webhook fails

---

## Real-Time Notification Flow

Toast notifications provide feedback during async bottle creation.

```mermaid
sequenceDiagram
    participant Queue as bottles_queue<br/>(Supabase)
    participant Hook as useBottleQueue<br/>(Real-time subscription)
    participant Store as NotificationStore<br/>(Zustand-like)
    participant UI as Toast UI<br/>(Sonner)
    participant User

    Queue->>Hook: UPDATE event (status change)
    activate Hook
    Hook->>Hook: Detect status change

    alt Status: queued or uploading
        Hook->>Store: addLoadingToast(id, "Sealing your message...")
        Store->>UI: Show loading toast
        UI-->>User: Display: "Sealing your message..."
    else Status: minting
        Hook->>Store: addLoadingToast(id, "Casting your bottle...")
        Store->>UI: Update toast message
        UI-->>User: Display: "Casting your bottle..."
    else Status: confirming
        Hook->>Store: addLoadingToast(id, "Almost there...")
        Store->>UI: Update toast message
        UI-->>User: Display: "Almost there..."
    else Status: completed
        Hook->>Store: removeLoadingToast(id)
        Hook->>Store: addCompletionNotification({id, message, ipfsCid, txHash})
        Store->>UI: Show success toast
        UI-->>User: Display: "Your bottle has been cast!"

        Note over Hook: Wait 1s then remove from queue
        Hook->>Hook: setTimeout 1000ms
        Hook->>Hook: Remove from queueItems state
    else Status: failed
        Hook->>Store: removeLoadingToast(id)
        Store->>UI: Show error toast
        UI-->>User: Display error message

        Note over Hook: Wait 1s then remove from queue
        Hook->>Hook: Remove from queueItems state
    end
    deactivate Hook
```

### Key Points
- **Real-time Updates**: Supabase subscription triggers on status changes
- **Progressive Messaging**: Toast messages match queue phase
- **Automatic Cleanup**: Completed/failed items removed after 1s delay
- **Loading States**: Single toast per bottle, updated in-place
- **Success Feedback**: Includes IPFS CID and transaction hash links

---

## Summary

### Data Flow Characteristics

| Flow | Complexity | Duration | Auth Required | Async |
|------|-----------|----------|---------------|-------|
| Bottle Creation | High | 5-10s | Yes | Yes (queue) |
| Like/Unlike | Low | 1-2s | Yes | No |
| Comment Creation | Medium | 3-5s | Yes | No |
| Authentication | Medium | 2-3s | No | No |
| Bottle Sync | Medium | Varies | No | Yes (cron) |
| Real-Time Notifications | Low | Instant | No | Yes (subscription) |

### Common Patterns

1. **JWT Validation**: All authenticated flows validate JWT via middleware
2. **Optimistic Updates**: UI updates before API confirmation
3. **Dual Write**: Critical data written to both DB and blockchain
4. **IPFS First**: Always upload to IPFS before blockchain
5. **Error Handling**: Graceful degradation, clear error messages
