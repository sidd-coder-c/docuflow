# Deployment notes

## Immediate demo deployment

The project works without a database or secret by running with `NEXT_PUBLIC_DEMO_MODE=true` (or simply by leaving Supabase variables unset). It stores demo workspace data in browser localStorage, validates invoices in the browser, and creates real Excel/CSV files locally.

## Production deployment

Use a Supabase project for Auth, Postgres, RLS and private Storage, and Vercel for the Next.js app. Supabase's current Next.js guidance uses `@supabase/ssr` and cookie-based sessions; the project uses those utilities and a `proxy.ts` session guard. Configure `/auth/callback` as an allowed redirect URL in Supabase.

Run `db/001_init.sql`, create a private `documents` bucket, and set the environment variables in `.env.example`.

Production processing route:
`POST /api/documents/:id/process`

Supported initial formats: PDF/JPG/JPEG/PNG; server validation is 10 MB/file and 20 files/batch.

## Cost planning (illustrative)

Actual cost depends heavily on invoice page count, image dimensions, provider/model and storage/egress. Current platform list prices are published by Vercel, Supabase and OpenAI and can change.

- Vercel Hobby: $0/month for personal/non-commercial projects; Vercel Pro: $20/month with $20 usage credit. See current pricing: https://vercel.com/pricing
- Supabase Free: $0/month with limited resources; Pro starts at $25/month and includes $10/month compute credits. See current pricing: https://supabase.com/pricing
- For a rough GPT-5 text-only extraction assumption of 2,000 input tokens + 500 output tokens per invoice, the model portion is about $0.75 / 100 invoices, $7.50 / 1,000 and $75 / 10,000 at the published $1.25/M input and $10/M output rates. This excludes OCR/vision token overhead, retries, storage, egress and hosting.

For a more realistic production budget, measure 100 real invoices first and use actual token/page usage rather than treating the above as a quote.
