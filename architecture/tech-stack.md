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

**Tailwind CSS**
- Version: `^3.x`
- Approach: Utility-first CSS framework with custom design system
- Configuration: Custom theme in `tailwind.config.js`
- Why: Rapid development with consistent design tokens

**Design Systems**

1. **Glass-Morphism (Ocean Aesthetic)**
   - Color palette: Cyan/turquoise (`#40E0D0`, `#20B2AA`, `#7FFFD4`)
   - Components: `.glass-surface`, `.glass-button`, `.glass-button-sm`
   - Effects: Backdrop blur (10px/16px), gradient overlays, glow shadows
   - Used for: UI controls, modals, interactive elements
   - Pattern: Semi-transparent backgrounds with aqua tint and subtle glows

2. **Parchment Aesthetic**
   - Color palette: Beige/brown (`#f5f5dc`, `#e8e4d0`, `#2d1a0a`)
   - Components: `.parchment-surface`, `.parchment-button`, `.parchment-modal`
   - Used for: Bottle creation forms, content-focused overlays
   - Pattern: Warm, vintage appearance with subtle shadows

**Custom CSS Classes** (`app/globals.css`):
- Glass components with hover states and glow effects
- Parchment components with vintage styling
- Animation utilities (fade-in, slide-in, bounce-in)
- Custom keyframe animations

### Utilities

**anime.js**
- Version: `^4.2.2`
- Purpose: Professional animation library
- Features: Easing functions, timeline control, promise-based
- Why: High-quality animations for bottle creation sequence
- Used in: CreateBottleModal (roll, sparkle burst, fly-away)

**usehooks-ts**
- Version: `^3.x`
- Purpose: React hooks library
- Hooks Used: `useWindowSize`, `useDebounce`

**sonner**
- Version: `^2.x`
- Purpose: Toast notifications
- Why: Beautiful, accessible, easy to use

**SIWE (Sign-In With Ethereum)**
- Version: `^3.x`
- Purpose: Wallet-based authentication
- Standard: EIP-4361
- Why: Decentralized authentication without passwords

### Animation & UI Libraries

**anime.js (animejs)**
- Version: `^4.2.2`
- Purpose: Professional animation orchestration
- Features: Timeline sequencing, spring easing, promise-based animations
- Why: High-quality, production-ready animations with precise control
- Used in:
  - `LoadingScreen.tsx`: Sequential text reveals with sprite animations
  - `CreateBottleModal.tsx`: Parchment roll, bottle filling, sparkle burst sequences
  - `LOSCOLMEBROTHERSLogo.tsx`: Spring-based hover animations
  - `SparkleEffect.tsx`: Burst pattern animations

**React Spring (via @react-spring/konva)**
- Version: `^9.7.3`
- Purpose: Physics-based animations for Konva canvas
- Why: Spring physics for natural motion in canvas elements
- Used in: Bottle physics and interactive elements

**Manual RAF (Request Animation Frame)**
- Purpose: Custom frame-by-frame animations
- Used in: Sprite-based effects and performance-critical animations

### Typography

**ApfelGrotezk** (Custom Font)
- Source: `https://assets.loscolmebrothers.com/fonts/ApfelGrotezk/`
- Weights: Regular (400), Mittel (500), Satt (700), Fett (800)
- Format: OpenType (.otf)
- Usage: Primary UI font for all interface text
- Applied via: Tailwind utility class `.font-apfel`

**AndreaScript** (Custom Font)
- Source: `https://assets.loscolmebrothers.com/fonts/AndreaScript.ttf`
- Weight: Normal
- Format: TrueType (.ttf)
- Usage: Decorative text (e.g., "forever" in loading screen)
- Style: Script/cursive font for emphasis and branding

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
- Config: `next/core-web-vitals` extended with Prettier
- Plugins: `eslint-plugin-prettier`
- Rules: Standard React/Next.js rules + Prettier formatting
- Why: Catch errors early, enforce consistency

**Prettier**
- Version: `^3.6.2`
- Config: `.prettierrc` with project standards
- Format: Semi-colons, double quotes, 2-space tabs, LF line endings
- Integration: ESLint plugin for unified linting
- Scripts: `yarn format`, `yarn format:check`
- Why: Consistent code formatting across the codebase

**Testing Infrastructure**
- **Cypress**: E2E testing framework (v15.6.0)
- **Jest**: Unit testing framework (v30.2.0)
- **React Testing Library**: Component testing (@testing-library/react v16.3.0)
- Config: Next.js 14 App Router compatible
- Scripts: `yarn test`, `yarn test:e2e`, `yarn test:watch`, `yarn test:coverage`
- Documented in: `TESTING.md`
- Why: Quality assurance and regression prevention

**CI/CD Verification**
- Script: `yarn ci`
- Pipeline: `lint → format:check → build → test → test:e2e`
- Purpose: Comprehensive quality gates before deployment
- Ensures: Code quality, formatting consistency, build success, test passage

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
  "@loscolmebrothers/forever-message-types": "^3.0.0",
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
  "sonner": "^2.x",
  "animejs": "^4.2.2",
  "@react-spring/konva": "^9.7.3",
  "date-fns": "^4.1.0",
  "usehooks-ts": "^3.x",
  "@supabase/supabase-js": "^2.x",
  "ethers": "^6.x",
  "konva": "^9.x"
}
```

### Dev Dependencies

```json
{
  "@types/jest": "^30.0.0",
  "@types/node": "^24.10.1",
  "@types/react": "^18.3.0",
  "@types/react-dom": "^18.3.0",
  "@testing-library/react": "^16.3.0",
  "@testing-library/jest-dom": "^6.9.1",
  "@testing-library/user-event": "^14.6.1",
  "cypress": "^15.6.0",
  "jest": "^30.2.0",
  "jest-environment-jsdom": "^30.2.0",
  "prettier": "^3.6.2",
  "eslint-config-prettier": "^10.1.8",
  "eslint-plugin-prettier": "^5.5.4",
  "typescript": "^5.0.0"
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

## UI/UX Design Patterns

### Bottle Creation Flow

**CreateBottleModal Design:**
- **Layout**: Minimal 3-line parchment strip (75px textarea)
- **Message Length**: 120 characters maximum (optimized for readability)
- **Input**: Single-line textarea (no multi-line)
- **Submit Button**: Red wax seal (96px desktop / 80px mobile)
- **Position**: Seal centered, half-in/half-out of modal
- **Character Counter**: 11px, floated right, 0.4 opacity
- **Close Button**: 24px, inside modal, black background, 0.6 opacity

**Animation Sequence (anime.js):**
1. **Roll Phase** (~800ms)
   - Parchment rolls up vertically (scaleY: 0.1)
   - Easing: `inOut(quad)` for professional motion
   - Seal scales and hides during roll

2. **Bottle Filling Phase** (~1200ms)
   - Seal transforms into bottle appearance
   - Bottle grows with elastic bounce
   - Easing: `out(elastic(1, .6))` for playful effect
   - Opacity animates from 0 to 1

3. **Sparkle Burst** (~800ms)
   - 16 sparkle sprites appear around bottle
   - Each sparkle: rotation, scale pulse, fade out
   - Easing: `out(expo)` for explosive burst
   - Uses pixellab sparkle sprites (32×32px)

4. **Fly Away** (~600ms)
   - Bottle flies upward off-screen
   - Easing: `in(quad)` for smooth departure
   - Modal closes after animation completes

**Toast Notifications:**
- Success toast appears AFTER modal closes
- Timing ensures user sees completion
- Uses Sonner library for consistency

### Parchment Aesthetic

**Visual Theme:**
- Background: Beige parchment texture (#f5f5dc)
- Overlay: Semi-transparent parchment image (30% opacity)
- Fonts: AndreaScript (headings), ApfelGrotezk (body)
- Colors: Dark brown text (#2c1810)
- Shadows: Subtle for depth without modern feel

**Assets:**
- Wax seal: `public/assets/effects/red-wax-seal.png` (64×64px)
- Sparkles: `public/assets/effects/sparkle-*.png` (32×32px)
- Bottle sprites: `public/assets/bottle-sprites/` (various)

### Loading Screen Experience

**LoadingScreen Component Design:**
- **Purpose**: Onboarding experience explaining app functionality
- **Layout**: Full-screen overlay (`z-index: 9999`) with dark ocean background
- **Animation Engine**: anime.js timeline orchestration

**Animation Sequence** (4.6s total):
1. **Text Line 1** (250ms): "Connect your wallet."
   - Spring entrance (bounce: 0.4, translateY: 10→0)

2. **Text + Bottle Sprite** (900ms): "Cast a bottle." + sprite reveal
   - Bottle sprite scales from 0.5→1 with spring bounce (0.65)
   - Positioned inline with text

3. **Text Line 2** (1800ms): "Read and like ❤️ other people's bottles."
   - Heart sprite appears simultaneously
   - Spring animation with slight upward position

4. **"Forever" Text** (2800ms): Script font reveal
   - Uses AndreaScript font at 2xl size
   - Animated bottle sprite and sparkles appear

5. **Sparkle Animations** (3200ms): Continuous loop
   - Two sparkle sprites rotate and pulse
   - Rotate: 0→180→360° and 0→-180→-360°
   - Opacity pulse: 1→0.6→1
   - Duration: 2000ms / 2500ms loops

6. **Background Gradient** (3400ms): Ambient animation
   - Radial gradient with cyan/turquoise glow
   - Moves in figure-8 pattern
   - 8000ms loop duration

7. **"Dive in" Button** (3200ms): CTA reveal
   - Glass button with fade-in
   - Click triggers blur exit animation (500ms)

**Social Links** (bottom-center):
- GitHub repository link
- Base blockchain explorer link
- Disclaimer: "(We pay for all the blockchain fees)"
- Fade-in at 3400ms with spring animation

**Responsive Behavior**:
- Text wraps naturally on mobile via flexbox
- Sprites remain inline with text
- Social links stay centered at bottom

### Glass-Morphism Design System

**Color Philosophy**: Water/ocean aesthetic with cyan/turquoise tones

**Component Classes**:

1. **`.glass-surface`** - Base surface
   - Backdrop blur: 10px
   - Background: Gradient from `rgba(64, 224, 208, 0.12)` to `rgba(32, 178, 170, 0.16)`
   - Border: `rgba(127, 255, 212, 0.25)` (aquamarine glow)
   - Shadow: Inset light + outer shadow with no glow at rest

2. **`.glass-button`** - Interactive button
   - Base: Same as glass-surface + padding (px-6 py-3)
   - Font: ApfelGrotezk Medium (500 weight)
   - Hover: Scale 1.02 + intensified gradient + 20px aqua glow
   - Active: Scale 0.98 + 30px aqua glow
   - Transition: 300ms ease-out

3. **`.glass-button-sm`** - Small variant
   - Padding: px-4 py-2
   - Font size: text-sm
   - Same hover/active behavior

**Usage Pattern**:
```tsx
<button className="glass-button">
  Dive in
</button>
```

**Design Tokens** (Tailwind config):
```js
colors: {
  glass: {
    tint: { light: "rgba(64, 224, 208, 0.04)", DEFAULT: "rgba(64, 224, 208, 0.08)" },
    border: { DEFAULT: "rgba(127, 255, 212, 0.25)", dark: "rgba(72, 209, 204, 0.35)" },
    text: { DEFAULT: "rgba(255, 255, 255, 0.9)", muted: "rgba(255, 255, 255, 0.7)" }
  }
}
```

### Los Colmebrothers Branding

**LOSCOLMEBROTHERSLogo Component**:
- **Position**: Fixed bottom-center (`left-1/2, transform: translateX(-50%)`)
- **Layer**: `z-index: 50`
- **Appearance**: Glass button with cyan-100 background, 30% opacity
- **Content**: Landscape SVG logo from `assets.loscolmebrothers.com`
- **Dimensions**: 120×30 SVG scaled to h-4 (16px height)
- **Link**: External link to `loscolmebrothers.com`

**Hover Animation** (anime.js + spring easing):
- Duration: 300ms entrance / 500ms exit
- Easing: `spring({ bounce: 0.4, duration: 400 })`
- Transform: `scale(1) → scale(1.1)` on hover
- Maintains translateX(-50%) for centering

**Styling Pattern**:
- Uses `.glass-button-sm` class for glass aesthetic
- Custom opacity (30%→50% on hover)
- No bottom border radius for "emerging from bottom" effect

### Responsive Design Patterns

**Mobile-First Approach**:
- Flexbox with `flex-wrap` for natural text wrapping
- Window size hooks (`useWindowSize` from usehooks-ts)
- Breakpoint: 768px for desktop layout changes
- Component-level responsive state (`isMobile` state)

**Loading Screen Responsive Behavior**:
```tsx
<div className="flex flex-wrap items-center justify-center gap-x-2 md:gap-x-3">
  {/* Content wraps on mobile, stays inline on desktop */}
</div>
```

**Pattern**:
- Use `gap-x-{size}` for horizontal spacing
- Use `md:gap-x-{larger}` for desktop expansion
- Keep sprites inline with `whitespace-nowrap` on text sections
- Allow natural wrapping at section boundaries

### Bottle Modal (Temporary)

Currently displays "Under Construction" placeholder:
- Simple modal with construction emoji (🚧)
- Parchment background maintained for consistency
- Message: "We're working on making this bottle even more special. Check back soon!"
- Full IPFS content loading will be implemented later

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
