```yaml
number: 15732
title: "[`pylint`] Do not trigger `PLR6201` on empty collections"
type: pull_request
state: merged
author: JelleZijlstra
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: plr6201-empty
created_at: 2025-01-24T20:41:10Z
updated_at: 2025-01-25T03:46:15Z
url: https://github.com/astral-sh/ruff/pull/15732
synced_at: 2026-01-12T15:55:52Z
```

# [`pylint`] Do not trigger `PLR6201` on empty collections

---

_@JelleZijlstra_

Fixes #15729.

---

_Comment by @github-actions[bot] on 2025-01-24 20:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/6586d9078ea4c12187cef6dba8a4a801a2d89b0b/tools/lib/provision.py#L152'>tools/lib/provision.py:152:42:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/zulip/zulip/blob/6586d9078ea4c12187cef6dba8a4a801a2d89b0b/tools/lib/provision.py#L152'>tools/lib/provision.py:152:87:</a> PLR6201 Use a set literal when testing for membership
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6201 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @ntBre on 2025-01-24 21:45_

---

_Label `fixes` added by @ntBre on 2025-01-24 21:45_

---

_@dylwil3 approved on 2025-01-25 02:41_

LGTM! Thank you!

I also agree with you that if someone writes `1 in []` it's probably an error, but this isn't the right rule to describe the error...

---

_Renamed from "Do not trigger PLR6201 on empty collections" to "[`pylint`] Do not trigger `PLR6201` on empty collections" by @dylwil3 on 2025-01-25 02:41_

---

_Label `preview` added by @dylwil3 on 2025-01-25 02:41_

---

_Label `fixes` removed by @dylwil3 on 2025-01-25 02:42_

---

_Label `rule` added by @dylwil3 on 2025-01-25 02:42_

---

_Merged by @dylwil3 on 2025-01-25 02:42_

---

_Closed by @dylwil3 on 2025-01-25 02:42_

---

_Branch deleted on 2025-01-25 03:46_

---
