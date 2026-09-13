# Multi-Tenant SaaS Task Management Platform API

[![Node.js](https://img.shields.io/badge/Node.js-18.x-339933?logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-4.x-000000?logo=express&logoColor=white)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![JWT](https://img.shields.io/badge/Auth-JWT-000000?logo=jsonwebtokens&logoColor=white)](https://jwt.io/)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![Vercel](https://img.shields.io/badge/Deploy-Vercel-000000?logo=vercel&logoColor=white)](https://vercel.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A scalable, secure, and production-ready **Multi-Tenant Task Management Backend API** built with **Node.js, Express, and MongoDB**. Designed for enterprise SaaS applications, it enforces complete multi-tenant data isolation, granular Role-Based Access Control (RBAC), automated background task scheduling, and comprehensive security middleware.

---

## 🌟 Key Features

- **Multi-Tenant Data Isolation**: Robust tenant identification and isolation middleware ensuring organizations can only query and mutate their own workspace data.
- **Role-Based Access Control (RBAC)**: Fine-grained permissions for **Admin**, **Manager**, and **Member** roles.
- **Stateless JWT Authentication**: Secure authentication pipeline featuring password hashing via `bcryptjs` and token validation middleware.
- **Task Lifecycle Management**: Full CRUD operations for tasks with priorities (*Low, Medium, High, Urgent*), status tracking (*Todo, In-Progress, Review, Completed*), tags, and member assignment.
- **Automated Background Cron Jobs**: Background worker powered by `node-cron` to automatically check and flag expired/overdue tasks.
- **Security & DDoS Protection**:
  - `helmet` for HTTP security headers
  - `express-rate-limit` for endpoint rate limiting
  - Configurable `cors` origin controls
  - `express-validator` payload validation and sanitization
- **Dual Deployment Ready**: Seamlessly runs as a standalone server, inside a Docker container, or as a serverless function on Vercel.
- **Automated Test Suite**: Integrated unit and integration testing suite configured with Jest and Supertest.

---

## 🛠️ Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Runtime & Framework** | Node.js (18+), Express.js | Core API backend engine |
| **Database & ODM** | MongoDB, Mongoose | Schema modeling, multi-tenant indexing |
| **Authentication & Auth** | JSON Web Tokens (JWT), Bcrypt.js | Stateless session management & password hashing |
| **Security** | Helmet, CORS, Express-Rate-Limit | Header hardening, origin protection & DDoS throttling |
| **Validation** | Express-Validator | Request body and param sanitization |
| **Background Processing**| Node-Cron | Automated recurring task expiration scheduler |
| **Containerization** | Docker, Alpine Linux | Production containerization and health checks |
| **Testing** | Jest, Supertest | Integration and unit testing |

---

## 📂 Project Structure

```plaintext
Multi-Tenant-Task-Management-Platform/
├── api/
│   └── index.js              # Vercel serverless function entrypoint
├── jobs/
│   └── taskExpiration.js     # Automated background task expiration cron
├── middleware/
│   ├── auth.js               # JWT verification & RBAC authorization
│   └── tenant.js             # Multi-tenant context and isolation
├── models/
│   ├── Organization.js       # Tenant / organization schema
│   ├── Task.js               # Task schema with tenant binding
│   └── User.js               # User schema with roles and tenant reference
├── routes/
│   ├── auth.js               # Registration, login, profile, user management
│   ├── health.js             # Liveness and readiness health checks
│   ├── organizations.js      # Tenant configuration & member invitations
│   └── tasks.js              # Multi-tenant task CRUD operations
├── tests/
│   ├── auth.test.js          # Authentication test suite
│   └── tasks.test.js         # Task endpoints test suite
├── .env.example              # Environment variables template
├── .gitignore                # Git ignore rules for credentials
├── Dockerfile                # Production Docker container definition
├── package.json              # Dependencies and test runner scripts
├── server.js                 # Central Express app setup and middleware
└── vercel.json               # Vercel serverless routing configuration
```

---

## 📡 REST API Reference

### 🔐 Authentication (`/api/auth`)
| Method | Endpoint | Description | Auth Required |
|---|---|---|:---:|
| `POST` | `/api/auth/register` | Register an organization and admin user | ❌ |
| `POST` | `/api/auth/login` | Authenticate user and receive JWT token | ❌ |
| `GET` | `/api/auth/me` | Fetch currently authenticated user profile | ✅ |
| `POST` | `/api/auth/users` | Invite / create a user in current organization | ✅ (Admin/Manager) |
| `GET` | `/api/auth/users` | List all users belonging to the tenant | ✅ |

### 📋 Tasks (`/api/tasks`)
| Method | Endpoint | Description | Auth Required |
|---|---|---|:---:|
| `GET` | `/api/tasks` | Get all tasks for current tenant (filter by status/priority) | ✅ |
| `POST` | `/api/tasks` | Create a new task in current organization | ✅ |
| `GET` | `/api/tasks/:id` | Get details of a specific task | ✅ |
| `PUT` | `/api/tasks/:id` | Update task status, priority, or assignment | ✅ |
| `DELETE`| `/api/tasks/:id` | Delete task from organization | ✅ (Admin/Manager) |

### 🏢 Organizations (`/api/organizations`)
| Method | Endpoint | Description | Auth Required |
|---|---|---|:---:|
| `GET` | `/api/organizations/current` | Get tenant organization details | ✅ |
| `PUT` | `/api/organizations/current` | Update organization settings | ✅ (Admin) |

### 💓 Health Check (`/api/health`)
| Method | Endpoint | Description | Auth Required |
|---|---|---|:---:|
| `GET` | `/api/health` | System health check (API & Database status) | ❌ |

---

## ⚙️ Getting Started

### Prerequisites
- [Node.js](https://nodejs.org/) v18.0.0 or higher
- [MongoDB](https://www.mongodb.com/) (local instance or MongoDB Atlas cluster)
- [Git](https://git-scm.com/)

### 1. Clone the Repository
```bash
git clone https://github.com/Engineer-anand/Multi-Tenant-Task-Management-Platform.git
cd Multi-Tenant-Task-Management-Platform
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Setup Environment Variables
Copy `.env.example` to `.env`:
```bash
cp .env.example .env
```
Configure your variables in `.env`:
```env
NODE_ENV=development
PORT=5000
MONGODB_URI=mongodb://localhost:27017/task-platform
JWT_SECRET=your-super-secret-jwt-key-here
FRONTEND_URL=http://localhost:3000
```

### 4. Run Locally
- **Development mode (with nodemon):**
  ```bash
  npm run dev
  ```
- **Production mode:**
  ```bash
  npm start
  ```

### 5. Run Automated Tests
```bash
npm test
```

---

## 🐳 Docker Deployment

Build and run using Docker:
```bash
docker build -t task-platform-api .
docker run -p 5000:5000 --env-file .env task-platform-api
```

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.