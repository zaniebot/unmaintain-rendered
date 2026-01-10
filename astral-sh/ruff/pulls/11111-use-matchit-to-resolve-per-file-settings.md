```yaml
number: 11111
title: Use Matchit to Resolve Per-File Settings
type: pull_request
state: merged
author: ibraheemdev
labels:
  - performance
assignees: []
merged: true
base: main
head: matchit
created_at: 2024-04-23T18:07:46Z
updated_at: 2024-04-24T14:25:47Z
url: https://github.com/astral-sh/ruff/pull/11111
synced_at: 2026-01-10T22:37:01Z
```

# Use Matchit to Resolve Per-File Settings

---

_Pull request opened by @ibraheemdev on 2024-04-23 18:07_

## Summary

Continuation of https://github.com/astral-sh/ruff/pull/9444.

> When the formatter is fully cached, it turns out we actually spend meaningful time mapping from file to `Settings` (since we use a hierarchical approach to settings). Using `matchit` rather than `BTreeMap` improves fully-cached performance by anywhere from 2-5% depending on the project, and since these are all implementation details of `Resolver`, it's minimally invasive.

`matchit` supports escaping routing characters so this change should now be fully compatible.

## Test Plan

On my machine I'm seeing a ~3% improvement with this change.

```
hyperfine --warmup 20 -i "./target/release/main format ../airflow" "./target/release/ruff format ../airflow"
Benchmark 1: ./target/release/main format ../airflow
  Time (mean ± σ):      58.1 ms ±   1.4 ms    [User: 63.1 ms, System: 66.5 ms]
  Range (min … max):    56.1 ms …  62.9 ms    49 runs
 
Benchmark 2: ./target/release/ruff format ../airflow
  Time (mean ± σ):      56.6 ms ±   1.5 ms    [User: 57.8 ms, System: 67.7 ms]
  Range (min … max):    54.1 ms …  63.0 ms    51 runs
 
Summary
  ./target/release/ruff format ../airflow ran
    1.03 ± 0.04 times faster than ./target/release/main format ../airflow
```

---

_Comment by @github-actions[bot] on 2024-04-23 18:28_

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

_Review comment by @BurntSushi on `crates/ruff_workspace/src/resolver.rs`:160 on 2024-04-23 18:28_

I was curious about this, and I think I convinced myself that this is correct because all other possible errors (aside from `Conflict`) can only occur as a result of characters that the router treats as special. And the escaping done above means that there should be precisely zero special characters in the path.

(It might make sense to add an `escape` routine to `matchit`. Similar to `regex::escape`. That way, you can provide more of a stronger guarantee here.)

---

_@BurntSushi approved on 2024-04-23 18:29_

Nice!!!

---

_@ibraheemdev reviewed on 2024-04-23 18:32_

---

_Review comment by @ibraheemdev on `crates/ruff_workspace/src/resolver.rs`:160 on 2024-04-23 18:32_

Yeah after escaping we guarantee that we're inserting a static route, so none of the other errors are possible (barring a bug in `matchit`). An `escape` function in `matchit` is a good idea. I was also considering optimizing the double `String::replace` but decided it wasn't worth it for us.

---

_@charliermarsh reviewed on 2024-04-23 18:36_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/resolver.rs`:151 on 2024-04-23 18:36_

Are there any other characters that we need to watch out for here? In my previous PR I mentioned colons, which seems irrelevant, but e.g. `{*foo}` is a special routing parameter in matchit, right? I assume that's _also_ irrelevant as long as we're escaping the `{` and `}` characters.

---

_@ibraheemdev reviewed on 2024-04-23 18:41_

---

_Review comment by @ibraheemdev on `crates/ruff_workspace/src/resolver.rs`:151 on 2024-04-23 18:41_

`*` is only considered after a non-escaped `{`, so that shouldn't be a problem.

---

_@charliermarsh approved on 2024-04-23 18:47_

---

_Label `performance` added by @charliermarsh on 2024-04-23 18:47_

---

_Merged by @ibraheemdev on 2024-04-24 14:25_

---

_Closed by @ibraheemdev on 2024-04-24 14:25_

---
