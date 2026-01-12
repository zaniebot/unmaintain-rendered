```yaml
number: 17404
title: "[red-knot] Do not merge: Run ecosystem checks with 3.9"
type: pull_request
state: closed
author: sharkdp
labels:
  - ci
  - do-not-merge
  - ty
assignees: []
draft: true
base: main
head: david/do-not-merge-requires-python-test
created_at: 2025-04-15T07:19:57Z
updated_at: 2025-05-19T18:07:07Z
url: https://github.com/astral-sh/ruff/pull/17404
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Do not merge: Run ecosystem checks with 3.9

---

_@sharkdp_

## Summary

Just a stupid way to run the ecosystem checks with 3.9 instead of 3.7 (see https://github.com/astral-sh/ruff/pull/17402#issuecomment-2804064495 for details). We obviously want to fix this in another way, but this should allow us to see the mypy_primer diff.

---

_Label `ci` added by @sharkdp on 2025-04-15 07:19_

---

_Label `do-not-merge` added by @sharkdp on 2025-04-15 07:19_

---

_Label `red-knot` added by @sharkdp on 2025-04-15 07:19_

---

_Comment by @github-actions[bot] on 2025-04-15 07:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyp (https://github.com/hauntsaninja/pyp)
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/pyp/pyp.py:606:28: Unused blanket `type: ignore` directive
- Found 19 diagnostics
+ Found 20 diagnostics

```
</details>


---

_Closed by @sharkdp on 2025-05-19 18:07_

---
