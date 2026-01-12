```yaml
number: 8225
title: Optimize workflow run
type: pull_request
state: merged
author: Cjkjvfnby
labels:
  - ci
assignees: []
merged: true
base: main
head: sa/optimize-workflow-run
created_at: 2023-10-25T19:39:42Z
updated_at: 2023-11-30T00:09:33Z
url: https://github.com/astral-sh/ruff/pull/8225
synced_at: 2026-01-10T23:40:55Z
```

# Optimize workflow run

---

_Pull request opened by @Cjkjvfnby on 2023-10-25 19:39_

## Summary
- Don't run jobs that are not affected
- Replace `scripts/*` with `scripts/**`

## Test Plan
This PR changes ci.yaml, so it will trigger all jobs.  there is no good way to test if something is skipped before merging this to the main and creating other PRs.

I am not familiar with the project well, so might miss some configs that are important. 


---

_@zanieb reviewed on 2023-10-25 19:52_

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:28 on 2023-10-25 19:52_

```suggestion
      code: ${{ steps.changed.outputs.code_any_changed }}
```
?

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:70 on 2023-10-26 00:04_

```suggestion
              - pyproject.toml
              - rust-toolchain.toml
              - fuzz/**
```

Does the plugin support a way to say: Only run when e.g. all changed files are markdown files? It would reduce that we missed adding an entry. I rather have the job runs too often that miss out. 

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:64 on 2023-10-26 00:05_

```suggestion
              - crates/**
              - scripts/**
```

---

_@MichaReiser reviewed on 2023-10-26 00:08_

Thanks for improving our pipeline. 

I like the idea of running fewer jobs but I'm unsure if the added complexity is worth it, considering that most jobs include code changes. It's also very easy to forget files. 

---

_Review comment by @Cjkjvfnby on `.github/workflows/ci.yaml`:70 on 2023-10-29 12:51_

As I see no, it could not.  

But it could be addressed by running them on master, after the merge.

---

_@Cjkjvfnby reviewed on 2023-10-29 12:51_

---

_@Cjkjvfnby reviewed on 2023-10-29 12:51_

---

_Review comment by @Cjkjvfnby on `.github/workflows/ci.yaml`:64 on 2023-10-29 12:51_

Nice catch, I have copypasted this form existing code.

---

_Closed by @Cjkjvfnby on 2023-10-29 13:16_

---

_Reopened by @Cjkjvfnby on 2023-10-29 13:17_

---

_Comment by @Cjkjvfnby on 2023-10-29 13:17_

Force pushed changes, after rebasing on top of the main

---

_@zanieb reviewed on 2023-11-01 20:38_

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:70 on 2023-11-01 20:38_

Why not with something like

```
- **/*
- !**/*.md
```

I would be surprised if this was not feasible.

---

_@zanieb requested changes on 2023-11-01 20:39_

I don't think we want to maintain this list of files. I'd prefer the "negated" style that @MichaReiser requested as well.

---

_Comment by @github-actions[bot] on 2023-11-04 13:45_

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

_Comment by @Cjkjvfnby on 2023-11-04 13:48_

The first implementation with negation.


code and formatter sections are the same, probably some exclusions should be added to formatted. I don't have expertise in the project to do it. 



---

_Label `internal` added by @MichaReiser on 2023-11-27 23:07_

---

_Label `ci` added by @MichaReiser on 2023-11-27 23:07_

---

_Label `internal` removed by @MichaReiser on 2023-11-27 23:07_

---

_@MichaReiser requested changes on 2023-11-27 23:12_

The formatter now runs on all linter changes because it doesn't exclude the linter specific crates (or development-only crates). 

I'm okay merging this but I would prefer reverting the changes to the linter/formatter filesets to keep the changes isolated. 



---

_Comment by @Cjkjvfnby on 2023-11-29 20:38_

> The formatter now runs on all linter changes because it doesn't exclude the linter specific crates (or development-only crates).

I don't know Rust, so could not do it myself. 

> I'm okay merging this but I would prefer reverting the changes to the linter/formatter filesets to keep the changes isolated.

Done.


---

_Comment by @codspeed-hq[bot] on 2023-11-29 20:51_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Cjkjvfnby:sa/optimize-workflow-run)

### Merging #8225 will **improve performances by 11.42%**

<sub>Comparing <code>Cjkjvfnby:sa/optimize-workflow-run</code> (8ad224b) with <code>main</code> (5d554ed)</sub>



### Summary

`⚡ 5` improvements
`✅ 20` untouched benchmarks

`🆕 5` new benchmarks



### Benchmarks breakdown

|     | Benchmark | `main` | `Cjkjvfnby:sa/optimize-workflow-run` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 17.2 ms | 16.2 ms | +6.03% |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 4.2 ms | 3.9 ms | +6.42% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 80.3 ms | 72.2 ms | +11.18% |
| 🆕 | `linter/all-with-preview-rules[unicode/pypinyin.py]` | N/A | 17.2 ms | N/A |
| 🆕 | `linter/all-with-preview-rules[numpy/globals.py]` | N/A | 4.2 ms | N/A |
| 🆕 | `linter/all-with-preview-rules[large/dataset.py]` | N/A | 185.6 ms | N/A |
| 🆕 | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | N/A | 37.1 ms | N/A |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 36.8 ms | 33.9 ms | +8.58% |
| ⚡ | `linter/all-rules[large/dataset.py]` | 186.2 ms | 167.1 ms | +11.42% |
| 🆕 | `linter/all-with-preview-rules[pydantic/types.py]` | N/A | 79.9 ms | N/A |


---

_@MichaReiser approved on 2023-11-30 00:09_

---

_Merged by @MichaReiser on 2023-11-30 00:09_

---

_Closed by @MichaReiser on 2023-11-30 00:09_

---
