# Deploy DocuFlow_07 now

## 1) Demo mode, no API keys

```bash
npm install
npm run dev
```

Then open `http://localhost:3000` and click **Start free**. Leave Supabase variables empty. The demo flow is fully usable in-browser: upload PDF/JPG/JPEG/PNG, process, review/edit fields, approve/reject/reprocess, and export XLSX/CSV.

## 2) Host on Vercel

From the project root:

```bash
npx vercel
```

Follow the prompts. For a production deployment:

```bash
npx vercel --prod
```

Add environment variables in the Vercel project settings. Vercel supports Next.js deployment without a custom server configuration.

## 3) Turn on production persistence/auth

Create a Supabase project and run `db/001_init.sql`. Create a private Storage bucket named `documents`. Add the environment variables from `.env.example` and set `NEXT_PUBLIC_DEMO_MODE=false`.

Set your Supabase Auth site URL and redirect URL to your deployed origin plus `/auth/callback`.

## 4) Turn on real AI processing

Set:

```text
OCR_PROVIDER=openai
LLM_PROVIDER=openai
OPENAI_API_KEY=...
OPENAI_MODEL=gpt-5
```

The code keeps OCR and extraction behind provider interfaces. The API uses server-side keys only.

## Important

The first deployment can be a demo deployment without any credentials. Production document persistence and real provider processing require the Supabase/OpenAI setup above.
