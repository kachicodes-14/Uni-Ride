# 🛺 UniRide — Smart Electric Tricycle Booking Platform

[![CI](https://github.com/your-org/uniride/actions/workflows/ci.yml/badge.svg)](https://github.com/your-org/uniride/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue.svg)](https://www.typescriptlang.org/)
[![Next.js](https://img.shields.io/badge/Next.js-14-black.svg)](https://nextjs.org/)

> **UniRide** is a full-stack, production-ready ride-hailing platform designed for the University of Benin (UNIBEN) campus, connecting students with electric keke (tricycle) drivers via a mobile-first Progressive Web App.

---

## 📦 Monorepo Structure

```
uniride/
├── apps/
│   ├── api/           # Node.js + Express REST API + Socket.io
│   └── web/           # Next.js 14 PWA (student, driver, admin)
├── packages/
│   └── types/         # Shared TypeScript interfaces
├── .github/
│   └── workflows/     # CI/CD pipelines
├── docker-compose.yml          # Local development
└── docker-compose.prod.yml     # Production deployment
```

---

## 🚀 Tech Stack

| Layer        | Technology                                              |
|--------------|---------------------------------------------------------|
| Frontend     | Next.js 14 (App Router), TypeScript, Tailwind CSS      |
| State        | Zustand, TanStack Query                                 |
| Backend      | Node.js, Express 4, TypeScript                          |
| Database     | PostgreSQL 16 + Prisma ORM                             |
| Cache        | Redis 7                                                 |
| Real-time    | Socket.io                                               |
| Auth         | JWT (access + refresh tokens), bcrypt                   |
| Payments     | Paystack (Nigerian payment gateway)                     |
| Notifications| Firebase Cloud Messaging (FCM)                          |
| Storage      | AWS S3 / Cloudinary                                     |
| Email        | Nodemailer (SMTP / Mailgun)                             |
| Logging      | Winston + Morgan                                        |
| Deployment   | Docker + Docker Compose + Nginx                         |
| CI/CD        | GitHub Actions                                          |

---

## ⚡ Quick Start

### Prerequisites
- Node.js ≥ 20
- npm ≥ 10
- Docker & Docker Compose
- PostgreSQL 16 (or use Docker)
- Redis 7 (or use Docker)

### 1. Clone & Install
```bash
git clone https://github.com/your-org/uniride.git
cd uniride
npm install
```

### 2. Environment Setup
```bash
# Root
cp .env.example .env

# API
cp apps/api/.env.example apps/api/.env

# Web
cp apps/web/.env.example apps/web/.env
```
Fill in all required values (see [Environment Variables](#environment-variables)).

### 3. Start with Docker (Recommended)
```bash
docker-compose up -d
```

### 4. Manual Start (Development)
```bash
# Start DB + Redis
docker-compose up -d postgres redis

# Run migrations & seed
cd apps/api
npx prisma migrate dev
npx prisma db seed

# Start both apps from root
cd ../..
npm run dev
```

Access:
- **Student App**: http://localhost:3000
- **API**: http://localhost:4000
- **API Docs**: http://localhost:4000/api/docs

---

## 🌍 Environment Variables

### `apps/api/.env`
| Variable | Description |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string |
| `REDIS_URL` | Redis connection string |
| `JWT_SECRET` | Secret for access tokens |
| `JWT_REFRESH_SECRET` | Secret for refresh tokens |
| `PAYSTACK_SECRET_KEY` | Paystack secret key (`sk_live_...`) |
| `PAYSTACK_PUBLIC_KEY` | Paystack public key (`pk_live_...`) |
| `FCM_SERVER_KEY` | Firebase Cloud Messaging server key |
| `SMTP_HOST` | Email SMTP host |
| `SMTP_USER` | Email SMTP user |
| `SMTP_PASS` | Email SMTP password |
| `CLIENT_URL` | Frontend URL for CORS |
| `PORT` | API port (default: 4000) |

### `apps/web/.env.local`
| Variable | Description |
|---|---|
| `NEXT_PUBLIC_API_URL` | Backend API base URL |
| `NEXT_PUBLIC_SOCKET_URL` | Socket.io server URL |
| `NEXT_PUBLIC_PAYSTACK_PUBLIC_KEY` | Paystack public key |
| `NEXT_PUBLIC_GOOGLE_MAPS_KEY` | Google Maps API key |

---

## 🏗️ Features

### Student App
- 🗺️ Interactive campus map with pickup/drop-off selection
- 🛺 Real-time keke matching and live tracking
- 💳 Digital wallet with Paystack top-up
- 📅 Ride scheduling
- 🤝 Share & Split fare
- 🔒 4-digit safety PIN system
- 🌙 Night safety mode
- ⭐ Post-ride ratings
- 🎁 Referral & rewards

### Driver Dashboard
- 📍 Real-time GPS location sharing
- 🟢 Online/offline toggle
- 🔔 Incoming ride request notifications
- 💰 Daily/weekly earnings tracker
- 📊 Trip history and ratings

### Admin Dashboard
- 📈 Revenue analytics & charts
- 👥 User management (approve/suspend drivers)
- 🛺 Live ride monitoring
- 🏠 Hub management
- 📊 Platform health metrics

---

## 📡 API Reference

Base URL: `http://localhost:4000/api/v1`

### Auth
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/register` | Register new user |
| POST | `/auth/login` | Login |
| POST | `/auth/refresh` | Refresh access token |
| POST | `/auth/logout` | Logout |
| GET  | `/auth/me` | Get current user |
| POST | `/auth/verify-email` | Verify email address |
| POST | `/auth/forgot-password` | Request password reset |
| POST | `/auth/reset-password` | Reset password |

### Rides
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/rides` | Create a booking |
| GET  | `/rides` | Get user ride history |
| GET  | `/rides/:id` | Get ride details |
| PATCH | `/rides/:id/cancel` | Cancel a ride |
| POST | `/rides/:id/rate` | Rate a completed ride |

### Driver
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET  | `/drivers/nearby` | Get nearby available kekes |
| PATCH | `/drivers/location` | Update driver location |
| PATCH | `/drivers/status` | Toggle online/offline |
| PATCH | `/rides/:id/accept` | Accept a ride request |
| PATCH | `/rides/:id/start` | Start a ride |
| PATCH | `/rides/:id/complete` | Complete a ride |

### Wallet
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET  | `/wallet` | Get wallet balance & transactions |
| POST | `/wallet/fund` | Initiate Paystack payment |
| POST | `/wallet/verify` | Verify Paystack payment |
| POST | `/wallet/withdraw` | Request withdrawal |

### Admin
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET  | `/admin/dashboard` | Platform stats |
| GET  | `/admin/users` | All users |
| GET  | `/admin/rides` | All rides |
| GET  | `/admin/drivers` | All drivers |
| PATCH | `/admin/drivers/:id/approve` | Approve driver |
| PATCH | `/admin/drivers/:id/suspend` | Suspend driver |
| GET  | `/admin/revenue` | Revenue report |

---

## 🐳 Docker Deployment

### Production
```bash
cp .env.example .env  # Fill in production values
docker-compose -f docker-compose.prod.yml up -d
```

### SSL / Nginx
The production compose includes an Nginx reverse proxy. Point your domain's DNS to the server IP, then:
```bash
# Install certbot on the host
certbot certonly --standalone -d yourdomain.com
# Certificates are mounted into the nginx container
```

---

## 🧪 Testing

```bash
# API unit tests
cd apps/api && npm test

# API integration tests
cd apps/api && npm run test:integration

# Web E2E tests (Playwright)
cd apps/web && npm run test:e2e
```

---

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feat/amazing-feature`)
3. Commit with conventional commits (`git commit -m 'feat: add amazing feature'`)
4. Push to branch (`git push origin feat/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

MIT © 2025 UniRide / UNIBEN Smart Mobility Initiative
