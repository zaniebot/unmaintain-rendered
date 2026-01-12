```yaml
number: 8240
title: "Use `SourceKind::diff` for formatter"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - formatter
assignees: []
merged: true
base: main
head: dhruv/formatter-diff
created_at: 2023-10-26T03:45:51Z
updated_at: 2023-10-26T07:18:56Z
url: https://github.com/astral-sh/ruff/pull/8240
synced_at: 2026-01-12T02:32:42Z
```

# Use `SourceKind::diff` for formatter

---

_Pull request opened by @dhruvmanila on 2023-10-26 03:45_

## Summary

This PR refactors the formatter diff code to reuse the `SourceKind::diff` logic. This has the benefit that the Notebook diff now includes the cell numbers which was not present before.

## Test Plan

Update the snapshots and verified the cell numbers.


---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/source_kind.rs`:162 on 2023-10-26 03:47_

We could potentially create a new `SourceError::IncompatibleDiffSource` variant but then the `Diagnostics:from_source_error` needs to be updated which I don't think is correct because the diff error shouldn't really be a diagnostic.

---

_@dhruvmanila reviewed on 2023-10-26 03:47_

---

_Review requested from @konstin by @dhruvmanila on 2023-10-26 03:48_

---

_Label `formatter` added by @dhruvmanila on 2023-10-26 03:48_

---

_Comment by @github-actions[bot] on 2023-10-26 04:01_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -0, 1 error(s))

<details><summary>build (error)</summary>
https://github.com/pypa/build ref main select  ignore  exclude 
<p>

```
ruff failed
  Cause: TOML parse error at line 2, column 19
  |
2 | requires = ['bad' 'syntax']
  |                   ^
invalid array
expected `]`


```

</p>
</details>



---

_@charliermarsh reviewed on 2023-10-26 04:05_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/source_kind.rs`:162 on 2023-10-26 04:05_

I really wish we had a way to enforce this statically through types (i.e., that this path should never be reached).

---

_@charliermarsh approved on 2023-10-26 04:05_

---

_@MichaReiser approved on 2023-10-26 05:02_

---

_Merged by @dhruvmanila on 2023-10-26 05:38_

---

_Closed by @dhruvmanila on 2023-10-26 05:38_

---

_Branch deleted on 2023-10-26 05:38_

---

_Comment by @konstin on 2023-10-26 07:18_

thanks!

---
