```yaml
number: 20231
title: "Stabilize `in-empty-collection` (`RUF060`)"
type: pull_request
state: closed
author: ntBre
labels:
  - rule
assignees: []
base: brent/0.13.0
head: brent/ruf060
created_at: 2025-09-04T13:33:36Z
updated_at: 2025-09-04T20:17:44Z
url: https://github.com/astral-sh/ruff/pull/20231
synced_at: 2026-01-10T17:46:21Z
```

# Stabilize `in-empty-collection` (`RUF060`)

---

_Pull request opened by @ntBre on 2025-09-04 13:33_

Tests look good, just two very small comma changes to the docs


---

_Added to milestone `v0.13` by @ntBre on 2025-09-04 13:33_

---

_Label `rule` added by @ntBre on 2025-09-04 13:33_

---

_Comment by @github-actions[bot] on 2025-09-04 13:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b4e06a6fa01b81e53e33c0d9f458bebde12ac5bc/tools/lib/provision.py#L155'>tools/lib/provision.py:155:77:</a> RUF060 Unnecessary membership test on empty collection
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/bf92debb4fd874289f6aed29c6947b611b1de0ae/testing/test_assertrewrite.py#L630'>testing/test_assertrewrite.py:630:20:</a> RUF060 Unnecessary membership test on empty collection
+ <a href='https://github.com/pytest-dev/pytest/blob/bf92debb4fd874289f6aed29c6947b611b1de0ae/testing/test_assertrewrite.py#L630'>testing/test_assertrewrite.py:630:32:</a> RUF060 Unnecessary membership test on empty collection
+ <a href='https://github.com/pytest-dev/pytest/blob/bf92debb4fd874289f6aed29c6947b611b1de0ae/testing/test_assertrewrite.py#L637'>testing/test_assertrewrite.py:637:39:</a> RUF060 Unnecessary membership test on empty collection
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF060 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-09-04 14:35_

---

_Review requested from @dylwil3 by @ntBre on 2025-09-04 14:35_

---

_Comment by @ntBre on 2025-09-04 17:37_

https://github.com/astral-sh/ruff/issues/20238

---

_Closed by @ntBre on 2025-09-04 17:37_

---

_Comment by @dylwil3 on 2025-09-04 20:14_

I'd be okay with stabilizing once #20249 merges

---

_Comment by @ntBre on 2025-09-04 20:17_

I usually consider any "recent" bug report and fix to be a blocker, but this one does seem fairly minor :shrug: I'm happy either way but probably lean toward leaving it in preview, it's only 120 days old anyway.

---
