# Product Requirements Document (PRD) — Linkly

## 1. Product Overview
**Product Name**: Linkly  
**Repository**: [https://github.com/gitmxr/linkly.git](https://github.com/gitmxr/linkly.git)  
**Description**: Linkly is a lightweight, modern, and high-performance URL shortener service offering instant link compression, custom vanity aliases, expiration timestamps, atomic click tracking analytics, QR code generation, and a sleek glassmorphic UI.

---

## 2. Target Audience & Use Cases
- **Content Creators & Marketers**: Creating clean, memorable links for social media, marketing campaigns, and print materials (via QR codes).
- **Developers & Teams**: Integrating link shortening into applications, microservices, and internal workflows via a clean REST API.
- **Everyday Users**: Quickly shortening long URLs with custom aliases and tracking engagement.

---

## 3. Core Features & Specifications

### 3.1 URL Shortening Engine
- **Short Code Generation**: Cryptographically secure 6-character Base62 string generation (`[a-zA-Z0-9]`) using rejection sampling to eliminate modulo bias.
- **Collision Handling**: Automatic retry loop with maximum retry cap to handle rare hash/code collisions.
- **Custom Vanity Aliases**: Users can supply custom aliases (3–30 characters, alphanumeric with `-` and `_`). Duplicate aliases trigger a HTTP 409 Conflict.
- **URL Normalization**: Automatically trims input and prepends `http://` if no protocol is specified.
- **Expiration Dates**: Optional `expires_at` ISO-8601 timestamp for temporary short links.

### 3.2 Redirection & Analytics Engine
- **HTTP 302 Redirection**: Instant redirection (`GET /:code`) directly to destination target.
- **Atomic Click Tracking**: Increments `clicks` counter upon successful redirection.
- **Link Analytics Endpoint**: `GET /api/urls/:code/analytics` returns click count and creation timestamp.

### 3.3 Frontend Web Application
- **Modern Glassmorphic UI**: Single-page application styled with CSS custom properties and ambient glowing gradient background.
- **Theme Switching**: Dark and light mode toggle persisted via `localStorage`.
- **QR Code Preview**: Instant QR code generation for every created short link.
- **Copy to Clipboard**: One-click copying of generated short links.
- **Recent Links History**: Persists up to 10 recently created links in browser `localStorage`.
- **Interactive Analytics Modal**: Quick view modal to inspect click metrics directly in the browser.

### 3.4 Data & Deployment Architecture
- **Database**: LibSQL / SQLite (`@libsql/client`) enabling zero-configuration local file storage (`data/urls.db`), in-memory testing (`:memory:`), and remote Cloud DB via Turso (`libsql://...`).
- **Serverless & Edge Ready**: Full compatibility with Vercel Serverless Functions (`api/index.ts` + `vercel.json`).

---

## 4. Technical Constraints & Non-Functional Requirements
- **Language & Runtime**: Node.js v18+, TypeScript with strict type-checking enabled.
- **API Response Format**: Consistent JSON responses with standard error payloads.
- **Validation**: Strict schema validation powered by Zod.
- **Test Coverage**: Full unit and integration test suites using Jest and Supertest (>90% target coverage).
