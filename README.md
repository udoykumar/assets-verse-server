# 🚀 AssetVerse – Server (Backend)

AssetVerse is a B2B Asset & HR Management Platform designed for companies to manage assets, employees, requests, affiliations, and HR subscriptions.
This repository contains the Node.js + Express + MongoDB backend API with secure JWT authentication, role-based authorization, and integrated Stripe payment gateway.

---

### Smart Asset & HR Management Platform
**Live Demo** → https://assetverse.netlify.app/

**Frontend Repo** → https://github.com/ebrahim2355/assetverse-client

---

## 📌 Features

### 🔐 **Authentication & Authorization**

- Firebase Authentication on frontend + custom JWT issuance on backend.

- Secure token validation middleware (verifyToken).

- Role-based access control for HR-only endpoints (verifyHR).

### 🧑‍🤝‍🧑 **User Management**

- Register Employee

- Register HR

- Update user info

- Retrieve users & roles

### 🏢 **HR & Employee Operations**

- Auto-affiliation tracking

- Team management

- Employees with assigned assets count

### 📦 **Asset Management**

- Add, Edit, Delete asset (HR only)

- Pagination + Search + Filters

- Returnable & Non-returnable types supported

### 📥 **Requests System**

- Employees can request assets

- HR can view/update requests

## 📊 **Analytics**

- Asset distribution chart

- Top requested assets chart

### 💳 **Stripe Payment Integration**

- Create Checkout Session

- Store payment history

- Dynamic package limit update

---

## 🛠️ Tech Stack

| Technology                       | Purpose                 |
| -------------------------------- | ----------------------- |
| **Node.js**                      | Backend runtime         |
| **Express.js**                   | API Framework           |
| **MongoDB (Atlas)**              | Database                |
| **Stripe**                       | Subscription & payments |
| **JWT**                          | Authentication          |
| **Firebase (client-side login)** | User auth provider      |
| **CORS**                         | Resource access control |

---

## 📂 Project Structure

.
├── index.js  (main file)
├── dotenv     (environment variables)
└── package.json

### **Inside server:**

- `/jwt` – Issue token

- `verifyToken` – Validate token

- `verifyHR` – RBAC middleware

- `/users` – Register & get users

- `/assets` – All asset APIs

- `/requests` – Asset request APIs

- `/affiliations` – HR affiliation APIs

- `/payments` – Stripe billing APIs

- `/analytics` – Dashboard analytics

---

## 🔑 Environment Variables

Create a .env file:
```bash
PORT=3000
DB_USER=your_mongo_user
DB_PASS=your_mongo_pass
JWT_SECRET=your_secret_key
STRIPE_SECRET=your_stripe_secret_key
SITE_DOMAIN=http://localhost:5173
```

---

## 🧪 Authentication Flow

### ✔️ 1. Frontend Login (Firebase)

User logs in → Firebase returns authenticated user.

### ✔️ 2. Frontend requests JWT from backend
```bash
axios.post("/jwt", { email })
```

**Backend returns:**
```bash
{
  "token": "your.jwt.token"
}
```

### ✔️ 3. Token stored in localStorage
```bash
access-token = <jwt>
```

### ✔️ 4. Axios interceptor attaches token
```bash
Authorization: Bearer <jwt>
```

### ✔️ 5. verifyToken middleware runs

- Confirms token is valid

- Attaches decoded email to req.decoded

### ✔️ 6. verifyHR checks role in DB

Only HR can:

- Add asset

- Edit/Delete asset

- View analytics

- View HR requests

- Remove affiliation

---

## 📡 Main API Endpoints (Summary)

### 🔐 Authentication
| Method | Route  | Description |
| ------ | ------ | ----------- |
| POST   | `/jwt` | Issue JWT   |

### 👥 Users
| Method | Route                | Description       |
| ------ | -------------------- | ----------------- |
| POST   | `/users/employee`    | Register employee |
| POST   | `/users/hr`          | Register HR       |
| GET    | `/users`             | Get all users     |
| GET    | `/users/:email`      | Get user details  |
| GET    | `/users/:email/role` | Get role          |

### 📦 Assets
| Method | Route              | Auth    | Description          |
| ------ | ------------------ | ------- | -------------------- |
| GET    | `/assets`          | Public  | Paginated asset list |
| GET    | `/assets/:hrEmail` | Token   | HR-based assets      |
| POST   | `/assets`          | HR Only | Create asset         |
| PATCH  | `/assets/:id`      | HR Only | Edit asset           |
| DELETE | `/assets/:id`      | HR Only | Delete asset         |

### 📨 Requests
| Method | Route           | Auth    | Description             |
| ------ | --------------- | ------- | ----------------------- |
| POST   | `/requests`     | Public  | Employee requests asset |
| GET    | `/requests`     | HR Only | HR views requests       |
| PATCH  | `/requests/:id` | HR Only | Update status           |

### 🤝 Affiliations
| Method | Route                         | Auth    | Description             |
| ------ | ----------------------------- | ------- | ----------------------- |
| POST   | `/affiliations`               | Token   | Auto-associate employee |
| GET    | `/affiliations/team/:hrEmail` | Public  | HR team                 |
| DELETE | `/affiliations/remove/:email` | HR Only | Remove employee         |

### 💳 Payments
| Method | Route                      | Description          |
| ------ | -------------------------- | -------------------- |
| POST   | `/create-checkout-session` | Stripe checkout link |
| POST   | `/payments`                | Store payment        |
| GET    | `/checkout-session/:id`    | Get Stripe session   |

### 📊 Analytics
| Method | Route                                    | Auth    | Description    |
| ------ | ---------------------------------------- | ------- | -------------- |
| GET    | `/analytics/asset-distribution/:hrEmail` | HR Only | Pie chart data |
| GET    | `/analytics/top-requests/:hrEmail`       | HR Only | Bar chart data |

---

## ▶️ Run the Server

Install packages:
```bash
npm install
npm run dev
// or
nodemon index.js
```
**Server runs on:**
http://localhost:3000
