```yaml
number: 11089
title: Add a new CI job to fuzz the parser
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: fuzz-ci
created_at: 2024-04-22T12:27:26Z
updated_at: 2024-04-23T10:33:29Z
url: https://github.com/astral-sh/ruff/pull/11089
synced_at: 2026-01-12T15:55:34Z
```

# Add a new CI job to fuzz the parser

---

_@AlexWaygood_

## Summary

This PR adds a new CI job that tests the parser by running the new `scripts/fuzz-parser` script. The job runs the parser over 1,000 randomly generated (but all syntactically valid) Python source files. For any files that the parser fails to parse, the script minimises the generated source code into a minimal reproducible example. On completion of the script, if any bugs were encountered, the script fails with exit code 1.

## How many seeds should we pass to the script?

This PR currently passes 1,001 seeds to the script for it to test. In an ideal world, we'd obviously run it over a larger range of seeds, but I think 1,001 seeds is enough for it to serve as a useful regression test (I discovered the bug that was fixed in #11009 on seed 6 with an early version of the `fuzz-parser` script).

The new job takes around ten minutes to run in CI. That's quicker than the ecosystem job if the ecosystem job runs both the formatter and linter ecosystem checks, but it's slower than the ecosystem job if that job runs only the linter ecosystem checks. If we think 10 minutes is too long for this job, we could pass a smaller range of seeds to the script in CI.

One way we could get the "best of both worlds" might be to pass it a small range of seeds for CI on PRs (500?) but have a nightly job that passes a large range of seeds (5,000? 10,000?). We could have a bot that automatically opens an issue when the nightly job fails -- we do something like this at typeshed: https://github.com/python/typeshed/issues/11801

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-04-22 12:27_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-04-22 12:27_

---

_Comment by @github-actions[bot] on 2024-04-22 12:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `ci` added by @MichaReiser on 2024-04-23 07:19_

---

_@MichaReiser approved on 2024-04-23 07:20_

---

_Review comment by @dhruvmanila on `.github/workflows/ci.yaml`:246 on 2024-04-23 08:20_

Does having the same seed value mean that we'll be fuzzing over the _same_ randomly generated source code? It seems like so:

```
In [7]: for i in range(100):
   ...:     a = generate(i)
   ...:     b = generate(i)
   ...:     print(f"{i}: {a == b}")
   ...: 
0: True
1: True
2: True
3: True
4: True
5: True
6: True
7: True
8: True
9: True
10: True
11: True
12: True
13: True
14: True
15: True
...
```

Do you think it would be useful to have a randomly generated seed value in here? I don't know how easy it is to add something like that to GitHub Actions.

---

_@dhruvmanila approved on 2024-04-23 08:26_

Thank you! I'm not exactly sure about having this in CI, but I'm fine adding it for now. For now, I just run the fuzzer locally with random seed numbers for around 2-3k inputs. Do you know how this affects the CI minutes?

> The new job takes around ten minutes to run in CI. That's quicker than the ecosystem job if the ecosystem job runs both the formatter and linter ecosystem checks, but it's slower than the ecosystem job if that job runs only the linter ecosystem checks. If we think 10 minutes is too long for this job, we could pass a smaller range of seeds to the script in CI.

I think we can start with 500 for now if it's possible to add a random seed number for every invocation of the CI.

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:246 on 2024-04-23 08:31_

Yup, the script is deterministic — the same seed always generates the same random source file.

I think it depends on whether we think the purpose of running it in CI is primarily to catch regressions or to find previously unknown bugs. If the purpose is to catch regressions, we should run it over the same set of seeds on each PR (because otherwise we won't know that it's a _new_ bug without doing some investigation). If the purpose is to find _unknown_ bugs (new or pre-existing), then maybe a nightly workflow running it over a large set of randomly selected seeds would be better. If both, then maybe we should do both 😛

---

_@AlexWaygood reviewed on 2024-04-23 08:31_

---

_@AlexWaygood reviewed on 2024-04-23 08:33_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:246 on 2024-04-23 08:33_

> Do you think it would be useful to have a randomly generated seed value in here? I don't know how easy it is to add something like that to GitHub Actions.

If we want to do this, my instinct would be to just add a CLI option `--random-seeds` or something, and then use Python's `random` module to generate the seeds from inside the script

---

_@dhruvmanila reviewed on 2024-04-23 08:38_

---

_Review comment by @dhruvmanila on `.github/workflows/ci.yaml`:246 on 2024-04-23 08:38_

Oh, that's a good point. I think it's fine to run it on the same set of seeds and I can provide some context in the contributing docs regarding this. Thanks for the context.

---

_Comment by @AlexWaygood on 2024-04-23 10:17_

I've reduced the number of seeds in CI to 501. I'll land this now as a regression test to run on PRs, and I'll separately explore running a larger, random set of seeds as a nightly workflow

---

_Merged by @AlexWaygood on 2024-04-23 10:24_

---

_Closed by @AlexWaygood on 2024-04-23 10:24_

---

_Branch deleted on 2024-04-23 10:26_

---

_Comment by @AlexWaygood on 2024-04-23 10:28_

> For now, I just run the fuzzer locally with random seed numbers for around 2-3k inputs. Do you know how this affects the CI minutes?

It seems like in CI (which uses a debug build and runs on machines with only a few cores, so seems to be a fair bit slower than running the script locally), it reliably fuzzes around 100 seeds a minute. So passing 500 seeds means the script takes 5 minutes; passing 1,000 seeds means the script takes 10 minutes; etc.

---
