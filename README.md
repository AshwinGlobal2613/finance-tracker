# FinTrack — Personal Finance Tracker

A single-file, offline personal finance app. No install, no server, no accounts.

## Run it locally
Double-click **`index.html`** to open it in any browser. That's it.

## Deploy (GitHub + Vercel)
This is a static site — no build step. Vercel serves `index.html` as-is.
1. Push this folder to a GitHub repo.
2. On [vercel.com](https://vercel.com) → **Add New → Project** → import the repo.
3. Framework Preset: **Other**. Leave Build Command and Output Directory **empty**.
4. **Deploy** → you get a live `https://<project>.vercel.app` URL.

Every push to the default branch auto-redeploys.

## Features
- **Income & expense tracking** — add transactions with amount, category, date, and description
- **Live summary cards** — income, expenses, net balance, and savings rate for the selected month
- **Month switcher** — review any month with the picker in the header
- **Search & filter** — find transactions by text, category, or type
- **Spending donut chart** — visual breakdown of expenses by category
- **Monthly budgets** — set per-category limits with progress bars that warn as you approach/exceed them
- **Export / Import** — back up or restore all data as a JSON file

## Accounts & data (Supabase)
The app uses **Supabase** for email/password sign-in and stores each user's
transactions and budgets in a Postgres database, isolated per user by Row-Level
Security. Sign-in lives in `signin.html`; the tracker redirects there until you're
authenticated. Once signed in, your data syncs across any device.

**First-time setup:** follow [`SUPABASE-SETUP.md`](SUPABASE-SETUP.md) — create a free
Supabase project, run the provided SQL, and paste your Project URL + anon key into
`config.js`. Both values are safe to commit (RLS protects the data, not key secrecy).

Use **Export** any time for a portable JSON backup; **Import** loads a backup into
your account.

## Categories
Expenses: Groceries, Dining, Transport, Housing, Utilities, Health, Shopping, Entertainment, Education, Travel, Other.
Income: Salary, Freelance, Investments, Gift, Other.
