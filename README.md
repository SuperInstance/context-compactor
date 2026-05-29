# context-compactor

Cloudflare Worker that compresses long text to fit memory limits — no external API calls required.

## What This Gives You

- **Extractive summarization** — scores sentences by word frequency, keeps the most informative ones
- **Sliding window compression** — chunks text into summary segments with configurable window size
- **Keyphrase extraction** — pulls top bigrams as a compact representation
- **Zero dependencies** — runs entirely on-Worker, no LLM or third-party API
- **KV-cached results** — SHA-256 keyed cache with 24-hour TTL

## Quick Start

```bash
# Deploy to Cloudflare Workers
wrangler deploy

# Compress text via API
curl -X POST https://context-compactor.<your-subdomain>.workers.dev/api/compact \
  -H "Content-Type: application/json" \
  -d '{"text": "Your long text here...", "strategy": "extractive", "ratio": 0.3}'
```

### Strategies

| Strategy | Description | Best for |
|---|---|---|
| `extractive` | Top-N sentences by TF scoring | General compression |
| `sliding` | Windowed chunk summaries | Very long documents |
| `keyphrase` | Top bigrams as keyphrases | Keyword extraction |

### Response

```json
{
  "compacted": "Compressed text...",
  "strategy": "extractive",
  "inputLength": 5000,
  "outputLength": 1500,
  "compressionRatio": 0.3
}
```

## How It Fits

A Cocapn Fleet vessel. Part of the SuperInstance ecosystem for distributed AI operations.

Related repos:
- [cocapn-shells](https://github.com/SuperInstance/cocapn-shells) — fleet shell infrastructure
- [context-compactor-v2](https://github.com/SuperInstance/context-compactor-v2) — next-gen compactor
- [context-lattice](https://github.com/SuperInstance/context-lattice) — context window management

## License

Apache 2.0
