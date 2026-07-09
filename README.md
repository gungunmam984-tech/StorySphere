# Lumière Journal — Premium Blog

A full-stack, premium blog built with **Next.js (App Router)**, **Tailwind CSS**, **Drizzle ORM**
and **PostgreSQL**. Every page is a real, server-rendered route with live data — no dummy links.

## ✨ Features

- Elegant, magazine-style design (custom serif/sans typography, warm color palette)
- Home page with hero, featured articles, categories, latest articles, popular posts & newsletter
- `/blog` — full article archive with category, tag and search filtering
- `/blog/[slug]` — full article page with view counter, tags, author bio and **live comments**
- `/category/[slug]` — category landing pages
- `/author/[slug]` — author profile pages with their articles
- `/search` — full-text style search across all articles
- `/about`, `/contact` (working contact form → database), `/privacy`, `/terms`
- Newsletter subscription form wired to PostgreSQL
- Comment system (post + list) wired to PostgreSQL
- Custom 404 page
- `/api/health` health-check endpoint

## 🧱 Tech stack

- Next.js 16 (App Router, Server Components)
- TypeScript
- Tailwind CSS 4
- Drizzle ORM + PostgreSQL (`pg` driver)
- Zod for API validation

## 🚀 Local development

1. Install dependencies:
   ```bash
   npm install
   ```
2. Copy `.env.example` to `.env` and set `DATABASE_URL` to your PostgreSQL connection string.
3. Push the schema to your database:
   ```bash
   npx drizzle-kit push
   ```
4. Seed the database with demo content (categories, authors, 12 full articles):
   ```bash
   npx tsx src/db/seed.ts
   ```
5. Run the dev server:
   ```bash
   npm run dev
   ```

## 📦 Deploying to GitHub + Vercel

### 1. Push to GitHub

```bash
git init
git add .
git commit -m "Initial commit: Lumière Journal premium blog"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

### 2. Create a production PostgreSQL database

Use any managed Postgres provider, for example:

- [Vercel Postgres](https://vercel.com/docs/storage/vercel-postgres) (Neon-backed, easiest to wire up)
- [Neon](https://neon.tech)
- [Supabase](https://supabase.com)

Copy the connection string it gives you (make sure it includes `?sslmode=require` if required by your provider).

### 3. Import the project into Vercel

1. Go to [vercel.com/new](https://vercel.com/new) and import your GitHub repository.
2. Framework preset: **Next.js** (auto-detected).
3. Add an environment variable:
   - `DATABASE_URL` = your production Postgres connection string
4. Click **Deploy**.

### 4. Initialize the production database schema & content

After the first deploy, run these once from your local machine (pointed at the production
database) or via the Vercel CLI:

```bash
DATABASE_URL="your-production-connection-string" npx drizzle-kit push
DATABASE_URL="your-production-connection-string" npx tsx src/db/seed.ts
```

This creates all tables and populates the blog with categories, authors and 12 full articles.

### 5. Redeploy

Trigger a redeploy from the Vercel dashboard (or push a new commit) — your premium blog is now
live with a real database behind it.

## 🗂 Project structure

```
src/
  app/                 → all routes (pages + API routes)
  components/          → shared UI components (Navbar, Footer, PostCard, forms, etc.)
  db/                   → Drizzle schema, db client, seed script
  lib/                  → query helpers & formatting utilities
```

## 🔐 Environment variables

| Variable       | Description                          |
| -------------- | ------------------------------------ |
| `DATABASE_URL` | PostgreSQL connection string         |

## 📝 Content model

- **Categories** — Technology, Travel, Lifestyle, Finance, Health
- **Authors** — 3 writers with bios and avatars
- **Posts** — 12 long-form articles with tags, read time, view counts and cover images
- **Comments** — visitor comments per article, stored in Postgres
- **Subscribers** — newsletter signups, stored in Postgres
- **Contact messages** — messages submitted via the contact form, stored in Postgres
