# MERN Authentication

A full-stack authentication system built with the MERN stack — **M**ongoDB, **E**xpress, **R**eact, **N**ode.js. Includes email verification, OTP login, JWT refresh tokens, CSRF protection, and role-based access (user/admin).

![MERN](https://img.shields.io/badge/Stack-MERN-00d4ff?style=flat&logo=mongodb&logoColor=green)
![React](https://img.shields.io/badge/React-19-61dafb?style=flat&logo=react)
![Vite](https://img.shields.io/badge/Vite-7-646cff?style=flat&logo=vite)

---

## Features

- **User registration** — Sign up with name, email, password
- **Email verification** — Verify account via link sent to email
- **Login** — Email + password with optional OTP verification
- **OTP verification** — One-time code for extra security
- **JWT auth** — Access + refresh tokens (httpOnly cookies)
- **CSRF protection** — Token-based CSRF for state-changing requests
- **Redis sessions** — Refresh token storage and invalidation
- **Protected routes** — Dashboard and profile for logged-in users
- **Admin role** — Role-based route (`/admin`) for admin users
- **Responsive UI** — React + Tailwind CSS, toast notifications

---

## Tech Stack

| Layer      | Tech |
|-----------|------|
| **Frontend** | React 19, Vite 7, React Router 7, Tailwind CSS 4, Axios, React Toastify |
| **Backend**  | Node.js, Express 5, Mongoose, JWT, bcrypt, Zod |
| **Database** | MongoDB |
| **Cache**    | Redis |
| **Email**    | Nodemailer (SMTP) |

---

## Project Structure

```
Mern-Auth/
├── backend/
│   ├── config/          # db, JWT, CSRF, mail, Zod schemas
│   ├── controllers/     # user auth logic
│   ├── middlewares/     # isAuth, TryCatch, admin
│   ├── models/          # User (Mongoose)
│   ├── routes/          # /api/v1 user routes
│   └── index.js
├── frontend/
│   ├── src/
│   │   ├── context/     # AppContext (auth state)
│   │   ├── pages/       # Home, Login, Register, Verify, VerifyOtp, Dashboard
│   │   ├── apiIntercepter.js
│   │   └── App.jsx
│   └── vite.config.js
└── README.md
```

---

## Prerequisites

- **Node.js** (v18+)
- **MongoDB** (local or Atlas)
- **Redis** (local or cloud, e.g. Upstash)

---

## Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/AJKakarot/Mern-Authentication.git
cd Mern-Authentication
```

### 2. Backend setup

```bash
cd backend
npm install
```

Create a `.env` file in `backend/`:

```env
# Server
PORT=5000
FRONTEND_URL=http://localhost:5173

# Database
MONGO_URI=mongodb://localhost:27017
# Or MongoDB Atlas: mongodb+srv://user:pass@cluster.mongodb.net

# Redis
REDIS_URL=redis://localhost:6379
# Or e.g. Upstash: rediss://default:xxx@xxx.upstash.io:6379

# JWT
JWT_SECRET=your-super-secret-jwt-key
REFRESH_SECRET=your-refresh-secret-key

# Email (Nodemailer SMTP)
SMTP_USER=your-email@gmail.com
SMTP_PASSWORD=your-app-password
APP_NAME=MERN Auth

# Admin: this email gets role "admin" (on register or next login)
ADMIN_EMAIL=your-admin@email.com
```

Run the server:

```bash
npm run dev
```

Backend runs at **http://localhost:5000**.

### 3. Frontend setup

```bash
cd frontend
npm install
```

Create a `.env` file in `frontend/` (if your app reads API URL from env):

```env
VITE_API_URL=http://localhost:5000
```

Start the dev server:

```bash
npm run dev
```

Frontend runs at **http://localhost:5173**.

---

## API Reference

Base URL: `http://localhost:5000/api/v1`

| Method | Endpoint           | Auth | Description              |
|--------|--------------------|------|--------------------------|
| POST   | `/register`        | No   | Register new user        |
| POST   | `/verify/:token`   | No   | Verify email via token   |
| POST   | `/login`           | No   | Login (email + password) |
| POST   | `/verify`          | No   | Verify OTP               |
| GET    | `/me`              | Yes  | Get current user profile |
| POST   | `/refresh`         | No   | Refresh access token     |
| POST   | `/logout`          | Yes  | Logout (CSRF required)   |
| POST   | `/refresh-csrf`    | Yes  | Get new CSRF token       |
| GET    | `/admin`           | Yes  | Admin-only route         |

Auth: send cookies (credentials) with requests; access token in httpOnly cookie.

---

## Auth Flow

1. **Register** → User gets verification email → **Verify** (link with token).
2. **Login** → Optional OTP step → Access + refresh tokens in cookies.
3. **Protected routes** → Frontend sends cookies; backend validates JWT.
4. **Logout** → CSRF token + cookies cleared; refresh token removed from Redis.

---

## Scripts

**Backend**

- `npm run dev` — Run with nodemon
- `npm start` — Run with node

**Frontend**

- `npm run dev` — Vite dev server
- `npm run build` — Production build
- `npm run preview` — Preview production build

---

## License

ISC
