# Vercel Deployment Guide

## Overview

This guide covers deploying the Multi-Tenant Inventory Management System to Vercel (Frontend) and a backend hosting service.

## Architecture

- **Frontend**: Deploy to Vercel (React + Vite)
- **Backend**: Deploy to Render/Railway/Heroku (Node.js + Express)

---

## Part 1: Frontend Deployment on Vercel

### Step 1: Prepare Frontend

1. **Create `.env.production` file** in `frontend/` directory:
   ```env
   VITE_API_URL=https://your-backend-url.onrender.com
   ```
   (Replace with your actual backend URL after deploying backend)

2. **Verify build works locally**:
   ```bash
   cd frontend
   npm run build
   ```
   This should create a `dist/` folder.

### Step 2: Deploy to Vercel

#### Option A: Using Vercel CLI (Recommended)

1. **Install Vercel CLI**:
   ```bash
   npm install -g vercel
   ```

2. **Login to Vercel**:
   ```bash
   vercel login
   ```

3. **Navigate to frontend directory**:
   ```bash
   cd frontend
   ```

4. **Deploy**:
   ```bash
   vercel
   ```
   - Follow the prompts
   - Select your project
   - Confirm settings

5. **Deploy to production**:
   ```bash
   vercel --prod
   ```

#### Option B: Using GitHub Integration (Easier)

1. **Go to [Vercel Dashboard](https://vercel.com/dashboard)**
2. **Click "Add New Project"**
3. **Import your GitHub repository**: `ftnoddy/multi-tenant-inventory-system`
4. **Configure Project**:
   - **Root Directory**: `frontend`
   - **Framework Preset**: Vite
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
   - **Install Command**: `npm install`

5. **Environment Variables**:
   - Add `VITE_API_URL` = `https://your-backend-url.onrender.com`
   - (Update this after backend is deployed)

6. **Click "Deploy"**

### Step 3: Update Environment Variables

After backend is deployed, update `VITE_API_URL` in Vercel:
1. Go to Project Settings → Environment Variables
2. Update `VITE_API_URL` with your backend URL
3. Redeploy

---

## Part 2: Backend Deployment on Render

### Step 1: Prepare Backend

1. **Create `render.yaml`** in `backend/` directory (optional, for easy setup)

2. **Update CORS** in `backend/src/app.ts`:
   ```typescript
   origin: process.env.FRONTEND_URL || 'https://your-frontend.vercel.app'
   ```

### Step 2: Deploy to Render

1. **Go to [Render Dashboard](https://dashboard.render.com)**
2. **Click "New +" → "Web Service"**
3. **Connect your GitHub repository**
4. **Configure Service**:
   - **Name**: `multi-tenant-inventory-backend`
   - **Environment**: Node
   - **Build Command**: `cd backend && npm install && npm run build`
   - **Start Command**: `cd backend && npm start`
   - **Root Directory**: `backend`

5. **Environment Variables**:
   ```
   NODE_ENV=production
   PORT=3000
   MONGO_URI=your_mongodb_atlas_connection_string
   JWT_SECRET=your_jwt_secret_key
   JWT_EXPIRATION=24h
   ORIGIN=https://your-frontend.vercel.app
   CREDENTIALS=true
   FRONTEND_URL=https://your-frontend.vercel.app
   ```

6. **Click "Create Web Service"**

7. **Wait for deployment** (takes 5-10 minutes)

8. **Copy the service URL** (e.g., `https://multi-tenant-inventory-backend.onrender.com`)

### Step 3: Update Frontend API URL

1. Go back to Vercel
2. Update `VITE_API_URL` environment variable with your Render backend URL
3. Redeploy frontend

---

## Alternative: Backend on Railway

### Step 1: Deploy to Railway

1. **Go to [Railway](https://railway.app)**
2. **Click "New Project" → "Deploy from GitHub repo"**
3. **Select your repository**
4. **Configure**:
   - **Root Directory**: `backend`
   - **Build Command**: `npm install && npm run build`
   - **Start Command**: `npm start`

5. **Add Environment Variables** (same as Render)

6. **Deploy**

---

## Alternative: Backend on Heroku

### Step 1: Prepare for Heroku

1. **Create `Procfile`** in `backend/` directory:
   ```
   web: npm start
   ```

2. **Update `package.json`** scripts (already done):
   ```json
   "start": "npm run build && cross-env NODE_ENV=production nodemon dist/server.js"
   ```

### Step 2: Deploy to Heroku

1. **Install Heroku CLI**:
   ```bash
   npm install -g heroku
   ```

2. **Login**:
   ```bash
   heroku login
   ```

3. **Create app**:
   ```bash
   cd backend
   heroku create multi-tenant-inventory-backend
   ```

4. **Set environment variables**:
   ```bash
   heroku config:set NODE_ENV=production
   heroku config:set MONGO_URI=your_mongodb_connection_string
   heroku config:set JWT_SECRET=your_jwt_secret
   heroku config:set ORIGIN=https://your-frontend.vercel.app
   ```

5. **Deploy**:
   ```bash
   git push heroku main
   ```

---

## Step-by-Step Quick Deploy

### 1. Deploy Backend First (Render - Free Tier)

1. Go to https://dashboard.render.com
2. New → Web Service
3. Connect GitHub repo: `ftnoddy/multi-tenant-inventory-system`
4. Settings:
   - **Root Directory**: `backend`
   - **Build**: `npm install && npm run build`
   - **Start**: `npm start`
5. Add env vars (MONGO_URI, JWT_SECRET, etc.)
6. Deploy → Copy URL (e.g., `https://xxx.onrender.com`)

### 2. Deploy Frontend (Vercel - Free Tier)

1. Go to https://vercel.com/dashboard
2. Add New Project
3. Import: `ftnoddy/multi-tenant-inventory-system`
4. Settings:
   - **Root Directory**: `frontend`
   - **Framework**: Vite
   - **Build**: `npm run build`
   - **Output**: `dist`
5. Add env var: `VITE_API_URL` = `https://xxx.onrender.com` (from step 1)
6. Deploy

### 3. Update CORS

1. Go to Render dashboard
2. Update `ORIGIN` env var to your Vercel URL
3. Restart service

---

## Environment Variables Checklist

### Backend (Render/Railway/Heroku):
```
NODE_ENV=production
PORT=3000
MONGO_URI=mongodb+srv://...
JWT_SECRET=your_secret_key_here
JWT_EXPIRATION=24h
ORIGIN=https://your-app.vercel.app
CREDENTIALS=true
FRONTEND_URL=https://your-app.vercel.app
```

### Frontend (Vercel):
```
VITE_API_URL=https://your-backend.onrender.com
```

---

## Testing Deployment

1. **Frontend**: Visit `https://your-app.vercel.app`
2. **Backend**: Test API at `https://your-backend.onrender.com/api/inventory/dashboard/stats`
3. **Login**: Use test credentials from README.md

---

## Troubleshooting

### Frontend Issues:
- **Build fails**: Check `npm run build` works locally
- **API errors**: Verify `VITE_API_URL` is correct
- **CORS errors**: Update backend `ORIGIN` env var

### Backend Issues:
- **Build fails**: Check Node.js version (should be 16+)
- **Database connection**: Verify `MONGO_URI` is correct
- **Port issues**: Render/Railway auto-assigns PORT

---

## Free Tier Limits

### Vercel:
- ✅ Unlimited deployments
- ✅ 100GB bandwidth/month
- ✅ Automatic HTTPS

### Render:
- ✅ 750 hours/month free
- ✅ Auto-sleeps after 15min inactivity (wakes on request)
- ✅ Free SSL

### Railway:
- ✅ $5 free credit/month
- ✅ Pay-as-you-go after

---

## Post-Deployment

1. **Update README.md** with live URLs
2. **Test all features** on live deployment
3. **Set up monitoring** (optional)
4. **Configure custom domain** (optional)

---

## Quick Commands Reference

```bash
# Frontend - Vercel
cd frontend
vercel login
vercel
vercel --prod

# Backend - Heroku
cd backend
heroku login
heroku create app-name
heroku config:set KEY=value
git push heroku main

# Check logs
vercel logs
heroku logs --tail
```

