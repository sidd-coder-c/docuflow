# DocuFlow_07

India-focused AI document processing MVP: PDF/JPG/PNG invoice -> OCR -> structured invoice data -> deterministic validation -> review -> Excel/CSV export.

## Run now

```bash
npm install
npm run dev
```

Open `http://localhost:3000`. With no environment variables it runs in clearly labeled DEMO MODE. Demo data is fictional and stored in browser localStorage so the product can be evaluated immediately.

## Production mode

1. Create a Supabase project.
2. Run `db/001_init.sql` in Supabase SQL Editor.
3. Create a Storage bucket named `documents` and keep it private.
4. Configure Supabase Auth email settings and redirect URL to your deployed `/auth/callback`.
5. Copy `.env.example` to `.env.local` and set real values. Set `NEXT_PUBLIC_DEMO_MODE=false`, `OCR_PROVIDER` and `LLM_PROVIDER` to real providers.
6. Deploy to Vercel or another Next.js host.

Supabase's current Next.js SSR guidance uses `@supabase/ssr` with cookie-based sessions and `proxy.ts` for token refresh; this project follows that pattern.

## Real provider integration

- `lib/providers/ocr.ts`: OCR abstraction. Mock provider is included. Replace/add a provider implementation and set `OCR_PROVIDER`.
- `lib/providers/extraction.ts`: Structured invoice extraction abstraction. OpenAI provider uses the official SDK and a JSON schema, while mock mode is deterministic and clearly labeled.

For a production OCR provider, store the raw OCR text server-side only when necessary and apply your retention policy.

## Exports

Browser exports use SheetJS for Excel/CSV. The workbook has `Invoice Summary`, `Line Items`, and `Validation Issues` sheets.

## Deploy

### Vercel

```bash
npm i -g vercel
vercel login
vercel
vercel --prod
```

Or connect the repo in the Vercel dashboard. Set the same environment variables in Project Settings.

### Docker

A minimal Dockerfile can be added if self-hosting is preferred. Vercel is the simplest path for this App Router deployment.

## Security checklist

- Never expose `SUPABASE_SERVICE_ROLE_KEY` to the browser.
- Keep uploaded documents in a private Storage bucket.
- Use RLS policies in `db/001_init.sql`.
- Keep authenticated pages dynamic; do not cache session-bearing responses.
- Validate file type and size server-side, not only in the UI.
- Treat OCR/LLM output as untrusted input and validate against schemas.
- Do not report unsupported accuracy/compliance claims.

## Illustrative running cost

See `docs.md` for current platform references and an explicitly assumption-based AI estimate. Hosted demo mode can be run without AI provider charges.
