# Hack4IMPACTTrack2-DedSec

![HACK4IMPACT](https://img.shields.io/badge/HACK4IMPACT-Track%202-0D2163?style=for-the-badge)
![SDIS KIIT](https://img.shields.io/badge/SDIS-KIIT%202026-22C55E?style=for-the-badge)
![Domain](https://img.shields.io/badge/Domain-Green%20Infrastructure%20%26%20Smart%20Cities-2DD4BF?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

## Team Name
DedSec

## Team Members
- Nidhi Singh (Team Leader)

## Domain
Green Infrastructure & Smart Cities

## Problem Statement
Fast Fashion Waste: The Hidden Cost of Throwaway Clothing Culture

The global fashion industry generates 92 million tonnes of textile waste
every year. In India, 70% of urban wardrobe clothing is worn fewer than
3 times before being discarded. Existing resale platforms are fragmented
with poor UX and no structured seller tools, driving continued demand for
new fast-fashion production and accelerating carbon emissions and water
consumption.

## Tech Stack
| Layer | Technologies |
|---|---|
| Frontend Framework | React 19, React DOM |
| State Management | Redux Toolkit, React Redux |
| Routing | React Router DOM |
| UI & Animations | CSS, Framer Motion, Lucide React |
| Build Tooling | React Scripts (Create React App) |
| Testing | React Testing Library, Jest DOM |

## Repository Structure
```text
Hack4IMPACTTrack2-DedSec/
├── public/
│   ├── index.html
│   ├── manifest.json
│   └── media/
├── src/
│   ├── components/
│   │   ├── auth/
│   │   ├── buyer/
│   │   ├── common/
│   │   ├── seller/
│   │   └── admin/
│   ├── redux/
│   │   ├── slices/
│   │   └── store.js
│   ├── utils/
│   ├── App.js
│   ├── index.js
│   ├── setupProxy.js
│   └── context/
├── package.json
├── package-lock.json
└── README.md
```
 
## Checkpoint 1 — [24hr stable]
**Pushed:** 20th March, 8:00 PM

### What is done
- Frontend project structure finalized in `HACK4IMPACTTRACK2-DEDSEC` with:
  - `src/`
  - `public/`
  - `package.json`
  - `package-lock.json`
- Core frontend modules are in place:
  - Auth, buyer, seller, admin, profile, settings, orders
  - Shared UI components and layout
  - Redux store + slices + middleware
  - Utility and theme/context setup
- Static assets and media integrated in `public/media`.
- Build validation completed: frontend compiles successfully (`npm run build` passed).

### What is pending
- Start dev server and run full page-level manual QA (`npm start`).
- Backend API/proxy env wiring verification for local development.
- Final UI regression pass and route-by-route checks.

### Blockers
- Dev server must remain running to test on `http://localhost:3000`.
- Local API env values may need alignment (`REACT_APP_BACKEND_PORT`, `REACT_APP_API_BASE_URL`) for full data flow.

## How to run locally
1. Clone the repo (or your fork) and enter the folder:
   ```bash
   git clone https://github.com/nidhiatwork01-cmyk/Hack4IMPACTTrack2-DedSec.git
   cd Hack4IMPACTTrack2-DedSec
   ```
   If you forked first, clone your fork URL instead.
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the development server:
   ```bash
   npm start
   ```
4. Open `http://localhost:3000` in your browser.
