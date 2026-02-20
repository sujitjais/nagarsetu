# 🏛️ NagarSeva — Smart Civic Issue Reporting & Resolution Platform

**Report · Track · Resolve · Your City, Your Voice**

A production-ready, full-stack civic issue reporting platform for India, deployable on Vercel in minutes.

---

## 🚀 Features

- ✅ **OTP-based Login** — Email OTP auth, no password needed
- 📍 **GPS Location Permission** — Auto-detects location with Geolocation API
- 📸 **Camera Permission** — Live camera capture for photo evidence
- 🤖 **AI Duplicate Detection** — Clusters nearby issues (client-side)
- 📊 **Real-time Dashboard** — Live stats for citizens and authorities
- 🏢 **Admin Panel** — Full issue management for officers
- 📱 **Responsive + PWA** — Works on mobile, tablet, desktop
- 📶 **Low-Bandwidth Mode** — SMS/IVR channel info for 2G areas
- 🔔 **Notifications** — In-app + email status updates
- 🗺️ **Heatmap** — SVG city map with issue visualization

---

## 📁 Project Structure

```
nagarseva/
├── pages/
│   ├── index.js              ← Main single-page app
│   ├── track/[ref].js        ← Public complaint tracking
│   ├── _app.js
│   └── api/
│       ├── auth/
│       │   ├── send-otp.js   ← OTP generation + email
│       │   ├── verify-otp.js ← OTP verification + JWT
│       │   └── me.js         ← Current user + logout
│       ├── issues/
│       │   ├── index.js      ← List issues (with filters)
│       │   ├── create.js     ← Create new issue (auth)
│       │   └── [id].js       ← Get / Update / Upvote
│       ├── admin/
│       │   └── stats.js      ← Admin dashboard stats
│       └── notifications.js
├── lib/
│   ├── db.js                 ← PostgreSQL pool
│   ├── auth.js               ← JWT + cookie helpers
│   ├── mailer.js             ← OTP email sending
│   └── s3.js                 ← AWS S3 file upload
├── styles/globals.css
├── sql/schema.sql            ← Full DB schema + seed data
├── .env.example
├── vercel.json
└── package.json
```

---

## ⚡ Quick Deploy to Vercel

### Step 1: Set up PostgreSQL

Use **Neon** (free tier, recommended for Vercel):
1. Go to [neon.tech](https://neon.tech) → Create project "nagarseva"
2. Copy the connection string
3. Open **SQL Editor** → paste contents of `sql/schema.sql` → Run

Or use **Supabase**: [supabase.com](https://supabase.com) → New Project → SQL Editor

### Step 2: Deploy to Vercel

```bash
# Install Vercel CLI
npm install -g vercel

# From project directory
vercel

# Follow prompts: Yes to all defaults
```

**Or via GitHub:**
1. Push this folder to a GitHub repo
2. Go to [vercel.com](https://vercel.com) → New Project → Import repo
3. Framework: Next.js (auto-detected)
4. Click Deploy

### Step 3: Set Environment Variables

In Vercel Dashboard → Your Project → Settings → Environment Variables:

| Variable | Value | Required |
|----------|-------|----------|
| `DATABASE_URL` | `postgresql://...` from Neon/Supabase | ✅ |
| `JWT_SECRET` | Random 32+ char string | ✅ |
| `SMTP_HOST` | `smtp.gmail.com` | ✅ for emails |
| `SMTP_PORT` | `587` | ✅ |
| `SMTP_USER` | your Gmail address | ✅ |
| `SMTP_PASS` | Gmail App Password | ✅ |
| `AWS_ACCESS_KEY_ID` | AWS IAM key | ⚠️ For real uploads |
| `AWS_SECRET_ACCESS_KEY` | AWS secret | ⚠️ |
| `AWS_REGION` | `ap-south-1` | ⚠️ |
| `S3_BUCKET_NAME` | Your S3 bucket name | ⚠️ |
| `NEXT_PUBLIC_APP_URL` | `https://your-app.vercel.app` | ✅ |

> ⚠️ Without AWS S3, photos will show a placeholder URL. App still works fully.

### Step 4: Redeploy

```bash
vercel --prod
```

---

## 💻 Local Development

```bash
# Clone / copy project folder
cd nagarseva

# Install dependencies
npm install

# Copy env file
cp .env.example .env.local
# Fill in DATABASE_URL and other values

# Run the schema
psql $DATABASE_URL < sql/schema.sql

# Start dev server
npm run dev
# → http://localhost:3000
```

**In development, OTPs are printed to the console** (no SMTP needed).

---

## 📱 How to Test

1. Open the app → Click **Login / Register**
2. Enter your email → Click **Continue**
3. Check console (dev) or email (prod) for 6-digit OTP
4. Enter OTP → You're logged in!
5. Click **+ Report Issue** → Allow location permission → Allow camera
6. Fill the 4-step form and submit
7. Check **My Issues** to see your complaint

**Admin access:**
- Update `users` table: `UPDATE users SET role='admin' WHERE email='your@email.com';`
- Reload → Admin Dashboard tab appears

---

## 🗄️ Database Notes

- Run `sql/schema.sql` once on your PostgreSQL instance
- Requires PostgreSQL 12+ (Neon/Supabase/Railway all work)
- PostGIS extension is optional (for geospatial queries)
- Default admin: `admin@nagarseva.gov.in` (promote via SQL)

---

## 🔐 Security

- OTPs expire in 10 minutes, single-use
- JWT tokens expire in 7 days, stored in httpOnly cookies
- Rate limiting should be added via Vercel Edge Config in production
- S3 bucket: Set CORS to allow your Vercel domain

---

## 📊 Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 14, React 18, vanilla CSS-in-JS |
| Backend | Next.js API Routes (serverless) |
| Database | PostgreSQL (Neon/Supabase) |
| Auth | JWT + OTP via Email |
| Storage | AWS S3 (media uploads) |
| Email | Nodemailer (SMTP) |
| Deploy | Vercel |

---

Built for Bharat 🇮🇳 · Serving 1.2B+ Citizens
