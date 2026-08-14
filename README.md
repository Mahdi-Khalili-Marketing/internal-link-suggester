# 🔗 AI-Powered Semantic Internal Linking Engine
### Automated n8n Workflow with OpenAI Embeddings & Google Sheets

An end-to-end automation workflow built in **n8n** that scans your website articles, generates semantic vector embeddings using **OpenAI**, calculates topic similarity, and suggests high-value internal links with exact anchor texts and contextual sentences.

---

## 📊 1. Google Sheets Setup & Data Schema

Create a single Google Sheets spreadsheet containing **three tabs** with the exact names and column headers listed below:

### Tab 1: `Master_Posts`
> Stores your source article data exported from your CMS (WordPress / Webflow / Custom).

| Column A | Column B | Column C | Column D | Column E |
| :--- | :--- | :--- | :--- | :--- |
| **Post ID** | **Post Title** | **Post URL** | **Plain Text Content** | **Publish Date** |

---

### Tab 2: `embedding store`
> Acts as the vector database storing generated embeddings for each article.

| Column A | Column B | Column C | Column D | Column E |
| :--- | :--- | :--- | :--- | :--- |
| **Post ID** | **Post Title** | **Post URL** | **Plain Text Content** | **Embedding** |

---

### Tab 3: `LinkSuggestionsLog`
> The final output log containing ready-to-use internal linking recommendations.

| Column A | Column B | Column C | Column D | Column E | Column F |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Post ID** | **Source URL** | **Anchor Text** | **Link** | **Reason** | **Contextual Sentence** |

> ⚠️ **Important:** Column headers and sheet tab names are case-sensitive and must match the names above exactly.

---

## ⚙️ 2. Credentials & Configuration

Before running the workflow in **n8n**, configure the following credentials:

1. **Google Sheets OAuth2:**
   - Create a Google Cloud Console project, enable Google Sheets API, and connect the OAuth2 credential in n8n.
2. **OpenAI API Key:**
   - Add your OpenAI API Key into the n8n credentials manager (used for `text-embedding-3-small` and LLM link-reasoning nodes).
3. **Spreadsheet Document ID:**
   - Copy your Google Spreadsheet ID (from the URL: `https://docs.google.com/spreadsheets/d/{SPREADSHEET_ID}/edit`) and update the **Doc ID / Sheet Name** parameters across all Google Sheet nodes.

---

## 🚀 3. Workflow Execution Pipeline
[Master_Posts] ──▶ [Generate Embeddings] ──▶ [embedding store] ──▶ [Cosine Similarity & LLM] ──▶ [LinkSuggestionsLog]
1. **Ingest Content (`Master_Posts`):**
   - Populate `Master_Posts` with your published posts (via WordPress REST API, HTTP Request node, or manual CSV import).
2. **Vectorization (`embedding store`):**
   - n8n reads unprocessed posts from `Master_Posts`, sends cleaned plain-text content to the OpenAI Embeddings API, and saves the vector output into `embedding store`.
3. **Semantic Comparison & AI Filtering (`LinkSuggestionsLog`):**
   - The engine computes cosine similarity across post vectors, selects the most relevant candidate articles, and prompts GPT to select an exact, unlinked anchor phrase and sentence.
   - The final link proposal is logged directly to `LinkSuggestionsLog`.

---

## 💡 4. Key Best Practices & Output Format

- **Ready-to-Use Context:** Every output row in `LinkSuggestionsLog` provides:
  - **Anchor Text:** Exact 3–7 word phrase extracted directly from the post.
  - **Target Link:** Recommended relevant destination URL.
  - **Reason:** Strategic SEO rationale explaining why this link adds editorial value.
  - **Contextual Sentence:** The full sentence where the anchor is located for fast 1-click placement.
- **Anti-Spam & Deduplication:** The prompt prevents linking to already-referenced URLs and strictly disallows generic anchors like *"click here"* or *"read more"*.

---

## 👨‍💻 Author & Maintainer
Built by **[Mahdi Khalili](https://www.linkedin.com/in/mahdi-khalili-marketing/)** — Performance Marketing & AI Automation Specialist.
