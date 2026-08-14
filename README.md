# DevPortfolio — Full Stack Portfolio Website

A production-ready personal portfolio with a public site (React + Vite + Tailwind) and a secure admin dashboard (Express + MongoDB). Ships with demo data, dark/light themes, and graceful fallbacks so the frontend works even before the backend is configured.

## Tech Stack

| Layer    | Stack |
|----------|-------|
| Frontend | React 18, Vite 5, Tailwind CSS, Framer Motion, Lenis, Axios, React Hook Form, react-router-dom |
| Backend  | Node.js, Express, MongoDB (Mongoose), JWT, Multer, Cloudinary, Nodemailer |
| Deploy   | Vercel (frontend) + Render (backend) |

## Project Structure

```
portfolio/
├── frontend/          # React app (Vite)
│   ├── src/
│   │   ├── api/       # axios client + typed API wrappers
│   │   ├── data/      # fallback/demo data (used when API is offline)
│   │   ├── pages/     # public pages + admin pages
│   │   ├── components/  # layout, cards, sections, ui kit
│   │   ├── hooks/     # useFetch, usePageMeta, useSmoothScroll
│   │   └── context/   # Theme, Auth
│   └── vercel.json    # SPA rewrites
├── backend/           # Express REST API
│   ├── server.js      # bootstrap + demo-data seeding on startup
│   └── src/           # routes, controllers, models, middleware, utils
└── render.yaml        # Render blueprint for the backend
```

## Getting Started

### 1. Backend

```bash
cd backend
npm install
cp .env.example .env   # fill in real values (see below)
npm run dev            # starts on http://localhost:5000
```

On first start the server seeds a demo admin and sample content automatically.

**Default admin credentials:** `admin` / `ChangeMe123!` (change after first login).

### 2. Frontend

```bash
cd frontend
npm install
cp .env.example .env   # optional: VITE_API_URL for a remote backend
npm run dev            # starts on http://localhost:5173
```

Vite proxies `/api` to `http://localhost:5000` in development, so no env config is needed locally. If the backend is unreachable, every page falls back to bundled demo data.

## Environment Variables

### Backend (`backend/.env`)

| Variable | Required | Description |
|----------|----------|-------------|
| `MONGO_URI` | Yes | MongoDB connection string (MongoDB Atlas recommended) |
| `JWT_ACCESS_SECRET` / `JWT_REFRESH_SECRET` | Yes | Random 32+ char secrets |
| `CLIENT_URL` | No | Comma-separated CORS origins (default `http://localhost:5173`) |
| `CLOUDINARY_CLOUD_NAME` / `API_KEY` / `API_SECRET` | No | Image hosting; falls back to local `/uploads` |
| `SMTP_*` | No | Email notifications for contact form |
| `EMAILJS_*` | No | Frontend EmailJS fallback for the contact form |

### Frontend (`frontend/.env`)

| Variable | Description |
|----------|-------------|
| `VITE_API_URL` | Backend base URL, e.g. `https://your-api.onrender.com/api`. Defaults to `/api` (same-origin/proxy) |

## Admin Panel

- URL: `/admin` (protected route; login at `/admin/login`)
- Manage: profile, projects, certificates, skills, experience, education, achievements, gallery, blog posts, testimonials
- Contact messages: read / mark replied / delete
- SEO settings per page (titles, descriptions, Open Graph)
- Image uploads go to Cloudinary (or local `/uploads`)

## Deployment

### Frontend → Vercel

1. Import the `frontend/` folder as a new project (framework: Vite, output: `dist`).
2. Add env var `VITE_API_URL` pointing at your deployed backend.
3. `frontend/vercel.json` already handles SPA rewrites + asset caching.

### Backend → Render (or any Node host)

`render.yaml` includes the full blueprint (health check at `/api/health`, env vars). Or use the included root `.gitignore` + `npm start` on any host that can reach MongoDB.

## Scripts

| Command | Where | What it does |
|---------|-------|--------------|
| `npm run dev` | backend | Start API with file watching |
| `npm start` | backend | Start API (production) |
| `npm run seed` | backend | Force re-seed demo data |
| `npm run dev` | frontend | Start Vite dev server |
| `npm run build` | frontend | Production build to `dist/` |

## Notes

- Dark/light theme persists in localStorage; SSR-safe inline script prevents flash.
- All list endpoints support `search`, `page`, `limit`, `sort`, and category filtering.
- Blog content is Markdown, rendered with syntax highlighting.
- Rate limiting is applied on `/api` (global) and `/api/auth` (login attempts).
