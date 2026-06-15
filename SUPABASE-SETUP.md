# Supabase setup — accounts & synced data

The app now uses **Supabase** for sign-in and to store each user's transactions and
budgets. Data is isolated per user by **Row-Level Security (RLS)**, so users can only
ever read or write their own rows. Do this once.

## 1. Create a Supabase project
1. Go to [supabase.com](https://supabase.com) → sign in → **New project**.
2. Pick a name, a strong database password, and a region. Wait ~1 minute for it to provision.

## 2. Create the tables + security policies
In the project, open **SQL Editor → New query**, paste the block below, and click **Run**.

```sql
-- Transactions
create table public.transactions (
  id          uuid primary key default gen_random_uuid(),
  user_id     uuid not null default auth.uid() references auth.users(id) on delete cascade,
  type        text not null check (type in ('income','expense')),
  amount      numeric(12,2) not null check (amount >= 0),
  category    text not null,
  description text default '',
  date        date not null default current_date,
  created_at  timestamptz not null default now()
);

-- Budgets (one limit per category per user)
create table public.budgets (
  id         uuid primary key default gen_random_uuid(),
  user_id    uuid not null default auth.uid() references auth.users(id) on delete cascade,
  category   text not null,
  amount     numeric(12,2) not null check (amount >= 0),
  created_at timestamptz not null default now(),
  unique (user_id, category)
);

create index transactions_user_date_idx on public.transactions (user_id, date);

-- Lock everything down, then allow each user to access ONLY their own rows
alter table public.transactions enable row level security;
alter table public.budgets enable row level security;

create policy "own transactions" on public.transactions
  for all using (auth.uid() = user_id) with check (auth.uid() = user_id);

create policy "own budgets" on public.budgets
  for all using (auth.uid() = user_id) with check (auth.uid() = user_id);
```

## 3. Connect the app
1. In Supabase: **Project Settings → API**. Copy the **Project URL** and the **anon / public** key.
2. Open `config.js` in this project and paste them in:
   ```js
   window.SUPABASE_URL = "https://abcdxyz.supabase.co";
   window.SUPABASE_ANON_KEY = "eyJhbGciOi...";   // the anon/public key
   ```
   > These two values are safe to commit and expose publicly — RLS (step 2) is what
   > actually protects the data, not secrecy of the anon key. Never put the
   > **service_role** key in front-end code.

## 4. Email confirmation (choose one)
By default Supabase emails a confirmation link before a new account can sign in.
- **Keep it on** (recommended for production): after signing up, users click the email
  link, then sign in. The app shows "Check your email to confirm."
- **Turn it off** (faster for testing): **Authentication → Providers → Email** →
  disable *Confirm email*. New accounts can then sign in immediately.

Optional: under **Authentication → URL Configuration**, set your Vercel URL as the
Site URL so confirmation links point at your live site.

## 5. Deploy
Commit and push — Vercel redeploys automatically:
```powershell
git add .
git commit -m "Add Supabase auth and synced data"
git push
```

## How it works
- `signin.html` — sign up / sign in (Supabase Auth, email + password).
- `index.html` — checks for a session on load; with none it redirects to `signin.html`.
  All transactions/budgets are read from and written to Supabase, scoped to the
  signed-in user. The **Sign out** button is in the header.
- No data is stored in the browser anymore, so your tracker now follows you across
  devices once you sign in.
