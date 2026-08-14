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
