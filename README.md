# FitView AI — Virtual Try-On Platform for Retail

> AI for Bharat Hackathon | Professional Track | Problem Statement 01: AI for Retail, Commerce & Market Intelligence

## Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Our Solution](#our-solution)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [Tech Stack](#tech-stack)
- [AI/ML Pipeline](#aiml-pipeline)
- [Database Design](#database-design)
- [API Design](#api-design)
- [Project Structure](#project-structure)
- [Implementation Plan](#implementation-plan)
- [Setup & Installation](#setup--installation)
- [Demo Flow](#demo-flow)
- [Business Impact](#business-impact)
- [Team](#team)
- [License](#license)

---

## Overview

**FitView AI** is an AI-powered virtual try-on platform for the Indian retail clothing market. Brands provide images of their models, and users select from 4-5 pre-built models to virtually try on clothes before purchasing — reducing return rates, improving customer confidence, and giving retailers actionable intelligence on demand and sizing trends.

---

## Problem Statement

Online fashion retail in India faces critical challenges:

| Problem | Impact |
|---------|--------|
| Customers cannot visualize fit before buying | 25-40% return rate on clothing orders |
| Size charts vary across brands | Wrong size is the #1 reason for returns |
| No personalized shopping experience | Low conversion rates, high bounce |
| Retailers lack try-on engagement data | Poor demand forecasting and inventory waste |

> The Indian online fashion market is projected to reach $35B by 2027. Even a 5% reduction in returns saves the industry hundreds of crores annually.

---

## Our Solution

A full-stack web platform with three pillars:

### 1. Model Selection
Brands provide images of their own models. Users choose from 4-5 pre-built model images representing different body types — no avatar creation or photo upload needed.

### 2. Virtual Try-On Engine
Users browse a clothing catalog and tap "Try On" to see garments realistically rendered on their selected model — powered by state-of-the-art diffusion-based try-on models hosted on serverless GPUs.

### 3. Retail Intelligence Dashboard
Retailers get insights on which products are tried on most, size distribution demand, conversion funnels, and AI-powered demand forecasting.

---

## Key Features

### For Customers
- **Model Selection** — Choose from 4-5 brand-provided model images representing different body types
- **Virtual Try-On** — See clothes on the selected model with realistic draping and fit
- **AI Size Recommendation** — Get the right size suggested based on garment data
- **Style Suggestions** — AI recommends outfits based on your preferences and trending styles
- **Try-On History** — Review all past try-ons, save favorites, share with friends
- **Wishlist & Cart** — Seamless e-commerce flow integrated with the try-on experience

### For Retailers / Admins
- **Analytics Dashboard** — Track try-on counts, conversion rates, popular products
- **Demand Forecasting** — Predict which styles and sizes will trend based on try-on engagement data
- **Inventory Intelligence** — Restocking suggestions powered by real-time try-on analytics
- **Return Rate Predictor** — Flag products with high size-mismatch risk before they ship

---

## System Architecture

```
+-----------------------------------------------------+
|                    CLIENT (Next.js)                   |
|                                                       |
|  +-------------+  +--------------+  +---------------+ |
|  | Model       |  | Try-On       |  | Product       | |
|  | Selector    |  | Viewer       |  | Catalog &     | |
|  | (Brand      |  | (Result      |  | Cart          | |
|  |  Models)    |  |  Display)    |  |               | |
|  +------+------+  +------+-------+  +-------+-------+ |
+---------|--------------|--------------------|----------+
          |              |                    |
          v              v                    v
+-----------------------------------------------------+
|                 BACKEND API (FastAPI)                  |
|                                                       |
|  +-------------+  +--------------+  +---------------+ |
|  | Auth &      |  | Try-On       |  | Analytics &   | |
|  | User        |  | Service      |  | Forecasting   | |
|  | Service     |  | (ML Bridge)  |  | Service       | |
|  +------+------+  +------+-------+  +-------+-------+ |

+---------|--------------|--------------------|----------+
          |              |                    |
          v              v                    v
+----------------+ +------------------+ +----------------+
|   MongoDB      | |  AI/ML Engine    | | Cloud Storage  |
|                | |                  | |                |
| - Users        | | - Virtual Try-On | | - Product      |
| - Products     | |   (IDM-VTON)     | |   Images       |
| - Models       | | - Size Engine    | | - Model        |
|   (Brand)      | |                  | |   Images       |
| - Try-On       | |                  | | - Try-On       |
|   Sessions     | |                  | |   Results      |
| - Analytics    | |                  | |                |
|                | |                  | |                |
| + Redis Cache  | | Hosted on        | | AWS S3 /       |
|                | | Banana.dev /     | | Cloudinary     |
|                | | Replicate        | |                |
+----------------+ +------------------+ +----------------+
```

---

## Tech Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| **Frontend** | Next.js 14, React, TailwindCSS | SSR, fast routing, modern UI |
| **UI Components** | React, TailwindCSS | Model selector, try-on viewer |
| **Backend** | FastAPI (Python) | Async, fast, native ML ecosystem |
| **Database** | MongoDB Atlas | Flexible schema for products & user data |
| **Cache** | Redis | Fast session and try-on result caching |
| **AI — Virtual Try-On** | IDM-VTON / OOTDiffusion | State-of-the-art diffusion-based garment try-on |
| **AI — Hosting** | Banana.dev / Replicate | Serverless GPU inference (pay-per-call, no idle cost) |
| **AI — Recommendations** | Sentence Transformers + Collaborative Filtering | Style and product suggestions |
| **Storage** | AWS S3 / Cloudinary | Product images, model images, try-on results |
| **Auth** | Firebase Auth or JWT | Secure user authentication |
| **Deployment** | Vercel (frontend) + Railway/EC2 (backend) | Quick deploy, scalable |

---

## AI/ML Pipeline

### Step 1: Model Selection
```
Brand uploads 4-5 model images ──> Stored in cloud storage
                                          |
User selects a model ────────────────────>┘
                                          v
                                   Selected Model Image
```

### Step 2: Virtual Try-On
```
Selected Model Image ──┐
                       ├──> IDM-VTON / OOTDiffusion Model ──> Try-On Result Image
Garment Image ─────────┘         (hosted on Banana.dev)
  + Garment Mask
  + Pose Data
```

### Step 3: Size Recommendation
```
Garment Size Chart ──────┐
                         ├──> Size Matching Algorithm ──> Recommended Size
User Selected Size ──────┘                                + Fit Score (tight/perfect/loose)
```

### Step 4: Style Recommendation
```
User Preferences ──────────┐
Past Try-On History ────────┤
                            ├──> Recommendation Engine ──> Suggested Outfits
Trending Products ──────────┘
```

### Model Details

| Model | Purpose | Input | Output |
|-------|---------|-------|--------|
| **IDM-VTON** | Virtual try-on | Model image + garment image | Dressed model image |
| **Sentence Transformers** | Style similarity | Product descriptions | Embedding vectors |

---

## Database Design

### Users Collection
```json
{
  "_id": "ObjectId",
  "name": "string",
  "email": "string",
  "password_hash": "string",
  "phone": "string",
  "preferred_model_id": "ObjectId (ref: Models)",
  "preferences": {
    "styles": ["casual", "ethnic", "formal"],
    "favorite_brands": [],
    "size_preference": "M"
  },
  "created_at": "datetime",
  "updated_at": "datetime"
}
```

### Models Collection (Brand-Provided)
```json
{
  "_id": "ObjectId",
  "name": "string",
  "brand": "string",
  "body_type": "slim | average | athletic | plus-size",
  "image_url": "url",
  "pose_data": "url",
  "created_at": "datetime"
}
```

### Products Collection
```json
{
  "_id": "ObjectId",
  "name": "string",
  "brand": "string",
  "category": "topwear | bottomwear | ethnic | western",
  "sub_category": "shirt | kurta | jeans | saree",
  "price": "number",
  "currency": "INR",
  "sizes": [
    {
      "label": "M",
      "chest_cm": 100,
      "waist_cm": 86,
      "length_cm": 72,
      "stock": 45
    }
  ],
  "images": {
    "front": "url",
    "back": "url",
    "flat_lay": "url"
  },
  "garment_mask_url": "url",
  "tags": ["cotton", "summer", "trending"],
  "trending_score": "number",
  "try_on_count": "number",
  "created_at": "datetime"
}
```

### TryOn Sessions Collection
```json
{
  "_id": "ObjectId",
  "user_id": "ObjectId (ref: Users)",
  "model_id": "ObjectId (ref: Models)",
  "product_id": "ObjectId (ref: Products)",
  "size_selected": "M",
  "size_recommended": "L",
  "result_image_url": "url",
  "fit_score": 0.87,
  "liked": false,
  "added_to_cart": false,
  "purchased": false,
  "created_at": "datetime"
}
```

### Analytics Collection (Aggregated Daily)
```json
{
  "_id": "ObjectId",
  "date": "date",
  "product_id": "ObjectId (ref: Products)",
  "try_on_count": "number",
  "unique_users": "number",
  "cart_additions": "number",
  "conversion_rate": "number",
  "most_tried_size": "M",
  "size_distribution": {
    "S": 12, "M": 45, "L": 30, "XL": 13
  },
  "avg_fit_score": 0.82
}
```

---

## API Design

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Register new user |
| POST | `/api/auth/login` | Login, returns JWT |
| GET | `/api/auth/me` | Get current user profile |

### Models (Brand-Provided)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/models` | List all available brand models |
| GET | `/api/models/{id}` | Get model details |
| POST | `/api/models` | Upload a new model image (admin/brand) |

### Products
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/products` | List products (with filters, pagination) |
| GET | `/api/products/{id}` | Get product details |
| GET | `/api/products/trending` | Get trending products |
| POST | `/api/products` | Add product (admin) |

### Virtual Try-On
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/tryon` | Run virtual try-on (model_id + product_id + size) |
| GET | `/api/tryon/history/{user_id}` | Get user's try-on history |
| GET | `/api/tryon/{session_id}` | Get specific try-on result |

### Recommendations
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/recommend/size?user_id=X&product_id=Y` | Get size recommendation |
| GET | `/api/recommend/style/{user_id}` | Get style suggestions |

### Analytics (Retailer)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/analytics/dashboard` | Overview metrics |
| GET | `/api/analytics/product/{id}` | Per-product try-on analytics |
| GET | `/api/analytics/forecast` | Demand forecasting data |
| GET | `/api/analytics/size-trends` | Size demand distribution |

---

## Project Structure

```
fitview-ai/
├── frontend/                    # Next.js application
│   ├── public/
│   │   └── assets/             # Static images, icons
│   ├── src/
│   │   ├── app/                # Next.js app router pages
│   │   │   ├── page.tsx        # Landing page
│   │   │   ├── login/
│   │   │   ├── register/
│   │   │   ├── models/         # Model selection page
│   │   │   ├── catalog/        # Product listing
│   │   │   ├── product/[id]/   # Product detail + try-on
│   │   │   ├── tryon/          # Try-on results
│   │   │   ├── cart/
│   │   │   └── dashboard/      # Retailer analytics
│   │   ├── components/
│   │   │   ├── ModelSelector.tsx
│   │   │   ├── TryOnViewer.tsx
│   │   │   ├── ProductCard.tsx
│   │   │   ├── SizeRecommender.tsx
│   │   │   ├── StyleSuggestions.tsx
│   │   │   ├── Navbar.tsx
│   │   │   └── AnalyticsCharts.tsx
│   │   ├── lib/
│   │   │   ├── api.ts          # API client
│   │   │   └── auth.ts         # Auth utilities
│   │   └── styles/
│   ├── tailwind.config.js
│   ├── next.config.js
│   └── package.json
│
├── backend/                     # FastAPI application
│   ├── app/
│   │   ├── main.py             # FastAPI entry point
│   │   ├── config.py           # Environment config
│   │   ├── models/
│   │   │   ├── user.py         # User schema
│   │   │   ├── brand_model.py  # Brand model schema
│   │   │   ├── product.py      # Product schema
│   │   │   ├── tryon.py        # Try-on session schema
│   │   │   └── analytics.py    # Analytics schema
│   │   ├── routes/
│   │   │   ├── auth.py         # Auth endpoints
│   │   │   ├── models.py       # Brand model endpoints
│   │   │   ├── products.py     # Product endpoints
│   │   │   ├── tryon.py        # Try-on endpoints
│   │   │   ├── recommend.py    # Recommendation endpoints
│   │   │   └── analytics.py    # Analytics endpoints
│   │   ├── services/
│   │   │   ├── model_service.py     # Brand model management
│   │   │   ├── tryon_service.py     # ML model integration
│   │   │   ├── size_engine.py       # Size recommendation
│   │   │   ├── style_engine.py      # Style recommendations
│   │   │   └── analytics_service.py # Analytics aggregation
│   │   ├── ml/
│   │   │   ├── tryon_model.py       # Banana.dev/Replicate client
│   │   │   └── embeddings.py        # Product embedding generation
│   │   └── database/
│   │       └── mongodb.py           # DB connection & helpers
│   ├── requirements.txt
│   └── Dockerfile
│
├── ml/                          # ML notebooks & scripts
│   ├── notebooks/
│   │   ├── tryon_exploration.ipynb
│   │   └── size_recommendation.ipynb
│   └── scripts/
│       ├── preprocess_garments.py   # Prepare garment masks
│       └── generate_embeddings.py   # Product embeddings
│
├── docker-compose.yml
├── .env.example
├── .gitignore
└── README.md
```

---

## Implementation Plan

### Phase 1: Foundation
**Goal:** Working auth, product catalog, and basic UI

- [ ] Initialize Next.js frontend with TailwindCSS
- [ ] Initialize FastAPI backend with MongoDB connection
- [ ] Implement user registration and login (JWT auth)
- [ ] Create product catalog with sample Indian clothing data
- [ ] Build product listing page with filters (category, price, size)
- [ ] Build product detail page
- [ ] Set up cloud storage for images (S3/Cloudinary)

### Phase 2: Model Selection System
**Goal:** Brands upload model images, users can browse and select a model

- [ ] Build model upload flow for brands/admins
- [ ] Store model images in cloud storage with metadata (body type, brand)
- [ ] Build model selector component on frontend (grid of 4-5 models)
- [ ] Allow users to pick and save a preferred model
- [ ] Create model listing API endpoints

### Phase 3: Virtual Try-On Engine
**Goal:** Core feature — users see clothes on their avatar

- [ ] Set up Banana.dev / Replicate account for GPU inference
- [ ] Integrate IDM-VTON or OOTDiffusion model
- [ ] Pre-process garment images (generate masks, flat-lay normalization)
- [ ] Build try-on API endpoint (accepts model image + garment, returns result)
- [ ] Build try-on viewer component on frontend
- [ ] Add loading states and result caching (Redis)
- [ ] Store try-on session history

### Phase 4: Intelligence Layer
**Goal:** Size recommendations, style suggestions, and retailer analytics

- [ ] Build size recommendation engine
- [ ] Integrate size suggestion into try-on flow
- [ ] Build style recommendation engine (collaborative filtering + embeddings)
- [ ] Create retailer analytics dashboard
- [ ] Add demand forecasting (time-series analysis on try-on data)
- [ ] Add size distribution analytics per product

### Phase 5: Polish & Demo Prep
**Goal:** Production-ready demo for judges

- [ ] UI/UX polish — animations, transitions, responsive design
- [ ] Error handling and edge cases
- [ ] Performance optimization (image compression, lazy loading)
- [ ] Create demo dataset (10-15 Indian clothing products with proper images)
- [ ] End-to-end testing of the full flow
- [ ] Prepare pitch deck and demo script

---

## Setup & Installation

### Prerequisites
- Node.js 18+
- Python 3.10+
- MongoDB Atlas account (or local MongoDB)
- Banana.dev / Replicate API key
- Cloudinary / AWS S3 account

### Frontend Setup
```bash
cd frontend
npm install
cp .env.example .env.local
# Add your API URL and keys to .env.local
npm run dev
# Runs on http://localhost:3000
```

### Backend Setup
```bash
cd backend
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
# Add your MongoDB URI, API keys, etc. to .env
uvicorn app.main:app --reload
# Runs on http://localhost:8000
```

### Environment Variables
```
# Backend .env
MONGODB_URI=mongodb+srv://...
JWT_SECRET=your-secret-key
BANANA_API_KEY=your-banana-key
REPLICATE_API_TOKEN=your-replicate-token
CLOUDINARY_URL=cloudinary://...
REDIS_URL=redis://localhost:6379

# Frontend .env.local
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_FIREBASE_API_KEY=your-key  # if using Firebase Auth
```

---

## Demo Flow

The demo follows a user journey optimized for judges:

```
1. LANDING PAGE
   "Welcome to FitView AI — Try before you buy"
                    |
                    v
2. SIGN UP / LOGIN
   Quick registration with basic details
                    |
                    v
3. SELECT A MODEL
   User picks from 4-5 brand-provided model images
   representing different body types
   "Choose the model closest to your body type"
                    |
                    v
4. BROWSE CATALOG
   Indian clothing: kurtas, shirts, ethnic wear
   Filter by category, price, size
                    |
                    v
5. SELECT A PRODUCT
   View product details, available sizes
   Click "Try On"
                    |
                    v
6. VIRTUAL TRY-ON
   Loading animation --> Result: selected model wearing the garment
   "AI recommends size L for the best fit (Fit Score: 92%)"
                    |
                    v
7. ADD TO CART / TRY MORE
   Save the look, try another size, or browse more
                    |
                    v
8. RETAILER DASHBOARD (Switch to admin view)
   - "Kurta X was tried on 340 times today"
   - "Size M demand is 3x higher than current stock"
   - "Predicted: ethnic wear demand +40% next week (festival season)"
```

---

## Business Impact

| Metric | Impact |
|--------|--------|
| **Return Rate Reduction** | 25-40% fewer returns by showing accurate fit |
| **Conversion Rate** | 2-3x higher conversion when users try on virtually |
| **Customer Engagement** | 4x longer session time with try-on feature |
| **Inventory Optimization** | Stock the right sizes based on real demand signals |
| **Cost Savings** | Reduced reverse logistics costs for retailers |

### Revenue Model (Post-Hackathon)
- **SaaS for Retailers** — Monthly subscription for try-on widget + analytics
- **Per-Try-On API** — Pay-per-use for smaller brands
- **Premium Consumer Features** — Style reports, wardrobe planning

---

## Team

| Role | Responsibility |
|------|---------------|
| **Frontend Developer** | Next.js UI, avatar viewer, product pages, responsive design |
| **Backend Developer** | FastAPI, database, API endpoints, auth |
| **ML Engineer** | Try-on model integration, avatar pipeline, body parsing |
| **Full-stack / Design** | Analytics dashboard, UI/UX design, pitch deck |

---

## License

This project is built for the **AI for Bharat Hackathon 2025** — Professional Track.

---

> Built with purpose — making online fashion inclusive, intelligent, and return-free.
# AI-for-bharat-Hackathon
