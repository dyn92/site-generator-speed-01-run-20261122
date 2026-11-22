# site-generator-speed-01-run-20261122

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/dyn92/site-generator-speed-01-run-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/dyn92/site-generator-speed-01-run-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

Entry for make.fun's ["Build a static site faster, 22 November 2026" bounty](https://make.fun/bounty/site-generator-speed-01-run-20261122) ($100 total, $10 x 10 places).

A faster drop-in for make.fun's own makefun-site 1.0.0: `solve.js` reimplements
its build — front matter, template dialect, Markdown subset, slug rule and
report — from the spec, and returns the same output tree byte for byte, only
faster. `main.ts` is the driver the harness runs: it reads every case from
`/input/cases.json`, answers each with `run`, and writes `/output/answers.json`.

Speed comes from the hot paths, not from shortcuts: the Markdown renderer
works in one streaming pass (a regex jump to each next special character,
plain runs copied as slices, no token objects, no per-character regex
escapes); front matter is parsed by scanning line starts instead of
splitting the whole file; slugifying takes a fast path for pure-ASCII text;
word counting uses a whitespace lookup table; and the template engine walks
its parsed trees with string building. Dependency-free: no imports at all in
`solve.js`, Node builtins only in `main.ts`. Deterministic: no clock, no
randomness, no environment reads — two runs answer byte for byte alike.

Verified against the practice set: 3/3 cases byte-identical to makefun-site
1.0.0's expected answers, and 36/36 further cases drawn from the kit's own
generator with fresh seeds, byte-identical to the oracle. Build-only time on
the practice set: ~150 ms vs ~475 ms for the house baseline (~3.2x); whole
driver run ~650 ms vs ~978 ms.
