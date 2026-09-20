# Nyay Saathi — legal documents in plain language

A GenAI assistant that helps ordinary people **understand, compare and prepare questions about legal documents** (rental agreements, employment offers, loan terms, policies). Built for the theme **AI for Legal Assistance & Access**, powered by the **Google Gemini API**.

> It provides general information only. It is **not legal advice** and does not replace a lawyer.

## What it does

| Tab | What you get |
|---|---|
| **Explain** | Plain-language summary, who is involved, key numbers and dates, clause-by-clause meaning, who must do what, gaps, and a jargon glossary |
| **Risks** | One-sided, costly, vague or unusual clauses. Each is **highlighted in the document itself** (like a highlighter pen) with why it matters and what to ask for |
| **Compare** | Two agreements or drafts side by side: what differs, which is better for *you*, what exists in only one |
| **Ask** | Chat grounded in the document. Every answer quotes the passage it relied on, and says plainly when the document does not cover the question |
| **Next steps** | Options with upsides and downsides, a prioritised checklist, papers to gather, deadlines, and a short brief to hand a lawyer |

Also included:

- **Multilingual output**: English, Hindi, Marathi, Bengali, Tamil, Telugu, Gujarati, Kannada, Malayalam, Punjabi, Urdu, Spanish, French.
- **Reads real files**: PDF, Word (.docx), text, and **photos or scans** (Gemini transcribes them).
- **Privacy tools**: one click hides emails, phone numbers, Aadhaar, PAN and account numbers before analysis. No server: your browser talks directly to Gemini.
- **Role-aware**: tell it you are the tenant, borrower or employee and it judges fairness from your side.
- **Print or save as PDF**, copy results, and a built-in sample rental agreement pair for a quick demo.
- Pointer to free legal aid in India (NALSA helpline 15100).

## Run locally

No build step. Serve the folder with any static server:

```bash
python3 -m http.server 8080
# open http://localhost:8080
```

Click **Settings**, paste a Gemini API key ([get one free](https://aistudio.google.com/apikey)), then click **Try a sample**.

## Deploy on GitHub Pages (live link)

1. Create a **public** repo and push these files to the `main` branch.
2. In the repo go to **Settings → Pages → Build and deployment → Source: GitHub Actions**.
3. The workflow in `.github/workflows/deploy.yml` runs on every push. When it finishes, the **Actions** tab shows the URL, normally `https://<your-username>.github.io/<repo-name>/`.

### Adding your Gemini API key

There are two ways. Pick one.

**Option A — visitors bring their own key (safest, default).**
Do nothing. Anyone opening the site pastes a key in **Settings**. It is stored only in their browser.

**Option B — bake your key in so judges can try it instantly.**
1. Repo **Settings → Secrets and variables → Actions → New repository secret**, name it `GEMINI_API_KEY`.
2. Re-run the workflow (Actions → Deploy to GitHub Pages → Run workflow).

⚠️ A static site has no backend, so **the key ends up in the page source and anyone can copy it.** Reduce the risk before you do this:

- In [Google AI Studio](https://aistudio.google.com/apikey) or the Google Cloud console, **restrict the key to HTTP referrers**: `https://<your-username>.github.io/*`.
- Use a key from a **separate project** with a low quota, and delete it after the event.
- Never commit the key to the repo. `config.js` should stay empty in git.

## Project layout

```
index.html                     page structure
styles.css                     design system
app.js                         Gemini client, file reading, prompts, rendering
samples.js                     fictional sample agreements for the demo
config.js                      runtime config (empty key in git)
.github/workflows/deploy.yml   syntax check + deploy to GitHub Pages
```

## How Gemini is used

- **Structured outputs**: every analysis uses `responseMimeType: application/json` with a `responseSchema`, so results render as clean cards instead of free text.
- **Grounding by quotation**: the model must quote the clause it relies on. The app finds that quote in the document and highlights it, so users can verify claims instead of trusting them.
- **Multimodal OCR**: scanned PDFs and photos are transcribed by Gemini, then analysed like any other text.
- **System instructions** keep answers plain-spoken, balanced, non-authoritative and resistant to instructions hidden inside a document.

## Limits

- Not legal advice. The assistant is told not to cite statutes or case law it cannot verify.
- Very long documents are truncated at about 350,000 characters.
- Free-tier Gemini quotas apply; the app retries once or twice when rate-limited.
- Gemini model names change over time. **Settings → Model** lets you switch, or enter any model name.
