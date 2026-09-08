<p align="center">
  <img src="assets/banner.png" alt="CobraSEC · Blue Arsenal · scout" width="100%">
</p>

# scout

**Watch free public news for a keyword. Alert only on what’s new.**

`scout` searches Google News RSS, Hacker News (Algolia), and Reddit for a phrase
you care about — a competitor name, a product launch, a topic. Results print to
the terminal and land under `./scout-results/`. With `--watch`, a local seen-cache
means the next run only shows links you haven’t seen before (cron-friendly).

It does **not** touch X/Twitter or Facebook. Those have no usable free public
search API; pretending otherwise would be lying.

```
scout 1.0.0  keywords=1  watch=off  results=scout-results
[+] 'openai'  matches=40  skipped=reddit  -> scout-results/openai_20260718-014500.txt
    · [google_news] OpenAI announces …
    · [hacker_news] Show HN: …
```

## Install

Python 3, stdlib only — no pip packages, no API keys.

```bash
cd scout
chmod +x scout
./scout "ai pentest"
```

## Usage

```bash
scout "ai pentest"                 # search now
scout openai                       # single token is fine
scout -f keywords.txt              # one keyword per line
scout "ai pentest" --watch         # only NEW links since last run
scout openai -v                    # full report on screen
scout openai --json                # also write .json
scout openai -o /tmp/out.txt
scout openai -q                    # path only
```

**keywords.txt**

```
# brands / topics
openai
anthropic launch
ai pentest
```

## Flags

| Flag | Meaning |
|------|---------|
| `keyword` | Search phrase |
| `-f FILE` | Keywords file (`#` comments / blanks skipped) |
| `--watch` | Only new links; updates `~/.config/cobra-scout/seen.json` |
| `-o FILE` | Exact output path (single keyword) |
| `--results-dir DIR` | Auto-save dir (default `./scout-results`) |
| `--json` | Also save JSON |
| `-v` / `--verbose` | Print full report |
| `-q` / `--quiet` | Only errors + path |
| `--timeout SEC` | Per-request timeout (default 12) |
| `--seen-file PATH` | Override watch cache path |

## Sources (honest list)

| Source | Endpoint |
|--------|----------|
| Google News | `news.google.com/rss/search?q=…` |
| Hacker News | `hn.algolia.com/api/v1/search?query=…&tags=story` |
| Reddit | `reddit.com/search.json?q=…` (often 403 from datacenter IPs — skipped cleanly) |

**Not covered:** X/Twitter, Facebook, LinkedIn, paid news APIs.

If a source dies mid-run, the others still finish. Matches are de-duped by link.

## Watch / cron

```bash
# first run seeds the cache
scout "openai" --watch
# later runs only print/save unseen links
scout "openai" --watch -q
```

```cron
*/30 * * * * cd /path/to/scout && ./scout -f keywords.txt --watch -q >> /var/log/scout.log 2>&1
```

Author: **Jakkxbt**.
