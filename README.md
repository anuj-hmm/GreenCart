# GreenCart — Full-Stack E-Commerce Grocery Platform

[![React](https://img.shields.io/badge/React-19.0-61DAFB?style=flat-square&logo=react&logoColor=black)](https://react.dev/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4.0-38B2AC?style=flat-square&logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![Node.js](https://img.shields.io/badge/Node.js-Express-339933?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?style=flat-square&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Stripe](https://img.shields.io/badge/Payments-Stripe-635BFF?style=flat-square&logo=stripe&logoColor=white)](https://stripe.com/)
[![Cloudinary](https://img.shields.io/badge/Cloud_Storage-Cloudinary-3448C5?style=flat-square&logo=cloudinary&logoColor=white)](https://cloudinary.com/)

A modern, full-stack grocery e-commerce web application engineered with the **MERN stack (MongoDB, Express.js, React 19, Node.js)**. GreenCart delivers an end-to-end shopping experience featuring real-time client-side search filtering, cart state synchronization, secure multi-channel payments via Stripe & Cash on Delivery (COD), automated webhook handling, and an administrative seller portal for live inventory and catalog management.

---

## 🌐 Live Demo & Deployment

- **Live Application**: [https://green-cart-frontend-theta.vercel.app](https://green-cart-frontend-theta.vercel.app)
- **Architecture**: Distributed Single Page Application (SPA) client and modular REST API backend with Vercel serverless deployment support.

---

## ✨ Core Highlights & Features

### 🛒 Customer Storefront
- **Instant Product Discovery & Search**: Fast, dynamic client-side querying across multiple product categories (Fresh Fruits, Organic Veggies, Dairy, Bakery & Breads, Instant Food, Grains, Drinks).
- **Persistent Cart Engine**: Cart data automatically synchronizes across sessions directly with MongoDB for authenticated users.
- **Address Management**: Multi-address creation and persistence for checkout selection.
- **Dual Payment Options**:
  - **Stripe Checkout**: Secure, hosted payment sessions supporting card payments with line-item pricing and automated tax calculation.
  - **Cash on Delivery (COD)**: Instant order placement for physical fulfillment.
- **Order History & Real-Time Tracking**: Detailed chronological order view displaying item breakdowns, fulfillment statuses, and payment states.

### 📦 Seller & Admin Portal
- **Protected Seller Authentication**: Isolated administrative credentials verification with secure cookie management.
- **Product Catalog Management**: Multi-image file uploads handled via `multer` and persisted to **Cloudinary Media Cloud**.
- **Real-Time Inventory Toggle**: One-click stock availability toggle (`inStock`) directly reflected on the customer marketplace.
- **Order Fulfillment Dashboard**: Unified seller view detailing customer delivery info, purchased items, order value, and real-time payment states.

### 🛡️ Security & Reliability
- **Stateless JWT Authentication**: Secure `httpOnly`, `SameSite`, and `Secure` cookie session handling to protect against XSS and CSRF.
- **Stripe Webhook Verification**: Cryptographic signature validation (`stripe-signature`) on `/stripe` endpoints for asynchronous order settlement and automatic cart clearing.
- **Password Hashing**: Bcrypt encryption with salted hashing algorithms.

---

## 🛠️ Tech Stack & Architecture

### **Frontend (Client)**
- **Framework**: React 19 + Vite
- **Routing**: React Router v7 (Nested routes, Protected routes)
- **Styling**: Tailwind CSS v4 + Outfit Typography
- **State & Communication**: React Context API (`AppContext`), Axios (Cookie credentials enabled)
- **Notifications**: React Hot Toast

### **Backend (Server)**
- **Runtime**: Node.js
- **Web Framework**: Express.js
- **Database & ODM**: MongoDB with Mongoose
- **Cloud Storage**: Cloudinary SDK (v2)
- **File Parsing**: Multer (Disk storage buffer)
- **Security & Auth**: JSON Web Tokens (`jsonwebtoken`), `bcryptjs`, `cookie-parser`, `cors`
- **Payment Gateway**: Stripe SDK & Stripe Webhooks

---

## 📂 Repository Structure

```text
anuj-hmm-greencart/
├── client/                     # Frontend Application (React 19 + Vite + Tailwind)
│   ├── src/
│   │   ├── assets/             # SVG icons, banner artwork, and static datasets
│   │   ├── components/         # Reusable UI components (Navbar, Modals, ProductCard, etc.)
│   │   │   └── seller/         # Seller-specific authentication and layout elements
│   │   ├── context/            # Centralized AppContext state management
│   │   └── pages/              # Primary views (Home, Cart, ProductCategory, Orders, etc.)
│   │       └── seller/         # Seller dashboard pages (AddProduct, Orders, ProductList)
│   ├── index.html              # HTML5 entry point
│   ├── vite.config.js          # Vite and Tailwind compiler configuration
│   └── vercel.json             # SPA routing rewrite configuration
│
└── server/                     # Backend REST API (Express.js + Node.js)
    ├── configs/                # MongoDB, Cloudinary, and Multer configurations
    ├── controllers/            # Controller business logic (Auth, Cart, Order, Product, Seller)
    ├── middlewares/            # Role-based JWT authentication middlewares
    ├── models/                 # Mongoose schemas (User, Product, Order, Address)
    ├── routes/                 # Express API endpoint definitions
    ├── server.js               # Application bootstrap and webhook endpoint
    └── vercel.json             # Serverless deployment configuration
```

---

## 📡 Comprehensive API Reference

### 👤 User Endpoints (`/api/user`)
| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/user/register` | Register new user account | No |
| `POST` | `/api/user/login` | Authenticate user & issue JWT cookie | No |
| `GET` | `/api/user/is-auth` | Verify current user session | Yes (`token`) |
| `GET` | `/api/user/logout` | Clear user authentication cookie | Yes (`token`) |

### 🏷️ Product Endpoints (`/api/product`)
| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/product/list` | Fetch all products in catalog | No |
| `POST` | `/api/product/id` | Fetch product details by ID | No |
| `POST` | `/api/product/add` | Upload images and add new product | Yes (`sellerToken`) |
| `POST` | `/api/product/stock` | Toggle in-stock / out-of-stock state | Yes (`sellerToken`) |

### 🛍️ Cart & Address Endpoints (`/api/cart`, `/api/address`)
| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/cart/update` | Synchronize user cart items object | Yes (`token`) |
| `POST` | `/api/address/add` | Add a new shipping address | Yes (`token`) |
| `GET` | `/api/address/get` | Retrieve user saved addresses | Yes (`token`) |

### 💳 Order & Payment Endpoints (`/api/order`)
| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/order/cod` | Place Cash on Delivery order | Yes (`token`) |
| `POST` | `/api/order/stripe` | Create Stripe Checkout payment session | Yes (`token`) |
| `POST` | `/stripe` | Stripe Webhook listener for payment events | Stripe Signature |
| `GET` | `/api/order/user` | Fetch order history for logged-in user | Yes (`token`) |
| `GET` | `/api/order/seller` | Fetch all store orders for administration | Yes (`sellerToken`) |

### 💼 Seller Endpoints (`/api/seller`)
| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/seller/login` | Admin/Seller login validation | No |
| `GET` | `/api/seller/is-auth` | Check seller authorization status | Yes (`sellerToken`) |
| `GET` | `/api/seller/logout` | Clear seller session cookie | Yes (`sellerToken`) |

---

## 🚀 Local Installation & Setup Guide

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/anuj-hmm-greencart.git
cd anuj-hmm-greencart
```

### 2. Configure Backend Server
Navigate to the `server/` directory:
```bash
cd server
npm install
```

Create a `.env` file in the `server/` root directory:
```env
PORT=4000
MONGODB_URI=your_mongodb_connection_uri
JWT_SECRET=your_jwt_secret_key
NODE_ENV=development

# Seller / Admin Credentials
SELLER_EMAIL=admin@greencart.com
SELLER_PASSWORD=your_secure_seller_password

# Cloudinary CDN Configuration
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret

# Stripe Payments Configuration
STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_WEBHOOK_SECRET=your_stripe_webhook_signing_secret
```

Start the development server with live reload:
```bash
npm run server
```
*Backend will run on `http://localhost:4000`.*

### 3. Configure Frontend Client
Open a new terminal window and navigate to the `client/` directory:
```bash
cd ../client
npm install
```

Create a `.env` file in the `client/` root directory:
```env
VITE_BACKEND_URL=http://localhost:4000
VITE_CURRENCY=$
```

Start the Vite local development server:
```bash
npm run dev
```
*Frontend will launch at `http://localhost:5173`.*

---

## 🔒 Security Best Practices Implemented
- **Credentials & Origin Control**: CORS configured with whitelist verification and `credentials: true` for secure cookie transmission.
- **No Sensitive Data in Payloads**: Passwords stripped from responses via Mongoose `.select('-password')`.
- **Payment Verification**: Stripe orders rely on cryptographically signed webhook callbacks rather than client-side status confirmation to prevent tampering.

---

## 📜 License
Copyright © 2026 Anuj Hansda
