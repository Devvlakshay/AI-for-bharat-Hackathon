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

**FitView AI** is an AI-powered virtual try-on platform for the Indian retail clothing market. Users create a personalized avatar matching their body type and virtually try on clothes before purchasing — reducing return rates, improving customer confidence, and giving retailers actionable intelligence on demand and sizing trends.

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

### 1. Personalized Avatar Creation
Users upload a photo or enter body measurements. Our AI generates a realistic avatar matching their body shape, skin tone, and proportions.

### 2. Virtual Try-On Engine
Users browse a clothing catalog and tap "Try On" to see garments realistically rendered on their avatar — powered by state-of-the-art diffusion-based try-on models hosted on serverless GPUs.

### 3. Retail Intelligence Dashboard
Retailers get insights on which products are tried on most, size distribution demand, conversion funnels, and AI-powered demand forecasting.

---

## Key Features

### For Customers
- **Avatar Builder** — Create a personalized avatar from a photo or body measurements (height, weight, chest, waist, hip)
- **Virtual Try-On** — See clothes on your avatar with realistic draping and fit
- **AI Size Recommendation** — Get the right size suggested based on your measurements vs garment data
- **Style Suggestions** — AI recommends outfits based on your preferences, body type, and trending styles
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
|  | Avatar      |  | Try-On       |  | Product       | |
|  | Builder     |  | Viewer       |  | Catalog &     | |
|  | (Photo/     |  | (Result      |  | Cart          | |
|  |  Measure)   |  |  Display)    |  |               | |
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
| - Users        | | - Body Parsing   | | - Product      |
| - Products     | |   (MediaPipe)    | |   Images       |
| - Try-On       | | - Avatar Gen     | | - Avatar       |
|   Sessions     | | - Virtual Try-On | |   Assets       |
| - Analytics    | |   (IDM-VTON)     | | - Try-On       |
|                | | - Size Engine    | |   Results      |
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
| **3D/Avatar Rendering** | Three.js / React Three Fiber | Interactive 3D avatar display |
| **Backend** | FastAPI (Python) | Async, fast, native ML ecosystem |
| **Database** | MongoDB Atlas | Flexible schema for products & user data |
| **Cache** | Redis | Fast session and try-on result caching |
| **AI — Body Parsing** | MediaPipe / OpenPose | Extract pose and body segmentation from photos |
| **AI — Avatar Generation** | Ready Player Me API / Custom pipeline | Generate avatar from measurements or photo |
| **AI — Virtual Try-On** | IDM-VTON / OOTDiffusion | State-of-the-art diffusion-based garment try-on |
| **AI — Hosting** | Banana.dev / Replicate | Serverless GPU inference (pay-per-call, no idle cost) |
| **AI — Recommendations** | Sentence Transformers + Collaborative Filtering | Style and product suggestions |
| **Storage** | AWS S3 / Cloudinary | Product images, avatars, try-on results |
| **Auth** | Firebase Auth or JWT | Secure user authentication |
| **Deployment** | Vercel (frontend) + Railway/EC2 (backend) | Quick deploy, scalable |

---

## AI/ML Pipeline

### Step 1: Body Understanding
```
User Photo ──> MediaPipe Pose Estimation ──> Keypoints + Segmentation
                                                    |
User Measurements (manual input) ───────────────────┤
                                                    v
                                            Body Profile
                                       (height, proportions,
                                        shape classification)
```

### Step 2: Avatar Generation
```
Body Profile ──> Avatar Generation Engine ──> Personalized Avatar
                  |
                  ├── Option A: Ready Player Me API (3D avatar)
                  └── Option B: Custom 2D avatar from body template matching
```

### Step 3: Virtual Try-On
```
Avatar Image ──────┐
                    ├──> IDM-VTON / OOTDiffusion Model ──> Try-On Result Image
Garment Image ─────┘         (hosted on Banana.dev)
  + Garment Mask
  + Pose Data
```

### Step 4: Size Recommendation
```
User Measurements ──────┐
                        ├──> Size Matching Algorithm ──> Recommended Size
Garment Size Chart ─────┘                                + Fit Score (tight/perfect/loose)
```

### Step 5: Style Recommendation
```
User Preferences ──────────┐
Past Try-On History ────────┤
                            ├──> Recommendation Engine ──> Suggested Outfits
Trending Products ──────────┤
Body Type Compatibility ────┘
```

### Model Details

| Model | Purpose | Input | Output |
|-------|---------|-------|--------|
| **MediaPipe Pose** | Body keypoint detection | User photo | 33 pose landmarks |
| **MediaPipe Segmentation** | Person segmentation | User photo | Body mask |
| **IDM-VTON** | Virtual try-on | Avatar + garment image | Dressed avatar image |
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
  "body_measurements": {
    "height_cm": "number",
    "weight_kg": "number",
    "chest_cm": "number",
    "waist_cm": "number",
    "hip_cm": "number",
    "shoulder_cm": "number"
  },
  "avatar_url": "string",
  "avatar_config": {
    "skin_tone": "string",
    "body_shape": "string"
  },
  "preferences": {
    "styles": ["casual", "ethnic", "formal"],
    "favorite_brands": [],
    "size_preference": "M"
  },
  "created_at": "datetime",
  "updated_at": "datetime"
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

### Avatar
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/avatar/create` | Create avatar from photo or measurements |
| GET | `/api/avatar/{user_id}` | Get user's avatar |
| PUT | `/api/avatar/{user_id}` | Update avatar measurements |

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
| POST | `/api/tryon` | Run virtual try-on (avatar_id + product_id + size) |
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
│   │   │   ├── avatar/         # Avatar builder page
│   │   │   ├── catalog/        # Product listing
│   │   │   ├── product/[id]/   # Product detail + try-on
│   │   │   ├── tryon/          # Try-on results
│   │   │   ├── cart/
│   │   │   └── dashboard/      # Retailer analytics
│   │   ├── components/
│   │   │   ├── AvatarBuilder.tsx
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
│   │   │   ├── product.py      # Product schema
│   │   │   ├── tryon.py        # Try-on session schema
│   │   │   └── analytics.py    # Analytics schema
│   │   ├── routes/
│   │   │   ├── auth.py         # Auth endpoints
│   │   │   ├── avatar.py       # Avatar endpoints
│   │   │   ├── products.py     # Product endpoints
│   │   │   ├── tryon.py        # Try-on endpoints
│   │   │   ├── recommend.py    # Recommendation endpoints
│   │   │   └── analytics.py    # Analytics endpoints
│   │   ├── services/
│   │   │   ├── avatar_service.py    # Avatar generation logic
│   │   │   ├── tryon_service.py     # ML model integration
│   │   │   ├── size_engine.py       # Size recommendation
│   │   │   ├── style_engine.py      # Style recommendations
│   │   │   └── analytics_service.py # Analytics aggregation
│   │   ├── ml/
│   │   │   ├── body_parser.py       # MediaPipe integration
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

### Phase 2: Avatar System
**Goal:** Users can create and view their personalized avatar

- [ ] Build avatar creation form (manual measurements input)
- [ ] Integrate MediaPipe for photo-based body measurement extraction
- [ ] Build avatar generation pipeline (template matching or Ready Player Me)
- [ ] Create avatar display component on frontend
- [ ] Store avatar data and images in database and cloud storage

### Phase 3: Virtual Try-On Engine
**Goal:** Core feature — users see clothes on their avatar

- [ ] Set up Banana.dev / Replicate account for GPU inference
- [ ] Integrate IDM-VTON or OOTDiffusion model
- [ ] Pre-process garment images (generate masks, flat-lay normalization)
- [ ] Build try-on API endpoint (accepts avatar + garment, returns result)
- [ ] Build try-on viewer component on frontend
- [ ] Add loading states and result caching (Redis)
- [ ] Store try-on session history

### Phase 4: Intelligence Layer
**Goal:** Size recommendations, style suggestions, and retailer analytics

- [ ] Build size recommendation engine (measurement comparison algorithm)
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
3. CREATE AVATAR
   User enters measurements OR uploads a photo
   --> AI generates personalized avatar
   "Here's your FitView avatar!"
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
   Loading animation --> Result: avatar wearing the garment
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
