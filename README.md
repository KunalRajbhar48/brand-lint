<p align="center">
  <img src="public/brandlint-mark.png" alt="BrandLint Logo" width="220" />
</p>

<h1 align="center">BrandLint</h1>

<p align="center">
  <strong>The pre-publish compiler and ESLint for brand content — with citations, evidence, and human sign-off.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Next.js-14.2-black?style=flat-square&logo=next.js" alt="Next.js" />
  <img src="https://img.shields.io/badge/TypeScript-5.0-blue?style=flat-square&logo=typescript" alt="TypeScript" />
  <img src="https://img.shields.io/badge/LLM-Groq%20Llama%203.3%2070B-orange?style=flat-square" alt="Groq" />
  <img src="https://img.shields.io/badge/Database-MongoDB%20Atlas-green?style=flat-square&logo=mongodb" alt="MongoDB" />
  <img src="https://img.shields.io/badge/Design-Swiss%20Editorial-blueviolet?style=flat-square" alt="Design" />
  <img src="https://img.shields.io/badge/License-MIT-gray?style=flat-square" alt="License" />
</p>

---

## 📌 Executive Summary

**BrandLint** turns unstructured brand guideline documents (PDF/Text) into a machine-readable, cited rulebook and cross-checks campaign submissions (marketing copy, poster graphics, and video) against those rules and a verified **Fact Vault**.

> **Core Philosophy:** *AI detects and explains. Humans decide and approve. Zero hallucinations, verbatim evidence grounding, and deterministic enforcement.*

---

## 🚀 Key Features

### 1. Guideline Ingestion & Rule Extraction (Admin)
- **PDF & Document Parsing**: Native server-side PDF stream parser converts multi-page PDFs, Markdown, and text into clean structured data.
- **Atomic Rule Extraction**: Powered by **Groq (Llama 3.3 70B Versatile)** with strict JSON schema constraints into 7 atomic rule types:
  - `banned_term` (superlatives, unverified claims, competitors)
  - `required_disclaimer` (statutory legal notices)
  - `tone` (urgency hype, aggressive language, exclamation spam)
  - `messaging` (target audience and voice alignment)
  - `claim_policy` (unsupported commitments and benchmarks)
  - `visual_colour` (brand palette and forbidden swatches)
  - `visual_contrast` (WCAG AA accessibility bounds)
- **Verbatim Evidence Citations**: Every single extracted rule includes exact substring source citations and confidence scores (0–100%). Rules with `< 0.6` confidence are visually flagged as **"Needs confirmation"**.
- **Side-by-Side Version Review**: Compares original raw guidelines against AI-enhanced structured guidelines, allowing admins to choose which version governs enforcement.

### 2. Fact Vault (Approved Claims Database)
- **Single Source of Truth**: Centralized registry for verified product claims, warranty terms, certifications, source documents, and external URLs.
- **Automated Ingestion**: Automatically extracts approved claims and source documents directly during guideline processing.
- **Real-Time Expiry Highlighting**: Instant visual alerts for expired marketing claims to prevent legal exposure.

### 3. Sequential Pre-Flight Check Engine (Creator)
- **Deterministic Pipeline (Zero LLM, 100% Reliable)**:
  - **Regex Banned-Term Matching**: Instant detection of prohibited superlatives.
  - **Statutory Disclaimer Validation**: Ensures mandatory legal wording is intact.
  - **Color Palette Compliance ($\Delta E$)**: CIEDE2000 color distance calculations against approved & forbidden brand swatches.
  - **WCAG AA Contrast Ratios**: Relative luminance analysis guaranteeing $\ge 4.5:1$ contrast.
- **Semantic AI Pipeline**:
  - **Tone & Voice Analysis**: Detects emotional manipulation, urgency pressure, and brand voice misalignment.
  - **Fact Vault Cross-Checking**: Extracts marketing claims and identifies direct contradictions (e.g., claiming a *"24-month warranty"* when Fact Vault record `FV-101` caps warranty at *"12 months"*).
- **Multimodal Poster OCR**: Extracts and lints on-screen copy embedded in poster images.

### 4. Video Compliance & Keyframe Engine
- **Pre-Flight Video Ingestion**: Audits campaign video assets (`.mp4`, `.mov`, `.webm`).
- **Keyframe OCR & Contrast**: Samples critical video frames for on-screen text legibility, disclaimers, and off-palette colors.
- **Timestamped Video Player**: Embedded interactive video player with a scrubbable timeline mapped to compliance flags.

### 5. Cited Issue Report ("The Wow Screen")
- **Brand Risk Score ($0\text{--}100$)**: Dynamically weighted risk index computed from issue severity (`critical`, `warning`, `info`) and AI confidence.
- **Interactive Highlighted Spans**: Color-coded inline text markers linked directly to issue cards.
- **Expandable "Why this flag?" Evidence Panel**: Verbatim citation quote, source document reference, and reasoning for **100% of issues**.
- **1-Click Fix Suggestions & Visual Diff**: Proposes compliant copy fixes with side-by-side / inline diff previews. Accepting the fix live-updates the copy and immediately recalculates the Brand Risk Score.

### 6. Reviewer Workflow & Multi-Tenant RBAC
- **Authoritative Review Queue**: Dedicated workspace for brand managers and legal counsel to review submissions, inspect evidence chains, and leave revision notes.
- **One-Click Decisions**: Fast **Approve** or **Request Changes** actions.
- **Multi-Tenant Workspaces**: Organization-level isolation for guidelines, rules, fact vaults, and submissions, supported by invite tokens and RBAC (`admin`, `creator`, `reviewer`).
- **Audit Event Trail**: Immutable timeline recording every action (`uploaded` $\rightarrow$ `extracted` $\rightarrow$ `submitted` $\rightarrow$ `analyzed` $\rightarrow$ `fix_applied` $\rightarrow$ `reviewed`).

---

## 🏗️ Architecture & Data Flow

```
[ Brand Guidelines ] ──► [ Server PDF Parser ] ──► [ Groq Llama 3.3 Engine ]
                                                          │
   ┌──────────────────────────────────────────────────────┴──────────────────┐
   ▼                                                                         ▼
[ Confirmed Rulebook ]                                              [ Fact Vault Database ]
   │                                                                         │
   └──────────────────────────┬──────────────────────────────────────────────┘
                              │
[ Creator Submission ]        ▼
(Copy / Poster / Video) ──► [ Sequential Check Engine ]
                              ├─► Deterministic Checks: Regex, Disclaimers, ΔE Colors, WCAG AA
                              └─► Semantic Checks: Tone Analysis, Fact Vault Contradictions
                              │
                              ▼
                        [ Cited Issue Report ] ──► (Brand Risk Score 0-100)
                              │
                        [ 1-Click Fix Diff ] ──► Live Re-calculation
                              │
                              ▼
                        [ Reviewer Sign-Off ] ──► [ Immutable Audit Log ]
```

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| **Frontend Framework** | Next.js 14 (Pages Router), React 18, TypeScript |
| **Styling & UI** | Swiss Editorial Design System, Tailwind CSS, Lucide Icons |
| **LLM & Inference** | Groq API (`Llama 3.3 70B Versatile`, temperature `0.1`, JSON schema mode) |
| **Color & Vision** | CIEDE2000 / Euclidean $\Delta E$ color distance, WCAG 2.1 relative luminance, Canvas OCR |
| **Database** | MongoDB Atlas with Mongoose / native driver + reactive in-memory cache |
| **Deployment** | Vercel / Docker / Node.js 18+ |

---

## 🚦 Getting Started

### 1. Prerequisites
- Node.js 18.x or later
- npm or pnpm
- (Optional) Groq API Key (A pre-configured key or in-app modal can be used)
- (Optional) MongoDB connection string for cloud persistence

### 2. Installation
```bash
# Clone the repository
git clone https://github.com/your-username/brandlint.git
cd brandlint

# Install dependencies
npm install
```

### 3. Environment Configuration
Create a `.env.local` file in the root directory:
```env
# Groq GenAI API Key (Llama 3.3 70B inference)
GROQ_API_KEY=your_groq_api_key_here

# MongoDB Connection String (optional, fallback in-memory store available)
MONGODB_URI=your_mongodb_connection_string

# Application Mode
NODE_ENV=development
```

### 4. Run Development Server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

### 5. Production Build
```bash
npm run build
npm start
```

---

## 🎬 2m 45s Demo Walkthrough (PRD Section 17.2)

1. **Problem Framing (0:00 - 0:15)**: Open landing page. Inspect the **Apex Audio** brand profile and Swiss Editorial interface.
2. **Admin Guideline Review (0:15 - 0:40)**: Switch to **Admin** role. Review extracted atomic rules with verbatim citations and confidence metrics. Note low-confidence rules flagged *"Needs confirmation"*. Click **Confirm Rules**.
3. **Fact Vault (0:40 - 0:55)**: Switch to **Fact Vault**. Highlight active warranty policy (*"12-month standard manufacturer warranty"*) and expired marketing claims highlighted in red.
4. **Creator Submission (0:55 - 1:25)**: Switch to **Creator** role. Click **“Load PRD Demo Case”** to populate `"India's #1 — 24-month warranty!"` with an off-palette poster graphic. Click **Run BrandLint Check**.
5. **Cited Issue Report (1:25 - 2:10)**: Risk score spikes to **100/100 (Critical)**. Inspect flagged violations:
   - Prohibited superlative: *"India's #1"*
   - Prohibited superlative: *"unbeatable"*
   - Fact Vault contradiction: *"24-month warranty"* vs. Fact Vault `FV-101` (*"12-month standard manufacturer warranty"*)
   - Tone violation: Aggressive urgency hype and exclamation spam
   - Missing disclaimer: *"Terms & conditions apply. See website for details."*
   - Off-palette color `#E11D48` ($\Delta E = 48.6$)
   - Contrast failure ($2.8:1$ vs. $4.5:1$ WCAG AA requirement)
6. **"Why this flag?" (2:10 - 2:30)**: Expand the warranty contradiction card. Show the verbatim citation quote and 98% confidence score.
7. **Apply 1-Click Fix (2:30 - 2:45)**: Click **Apply Fix** $\rightarrow$ visual diff opens $\rightarrow$ click **Accept Fix**. Copy updates live, reducing the risk score immediately.
8. **Reviewer Sign-Off (2:45 - 3:00)**: Switch to **Reviewer** role $\rightarrow$ open submission $\rightarrow$ add comment $\rightarrow$ click **Approve Submission**. The approval event is permanently recorded in the **Audit Log**.
9. **Conclusion (3:00 - 3:05)**: *"AI detects and explains. Humans decide and approve. This is the compiler for brand content."*

---

## 📡 API Reference Surface

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/auth/me` | Fetch authenticated user and active workspace session |
| `POST` | `/api/auth/demo-login` | Instant zero-credential role switching (Admin/Creator/Reviewer) |
| `GET` / `PATCH` | `/api/brands/[id]` | Read or update brand metadata, palettes, and contrast standards |
| `GET` / `POST` | `/api/guidelines` | List guidelines or upload new documents (PDF/Text) |
| `POST` | `/api/guidelines/parse-pdf` | Server-side binary PDF buffer parser |
| `POST` | `/api/guidelines/[id]/extract` | Trigger Groq LLM rule, claim, and palette extraction |
| `POST` | `/api/guidelines/[id]/confirm` | Lock in guideline rules for active enforcement |
| `GET` / `POST` | `/api/facts` | Query or register verified Fact Vault claims |
| `PATCH` / `DELETE`| `/api/facts/[id]` | Modify claim attributes, expiry dates, or delete records |
| `GET` / `POST` | `/api/submissions` | List or upload creative assets (copy, posters, videos) |
| `POST` | `/api/submissions/[id]/analyze` | Execute the hybrid sequential check engine |
| `POST` | `/api/submissions/extract-video`| Frame sampling and OCR on video submissions |
| `POST` | `/api/issues/[id]/apply-fix` | Apply 1-click suggested copy corrections |
| `POST` | `/api/submissions/[id]/review` | Submit human reviewer verdict (`approve` / `request_changes`) |
| `GET` | `/api/audit-logs` | Retrieve immutable compliance event history |
| `GET` / `POST` | `/api/config/groq` | Check status or update runtime Groq API credentials |

---

## 📄 License

Distributed under the **MIT License**. See `LICENSE` for more information.
