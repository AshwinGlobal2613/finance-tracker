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

## Data & privacy
Everything is stored locally in your browser via `localStorage` — nothing leaves your machine. The app ships with a handful of sample transactions for the current month; delete them and add your own. Use **Export** regularly if you want a portable backup.

## Categories
Expenses: Groceries, Dining, Transport, Housing, Utilities, Health, Shopping, Entertainment, Education, Travel, Other.
Income: Salary, Freelance, Investments, Gift, Other.
