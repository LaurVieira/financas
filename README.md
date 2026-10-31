# Minhas Finanças

A personal finance app built with HTML, CSS and vanilla JavaScript, for my own use. It tracks salary, extra income, monthly expenses (fixed, variable and installments) and CDB investments, month by month, and syncs across my computer and iPhone as an installable app (PWA).

**Live app:** https://laurvieira.github.io/financas/

## Features

- **Month navigation:** browse every month with the arrows. The back arrow stops at the month the app started, and the "Hoje" button returns to the current month.
- **Summary:** monthly leftover (income minus expenses), share of income already used, paid and remaining amounts, expenses by category, what is left to pay and the CDB totals.
- **Salary history:** a salary change applies from the chosen month onward and never alters previous months. Deleting the salary follows the same rule.
- **Extra income:** the "Outras" category counts only in the month it was received, and "Por período" counts in every month between a start and an end date.
- **Expenses:** name, amount, category, payment method (debit, Pix or boleto, or card) and due date. The **Fixo** type repeats every month, **Variável** counts only in the month it was added, and **Parcelado** shows the current installment (such as 3/10) and stops after the last one.
- **Paid tracking:** mark each expense as paid, independently for every month.
- **CDB investments:** amount, date, rate (% of CDI or fixed annual rate) and optional maturity. Shows gross value, gross yield, estimated income tax (regressive table) and net value, calculated up to today or up to the last day of the chosen month.
- **Sync and offline:** email and password login, cloud storage, and offline use that syncs again when the connection returns.
- **Backup:** download all data as a `.json` file.
- **Dark mode:** follows the system theme automatically.

## Tech Stack

- **HTML5:** single-page structure with a web app manifest.
- **CSS3:** native custom properties for light and dark themes, with Flexbox and Grid for a responsive layout.
- **JavaScript (ES6+):** DOM rendering, month-based rules, financial calculations and a service worker for offline use.
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
5. Open the site, create your account and install the app: use the install icon in Chrome, Edge or Chromium on desktop, or Share → Add to Home Screen in Safari on iPhone.
6. Disable new sign-ups in Supabase (Authentication → Sign In / Providers → Email) to keep the app private.

## Author

Laura Vieira ([@LaurVieira](https://github.com/LaurVieira))
