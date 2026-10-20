# Reseller Point BD — starter project

A Next.js web app that can be packaged as an Android app later, using Supabase Auth + Postgres as a shared backend. Includes user registration/login, product listing, orders, and a role-protected admin dashboard.

## Included
- `/` storefront
- `/register` registration with one-time ৳300 fee claim + payment method and transaction ID
- `/login`
- `/products`
- `/orders`
- `/admin` owner-only dashboard (server-side role check)
- `supabase/schema.sql` tables and RLS policies
- Responsive layout for mobile and desktop

## Before production
This is a starter, not yet a production payment integration. It does **not** collect money automatically. Users submit a transaction ID and the owner checks it in the actual bKash/Nagad merchant account before approving. Do not publish personal payment numbers in source code. For automatic payments, integrate an official payment gateway and verify payment callbacks server-side.

## Setup
1. Install Node.js 20+ and unzip this project.
2. Create a Supabase project at https://supabase.com.
3. In Supabase SQL Editor, run all of `supabase/schema.sql`.
4. In Supabase Project Settings → API, copy the Project URL and anon/public key.
5. Copy `.env.example` to `.env.local` and fill those values.
6. In Supabase Authentication settings, configure your site URL and redirect URLs for local and deployed domains. For simpler initial testing, disable email confirmation only if you understand the security trade-off; otherwise use email confirmation.
7. Install and run:
   ```bash
   npm install
   npm run dev
   ```
   Open http://localhost:3000.

## Create the owner admin
1. Register with your own email in the app and confirm the email if Supabase requires it.
2. In Supabase SQL Editor run (replace the email):
   ```sql
   update public.profiles set role='admin', payment_status='approved'
   where email='YOUR_OWNER_EMAIL';
   ```
3. Visit `/admin`. Only a user whose database profile role is `admin` can enter.
4. Keep your owner email/password private; enable MFA in Supabase if available. Never make an admin signup button or accept role from a registration form.

## Deploy the web app
1. Push this folder to a private GitHub repository.
2. Import it into Vercel (recommended for Next.js server features).
3. Add `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` in Vercel project environment variables.
4. Deploy, then add the deployed domain to Supabase Auth URL configuration.
5. Update `NEXT_PUBLIC_SITE_URL` if you later add it to the app.

Netlify may support Next.js with its adapter, but Vercel is the easiest deployment path for this starter.

## Android app
The web project is mobile-responsive. To create an installable Android wrapper after web deployment:
1. Deploy the web app to HTTPS.
2. Install Android Studio and Capacitor CLI in a separate packaging step, then configure the deployed HTTPS URL as the app's start URL (or bundle the web build). Do not put any server secrets in the Android app.
3. Test login/session persistence and back navigation on physical Android devices before release.
4. For a store-quality native app, build native navigation and review Google Play policies.

## Known next steps
- Add product image upload (Supabase Storage)
- Add order address/shipping fields
- Add password reset and email verification screens
- Add audit log and rate limiting
- Replace manual payment review with official gateway integration if needed
- Add tests and error monitoring


## Logo added
Your uploaded Reseller Point Bd image is included as `public/icon.png` and `public/logo.png`.

## Important before publishing
- This is a deployable starter project, not a website that is already online.
- Set up Supabase and run `supabase/schema.sql` in the Supabase SQL Editor.
- Add the Supabase URL and anon key to the deployment environment variables.
- Register your owner account, then promote only that account to `admin` in the Supabase SQL Editor as described in the setup instructions.
- Registration payment is a manual bKash/Nagad payment review flow; it does not automatically verify payments with bKash/Nagad.
- Never put a Supabase service-role key in browser code or share secret keys in chat.
- Publish through Vercel only after the setup steps are complete and tested.
