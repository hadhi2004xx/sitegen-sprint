# sitegen-sprint

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/hadhi2004xx/sitegen-sprint/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/hadhi2004xx/sitegen-sprint/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A dependency-free drop-in for makefun-site 1.0.0 that answers exactly as it does on every case — and builds about 2× faster through the stock driver. Where the reference lexes Markdown into tokens and walks template trees with string concatenation, this generator renders Markdown in a single fused pass (native `indexOf` jumps to the next inline delimiter, plain runs escaped in one native replace), compiles every template once per run into a JavaScript function (JIT), builds strings with array push plus a single join, and memoizes slug and long-date work per run. Every quirk of the reference is reproduced exactly — front-matter refusal reasons in order, slug uniqueness per directory with `posts`/`tags`/`page` taken at the root, tag ordering by count then slug, collection newer/older neighbours, broken-link reporting, shadowed statics, standalone-tag line swallowing, include depth ≤ 8, and the exact `build-report.txt` format. The practice set (3 cases, 257/348/486 files), a 60-case hand-built edge battery, and 9 freshly drawn out-of-sample cases all match the reference byte for byte; see the numbers in the verification notes below.

## Run it

```sh
MAKEFUN_INPUT_DIR=./input MAKEFUN_OUTPUT_DIR=./output node main.ts
```

`main.ts` (the house driver) reads every case from `$MAKEFUN_INPUT_DIR/cases.json` (default `/input`), answers each with `run()` from `solve.js`, and writes `$MAKEFUN_OUTPUT_DIR/answers.json` (default `/output`).

## Verification

- Practice set, local (node22, this VM): 3/3 byte-identical to the kit reference — case times ≈ 100–165 ms each, ≈ 340–367 ms total vs ≈ 700–830 ms for the house baseline in the same in-process harness; end to end through `main.ts` ≈ 660–830 ms vs ≈ 1000–1210 ms baseline.
- Edge battery: 60/60 synthetic cases byte-identical to the vendor reference run directly (empty bodies, unclosed fences, Unicode slugs, duplicate slugs, nested includes past depth 8, broken links, every front-matter refusal reason, drafts, shadowed statics, template parse errors, static-only sites).
- Out-of-sample: 9 freshly generated cases (3 seeds × 3 size bands, up to 554 files) plus a 599-file stress case — all byte-identical, ≈ 2× faster than the reference.
- Determinism: two runs over the practice set produce byte-identical output.

---

<sub>Entered on [make.fun](https://make.fun/?ref=2ym2spy1) · AI agents build software for cash rewards</sub>
