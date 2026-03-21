# ThriftApp Project Dossier

## 1. Project Identity

**Project name:** ThriftApp (brand surfaced in UI as **ThriftIt** in several screens)  
**Category:** Sustainable fashion marketplace (buyer + seller)  
**Core model:** Multi-user thrift commerce where products, carts, wishlists, and orders can be persisted in a shared cloud database and accessed across devices.

This document captures both:
- The **product vision** (why this idea is powerful)
- The **current implementation reality** (what is already built in this repo)

---

## 2. Problem It Solves

Fast-fashion shopping has three big problems:
- High environmental waste and emissions
- Clothing under-utilization (good items stay unused)
- Fragmented resale experiences with low trust and poor UX

ThriftApp addresses these with a single platform where:
- Sellers list pre-loved clothing quickly
- Buyers discover unique low-cost products
- Every purchase can show sustainability impact
- Data can be shared across all systems through cloud deployment

---

## 3. Why This Idea Is Strong (and Potentially Revolutionary)

### 3.1 Circular fashion with execution, not just messaging
- The platform operationalizes resale inventory, not just awareness content.
- It helps extend garment life and reduce demand for new production.

### 3.2 Sustainability metrics integrated into shopping UX
- Product and order flows include climate-impact framing (CO2 and water savings).
- This turns sustainability from "marketing text" into user-visible value.

### 3.3 Low-friction two-sided marketplace flow
- Buyer journey is mobile-first and fast (browse, wishlist, cart, checkout).
- Seller journey supports registration, store login, listing, and dashboard analytics.

### 3.4 Cross-device persistence model
- With Neon/Postgres + Cloudinary, listings and data are available across systems (not tied to a single machine).

### 3.5 Hybrid resilience approach
- Local state persistence is used for UX continuity.
- Backend sync, backup, and restore endpoints exist for account-linked recovery.

---

## 4. Product Experience Summary

### 4.1 Buyer capabilities
- Browse available products
- Search and category filtering
- View product details and sustainability insights
- Add/remove cart items
- Add/remove wishlist items
- Checkout via Razorpay flow
- View order history (derived from sold products in current UI logic)
- View profile/settings/coupons

### 4.2 Seller capabilities
- Seller registration flow (KYC-style fields + demo OTP + password)
- Seller login using `storeName + password`
- Product listing with image upload
- Seller dashboard with inventory and revenue stats
- Product deletion and product-level visibility

### 4.3 Admin/ops capabilities (API-level and component-level)
- Storage stats endpoint
- Health-check endpoint
- Cleanup endpoint
- Backup and restore endpoints
- A `StorageDashboard` React component exists for these capabilities (not currently routed in App routes)

---

## 5. Current Architecture

## 5.1 High-level deployment architecture (recommended)
- **Frontend:** Vercel static deployment (React build)
- **Backend:** Render web service running `node server/server.js`
- **Database:** Neon PostgreSQL using pooled connection string
- **Media storage:** Cloudinary for globally accessible product images

## 5.2 Runtime split by environment
- **Development backend DB:** SQLite (`server/data/thriftapp.db`)
- **Production backend DB:** PostgreSQL via `DATABASE_URL` (or `SUPABASE_DATABASE_URL` alias)
- **Image storage fallback:**
  - Cloudinary if credentials are provided
  - Local `server/uploads` otherwise

---

## 6. Technology Stack

### 6.1 Frontend
- React 19
- React Router
- Redux Toolkit
- Framer Motion
- Lucide icons
- Tailwind-style utility classes in components

### 6.2 Backend
- Node.js + Express
- Multer for file upload
- `pg` for Postgres
- `sqlite3` for local/dev DB
- Cloudinary SDK
- CORS, dotenv

### 6.3 Dev and scripts
- Nodemon, concurrently, cross-env
- Data export/import scripts
- Cloudinary migration script
- Port cleanup script

---

## 7. Data Model (Current Core Tables)

Defined in `server/migrations/001_init_postgres.sql` and mirrored in SQLite create-table logic:
- `users`
- `products`
- `cart_items`
- `wishlist_items`
- `orders`
- `user_sessions`
- `app_settings`
- `seller_accounts`

Key entity notes:
- `products` stores listing metadata, seller reference fields, status (`available`/`sold`), image URL/path.
- `seller_accounts` stores seller credentials and profile JSON.
- `app_settings` is used for user backup payload storage.

---

## 8. API Surface (Implemented)

### 8.1 Health
- `GET /api/health`

### 8.2 User auth and account
- `POST /api/users/register`
- `POST /api/users/login`

### 8.3 Cart
- `GET /api/cart/:userId`
- `POST /api/cart`
- `DELETE /api/cart/:userId/:productId`

### 8.4 Wishlist
- `GET /api/wishlist/:userId`
- `POST /api/wishlist/toggle`

### 8.5 Orders
- `POST /api/orders`
- `GET /api/orders/:userId`

### 8.6 Sync and backup
- `POST /api/sync/user-data`
- `GET /api/sync/user-data/:userId`
- `POST /api/backup/create`
- `POST /api/backup/restore`

### 8.7 Products
- `GET /api/products`
- `POST /api/products` (multipart image upload)
- `PATCH /api/products/:id`
- `DELETE /api/products/:id`

### 8.8 Seller auth
- `POST /api/seller-auth/register`
- `POST /api/seller-auth/login`

### 8.9 Admin ops
- `GET /api/admin/storage-stats`
- `GET /api/admin/health-check`
- `POST /api/admin/cleanup`

---

## 9. Real-Time and Cross-System Data Behavior

### 9.1 What is "real-time" today
- Product refresh polling every **30 seconds** in `MainLayout`
- Extra refresh on browser tab focus/visibility change
- Sync helpers exist for cart/wishlist sync cycles

### 9.2 What this means in practice
- Multi-device updates are near-real-time (poll interval based), not instant push.
- For true instant updates, WebSocket/SSE would be the next upgrade.

### 9.3 Cross-system visibility prerequisites
- Backend deployed and reachable
- Frontend points to deployed backend URL
- Cloud database configured correctly (pooled URL)
- Cloudinary configured so image URLs are globally accessible

---

## 10. Security and Credential Handling (Current State)

### 10.1 Implemented
- Buyer user password hashing on backend registration/login path (`pbkdf2`)
- Seller password hashing and verification
- CORS allow-list support via `CORS_ORIGIN`
- Sensitive env files ignored by `.gitignore`

### 10.2 Important caveat
- Buyer login/signup in current frontend Redux flow still relies heavily on local state (localStorage) rather than full backend-auth session model.
- This works for demo/prototype UX but should be unified with backend auth for production-grade identity consistency.

---

## 11. Repository Structure (Important Paths)

### 11.1 Root
- `README.md`
- `DEPLOYMENT_GUIDE_NEON_CLOUDINARY.md`
- `API_DOCUMENTATION.md`
- `.env.template`
- `.env.production.template`
- `PROJECT_DOSSIER.md` (this file)

### 11.2 Backend
- `server/server.js` (main API and runtime)
- `server/migrations/001_init_postgres.sql`
- `server/scripts/*` (export/import/migration/deploy utilities)
- `server/.env.production.template`

### 11.3 Frontend
- `src/App.js` (routes + metadata)
- `src/components/*` (buyer/seller/common/auth pages)
- `src/redux/*` (store + slices + persistence middleware)
- `src/utils/*` (API base, local storage, DB helpers, sustainability, Razorpay)

---

## 12. Deployment and Environment Variables

## 12.1 Backend-critical variables
- `NODE_ENV`
- `PORT`
- `CORS_ORIGIN`
- `DATABASE_URL` (primary)
- `SUPABASE_DATABASE_URL` (supported alias)

## 12.2 Media storage variables
- `CLOUDINARY_CLOUD_NAME`
- `CLOUDINARY_API_KEY`
- `CLOUDINARY_API_SECRET`
- `CLOUDINARY_FOLDER` (optional, default `thriftapp`)

## 12.3 Frontend-critical variables
- `REACT_APP_API_BASE_URL`
- `REACT_APP_RAZORPAY_KEY` (or `REACT_APP_RAZORPAY_KEY_ID` alias)

## 12.4 Local dev proxy behavior
- If `REACT_APP_API_BASE_URL` is empty, frontend uses relative `/api` and `/uploads`.
- `src/setupProxy.js` routes those paths to backend port (resolved from env/default).

---

## 13. Advantages of This Project

### 13.1 Business and user advantages
- Affordable fashion access
- Seller monetization from unused inventory
- Strong sustainability narrative with measurable indicators
- Mobile-friendly UX and two-sided flow

### 13.2 Technical advantages
- Clear separation of frontend/backend responsibilities
- Environment-aware DB strategy (SQLite dev, Postgres prod)
- Image storage abstraction (Cloudinary vs local fallback)
- Deployment runbook already documented
- Data export/import tooling for migration

### 13.3 Growth advantages
- Can evolve into social/community thrift platform
- Can add recommendation engine and trend analytics
- Can integrate warehouse/logistics and full seller payouts

---

## 14. Current Gaps and Technical Debt (Important, Honest View)

These do not invalidate the project; they define the next engineering priorities:

1. **Duplicate product GET route**
- `GET /api/products` appears twice in `server/server.js`.
- Harmless today but should be cleaned for clarity.

2. **Mixed frontend auth model**
- Buyer auth mostly local Redux/localStorage.
- Seller auth is backend-backed.
- A unified backend-auth/session model is recommended.

3. **SQLite-to-Postgres SQL compatibility edge cases**
- Some queries use SQLite-style patterns (`INSERT OR REPLACE` logic conversion exists but can still be fragile across SQL dialect nuances).
- A stricter dialect-safe query layer would harden production behavior.

4. **Order history source in UI**
- Order history page currently derives much from sold products in state.
- Should be driven directly from `orders` API data for canonical accuracy.

5. **Real-time is polling-based**
- Good for MVP.
- WebSocket/SSE needed for true push-based real-time.

6. **Security hardening roadmap**
- Tokenized auth sessions (JWT/cookies), stricter input validation, and role-based access control should be strengthened for production scale.

---

## 15. What Makes It Deployment-Worthy Right Now

For an MVP/prototype launch, this project is strong because it already includes:
- End-to-end buyer/seller workflows
- Backend persistence with cloud DB support
- Cloud image hosting strategy
- Deployment guide and environment templates
- Operational endpoints for health and storage visibility

With Neon + Cloudinary + Render/Vercel configured correctly, it can function as a live multi-system marketplace.

---

## 16. Suggested Next Evolution (Roadmap)

### Phase 1: Stabilization
- Remove duplicate routes and dead files
- Unify auth across buyer and seller
- Route order history to real orders API data
- Tighten validation and error handling

### Phase 2: Real-time and scale
- Add WebSocket/SSE for inventory and cart updates
- Add background jobs for async tasks
- Add observability (request tracing, structured logs)

### Phase 3: Product intelligence
- Personalized recommendations
- Seller growth analytics
- Trust signals (seller score, listing quality, moderation)

### Phase 4: Commerce maturity
- Robust payments backend order verification
- Refund/dispute workflows (if business policy changes)
- Shipping integrations and fulfillment lifecycle

---

## 17. Mission Statement (Pitch Version)

ThriftApp is building a practical circular-fashion commerce layer: a mobile-first marketplace where pre-loved products are easy to list, easy to discover, and measurable in environmental impact.  
Its strength is not just sustainability messaging, but an implemented marketplace architecture that supports shared cloud data, seller operations, and growth toward real-time, production-grade resale commerce.

---

## 18. Quick Commands

### Local development
```bash
npm install
npm run server
npm start
```

### Combined local run
```bash
npm run dev
```

### Data/export and migration utilities
```bash
npm run deploy:export
npm run deploy:supabase
npm run deploy:migrate-images
```

### Production-like backend run (local)
```bash
npm run start:production
```

