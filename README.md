# 🚀 Prowider Lead Distribution System

A full-stack backend-focused system that automatically assigns customer service leads to providers using **business rules, fair distribution, and concurrency-safe logic**.

---

# 📌 Problem Statement

When a customer submits a service request:
- Lead must be saved in database
- Automatically assigned to providers
- Must follow strict assignment rules
- Must ensure fairness + quota limits
- Must handle concurrent requests safely
- Providers must see updates in real-time

---

# 🧠 Core Features

## 1. Lead Management
- Create service leads via API or form
- Store customer details in database
- Prevent duplicate leads using:
  - Same phone + same service type (DB-level constraint)

---

## 2. Provider Assignment Logic

Each lead is assigned to **exactly 3 providers**:

### 🔒 Mandatory Rules
- Service 1 → Provider 1
- Service 2 → Provider 5
- Service 3 → Provider 1 + Provider 4

---

### ⚖️ Fair Distribution (Round Robin)
After mandatory assignment:
- Remaining slots filled from provider pools
- Uses round-robin algorithm
- Ensures equal distribution over time
- State persists in database

---

## 3. Provider Quota System
- Each provider has a monthly limit of **10 leads**
- System prevents over-assignment
- Skips providers who exceed quota

---

## 4. Concurrency Safety
- Uses database transactions
- Prevents race conditions
- Ensures correct assignment even under simultaneous requests

---

## 5. Webhook System (Idempotency)
- Simulates external events (e.g. payment confirmation)
- Each webhook event stored with unique ID
- Duplicate webhook calls are ignored safely
- Ensures idempotent execution

---

## 6. Real-Time Dashboard
- Provider dashboard updates automatically
- No manual refresh required
- Uses polling / SSE (implementation dependent)

---

# 🗄️ Database Models

- **Provider** → provider details + quota tracking  
- **Lead** → customer service requests  
- **Assignment** → mapping between leads and providers  
- **RoundRobinState** → stores fairness rotation state  
- **WebhookEvent** → ensures webhook idempotency  

---

# ⚙️ Tech Stack

- Next.js (Frontend + Backend APIs)
- PostgreSQL (Database)
- Prisma ORM
- Server-Side APIs
- Server-Sent Events / Polling

---

# 🚀 Setup Instructions

```bash
# Install dependencies
npm install

# Setup database
npx prisma generate
npx prisma migrate dev

# Run development server
npm run dev
