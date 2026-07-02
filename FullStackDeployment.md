# Deployment Guide

This guide helps you with deployment configuration and step-by-step instructions to deploy a full-stack app to Vercel and Render, plus optional production improvements (S3 for uploads, Resend for emails).

**1) Push repo to GitHub (if not already)**
- If you want a separate deployment repository, you can push the contents of this `deployment` folder as its own Git repo.
- Otherwise, push the full project (recommended) and use the monorepo paths when configuring Render/Vercel.


**2) Database Connection

### MongoDB Atlas
- Create cluster, DB user, and whitelist network access. Copy the connection string.

### PostgreSQL
- Project → New → PostgreSQL → name it your-db → create
- Copy these values:
  ```doc
  Internal Database URL
  Hostname
  Port
  Database
  Username
  Password
  ```
  Add them to your backend Environment Variables

### One Problem — Tables Won't Exist Yet
- Install [Render CLI](https://render.com/docs/cli) and login
- Go to your-db on render
- Click on `connect`
- Go to `External Database URL` and copy the `PSQL Command`
- Go to terminal and paste the `PSQL Command`.
- Once you're connected and see the `your-db=>` prompt, create your tables.


**3) Render (backend)**
- New → Web Service → Connect GitHub repo → Root directory: `backend`
- Build Command: `npm install && npm run build`
- Start Command: `npm start`
- Env vars (set in Render Dashboard):
  - `MONGODB_URI`
  - `JWT_SECRET`
  - `FRONTEND_URL` (set to your Vercel URL after deploying frontend)
  - `RESEND_API_KEY` (optional)
  - `ENABLE_REMINDER_WORKER` = `false` for the web service
  - `REMINDER_CHECK_MS` (optional)
- Deploy and check logs.

  ### Possible error
  If any module error arises like
  ```bash
  Error: Cannot find module '/opt/render/project/src/backend/dist/server.js'
  ```
  Check `tsconfig.json` and remove:
  ```json
  "allowImportingTsExtensions": true,
  "noEmit": true,
  ```
  push to github


**4) (Optional) Render Background Worker (reminders)**
- New → Background Worker → Root directory: `backend`
- Build: `npm install && npm run build`
- Start: `npm start`
- Env vars: Same as web but `ENABLE_REMINDER_WORKER=true`


**5) Vercel (frontend)**
- Create project → Import GitHub repo → Root directory: `frontend`
- Framework Preset: `vite`
- Build Command: `npm ci && npm run build`
- Output Directory: `dist`
- Add Environment Variable: `VITE_API_URL=https://<your-backend>/api`
- Deploy and copy the Vercel URL.
- Update `FRONTEND_URL`(Domain) in Render to the Vercel URL. Manually redeploy.
- CORS check
  
  ```bash
  app.use(
    cors({
      origin: [
        "http://localhost:5173",
        "https://your-app.vercel.app", //FRONTEND_URL
      ],
      credentials: true,
    })
  );
  ```

**6) SPA routing**
- Create vercel.json in your frontend root
   
  ```bash
  {
    "rewrites": [
      {
        "source": "/(.*)",
        "destination": "/index.html"
      }
    ]
  }
  ```
- Redeploy frontend
