```yaml
number: 8316
title: "Allow selective caching for `--fix` and `--diff`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/cache
created_at: 2023-10-28T22:00:02Z
updated_at: 2023-10-29T16:22:37Z
url: https://github.com/astral-sh/ruff/pull/8316
synced_at: 2026-01-12T02:11:58Z
```

# Allow selective caching for `--fix` and `--diff`

---

_Pull request opened by @charliermarsh on 2023-10-28 22:00_

## Summary

If a file has no diagnostics, then we can read and write that information from and to the cache, even if the fix mode is `--fix` or `--diff`. (Typically, we can't read or write such results from or to the cache, because `--fix` and `--diff` have side effects that take place during diagnostic analysis (writing to disk or outputting the diff).) This greatly improves performance when running `--fix` on a codebase in the common case (few diagnostics).

Closes #8311.
Closes https://github.com/astral-sh/ruff/issues/8315.

---

_Label `performance` added by @charliermarsh on 2023-10-28 22:00_

---

_Comment by @github-actions[bot] on 2023-10-28 22:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.




---

_Review requested from @zanieb by @charliermarsh on 2023-10-28 22:35_

---

_@zanieb approved on 2023-10-28 23:41_

Nice. Should we retain a note about why we don't always cache?

---

_Merged by @charliermarsh on 2023-10-29 16:06_

---

_Closed by @charliermarsh on 2023-10-29 16:06_

---

_Branch deleted on 2023-10-29 16:06_

---

_Comment by @lukaspiatkowski on 2023-10-29 16:16_

I can confirm this solves #8311, second run of `ruff --fix` took 0.13s on my repo as expected.

---

_Comment by @charliermarsh on 2023-10-29 16:22_

Nice, thanks for testing on `main`! Appreciate it!

---

_Comment by @charliermarsh on 2023-10-29 16:22_

I'd expect `ruff --fix` to still be much slower if most files have diagnostics, since we only skip the cache for files with no errors, but in practice that's very rare in real-world projects.

---
