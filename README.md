# Inbox Copilot — AI-Powered Email Client
 
A keyboard-first email client with an AI copilot that drafts replies and answers questions about your inbox, grounded in your actual prior threads using RAG. Built as a production SaaS with subscription billing, multi-account support, and real-time sync.
 
**Live Demo** → *(add deployed URL)*
 
---
 
## What It Does
 
Most email clients are passive — they store and display messages but leave all the thinking to you. Inbox Copilot makes your email history queryable and actionable:
 
- Ask "What did I promise this client last week?" and get a sourced answer from your actual threads
- Draft replies with full context from prior conversations, not hallucinated content
- Navigate and manage your inbox entirely from the keyboard
 
---
 
## Features
 
- **AI reply drafting** — context-aware drafts grounded in relevant prior threads via RAG
- **Inbox Q&A** — natural language questions answered from your real email history
- **Multi-account support** — connect Gmail, Outlook, and other providers through a unified API
- **Real-time sync** — webhooks + incremental sync tokens keep state current without redundant reprocessing
- **Semantic search** — find emails by meaning, not just keywords; powered by vector embeddings
- **Keyboard-first UX** — command palette, shortcuts for composing/replying/navigating
- **Subscription SaaS** — free vs. pro tiers with feature gating, Stripe billing, and webhook-driven subscription management
 
---
 
## Architecture
 
```
Email Providers (Gmail / Outlook)
         ↓
   Aurinko API (OAuth, webhooks, incremental sync)
         ↓
   PostgreSQL via Prisma ──── threads, metadata, user state
         ↓
   Embedding Pipeline (OpenAI)
         ↓
   Pinecone ─────────────────── semantic vector index
         ↓
   RAG Query (LangChain + OpenAI)
         ↓
   Copilot Response (draft reply / answer question)
```
 
The core constraint: AI responses are always grounded in emails that exist in your inbox. No hallucinated context.
 
---
 
## Tech Stack
 
| Layer | Technology |
|---|---|
| Framework | Next.js 14 (App Router) |
| Language | TypeScript |
| AI / LLM | OpenAI API |
| RAG | LangChain + Pinecone |
| Email Sync | Aurinko API |
| Auth | Clerk |
| Payments | Stripe + webhooks |
| Database | PostgreSQL via Prisma ORM |
| File Storage | AWS S3 |
| Styling | Tailwind CSS |
 
---
 
## Project Structure
 
```
├── app/              # Next.js App Router — pages and API routes
├── components/       # UI components (email list, compose, copilot panel)
├── lib/              # RAG pipeline, Aurinko sync, embedding logic
├── prisma/           # Schema and migrations
└── public/           # Static assets
```
 
---
 
## Running Locally
 
**Prerequisites:** Node.js 18+, PostgreSQL, Pinecone account, OpenAI key, Aurinko account, Clerk app, Stripe account, AWS S3 bucket
 
```bash
git clone https://github.com/VinayakMaharaj/Inbox-Copilot.git
cd Inbox-Copilot
npm install
```
 
Create a `.env.local` file:
 
```env
# Database
DATABASE_URL=
 
# Clerk
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=
 
# OpenAI
OPENAI_API_KEY=
 
# Pinecone
PINECONE_API_KEY=
PINECONE_INDEX_NAME=
 
# Aurinko (email sync)
AURINKO_CLIENT_ID=
AURINKO_CLIENT_SECRET=
AURINKO_SIGNING_SECRET=
 
# Stripe
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=
 
# AWS S3
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=
AWS_BUCKET_NAME=
```
 
```bash
npx prisma migrate dev
npm run dev
```
 
Open [http://localhost:3000](http://localhost:3000).
 
---
 
## Design Decisions
 
**Why RAG over fine-tuning?**
Fine-tuning would require retraining as the inbox grows and can't reference specific threads at query time. RAG retrieves semantically relevant emails at inference time — keeping responses current and grounded without retraining.
 
**Why Aurinko over direct Gmail/Outlook APIs?**
Aurinko abstracts OAuth flows, webhook management, and incremental sync across providers. Direct integrations with each provider would have tripled the setup work for the same result.
 
**Why Pinecone over pgvector?**
For a project already using PostgreSQL for relational data, pgvector would have been simpler. Pinecone's managed infrastructure and filtering capabilities made it the cleaner choice for the vector layer specifically — worth the added dependency.
 
**Webhook-first sync over polling**
Polling on a fixed interval either over-fetches (wasting API quota) or under-fetches (stale state). Webhooks + incremental delta tokens give low-latency updates with no redundant processing.
 
---
 
## Tradeoffs & Limitations
 
- AI response quality depends on embedding quality and retrieval accuracy
- Large inboxes increase indexing and storage costs
- RAG favors correctness over creativity — drafts may be less stylistically varied than a purely generative approach
 
These tradeoffs were intentional to prioritize reliability and user trust over stylistic flair.
 
---
 
## What I Learned
 
- Designing RAG pipelines over user-generated data with real retrieval constraints
- Handling real-time sync with external APIs using webhooks and delta tokens
- Balancing AI capability against cost and latency at the SaaS layer
- Building and monetizing a full production application end-to-end as a solo developer
 
---
 
## Author
 
**Vinayak Maharaj** — [LinkedIn](https://www.linkedin.com/in/vinayak-maharaj/) · [Portfolio](https://vinayakmaharaj.netlify.app/)
