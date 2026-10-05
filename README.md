<div align="center">

# 📍 TraceX

### Real-Time Geospatial Tracking & Geofencing System

TraceX is a high-performance location tracking system built for ultra-low latency coordinate streaming, intelligent boundary monitoring, and battery-conscious background processing.

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Cross--Platform-blue?style=for-the-badge" alt="Platform" />
  <img src="https://img.shields.io/badge/Architecture-Clean%20%2F%20Event--Driven-darkgreen?style=for-the-badge" alt="Architecture" />
  <img src="https://img.shields.io/badge/Status-Under%20Development-orange?style=for-the-badge" alt="Status" />
  <img src="https://img.shields.io/badge/License-MIT-purple?style=for-the-badge" alt="License" />
</p>

</div>

---

## 📑 Table of Contents

- [📌 Overview](#-overview)
- [✨ Key Features](#-key-features)
- [🛠️ Tech Stack](#️-tech-stack)
- [🏗️ System Architecture](#️-system-architecture)
- [📂 Project Structure](#-project-structure)
- [🚀 Getting Started](#-getting-started)
- [⚙️ Environment Configuration](#️-environment-configuration)
- [▶️ Launch Development Servers](#️-launch-development-servers)
- [📄 License](#-license)

---

## 📌 Overview

Continuous GPS polling often drains mobile batteries rapidly and suffers from high latency over unstable networks. **TraceX** solves this by implementing adaptive location throttling, background worker sync, and low-overhead socket streaming.

It delivers smooth live movement across custom interactive maps while minimizing unnecessary device overhead.

---

## ✨ Key Features

| | Feature | Description |
|---|---------|-------------|
| ⚡ | **Sub-Second Live Tracking** | Bidirectional real-time streams deliver smooth coordinate synchronization. |
| 🔋 | **Battery-Aware Geolocation** | Dynamic distance filtering and adaptive polling minimize background battery drain. |
| 🛡️ | **Smart Geofencing** | Automated entry/exit triggers with instant push and webhook notifications. |
| 🗺️ | **Interactive Route Replay** | Visual playback engine to inspect historical telemetry, elevation, and speed graphs. |
| 📶 | **Offline Persistence & Auto-Sync** | Buffers breadcrumb trails locally when connectivity drops and syncs seamlessly upon reconnect. |

---

## 🛠️ Tech Stack

### 📱 Client & Interfaces

| Category | Technology |
|----------|------------|
| 📲 **Framework** | React Native / Expo (Cross-Platform Mobile) |
| 🗺️ **Maps & Visualizations** | Mapbox GL / Google Maps SDK |
| 🧠 **State & Data Caching** | Zustand & TanStack Query |

### 🖥️ Backend & Real-Time Engine

| Category | Technology |
|----------|------------|
| ⚙️ **Runtime** | Node.js (Express / NestJS) |
| 🔌 **Real-Time Stream** | WebSockets / Socket.io |
| 🗄️ **Database & Geospatial Engine** | PostgreSQL (PostGIS) / Redis |
| 🔐 **Validation & Security** | Zod schemas, JWT & Role-Based Access Control (RBAC) |

---

## 🏗️ System Architecture

```text
       [ Expo Mobile App ]
               │
        (Background GPS)
               │
               ▼
      [ WebSocket Server ] ◄───► [ Redis Pub/Sub Cache ]
               │
               ├─────────────────────────┐
               ▼                         ▼
      [ PostGIS Database ]      [ Next.js Web Dashboard ]
   (Coordinates & Geofences)     (Real-Time Fleet View)
```

---

## 📂 Project Structure

```text
TraceX/
├── apps/
│   ├── mobile/                  # 📱 Expo (React Native) Client
│   │   ├── app/                 # Expo Router file-based screens
│   │   ├── components/          # Map markers, callouts, and bottom sheets
│   │   ├── hooks/               # Geolocation tracking & socket hooks
│   │   └── package.json
│   │
│   └── web/                     # 🖥️ Next.js Real-Time Monitoring Dashboard
│       ├── src/
│       │   ├── app/             # App Router pages and analytics views
│       │   ├── components/      # Map viewports and telemetry cards
│       │   └── lib/             # API client and WebSocket handlers
│       └── package.json
│
├── server/                      # ⚙️ Node.js Geospatial Backend Engine
│   ├── src/
│   │   ├── modules/
│   │   │   ├── auth/            # Authentication & session verification
│   │   │   ├── geofence/        # Boundary checking & breach dispatchers
│   │   │   └── tracking/        # Live coordinates ingestion pipeline
│   │   ├── sockets/             # Socket event emitters and listeners
│   │   └── index.ts             # Server entry point
│   └── package.json
│
├── docs/                        # 📚 Architecture diagrams and design assets
└── README.md
```

---

## 🚀 Getting Started

### 📋 Prerequisites

Before running the project, make sure you have the following installed:

- 🟢 **Node.js** (`>= 18.x`)
- 📦 **Package Manager** (`npm`, `pnpm`, or `yarn`)
- 🔧 **Git**
- 🐳 **Docker** (for PostgreSQL & Redis containers)

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/damith-akalanka/TraceX.git
cd TraceX
```

### 2️⃣ Install Dependencies

```bash
# Install dependencies across all workspaces
npm install
```

### 3️⃣ Start Infrastructure Services

```bash
# Start PostgreSQL container
docker run --name tracex-postgres \
  -e POSTGRES_PASSWORD=password \
  -e POSTGRES_DB=tracex_db \
  -p 5432:5432 -d postgres

# Start Redis container
docker run --name tracex-redis -p 6379:6379 -d redis
```

---

## ⚙️ Environment Configuration

Create the environment file for each module as shown below.

### 🖧 Backend Server — `server/.env`

```bash
PORT=5000
NODE_ENV=development
DATABASE_URL=postgresql://postgres:password@localhost:5432/tracex_db
REDIS_URL=redis://localhost:6379
JWT_SECRET=your_jwt_secret_key_here
MAPBOX_ACCESS_TOKEN=your_mapbox_token_here
```

### 🖥️ Next.js Web Dashboard — `apps/web/.env.local`

```bash
NEXT_PUBLIC_API_URL=http://localhost:5000/api
NEXT_PUBLIC_SOCKET_URL=http://localhost:5000
NEXT_PUBLIC_MAPBOX_TOKEN=your_mapbox_token_here
```

### 📱 Expo Mobile App — `apps/mobile/.env`

```bash
EXPO_PUBLIC_API_URL=http://<YOUR_LOCAL_IP>:5000/api
EXPO_PUBLIC_SOCKET_URL=http://<YOUR_LOCAL_IP>:5000
EXPO_PUBLIC_MAPBOX_TOKEN=your_mapbox_token_here
```

> 💡 **Tip:** On a physical device, use your computer's local network IP (e.g. `192.168.x.x`) instead of `localhost` for the mobile app.

> ⚠️ **Security:** Never commit your `.env` files or real secrets to Git. Use a strong, unique `JWT_SECRET` in production.

---

## ▶️ Launch Development Servers

Open **three separate terminal tabs** in the workspace root and run one service in each.

### 🖧 Tab 1 — Backend Server

```bash
cd server
npm run dev
```

> ✅ The API server will start on `http://localhost:5000` (or your configured `PORT`).

### 🖥️ Tab 2 — Next.js Web Dashboard

```bash
cd apps/web
npm run dev
```

> ✅ Open `http://localhost:3000` in your browser to access the real-time tracking dashboard.

### 📱 Tab 3 — Expo Mobile App

```bash
cd apps/mobile
npx expo start
```

> ✅ Scan the QR code with the **Expo Go** app on your physical iOS or Android device, or press `a` for the Android Emulator / `i` for the iOS Simulator.

---

## 📄 License

This project is licensed under the **MIT License**. 📜

---

<div align="center">

⭐ If you find **TraceX** useful, consider giving the repo a star! ⭐

</div>
