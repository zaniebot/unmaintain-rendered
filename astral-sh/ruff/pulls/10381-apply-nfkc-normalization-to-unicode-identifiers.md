```yaml
number: 10381
title: Apply NFKC normalization to unicode identifiers when storing bindings in the semantic model
type: pull_request
state: closed
author: AlexWaygood
labels: []
assignees: []
draft: true
base: main
head: unicode-normalize-identifiers
created_at: 2024-03-13T12:20:15Z
updated_at: 2024-03-14T17:42:41Z
url: https://github.com/astral-sh/ruff/pull/10381
synced_at: 2026-01-10T22:47:01Z
```

# Apply NFKC normalization to unicode identifiers when storing bindings in the semantic model

---

_Pull request opened by @AlexWaygood on 2024-03-13 12:20_

## Summary

Fixes #5003.

Python [applies NFKC normalization to identifiers that use unicode characters](https://docs.python.org/3/reference/lexical_analysis.html#identifiers). That means that F821 should not be emitted if ruff encounters the following snippet (but on `main`, it is), as from Python's perspective, these are all the same identifier:

```py
𝒞 = 500
print(𝒞)
print(C + 𝒞)  # ruff says `C` isn't defined
print(C / 𝒞)
print(C == 𝑪 == 𝒞 == 𝓒 == 𝕮)  # ruff says `C`, `𝑪`, ... isn't defined
```

I fixed this false positive by changing the `bindings` field in `ruff_python_semantic/scope.rs` so that identifiers are always unicode-normalized according to NFKC before being stored in the hashmap. An alternative approach I played around with was to unicode-normalize identifiers in the AST itself. However, this would have had undesirable consequences: the formatter would have started eagerly normalizing unicode characters in identifiers when it reformatted a Python file.

## Test Plan

`cargo test`


---

_Comment by @codspeed-hq[bot] on 2024-03-13 12:26_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:unicode-normalize-identifiers)

### Merging #10381 will **degrade performances by 5.17%**

<sub>Comparing <code>AlexWaygood:unicode-normalize-identifiers</code> (0385a62) with <code>main</code> (e944c16)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:unicode-normalize-identifiers)._

### Benchmarks breakdown

|     | Benchmark | `main` | `AlexWaygood:unicode-normalize-identifiers` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[numpy/globals.py]` | 635 µs | 669.6 µs | -5.17% |


---

_Comment by @AlexWaygood on 2024-03-13 12:27_

Ouch. I guess I need to start running benchmarks before filing PRs :)

---

_Converted to draft by @AlexWaygood on 2024-03-13 12:27_

---

_Comment by @github-actions[bot] on 2024-03-13 13:03_

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

_Comment by @AlexWaygood on 2024-03-14 17:42_

Closing in favour of #10412

---

_Closed by @AlexWaygood on 2024-03-14 17:42_

---

_Branch deleted on 2024-03-14 17:42_

---
