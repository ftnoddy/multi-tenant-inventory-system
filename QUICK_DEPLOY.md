# Quick Deployment Guide - Vercel + Render

## 🚀 Fast Track (5 minutes)

### Step 1: Deploy Backend to Render (Free)

1. **Go to**: https://dashboard.render.com
2. **Click**: "New +" → "Web Service"
3. **Connect**: Your GitHub repo (`ftnoddy/multi-tenant-inventory-system`)
4. **Settings**:
   - **Name**: `multi-tenant-inventory-backend`
   - **Root Directory**: `backend`
   - **Environment**: `Node`
   - **Build Command**: `npm install && npm run build`
   - **Start Command**: `npm start`
5. **Environment Variables** (Add these):
   ```
   NODE_ENV=production
   PORT=3000
   MONGO_URI=mongodb+srv://atindramohandas353:jTinQAfHxGYEeot4@cluster0.ijljceb.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0
   JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
   JWT_EXPIRATION=24h
   ORIGIN=*
   CREDENTIALS=true
   FRONTEND_URL=https://your-app.vercel.app
   ```
6. **Click**: "Create Web Service"
7. **Wait**: 5-10 minutes for deployment
8. **Copy**: Your backend URL (e.g., `https://multi-tenant-inventory-backend.onrender.com`)

### Step 2: Deploy Frontend to Vercel (Free)

#### Option A: GitHub Integration (Easiest)

1. **Go to**: https://vercel.com/dashboard
2. **Click**: "Add New Project"
3. **Import**: Your GitHub repo
4. **Configure**:
   - **Root Directory**: `frontend` (IMPORTANT!)
   - **Framework Preset**: `Vite`
   - **Build Command**: `npm run build` (auto-detected)
   - **Output Directory**: `dist` (auto-detected)
5. **Environment Variables**:
   - `VITE_API_URL` = `https://multi-tenant-inventory-backend.onrender.com` (from Step 1)
6. **Click**: "Deploy"
7. **Wait**: 2-3 minutes
8. **Copy**: Your frontend URL (e.g., `https://multi-tenant-inventory.vercel.app`)

#### Option B: Vercel CLI

```bash
cd frontend
npm install -g vercel
vercel login
vercel
# Follow prompts, then:
vercel --prod
```

### Step 3: Update CORS

1. **Go back to Render dashboard**
2. **Update Environment Variable**:
   - `ORIGIN` = `https://your-app.vercel.app` (from Step 2)
   - `FRONTEND_URL` = `https://your-app.vercel.app`
3. **Restart** the service (or it auto-restarts)

### Step 4: Test

1. Visit your Vercel URL
2. Login with test credentials:
   - **Email**: `owner@techcorp.com`
   - **Password**: `password123`

---

## 📋 Environment Variables Checklist

### Backend (Render):
```
✅ NODE_ENV=production
✅ PORT=3000
✅ MONGO_URI=your_mongodb_connection_string
✅ JWT_SECRET=your_secret_key
✅ JWT_EXPIRATION=24h
✅ ORIGIN=https://your-app.vercel.app
✅ CREDENTIALS=true
✅ FRONTEND_URL=https://your-app.vercel.app
```

### Frontend (Vercel):
```
✅ VITE_API_URL=https://your-backend.onrender.com
```

---

## 🔧 Troubleshooting

### Frontend shows "Network Error"
- ✅ Check `VITE_API_URL` is correct in Vercel
- ✅ Verify backend URL is accessible (visit in browser)

### CORS Error
- ✅ Update `ORIGIN` in Render to match Vercel URL
- ✅ Restart Render service

### Backend won't start
- ✅ Check build logs in Render
- ✅ Verify `MONGO_URI` is correct
- ✅ Check Node.js version (should be 16+)

### Database connection fails
- ✅ Verify MongoDB Atlas allows connections from anywhere (0.0.0.0/0)
- ✅ Check `MONGO_URI` format is correct

---

## 🎯 What You Get

- ✅ **Frontend**: `https://your-app.vercel.app`
- ✅ **Backend**: `https://your-backend.onrender.com`
- ✅ **Free hosting** on both platforms
- ✅ **Automatic HTTPS**
- ✅ **Auto-deploy on git push** (if connected)

---

## 📝 Notes

- Render free tier: Sleeps after 15min inactivity (wakes on first request)
- Vercel free tier: Unlimited deployments, 100GB bandwidth/month
- Both platforms auto-deploy on git push (if connected)

---

## 🆘 Need Help?

1. Check deployment logs in Vercel/Render dashboards
2. Test backend API directly: `https://your-backend.onrender.com/api/inventory/dashboard/stats`
3. Check browser console for frontend errors
4. Verify all environment variables are set correctly

