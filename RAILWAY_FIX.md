# Railway Deployment Fix

## Problem
Railway was detecting the root directory instead of the `backend/` directory where Django is located.

## Solution

### Option 1: Configure Root Directory in Railway Dashboard (Recommended)

1. Go to your Railway project
2. Click on your **service** (the Django app)
3. Go to **Settings** → **Service**
4. Set **Root Directory** to: `backend`
5. Railway will now detect Python/Django automatically

### Option 2: Use Railway Configuration

The repository now includes:
- `nixpacks.toml` at root - tells Railway how to build
- `backend/railway.json` - specifies backend as root directory
- Updated `railway.json` - references nixpacks config

After pushing, Railway should automatically detect the backend directory.

## Manual Configuration Steps

If automatic detection still fails:

1. **In Railway Dashboard:**
   - Service → Settings → Service
   - **Root Directory**: `backend`
   - **Build Command**: (leave empty, auto-detected)
   - **Start Command**: (leave empty, uses Procfile)

2. **Verify Environment Variables:**
   - `SECRET_KEY` - Required
   - `DEBUG=False` - Required
   - `OPENAI_API_KEY` - Required
   - `DATABASE_URL` - Auto-set by Railway PostgreSQL

3. **Redeploy:**
   - Click "Redeploy" or push a new commit

## Testing Locally

To test the build process locally:

```bash
cd backend
pip install -r requirements.txt
python manage.py collectstatic --noinput
python manage.py migrate
gunicorn golexai.wsgi:application --bind 0.0.0.0:8000
```

## Expected Build Output

Railway should:
1. Detect Python 3.13
2. Install dependencies from `backend/requirements.txt`
3. Run `collectstatic`
4. Start with Gunicorn

If you see errors, check the deployment logs in Railway dashboard.

