# ⚓ FuelEU Maritime Compliance Platform

A full-stack application implementing the **FuelEU Maritime Regulation (EU 2023/1805)** — monitoring vessel GHG emissions, computing compliance balances, and managing banking and pooling across shipping routes.

This project demonstrates **clean architecture (Hexagonal / Ports & Adapters)** using TypeScript on both frontend and backend, ensuring a clear separation between domain logic, application use cases, and infrastructure layers.

---

## 🚀 Tech Stack

| Layer | Technology |
| ------ | ----------- |
| Frontend | React + TypeScript + TailwindCSS + Vite |
| Backend | Node.js + Express + TypeScript + Prisma + PostgreSQL |
| Architecture | Hexagonal (Ports & Adapters / Clean Architecture) |
| Database | PostgreSQL (Neon) |
| Testing | Jest + Supertest |
| Documentation | AGENT_WORKFLOW.md, README.md, REFLECTION.md |

---

## 🧩 Features

### 1. Routes Dashboard
- Displays all vessel routes (`GET /routes`)
- Set a baseline route (`POST /routes/:routeId/baseline`)
- Compare routes vs baseline (`GET /routes/comparison`)

### 2. Compare Module
- Calculates GHG intensity differences and compliance %
- Displays baseline vs comparison table
- Marks compliant / non-compliant routes visually
- Uses target intensity = **89.3368 gCO₂e/MJ**

### 3. Banking Module
Implements **FuelEU Article 20 – Banking**  
- `GET /compliance/cb?year=YYYY` → Fetch compliance balance  
- `POST /banking/bank` → Bank positive compliance surplus  
- `POST /banking/apply` → Apply stored surplus to deficit  
- KPIs: `cb_before`, `applied`, `cb_after`

### 4. Pooling Module
Implements **FuelEU Article 21 – Pooling**  
- `GET /compliance/adjusted-cb?year=YYYY` → Get adjusted CB per ship  
- `POST /pools` → Create pooling between multiple ships  
- Ensures:  
  - ∑ adjustedCB ≥ 0  
  - Deficit ships don’t worsen  
  - Surplus ships don’t go negative

### 5. Health Checks
- `/health` → Returns `{ status: "ok" }`
- Useful for deployment readiness checks

---

## 🧱 Folder Structure

```
Rakesh-0211-fueleu_compliance_platform/
├── README.md
├── AGENT_WORKFLOW.md
├── REFLECTION.md
│
├── backend/
│   ├── package.json
│   ├── tsconfig.json
│   ├── jest.config.js
│   ├── prisma/
│   │   └── schema.prisma
│   ├── scripts/
│   │   └── seed.js
│   ├── src/
│   │   ├── prismaClient.ts
│   │   ├── adapters/
│   │   │   ├── inbound/http/
│   │   │   │   ├── routeController.ts
│   │   │   │   ├── bankingController.ts
│   │   │   │   └── poolsController.ts
│   │   │   └── outbound/prisma/
│   │   │       ├── RouteRepositoryPrisma.ts
│   │   │       ├── BankRepositoryPrisma.ts
│   │   │       └── PoolRepositoryPrisma.ts
│   │   ├── core/
│   │   │   ├── domain/entities/Route.ts
│   │   │   └── application/usecases/
│   │   │       ├── computeCB.ts
│   │   │       ├── banking.ts
│   │   │       └── createPool.ts
│   │   ├── infrastructure/server/
│   │   │   ├── app.ts
│   │   │   └── dev.ts
│   │   └── shared/constants.ts
│   └── tests/
│       ├── unit/
│       └── integration/
│
└── frontend/
    ├── package.json
    ├── vite.config.ts
    ├── tsconfig.json
    ├── src/
    │   ├── main.tsx
    │   ├── App.tsx
    │   ├── index.css
    │   │
    │   ├── adapters/
    │   │   ├── infrastructure/
    │   │   │   └── fetchHelpers.ts
    │   │   └── ui/
    │   │       ├── components/
    │   │       │   ├── Sidebar.tsx
    │   │       │   ├── Topbar.tsx
    │   │       │   ├── Card.tsx
    │   │       │   └── Table.tsx
    │   │       └── pages/
    │   │           ├── RoutesPage.tsx
    │   │           ├── ComparePage.tsx
    │   │           ├── BankingPage.tsx
    │   │           ├── PoolingPage.tsx
    │   │           └── DashboardPage.tsx
    │   │
    │   ├── core/
    │   │   ├── application/usecases/
    │   │   │   ├── computeCB.ts
    │   │   │   ├── banking.ts
    │   │   │   └── createPool.ts
    │   │   ├── domain/
    │   │   │   ├── Route.ts
    │   │   │   ├── Banking.ts
    │   │   │   └── Pool.ts
    │   │   └── shared/
    │   │       └── constants.ts
    │   │
    │   ├── assets/
    │   │   └── react.svg
    │   ├── AppRouter.tsx
    │   └── App.css
```

---

## ⚙️ Setup & Run Instructions

### Backend
```bash
cd backend
npm install
npx prisma generate
npx prisma migrate dev --name init
node scripts/seed.js
npm run dev
```

### Frontend
```bash
cd frontend
npm install
npm run dev
```

App will run at → [http://localhost:5173](http://localhost:5173)

---

## 🔗 API Endpoints

| Area | Endpoint | Method | Description |
|------|-----------|--------|-------------|
| Health | `/health` | GET | Returns API status |
| Routes | `/routes` | GET | Fetch all routes |
| Routes | `/routes/:routeId/baseline` | POST | Set a baseline route |
| Compare | `/routes/comparison` | GET | Compare routes vs baseline |
| CB | `/compliance/cb?year=YYYY` | GET | Get compliance balance |
| Bank | `/banking/bank` | POST | Store positive CB |
| Apply | `/banking/apply` | POST | Apply stored credits |
| Pool | `/pools` | POST | Create a pool |
| Adjusted CB | `/compliance/adjusted-cb?year=YYYY` | GET | Adjusted CB results |

---

## 🧠 AI Agent Collaboration

- **Tools Used:** GitHub Copilot, Cursor, Claude Code
- Used Copilot for React UI boilerplates
- Used Claude Code for refactoring controllers and Prisma integration
- Used Cursor Tasks for automated folder scaffolding

---

## 📚 References

FuelEU Maritime Regulation (EU) 2023/1805 – Annex IV, Articles 20–21  
Official Document: [FuelEU Methodologies PDF](./2025-May-ESSF-SAPS-WS1-FuelEU-calculation-methodologies_(1).pdf)

---

## 👨‍💻 Author

**Rakesh (Rakesh-0211)**  
GitHub: [https://github.com/Rakesh-0211/Varuna-Assignment](https://github.com/Rakesh-0211/Varuna-Assignment)
