# HabitPulse setup

HabitPulse works as a local-first tracker without an account. Its static copy has no Supabase project, email provider, deployment origin, or administrator account configured, so cloud registration and email delivery stay disabled until the owner completes the steps below. Never put a Supabase service-role key or email-provider key in browser code.

## Run and install

Serve this folder over HTTPS (or `localhost`) to enable service workers, install prompts, and browser camera/security capabilities. Opening `habitpulse.html` with `file://` still runs local tracking, but cannot install or use the service worker. `habitpulse-sw.js` only caches the HabitPulse shell and does not import the existing third-party worker.

## Supabase and RLS

1. Create a Supabase project and run [`supabase/schema.sql`](supabase/schema.sql) in its SQL editor.
2. Enable email confirmation and configure an SMTP provider in Supabase Auth. Keep the built-in auth rate limits enabled.
3. Put the project URL and publishable/anon key into `habitpulse-config.js`. These public values are not secrets. The app and `/admin/` then load the Supabase JS client; sign-up checks usernames using the database RPC. The unique database constraint remains the final authority if two sign-ups race.
4. Deploy `admin-set-tier` with the platform JWT verification enabled. Configure its `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`, and exact HTTPS `HABITPULSE_ALLOWED_ORIGIN` server-side. Only that edge function uses the service-role key; it verifies the caller's JWT and `is_admin` row before changing a tier. Promote the first administrator from the SQL editor after that person has registered:

   ```sql
   update public.user_profiles
   set is_admin = true
   where user_id = (select id from auth.users where email = 'OWNER_EMAIL');
   ```

5. Open `/admin/` on the hosted site. The page is not a security boundary by itself: the server-side admin check, column grants, and RLS policies are. Do not grant `is_admin` or change tiers from a browser query.

The current frontend keeps habits, logs, goals, reflections, weekly notes and preferences local and does not sync them to Supabase yet. The schema now has owner-scoped goals, routines, stacks, habit logs, focus minutes, mood, pause/archive ranges, profile preferences and weekly reviews with RLS. These tables are a protected backend foundation, not a claim that cloud sync is already active. Do not connect sync until conflict resolution, duplicate protection and account deletion have been implemented and tested.

## Email

Deploy `habit-email` with JWT verification disabled only as configured in `supabase/config.toml`; the handler authenticates webhooks with a long random `HABITPULSE_EMAIL_HOOK_SECRET` and unsubscribe URLs with a separate random `HABITPULSE_UNSUBSCRIBE_SECRET`. Also set `SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY`, `RESEND_API_KEY`, `HABITPULSE_EMAIL_FROM`, and `HABITPULSE_PUBLIC_URL` (the production HTTPS app URL) as Edge Function secrets. Configure an Auth user-created webhook to POST the auth record to `habit-email` with JSON `{ "type": "welcome", "record": { "id": "...", "email": "..." } }` and the hook secret in `x-habitpulse-hook`. Configure a daily Supabase Cron invocation with `{ "type": "daily" }` and that same header. The daily function selects only profiles that opted in, caps a run at 250 recipients, records the send date, and includes a signed, expiring one-click unsubscribe link. Test with a private test account before enabling the schedule. The transactional welcome message and optional motivational check-ins are not sent until this is deployed.

## Contact and store readiness

Support links are wired to `Amezougkhalil31@gmail.com` and WhatsApp `+212660725091`; they open only after a user selects a route. Before publishing, replace the placeholder privacy/terms contact details if ownership changes, add store-specific screenshots and disclosures, test install/upgrade behavior on real iOS and Android devices, and review the applicable store policies. This project has not been submitted to either store.
