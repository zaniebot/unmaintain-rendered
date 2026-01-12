```yaml
number: 11469
title: Fix division by zero error in ecosystem check
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/eco-fix
created_at: 2024-05-19T03:57:15Z
updated_at: 2024-05-19T14:08:11Z
url: https://github.com/astral-sh/ruff/pull/11469
synced_at: 2026-01-12T15:55:38Z
```

# Fix division by zero error in ecosystem check

---

_@zanieb_

e.g. https://github.com/astral-sh/ruff/actions/runs/9144809516/job/25143076896?pr=11468

<img width="1388" alt="Screenshot 2024-05-19 at 12 02 15 AM" src="https://github.com/astral-sh/ruff/assets/2586601/0df7cbcd-712c-4ea9-96f5-73f871570525">


---

_Label `internal` added by @zanieb on 2024-05-19 04:01_

---

_Comment by @codspeed-hq[bot] on 2024-05-19 04:03_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zb/eco-fix)

### Merging #11469 will **not alter performance**

<sub>Comparing <code>zb/eco-fix</code> (4f29594) with <code>main</code> (d9ec3d5)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-05-19 04:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/repositories/test_legacy_repository.py#L386'>tests/repositories/test_legacy_repository.py:386:39:</a> E999 SyntaxError: Got unexpected string
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @zanieb on 2024-05-19 13:09_

Looks like this is actually a bug in reporting clone failures per https://github.com/astral-sh/ruff/pull/11468/commits/e6ba39e905de69fbebce8c4dd686f5aec58de07e fixing.

Regardless this seems like an improvement for now?

---

_@charliermarsh approved on 2024-05-19 14:02_

---

_Merged by @zanieb on 2024-05-19 14:08_

---

_Closed by @zanieb on 2024-05-19 14:08_

---

_Branch deleted on 2024-05-19 14:08_

---
