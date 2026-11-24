# A faster makefun-site — site-generator-speed-01

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/deelyy78/makefun-site-generator-speed/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/deelyy78/makefun-site-generator-speed/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A drop-in replacement for makefun-site 1.0.0, make.fun's own static site
generator, for the make.fun competition
`site-generator-speed-01-run-20261122`. `solve.js` exports `run(input)`,
which returns exactly the output tree makefun-site 1.0.0's build returns
for every input — each page's `index.html`, the index and tag pages,
`build-report.txt` and each static file, byte for byte — in less CPU
time. `main.ts` is the harness driver: it reads the cases from
`/input/cases.json`, answers each one alone with `run`, and writes
`/output/answers.json`.

## How it is fast

The answers are the reference's; the work is organised differently:

- Inline Markdown escapes plain text in bulk runs instead of character
  by character, and finds its closers with table-driven scans.
- The block lexer dispatches on a line's first character instead of
  testing every block pattern against every line.
- `slugify` has an ASCII fast path (NFKD is the identity on ASCII) and
  is memoised per build, as are long dates; word counting scans instead
  of building a match array.
- Nothing is cached from one case to the next: every memo lives inside
  one `run()` call.

## Layout

- `solve.js` — the generator: front matter, Markdown, templates and the
  build pipeline, in one dependency-free ES module.
- `main.ts` — the program/v1 driver described above.
- `LICENSE` — MIT.
