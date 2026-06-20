# TAKAMATSU FLOW

Static landing page for GitHub Pages with a Supabase-backed bulletin board.

## What is already built

- The landing page is in `index.html`.
- The landing page has the bulletin board login/signup entry in the `#board` section.
- The bulletin board itself is in `board.html`.
- Posts are read from and inserted into the Supabase table `public.board_posts` after Supabase Auth login.
- New Auth signups are recorded in the Supabase table `public.auth_profiles`.
- The registration page is in `register.html`.
- Registration requests are inserted into the Supabase table `public.registration_requests`.
- The database setup SQL is in `supabase-schema.sql`.

## Supabase setup from account creation

I cannot create the Supabase account for you because it requires your email, login, and verification. Follow these steps once, then paste the public project values into `index.html`.

1. Open https://supabase.com/dashboard and sign up or log in.
2. Create a new organization if Supabase asks for one.
3. Create a new project.
   - Project name: `takamatsu-flow` or any name you prefer.
   - Database password: save it somewhere private.
   - Region: choose the closest region to your visitors.
4. Wait until the project finishes provisioning.
5. Open the project, then open SQL Editor.
6. Create a new query and paste the full contents of `supabase-schema.sql`.
7. Run the query.
8. Open Project Settings > API.
9. Copy these two values:
   - Project URL
   - anon public key
10. Replace these values in `index.html`:

```js
const SUPABASE_URL = "YOUR_SUPABASE_URL";
const SUPABASE_ANON_KEY = "YOUR_SUPABASE_ANON_KEY";
```

The anon key is public by design. Do not paste the service role key into frontend code.

## Database behavior

The bulletin board table uses Row Level Security policies that allow authenticated users to read posts and create posts only. Anonymous visitors cannot read or create board posts.

The registration table allows anonymous visitors to create registration requests only. Public read is not granted, so submitted email addresses are not readable through the anon key.

## Supabase Auth setup

The board uses Supabase Auth with email and password.

1. Open Supabase > Authentication > Providers.
2. Make sure Email is enabled.
3. Decide whether email confirmation is required.
   - If confirmation is enabled, new users must open the confirmation email before login.
   - If confirmation is disabled, users can log in immediately after signup.
4. Run `supabase-schema.sql` again in SQL Editor to apply the authenticated-only board policies.

The landing page switches between login and signup forms. Login requires email and password. Signup requires email, password, and password confirmation. Successful login redirects to `board.html`.

After running `supabase-schema.sql`, new signups will appear in `Table Editor > auth_profiles`. Passwords are never stored in this table.

If email confirmation is enabled, add this redirect URL in Supabase Authentication URL settings:

```text
https://umakoshi1120.github.io/traffic/board.html
```

For immediate login tests, disable email confirmation or open the confirmation email before logging in.

## Local check

After setting the URL and anon key, open `index.html` in a browser and try a test post. If the board says the Supabase table or RLS settings cannot be found, run `supabase-schema.sql` again in the same Supabase project.

After adding the registration page, run `supabase-schema.sql` again in Supabase SQL Editor, then open `register.html` and submit a test registration.
