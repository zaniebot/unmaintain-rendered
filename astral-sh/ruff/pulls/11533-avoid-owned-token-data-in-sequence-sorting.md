```yaml
number: 11533
title: Avoid owned token data in sequence sorting
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/unsorted-dunder
created_at: 2024-05-24T13:56:44Z
updated_at: 2024-05-27T00:20:21Z
url: https://github.com/astral-sh/ruff/pull/11533
synced_at: 2026-01-12T15:55:38Z
```

# Avoid owned token data in sequence sorting

---

_@dhruvmanila_

## Summary

This PR updates the sequence sorting (`RUF022` and `RUF023`) to avoid using the owned data from the string token. Instead, we will directly use the reference to the data on the AST. This does introduce a lot of lifetimes but that's required.

The main motivation for this is to allow removing the `lex_starts_at` usage easily.

### Alternatives

1. Extract the raw string content (stripping the prefix and quotes) using the `Locator` and use that for comparison
2. Build up an [`IndexVec`](https://github.com/astral-sh/ruff/blob/3e30962077a39ee3bf6a9ee93fb3c6aa5b1f7e4b/crates/ruff_index/src/vec.rs) and use the newtype index in place of the string value itself. This also does require lifetimes so we might as well just use the method in this PR.

## Test Plan

`cargo insta test` and no ecosystem changes


---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-05-24 13:56_

---

_Renamed from "Avoid token owned data in sequence sorting" to "Avoid owned token data in sequence sorting" by @dhruvmanila on 2024-05-24 13:59_

---

_Label `internal` added by @dhruvmanila on 2024-05-24 13:59_

---

_Comment by @github-actions[bot] on 2024-05-24 14:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@snowsignal approved on 2024-05-24 15:55_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:527 on 2024-05-26 18:19_

nit: this is a very long line ;)

```suggestion
                    unreachable!(
                        "Expected the number of string tokens to be equal \
                        to the number of string items in the sequence"
                    );
```

---

_@AlexWaygood approved on 2024-05-26 18:20_

LGTM. I don't mind the lifetimes.

---

_@charliermarsh approved on 2024-05-27 00:20_

---

_Merged by @charliermarsh on 2024-05-27 00:20_

---

_Closed by @charliermarsh on 2024-05-27 00:20_

---

_Branch deleted on 2024-05-27 00:20_

---
