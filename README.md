# Hack4IMPACTTrack2-DedSec

![HACK4IMPACT](https://img.shields.io/badge/HACK4IMPACT-Track%202-0D2163?style=for-the-badge)
![SDIS KIIT](https://img.shields.io/badge/SDIS-KIIT%202026-22C55E?style=for-the-badge)
![Domain](https://img.shields.io/badge/Domain-Green%20Infrastructure%20%26%20Smart%20Cities-2DD4BF?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)
[![Live Demo](https://img.shields.io/badge/LIVE-DEMO-111827?style=for-the-badge&logo=vercel&logoColor=white)](https://thriftapp-3ofiii.vercel.app/)

Thrift marketplace for reducing fast-fashion waste through circular commerce.

## Live App
### Production Deployment
Open the live app here:

**https://thriftapp-3ofiii.vercel.app/**

### Direct Login Experience
Use these demo credentials for a smooth experience:

- Seller login:
   - Store name: `hiten_thrifts`
   - Password: `iamhiten`
- Buyer login:
   - Email: `nidhilogins@gmail.com`
   - Password: `160222`

## Detailed Presentation
For a clearer walkthrough of the problem statement and proposed solution, view the project presentation:

**https://docs.google.com/presentation/d/16ns3WCkXV74T7MjbYxgThJWlvvG6jgJl/edit?usp=sharing&ouid=106225712437847549190&rtpof=true&sd=true**

## Team
- Team Name: DedSec
- Team Lead: Nidhi Singh
- Domain: Green Infrastructure and Smart Cities

## Problem Statement
Fast Fashion Waste: The Hidden Cost of Throwaway Clothing Culture.

The platform helps buyers and sellers exchange pre-loved clothing with a better UX, seller tooling, and integrated order flows.

## Architecture
This repository is full-stack:

- Frontend: React 19 + Redux Toolkit (in `src`)
- Backend: Express + SQLite (dev) / PostgreSQL (production) (in `server`)
- Media: local uploads in dev, Cloudinary in production
- Payment: Razorpay client integration

## Tech Stack
| Layer | Technologies |
|---|---|
| Frontend | React, React DOM, React Router DOM |
| State | Redux Toolkit, React Redux |
| Backend | Node.js, Express, CORS, Multer |
| Database | SQLite3 (dev), PostgreSQL via `pg` (prod) |
| Media | Cloudinary |
| Tooling | react-scripts (CRA), nodemon, concurrently |
| Testing | React Testing Library, Jest DOM |

## Project Structure
```text
Hack4IMPACTTrack2-DedSec/
├── public/                  # Frontend static assets
├── src/                     # Frontend app (components, redux, utils)
├── server/
│   ├── server.js            # Express API entrypoint
│   ├── config/
│   ├── migrations/
│   ├── routes/
│   ├── scripts/
│   └── data/
├── API_DOCUMENTATION.md
├── DEPLOYMENT_GUIDE_NEON_CLOUDINARY.md
├── package.json
└── README.md
```

## Prerequisites
- Node.js 18+
- npm 9+

## Environment Variables

### Frontend (`.env` in repo root)
You can copy from `.env.template`.

- `REACT_APP_API_BASE_URL` (optional)
   - Leave empty for local proxy-based development.
   - Set to full backend URL in deployed frontend builds.
- `REACT_APP_BACKEND_PORT` (optional, default `8000`)
   - Used by `src/setupProxy.js` to proxy `/api` and `/uploads`.
- `REACT_APP_RAZORPAY_KEY` or `REACT_APP_RAZORPAY_KEY_ID` (optional)
   - If absent, app falls back to a test key in code.

### Backend (`server/.env`)
For production examples, see `server/.env.production.template`.

Important keys:
- `NODE_ENV`
- `PORT`
- `CORS_ORIGIN`
- `DATABASE_URL` (or `SUPABASE_DATABASE_URL`)
- `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`, `CLOUDINARY_FOLDER`
- `RAZORPAY_KEY_ID`, `RAZORPAY_KEY_SECRET`

## Local Development

1. Clone and enter repository:
    ```bash
    git clone https://github.com/nidhiatwork01-cmyk/Hack4IMPACTTrack2-DedSec.git
    cd Hack4IMPACTTrack2-DedSec
    ```

2. Install dependencies:
    ```bash
    npm install
    ```

3. (Optional) Create root `.env` from template and edit values.

4. Start full-stack dev mode (recommended):
    ```bash
    npm run dev
    ```
    This runs backend (`nodemon server/server.js`) and frontend (`react-scripts start`) together.

5. Open `http://localhost:3000`.

## Useful Scripts
- `npm run dev`: run frontend + backend together
- `npm run server`: run backend only
- `npm start`: run frontend only
- `npm run build`: production frontend build
- `npm test`: run frontend tests
- `npm run start:production`: start backend in production mode

## Deployment Notes
- Frontend build output is generated in `build/`.
- Backend uses PostgreSQL in production and expects `DATABASE_URL` or `SUPABASE_DATABASE_URL`.
- See deployment docs:
   - `DEPLOYMENT_GUIDE_NEON_CLOUDINARY.md`
   - `API_DOCUMENTATION.md`

## Current Status
- Frontend builds successfully with `npm run build`.
- Backend entry file passes syntax check (`node --check server/server.js`).
- Workspace diagnostics currently report no code errors.
