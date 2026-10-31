# Minhas Finanças

A personal finance app built with HTML, CSS and vanilla JavaScript, for my own use. It tracks salary, fixed expenses and CDB investments, shows how much is left each month, and syncs across my computer and iPhone as an installable app (PWA).

**Live app:** https://laurvieira.github.io/financas/

## Features

- **Summary:** monthly leftover (income minus fixed expenses), share of income already committed, fixed expenses by category and the next due dates.
- **Income:** net salary, payday and extra income sources.
- **Fixed expenses:** name, amount, category and due day, with totals.
- **CDB investments:** amount, application date, rate (% of CDI or fixed annual rate) and optional maturity. Estimates gross yield, regressive income tax and net value up to today.
- **Sync:** email and password login, with data saved in the cloud and identical on every device.
- **Offline support:** the app opens without internet and syncs again when the connection returns.
- **Installable:** works as a standalone app on desktop (Chrome, Edge, Chromium) and on iPhone (Safari).
- **Backup:** download all data as a `.json` file.
- **Dark mode:** follows the system theme automatically.

## Tech Stack

- **HTML5:** single-page structure with a web app manifest.
- **CSS3:** native custom properties for light and dark themes, with Flexbox and Grid for a responsive layout.
- **JavaScript (ES6+):** DOM rendering, financial calculations and a service worker for offline use.
- **Supabase:** authentication and a PostgreSQL table protected by Row Level Security, accessed through `supabase-js`.
- **GitHub Pages:** static hosting.

## Getting Started

1. Create a free project at [supabase.com](https://supabase.com).
2. In the SQL Editor, run:

   ```sql
   create table if not exists public.financas (
     user_id uuid primary key references auth.users(id) on delete cascade,
     data jsonb not null default '{}'::jsonb,
     updated_at timestamptz not null default now()
   );

   alter table public.financas enable row level security;

   create policy "read own data" on public.financas
     for select using (auth.uid() = user_id);
   create policy "insert own data" on public.financas
     for insert with check (auth.uid() = user_id);
   create policy "update own data" on public.financas
     for update using (auth.uid() = user_id) with check (auth.uid() = user_id);
   create policy "delete own data" on public.financas
     for delete using (auth.uid() = user_id);
   ```

3. Clone the repository and edit `config.js` with your project URL and `anon` public key (Project Settings → API Keys). Never use the `service_role` key.
4. Serve the folder over HTTPS, for example with GitHub Pages (Settings → Pages → `main` / root).
5. Open the site, create your account and install the app. After that, disable new sign-ups in Supabase (Authentication → Sign In / Providers → Email) to keep the app private.

## Author

Laura Vieira ([@LaurVieira](https://github.com/LaurVieira))
