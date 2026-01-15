# Vercel Deployment Fix

## Issue
Vercel is trying to run `vite build` but can't find the `vite` command because it's not installing dependencies or running from the wrong directory.

## Solution

### Option 1: Configure Root Directory in Vercel Dashboard (Recommended)

1. **Go to your Vercel project settings**
2. **Settings → General → Root Directory**
3. **Set Root Directory to**: `frontend`
4. **Save**
5. **Redeploy**

This tells Vercel to treat the `frontend` folder as the project root.

### Option 2: Use Root vercel.json (Alternative)

I've created a `vercel.json` at the root that points to the frontend directory. If Option 1 doesn't work, Vercel will use this configuration.

### Option 3: Manual Build Command Override

In Vercel Dashboard:
1. **Settings → Build & Development Settings**
2. **Override** the following:
   - **Root Directory**: `frontend`
   - **Build Command**: `npm install && npm run build`
   - **Output Directory**: `dist`
   - **Install Command**: `npm install`
   - **Framework Preset**: `Vite`

## Verify Configuration

After setting up, your Vercel project should have:
- ✅ Root Directory: `frontend`
- ✅ Build Command: `npm install && npm run build` (or just `npm run build`)
- ✅ Output Directory: `dist`
- ✅ Framework: `Vite`

## Test Locally

Before deploying, test the build locally:
```bash
cd frontend
npm install
npm run build
```

This should create a `dist` folder. If it works locally, it should work on Vercel.

## Common Issues

### "vite: command not found"
- ✅ Make sure Root Directory is set to `frontend`
- ✅ Verify `package.json` exists in `frontend/`
- ✅ Check that `vite` is in `devDependencies` (it is)

### "Cannot find module"
- ✅ Make sure all dependencies are in `package.json`
- ✅ Vercel should auto-install, but verify install command

### Build succeeds but site doesn't load
- ✅ Check Output Directory is `dist`
- ✅ Verify `dist/index.html` exists after build

