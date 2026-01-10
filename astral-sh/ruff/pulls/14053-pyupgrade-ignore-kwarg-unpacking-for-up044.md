```yaml
number: 14053
title: "[`pyupgrade`] - ignore kwarg unpacking for `UP044`"
type: pull_request
state: merged
author: diceroll123
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-up044
created_at: 2024-11-01T22:26:49Z
updated_at: 2024-11-02T17:20:16Z
url: https://github.com/astral-sh/ruff/pull/14053
synced_at: 2026-01-10T20:59:37Z
```

# [`pyupgrade`] - ignore kwarg unpacking for `UP044`

---

_Pull request opened by @diceroll123 on 2024-11-01 22:26_

## Summary

Fixes #14047 

## Test Plan

`catgo test`

---

_Comment by @github-actions[bot] on 2024-11-01 22:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -18 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -18 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/cache.py#L73'>rotkehlchen/db/cache.py:73:36:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/cache.py#L77'>rotkehlchen/db/cache.py:77:36:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/cache.py#L81'>rotkehlchen/db/cache.py:81:36:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/cache.py#L85'>rotkehlchen/db/cache.py:85:36:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L699'>rotkehlchen/db/dbhandler.py:699:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L708'>rotkehlchen/db/dbhandler.py:708:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L717'>rotkehlchen/db/dbhandler.py:717:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L726'>rotkehlchen/db/dbhandler.py:726:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L735'>rotkehlchen/db/dbhandler.py:735:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L744'>rotkehlchen/db/dbhandler.py:744:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L753'>rotkehlchen/db/dbhandler.py:753:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L776'>rotkehlchen/db/dbhandler.py:776:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L786'>rotkehlchen/db/dbhandler.py:786:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L796'>rotkehlchen/db/dbhandler.py:796:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L806'>rotkehlchen/db/dbhandler.py:806:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L816'>rotkehlchen/db/dbhandler.py:816:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L826'>rotkehlchen/db/dbhandler.py:826:23:</a> UP044 [*] Use `*` for unpacking
- <a href='https://github.com/rotki/rotki/blob/2d190b227b73d69145c9943588b29897c464867f/rotkehlchen/db/dbhandler.py#L836'>rotkehlchen/db/dbhandler.py:836:23:</a> UP044 [*] Use `*` for unpacking
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP044 | 18 | 0 | 18 | 0 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2024-11-02 16:59_

---

_Label `bug` added by @charliermarsh on 2024-11-02 16:59_

---

_Merged by @charliermarsh on 2024-11-02 17:10_

---

_Closed by @charliermarsh on 2024-11-02 17:10_

---

_Comment by @MichaReiser on 2024-11-02 17:20_

Hmm, I should have spotted this in the ecosystem checks. Thanks for fixing this so quickly!

---
