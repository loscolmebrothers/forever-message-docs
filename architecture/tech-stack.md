# Technology Stack

Complete overview of all technologies, libraries, and tools used in Forever Message.

## Table of Contents
1. [Frontend](#frontend)
2. [Backend](#backend)
3. [Blockchain](#blockchain)
4. [Storage](#storage)
5. [Infrastructure](#infrastructure)
6. [Development Tools](#development-tools)

---

## Frontend

### Core Framework

**Next.js 14**
- Version: `^14.x`
- Features Used: App Router, Server Components, API Routes
- Why: Modern React framework with excellent performance, SSR/SSG support, and built-in routing
- Deployment: Static export with client-side hydration

**React 18**
- Version: `^18.x`
- Features: Hooks, Context API, Suspense
- Why: Industry-standard UI library with strong ecosystem

### UI & Visualization

**React Three Fiber**
- Version: `^8.x`
- Purpose: 3D rendering for ocean environment
- Based on: Three.js
- Why: Declarative 3D graphics in React

**Three.js**
- Version: `^0.x`
- Purpose: WebGL 3D engine
- Why: Most popular WebGL library, extensive community

**React Konva / Konva**
- Version: `^18.x` / `^9.x`
- Purpose: 2D canvas rendering for bottles
- Why: High-performance 2D graphics with React integration

### Wallet & Web3

**RainbowKit**
- Version: `^2.2.x`
- Purpose: Wallet connection UI
- Features: Multi-wallet support, beautiful UI, mobile-friendly
- Why: Best-in-class wallet UX with minimal setup
- Wallets Supported: MetaMask, Coinbase Wallet, WalletConnect, Rainbow, and 30+ more

**wagmi**
- Version: `^2.x`
- Purpose: React hooks for Ethereum
- Features: Type-safe hooks, automatic ABI typing, caching
- Why: Modern, TypeScript-first Ethereum library

**viem**
- Version: `^2.x`
- Purpose: Low-level Ethereum interactions
- Features: Type-safe, lightweight, tree-shakeable
- Why: Modern replacement for ethers.js, better TypeScript support

### State Management & Data Fetching

**SWR**
- Version: `^2.x`
- Purpose: Data fetching and caching
- Features: Revalidation, optimistic updates, pagination
- Why: Perfect for real-time data, built by Vercel

**React Hooks**
- Built-in: `useState`, `useEffect`, `useCallback`, `useMemo`, `useContext`
- Custom hooks: `useBottles`, `useLikes`, `useAuth`, `useBottleQueue`

### Styling

**CSS-in-JS (Inline Styles)**
- Approach: Inline style objects
- Why: Component co-location, dynamic styling, no build step
- Trade-off: No CSS extraction, larger bundle (acceptable for this project)

### Utilities

**usehooks-ts**
- Version: `^3.x`
- Purpose: React hooks library
- Hooks Used: `useWindowSize`, `useDebounce`

**sonner**
- Version: `^1.x`
- Purpose: Toast notifications
- Why: Beautiful, accessible, easy to use

**SIWE (Sign-In With Ethereum)**
- Version: `^3.x`
- Purpose: Wallet-based authentication
- Standard: EIP-4361
- Why: Decentralized authentication without passwords

---

## Backend

### API Framework

**Next.js API Routes**
- Runtime: Node.js 20.x (Netlify)
- Features: Serverless functions, automatic routing
- Timeout: 10s (default), 60s (bottle processing)

### Authentication

**Supabase Auth**
- Type: JWT-based authentication
- Storage: PostgreSQL (Supabase-managed)
- Features: Session management, refresh tokens, user metadata
- Why: Seamless integration with Supabase DB

### Database ORM

**Supabase Client SDK**
- Version: `^2.x`
- Purpose: Database queries
- Features: Type-safe queries, real-time subscriptions
- Why: Official SDK, excellent TypeScript support

---

## Blockchain

### Smart Contracts

**Solidity**
- Version: `^0.8.20`
- Compiler: solc via Hardhat
- Features: NFTs (ERC-721-like), events, access control

**Hardhat**
- Version: `^2.x`
- Purpose: Smart contract development
- Features: Local node, testing, deployment scripts
- Plugins: `hardhat-deploy`, `@nomicfoundation/hardhat-toolbox`

**OpenZeppelin Contracts**
- Version: `^5.x`
- Purpose: Battle-tested contract primitives
- Used: `Ownable`, utility functions

### Ethereum Interaction

**ethers.js v6**
- Version: `^6.x`
- Purpose: Contract interaction (server-side)
- Features: Wallet management, transaction sending, event parsing
- Why: Most popular Ethereum library, great documentation

**viem**
- Version: `^2.x`
- Purpose: Contract interaction (client-side)
- Why: Modern, type-safe alternative to ethers.js

### Network

**Base Sepolia Testnet**
- Chain ID: 84532
- RPC: Alchemy (https://base-sepolia.g.alchemy.com/v2/...)
- Explorer: Basescan Sepolia
- Why: Fast, cheap, Ethereum L2, production-ready

**Alchemy**
- Service: RPC provider
- Features: Enhanced APIs, webhooks, analytics
- Why: Reliable, free tier sufficient, excellent uptime

---

## Storage

### Database

**Supabase (PostgreSQL)**
- Version: PostgreSQL 15.x
- Plan: Free tier
- Features: Webhooks, row-level security, real-time subscriptions
- Tables: `bottles`, `bottles_queue`, `likes`, `comments`
- Why: Managed PostgreSQL with generous free tier

**Supabase Edge Functions**
- Runtime: Deno
- Purpose: Serverless background tasks
- Functions: `process-bottle`, `sync-bottles`
- Why: Free, reliable, integrated with database webhooks

### File Storage (IPFS)

**Storacha (formerly Web3.Storage)**
- Service: Decentralized file storage
- Protocol: IPFS + Filecoin
- Gateway: storacha.link
- Features: Content addressing, permanent storage, free tier
- Why: True decentralization, no vendor lock-in

**@web3-storage/w3up-client**
- Version: `^16.x`
- Purpose: Storacha client library
- Authentication: Principal key + delegation proof
- Why: Official SDK, great TypeScript support

---

## Infrastructure

### Hosting

**Netlify**
- Plan: Free tier
- Features: CDN, automatic deployments, edge functions
- Build: Next.js static export
- Why: Excellent DX, generous free tier, global CDN

### CI/CD

**Git + GitHub**
- Workflow: Feature branches → PR → main
- Worktrees: Parallel development branches
- Why: Standard, familiar, free

**Netlify Deploy**
- Trigger: Push to main branch
- Build Command: `yarn build`
- Output Directory: `.next`
- Environment Variables: Set in Netlify dashboard

### Domain & DNS

**Netlify DNS**
- Managed by Netlify
- Automatic HTTPS via Let's Encrypt

---

## Development Tools

### Package Management

**Yarn**
- Version: 1.22.x
- Workspaces: Monorepo support
- Why: Faster than npm, good monorepo support

### TypeScript

**TypeScript**
- Version: `^5.x`
- Config: `tsconfig.json` (strict mode)
- Why: Type safety, better IDE support, fewer bugs

### Code Quality

**ESLint**
- Config: Next.js default
- Rules: Standard React/Next.js rules
- Why: Catch errors early, enforce consistency

### Version Control

**Git**
- Branching: Feature branches, main branch
- Worktrees: Used for parallel agent development
- Why: Industry standard

---

## Package Versions

### Client Dependencies

```json
{
  "@rainbow-me/rainbowkit": "^2.2.9",
  "@tanstack/react-query": "^5.90.6",
  "@loscolmebrothers/forever-message-ipfs": "^4.1.0",
  "@loscolmebrothers/forever-message-types": "^1.1.0",
  "next": "^14.x",
  "react": "^18.x",
  "react-dom": "^18.x",
  "react-konva": "^18.x",
  "@react-three/fiber": "^8.x",
  "three": "^0.x",
  "wagmi": "^2.19.2",
  "viem": "^2.38.6",
  "siwe": "^3.0.0",
  "swr": "^2.x",
  "sonner": "^1.x",
  "usehooks-ts": "^3.x",
  "@supabase/supabase-js": "^2.x",
  "ethers": "^6.x",
  "konva": "^9.x"
}
```

### IPFS Package Dependencies

```json
{
  "@web3-storage/w3up-client": "^16.4.2",
  "@ucanto/principal": "^9.0.0",
  "@ucanto/transport": "^9.1.0"
}
```

### Contract Dependencies

```json
{
  "hardhat": "^2.x",
  "@openzeppelin/contracts": "^5.x",
  "@nomicfoundation/hardhat-toolbox": "^5.x",
  "ethers": "^6.x"
}
```

---

## Technology Decisions

### Why This Stack?

#### Frontend: Next.js + React
**Pros:**
- Modern framework with great DX
- Built-in API routes (no separate backend)
- Excellent performance (SSR, ISR, SSG)
- Strong ecosystem and community

**Alternatives Considered:**
- Vite + React: No SSR, no API routes
- SvelteKit: Smaller ecosystem
- Vue/Nuxt: Team more familiar with React

#### Blockchain: Base Sepolia + ethers.js/viem
**Pros:**
- Base is Ethereum L2 (fast, cheap, compatible)
- ethers.js is battle-tested
- viem is modern and type-safe

**Alternatives Considered:**
- Mainnet Ethereum: Too expensive for testnet
- Polygon: Less Ethereum-aligned
- Arbitrum: Similar to Base, but Base has better docs

#### Storage: Supabase + Storacha
**Pros:**
- Supabase is managed PostgreSQL (no ops)
- Storacha is true decentralization (IPFS + Filecoin)
- Both have generous free tiers

**Alternatives Considered:**
- Self-hosted PostgreSQL: Requires ops
- Pinata IPFS: More expensive
- Arweave: Different model (permanent storage)

#### Hosting: Netlify
**Pros:**
- Easy deployments (git push)
- Generous free tier
- Global CDN
- Great DX

**Alternatives Considered:**
- Vercel: Similar, but Netlify's free tier is better
- AWS: Too much manual config
- Self-hosted: Requires ops

#### Wallet: RainbowKit + wagmi
**Pros:**
- Beautiful UI out of the box
- Supports 30+ wallets
- Mobile-friendly
- Great TypeScript support

**Alternatives Considered:**
- Web3Modal: Less polished UI
- ConnectKit: Newer, less mature
- Custom wallet connect: Too much work

---

## Performance Characteristics

### Bundle Size
- Client bundle: ~800KB (gzipped)
- Largest contributors: Three.js, React Three Fiber, ethers.js
- Optimization: Tree-shaking, dynamic imports

### Load Times
- First Contentful Paint: ~1.5s
- Time to Interactive: ~2.5s
- Largest Contentful Paint: ~2.0s

### API Response Times
- Bottle list: ~200ms (DB query)
- Create bottle (queue): ~100ms (fast response)
- Like/unlike: ~1-2s (DB + blockchain)
- Comments: ~3-5s (IPFS + blockchain)

### Database Performance
- Typical query: <100ms
- Indexed columns: `created_at`, `bottle_id`, `user_id`
- Connection pooling: Managed by Supabase

---

## Security Considerations

### Frontend Security
- JWT tokens in memory only (not localStorage)
- HTTPS only (enforced by Netlify)
- CSP headers (content security policy)

### Backend Security
- Environment variables for secrets
- JWT validation on all protected routes
- SIWE signature verification
- Rate limiting (Netlify built-in)

### Smart Contract Security
- Audited OpenZeppelin contracts
- No reentrancy vulnerabilities
- Access control with Ownable
- Event-based architecture (no storage bloat)

### IPFS Security
- Content addressing (tamper-proof)
- Delegation proof not a secret (safe to commit)
- Principal key in env vars only

---

## Future Technology Considerations

### Short Term (3-6 months)
- **Viem migration**: Replace ethers.js with viem everywhere
- **TanStack Query v5**: Better data fetching patterns
- **Mainnet deployment**: Base mainnet launch

### Medium Term (6-12 months)
- **Account abstraction**: Gasless transactions via paymasters
- **Subgraph**: The Graph for faster event queries
- **Redis cache**: Hot data caching layer

### Long Term (12+ months)
- **React Server Components**: Full Next.js 14+ features
- **Zustand**: Better global state management
- **Test suite**: Playwright E2E + Vitest unit tests
