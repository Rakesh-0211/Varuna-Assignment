# ⚓ FuelEU Maritime Compliance Platform

A full-stack application implementing the **FuelEU Maritime Regulation (EU 2023/1805)** — monitoring vessel GHG emissions, computing compliance balances, and managing banking and pooling across shipping routes.

This project demonstrates **clean architecture (Hexagonal / Ports & Adapters)**, **TypeScript** on both client & server, and clear separation between **domain**, **application logic**, and **infrastructure**.

---

## 🚀 Tech Stack

| Layer | Technology |
|-------|-------------|
| **Frontend** | React + TypeScript + TailwindCSS + Vite |
| **Backend** | Node.js + Express + TypeScript + Prisma + PostgreSQL |
| **Architecture** | Hexagonal (Ports & Adapters / Clean Architecture) |
| **Database** | PostgreSQL (Neon) |
| **Testing** | Jest + Supertest |
| **Docs** | AGENT_WORKFLOW.md, README.md, REFLECTION.md |

---

## 🧩 Features

### 1. **Routes Dashboard**
- Displays all routes (`GET /routes`)
- Allows setting a baseline (`POST /routes/:routeId/baseline`)
- Shows comparison between baseline and others (`GET /routes/comparison`)

### 2. **Compare Module**
- Compares baseline vs others on **GHG intensity** and **compliance %**
- Displays chart & table difference
- Marks compliant / non-compliant routes visually

### 3. **Banking Module**
- Implements **FuelEU Article 20 — Banking**
- `GET /compliance/cb?year=YYYY`: View compliance balance
- `POST /banking/bank`: Bank positive surplus
- `POST /banking/apply`: Apply banked credits to deficit

### 4. **Pooling Module**
- Implements **FuelEU Article 21 — Pooling**
- `GET /compliance/adjusted-cb?year=YYYY`: Adjusted balances
- `POST /pools`: Creates pools among ships, ensuring net positive sum

### 5. **Health Checks**
- `/health` endpoint for API readiness

---

## 🧱 Folder Structure

```plaintext
shahab-16-fueleu_compliance_platform/
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
    │   ├── components/
    │   │   └── Sidebar.tsx
    │   └── pages/
    │       ├── RoutesPage.tsx
    │       ├── ComparePage.tsx
    │       ├── BankingPage.tsx
    │       ├── PoolingPage.tsx
    │       └── AdminPage.tsx
