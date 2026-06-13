# Project Name

slack-knowledge-base

---

## Attendee Details

**Name:** Drishika Mekonda
**GitHub Username:** drishika-mekonda
**LinkedIn Profile:** https://www.linkedin.com/in/drishika-mekonda-7255342a2
**GitHub Project Repository:** https://github.com/drishika-mekonda/slack-knowledge-base.git

---

## Problem Statement Selected

```txt
Intelligent Slack Knowledge Base
```

---

## Project Description

**What is the project about?**
- The Intelligent Slack Knowledge Base is an AI-powered retrieval and Q&A system that bridges company documents (such as PDF training manuals, onboarding sheets, and policy files) and Slack discussion histories into a single, unified search interface. Powered by Google's gemini-2.5-flash and gemini-embedding-2 models, it features semantic search, multi-turn chat memory, and automatic document summarization.

**Who is it for?**
- It is designed for modern corporate organizations, engineering squads, HR departments, and customer support teams who use Slack as their primary operational hub and need a secure way to access and search through shared resources.

**What problem does it solve?**
* It solves two major enterprise problems:
- Information Silos: Critical context is often lost inside dense, multi-page PDFs or scattered across thousands of nested Slack message threads.
- Data Leakage & Privacy: Generic AI models do not respect user boundaries. Our system implements strict role-based access control, isolating files into Personal (only you), Team (only your department), and Organization (global) security scopes.

**How does it help the user?**
- Instant Answers: Users can ask natural language questions and receive accurate answers grounded strictly in company documentation.
- Truthful Groundedness: Prevents AI hallucinations by providing clickable citation cards showing the raw source text segment.
- UX Fluency: Features a premium glassmorphic web dashboard that supports dynamic Light and Dark mode transitions, typing indicators, and markdown tags.
- Workplace Integration: Ingests Slack channel transcripts directly (complete with self-healing error guide prompts if the bot is missing from a channel) to capitalize on existing team knowledge.

---

## Approach

**How I understood the problem**
- We identified that corporate knowledge is split into two forms: structured static manuals (PDFs, templates, wikis) and unstructured dynamic conversations (Slack channels, threaded debugging sessions, discussions). Traditional search tools fail because they only index keywords, missing semantic intent. Meanwhile, standard AI solutions fail because they lack access control (a junior dev shouldn't retrieve senior management HR files) and suffer from hallucinations (making up policies).
- Our goal was to build a system that securely connects both sources, enforces strict authorization boundaries, and guarantees grounded, verifiable answers.

**What user flow I designed**
1. Onboarding: The user registers with a username, email, password, and a self-selected Team Name (e.g. Engineering, HR).
2. Ingestion:
- PDF Ingestion: The user uploads a file, selects its access scope (Personal, Team, Org), and the backend automatically chunks, tags, and vectorizes it.
- Slack Ingestion: The user enters a Slack channel ID or thread timestamp. The backend pulls the history, resolves usernames, and indexes the chronological transcript.
3. Knowledge Retrieval (Chat/Summary):
- The user asks a question in the chat and selects a scope.
- The system pulls relevant context chunks (filtering out unauthorized documents).
- The AI answers the question, outputting footnote references ([1], [2]).
- The user clicks on the footnote or citation cards to inspect the original text chunks.

**What features I decided to build**
- Multi-Scope Access Isolation: Personal, Team, and Organization scopes enforced via database and vector-store queries.
- Slack Transcript Ingestor: Captures message history and parent-child thread replies, compiling them into clean, tagged files.
- Traceable Citations & Footnotes: Expandable citation cards with file details and raw text snippet previews.
- Markdown Executive Summarizer: Automatic outline generator for long-form documents.
- Interactive UI: Glassmorphic dashboard styled with TailwindCSS v4, supporting real-time typing indicators and seamless Light/Dark theme switching.

**How AI is used in our solution**
We utilized the new google-genai SDK and mapped tasks to the optimal Gemini models:
- Vector Embeddings (gemini-embedding-2): Converts text chunks into high-dimensional vectors (3072 dimensions) to capture semantic meanings rather than simple keyword matches.
- Metadata Auto-Tagging (gemini-2.5-flash): Dynamically reads content previews during upload and generates descriptive category tags.
- Grounded Answer Generation (gemini-2.5-flash): Composes natural language answers. We use strict system prompts to constrain response logic to context blocks and generate citations.
- Document Summarization (gemini-2.5-flash): Synthesizes aggregated chunks into key takeaways.

**What makes our approach useful or different**
- Strict Security-First Scoping: Unlike general chat assistants, our RAG pipeline enforces scope boundaries at the vector search query level. Chunks are filtered before reaching the LLM context window using metadata tags matched to the user's JWT credentials.
- Context Preservation: Our sentence-boundary chunker ensures paragraphs are split cleanly, preserving readability.
- Slack Ingestion Self-Healing: Instead of crashing on Slack workspace permission errors (e.g., bot not invited), the UI catches specific exceptions and displays custom troubleshooting instructions.
- Dual Theme Design: Features a highly responsive frontend layout optimized for both light and dark display preferences.

---

## Tech Stack and Tools Used

**Frontend:** React 19, TypeScript, Vite 6, TailwindCSS v4, Axios, Lucide React
**Backend:** FastAPI (Python 3.12 / 3.13), SQLAlchemy 2.0 (ORM), Uvicorn 
**Database:**  SQLite (Relational DB for users, sessions, metadata), ChromaDB (Vector Database for embeddings)
**AI Tools/API:** Google Gemini API (gemini-2.5-flash for generation, categorization & summarization; gemini-embedding-2 for 3072-dimensional vector embeddings), official google-genai Python SDK
**Cloud/Deployment:** Docker, Docker Compose, Nginx (reverse-proxy and frontend static server)
**Other Tools:** Git/GitHub, pdfplumber (PDF text extraction), Slack SDK / Bolt API, direct bcrypt password hashing

---

## Key Features

1. Role & Team Access Isolation: Enforces Personal, Team, and Organization security scopes at the database and vector-query levels using JWT-derived metadata filters.
2. Channel & Nested Thread Slack Ingestion: Fetches public/private Slack histories and nested thread replies, converting them into chronological, username-resolved chat transcripts.
3. Conversational Chat with Grounded Citations: Multi-turn dialog memory with inline reference footnotes linked to expandable citation cards showing raw chunk previews.
4. AI Summary & Category Tagging: Automatically outlines documents in markdown format and auto-tags ingested sources during upload using Gemini.
5. Aesthetic Light & Dark Themes: Fully responsive glassmorphic layout that transitions smoothly between modes.

---

## What is Working?

- Secure user registration, login, and profile fetching using JWT validation.
- PDF document saving, text extraction (via pdfplumber), chunking, and ChromaDB indexing.
- Slack history and thread fetching, transforming usernames to real names, and vectorization.
- Semantic search querying with JWT-derived scope filters (Personal, Team, Org).
- Grounded answer generation using Gemini 2.5 Flash, complete with inline footnote citation references and expandable source drawers.
- Markdown-formatted document executive summarization.
- Dynamic theme toggle (Light / Dark mode) with unified transitions on all cards, forms, inputs, and selections.
- Microservices architecture containerized and running under Docker Compose.

---

## What is Still in Progress?

We are in the process of implementing Slack Slack Commands (e.g. /ask and /ingest) directly into the Slack workspace interface so users can query the database directly within Slack channels.

---

## Screenshots or Demo

Add screenshots, demo video link, or deployed project link if available.

**Deployed Link:**
**Demo Video Link:**
**Screenshots:** 
<img width="1907" height="922" alt="image" src="https://github.com/user-attachments/assets/440fdfa7-d54d-4677-911c-2de70efa648d" />
<img width="1913" height="905" alt="image" src="https://github.com/user-attachments/assets/938d4ca8-1eae-4967-9e5f-6c71069013b9" />
<img width="1908" height="900" alt="image" src="https://github.com/user-attachments/assets/0853a1fb-ef94-4939-ac71-8ce09ce50746" />
<img width="1912" height="912" alt="image" src="https://github.com/user-attachments/assets/5b8ac84c-3dac-4894-80fd-4b4451b14a54" />
<img width="1918" height="967" alt="image" src="https://github.com/user-attachments/assets/2d67e966-f59f-4731-9a34-46514fbe091e" />



---

## Challenges Faced

1. Python 3.13 Compatibility: The passlib bcrypt context failed on Python 3.13. Resolved by replacing the library calls with direct, native bcrypt password hashing and verification functions.
2. Gemini SDK List Embeddings: In the new google-genai SDK, batch list inputs can sometimes trigger list index collapses. Resolved by serializing text chunk list inputs into individual queries.
3. Slack Workspace Permissions: App bolt pipelines crash when the bot lacks channel membership scopes. Resolved by adding a specific catch block for the not_in_channel exception that outputs guide instructions to the user.

---

## Learnings

1. Configured custom TailwindCSS v4 variables under the @theme directive, binding them to root properties for smooth Light/Dark transitions.
2. Formulated strict LLM system prompts to prevent context hallucinations and enforce footnote output.
3. Structured unified SQLite metadata queries alongside ChromaDB filters to maintain access control parameters.

---

## Future Improvements

Add BM25 lexical search for hybrid keyword-semantic matching.
Support additional text formats like DOCX, XLSX, and CSV.
Integrate Microsoft Teams and Discord channel history importers.

---

## Final Note

This project is built from scratch with a strong focus on enterprise security, data privacy, and a premium user interface. It demonstrates a practical application of secure RAG (Retrieval-Augmented Generation) in corporate communication environments.
