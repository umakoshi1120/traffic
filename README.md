# TAKAMATSU FLOW

Static landing page for GitHub Pages with a Supabase-backed bulletin board.

## What is already built

- The landing page is in `index.html`.
- The bulletin board UI is embedded in the `#board` section.
- Posts are read from and inserted into the Supabase table `public.board_posts`.
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

The table uses Row Level Security policies that allow anonymous visitors to read posts and create posts only. Update and delete are not granted to public users.

The registration table allows anonymous visitors to create registration requests only. Public read is not granted, so submitted email addresses are not readable through the anon key.

## Local check

After setting the URL and anon key, open `index.html` in a browser and try a test post. If the board says the Supabase table or RLS settings cannot be found, run `supabase-schema.sql` again in the same Supabase project.

After adding the registration page, run `supabase-schema.sql` again in Supabase SQL Editor, then open `register.html` and submit a test registration.
