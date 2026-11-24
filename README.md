# 🔭 Perplexity Explorer

An interactive, single-file browser tool that teaches you what **perplexity** really means — using a real language model running entirely in your browser.

🚀 **Live demo:** https://perplexity-explorer-two.vercel.app

## What it does

| Section | Description |
|---|---|
| **1 — Type a sentence** | Load DistilGPT-2 (quantised, ~40 MB) via Transformers.js and compute per-token probabilities |
| **2 — Token table** | See each token's probability P, its 1/P, and a full three-step perplexity derivation |
| **3 — Next-token predictions** | Horizontal bar chart of the top 8 tokens the model predicts next |
| **4 — Try it by hand** | Live simulator — enter any tokens and probabilities, see perplexity update instantly (no model needed) |

## How to run locally

Just open `index.html` in Chrome, Firefox, or Safari — no build step, no dependencies.

```bash
# Or serve it with Python for the best experience:
python3 -m http.server 8080
# then open http://localhost:8080/index.html
```

## How it works

- Uses **[Transformers.js](https://github.com/xenova/transformers.js)** to run `Xenova/distilgpt2` (8-bit quantised) in WebAssembly inside the browser.
- Prepends the GPT-2 BOS token (`50256`) so the **first** word also gets a real probability.
- Applies **stable softmax** (subtract max logit first) to avoid numerical overflow.
- Computes perplexity in **log-space**: `exp( mean(-ln P) )` to handle long sentences safely.

## Tech stack

- Pure HTML + CSS + vanilla JavaScript — **one file, no framework, no backend**
- [Transformers.js v3](https://cdn.jsdelivr.net/npm/@huggingface/transformers@3) via CDN
- Model: `Xenova/distilgpt2` with `dtype: "q8"`
