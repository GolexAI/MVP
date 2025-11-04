# Railway Deployment Troubleshooting

## Current Issue: Railway Not Detecting Build

### Problem
Railway is looking at the root directory but Django is in `backend/` folder.

### Solution: Manual Configuration Required

Railway cannot auto-detect subdirectories. You **MUST** configure it manually:

## Step-by-Step Fix

### 1. Open Railway Dashboard
- Go to https://railway.app
- Select your project
- Click on your **service** (the Django app service)

### 2. Configure Root Directory
1. Click **Settings** tab
2. Scroll to **"Service"** section
3. Find **"Root Directory"** field
4. Enter: `backend`
5. Click **"Save"**

### 3. Configure Build Settings
1. Still in Settings → Service
2. **Build Command**: Leave empty (auto-detected)
3. **Start Command**: Leave empty (uses Procfile from backend/)

### 4. Trigger Redeploy
After saving:
1. Go to **"Deployments"** tab
2. Click **"Redeploy"** button (or **"Deploy"**)
3. Select the latest commit
4. Click **"Deploy"**

### 5. Verify Environment Variables
Go to **Settings** → **Variables** and ensure:
- `SECRET_KEY` is set
- `DEBUG=False`
- `OPENAI_API_KEY` is set
- `DATABASE_URL` is auto-set by Railway (from PostgreSQL service)

## Alternative: Check Service Connection

If Railway isn't watching your repo:

1. Go to **Settings** → **Source**
2. Verify GitHub repository is connected
3. Verify branch is set to `main`
4. Enable **"Auto Deploy"** if not enabled

## Verify Build Detection

After setting root directory to `backend`, Railway should:
- Detect Python from `backend/requirements.txt`
- Detect Django from `backend/manage.py`
- Use `backend/Procfile` for start command

## Expected Build Logs

You should see:
```
Detected Python project
Installing dependencies from requirements.txt
Collecting static files...
Running migrations...
Starting Gunicorn...
```

## If Still Not Working

1. **Delete and recreate service:**
   - Delete current service
   - Create new service from GitHub
   - **Immediately set root directory to `backend`** before first deploy

2. **Check Railway status:**
   - Railway status page: https://status.railway.app
   - Check for any outages

3. **Use Railway CLI:**
   ```bash
   npm i -g @railway/cli
   railway login
   railway link
   railway up
   ```

## Important Notes

- **Root Directory MUST be set manually** - Railway cannot auto-detect subdirectories
- **Root Directory must be set BEFORE first deploy** for best results
- **If you set it after, you must manually redeploy**
- Railway watches the GitHub repo, so commits should trigger deploys (if auto-deploy is enabled)

