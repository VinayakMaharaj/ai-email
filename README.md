# Inbox Copilot

Inbox Copilot is a **keyboard-first, AI-powered email client** designed to help users manage high-volume inboxes more efficiently through **real-time synchronization, semantic search, and AI-assisted drafting**.

The system combines a modern full-stack architecture with Retrieval-Augmented Generation (RAG) to ground AI responses in a user’s actual email history, enabling faster replies, better context retention, and reduced inbox friction.

---

## 🚀 Core Capabilities

### Multi-Account Email Client
- Supports multiple email providers (e.g. Gmail, Outlook) through a unified email API.
- Full support for reading, composing, replying, and threading conversations.
- Maintains local mailbox state synced with external providers.

### Real-Time Inbox Synchronization
- Uses **webhooks + incremental sync tokens** to keep inboxes up to date.
- Avoids redundant data fetching while ensuring low-latency updates.
- Designed to scale with inbox size and message volume.

### AI Email Copilot (RAG-Based)
- Drafts context-aware email replies grounded in relevant past conversations.
- Enables natural-language questions over the inbox (e.g. “What did I promise this client last week?”).
- Uses Retrieval-Augmented Generation to ensure responses are based on real emails rather than hallucinated context.

### Semantic & Full-Text Search
- Emails are embedded and indexed to support fast semantic search.
- Queries return the most relevant messages and threads based on meaning, not just keywords.
- Supports large inboxes without degrading query quality.

### Keyboard-First User Experience
- Command palette for navigation and actions.
- Keyboard shortcuts for composing, replying, and inbox management.
- Layout inspired by high-productivity email tools.

### Subscription & Usage Management
- Free and paid tiers with feature gating.
- Subscription billing handled via Stripe.
- Access to AI features controlled by subscription status and usage limits.

---

## 🧠 High-Level Architecture

1. **Authentication & User State**
   - Users authenticate via OAuth.
   - User accounts are synced and persisted in a relational database.

2. **Email Ingestion & Sync**
   - Email providers are accessed through a unified email API.
   - Initial inbox sync fetches messages and threads.
   - Webhooks trigger incremental updates using delta tokens.

3. **Search & Retrieval**
   - Email content is normalized and embedded.
   - Embeddings are stored in a vector-enabled search layer.
   - Semantic similarity search retrieves relevant emails for queries.

4. **Retrieval-Augmented Generation**
   - Retrieved emails are injected as context into AI prompts.
   - The language model generates grounded replies or answers.
   - Responses are constrained to inbox-derived context.

5. **Billing & Feature Gating**
   - Subscription status is tracked via Stripe webhooks.
   - Feature access (AI drafting, semantic search limits) is gated per tier.

---

## 🛠 Tech Stack

- **Frontend / Backend**: Next.js, TypeScript  
- **AI & Search**: OpenAI API, Retrieval-Augmented Generation (RAG), vector embeddings  
- **Data Layer**: PostgreSQL, Prisma ORM  
- **Search Engine**: Vector + full-text search indexing  
- **Authentication**: Clerk (OAuth + session management)  
- **Payments**: Stripe (subscriptions, webhooks)  
- **Deployment**: Serverless infrastructure

---

## 🎯 Design Goals

- **Grounded AI Responses**  
  Ensure AI outputs are always tied to real inbox data.

- **Low-Latency Sync**  
  Keep inbox state current without unnecessary reprocessing.

- **Productivity-First UX**  
  Optimize for keyboard navigation and minimal context switching.

- **Scalable SaaS Architecture**  
  Support growing inboxes, multiple accounts, and monetization.

---

## ⚠️ Tradeoffs & Limitations

- AI response quality depends on embedding quality and retrieval accuracy.
- Large inboxes increase indexing and storage costs.
- RAG prioritizes correctness over creativity, which may reduce stylistic variation in drafts.

These tradeoffs were intentional to favor reliability and user trust.

---

## 📈 What I Learned

- Designing **RAG pipelines over user-generated data** at scale.
- Handling real-time synchronization with external APIs using webhooks.
- Balancing AI capability with cost and latency constraints.
- Building and monetizing a production-style SaaS application end-to-end.

---

## 📄 License

This project is intended for educational and demonstration purposes.

