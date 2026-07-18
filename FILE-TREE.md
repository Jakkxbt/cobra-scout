# scout — file tree

```
scout/
├── scout            # Main executable CLI — multi-source keyword monitor + --watch cache
├── README.md        # Human docs — sources, usage, cron notes
├── FILE-TREE.md     # This file
├── .gitignore       # scout-results/, __pycache__, *.pyc
└── scout-results/   # Runtime: <keyword>_<timestamp>.txt (+ .json) — gitignored
```

Seen-cache lives outside the repo: `~/.config/cobra-scout/seen.json`
