# Contributing

This is a living, **citation-verified** map of open problems in multimodal AI. The whole value of the repo is that entries are trustworthy — so the bar for adding one is a verified source, not just a plausible-sounding claim.

## Entry format

Each open problem, inside the relevant `problems/0X-*.md` file, follows the same shape:

```markdown
## X.Y Problem Title
**Why it's hard:** 2–3 sentences on the core difficulty and why it's unsolved.

**Key papers:** Title, venue/year — <marker> [arXiv:XXXX.XXXXX](https://arxiv.org/abs/XXXX.XXXXX)
**Benchmarks:** Name (what it measures) — <marker> [link]
**Directions:** short list of promising research angles.
```

## Verification markers (required)

- ✅ **verified** — you opened the arXiv/venue page and confirmed the title *and* ID.
- ⚠️ **recent / re-check** — a recent preprint whose ID looks plausible but you didn't fetch to primary-source depth. Allowed, but must be marked.
- **Unverified / candidates** — anything you could not authenticate goes in the file's bottom section under this heading. **Never** mix it into the main lists.

Do not add a citation you have not checked. If in doubt, mark it ⚠️ or park it under *Unverified / candidates*.

## Adding a new cluster or problem

1. Put problems in the cluster file where they best fit; cross-link related ones with a relative link (e.g. `[cluster 3](problems/03-perception-frontiers.md)`).
2. If you add a cluster, add a row to the taxonomy table in `README.md` and a one-line summary.
3. Keep the tone terse and comparative — this is a map for *finding a topic*, not a textbook.

## Scope

Multimodal AI broadly: representation/fusion, perception, hallucination, video/temporal, generation, reasoning, agents, and the cross-cutting data/efficiency/evaluation substrate. Language-only or vision-only work belongs only where it directly bears on a multimodal open problem.
