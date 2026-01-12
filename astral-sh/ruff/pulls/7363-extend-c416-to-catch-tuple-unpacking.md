```yaml
number: 7363
title: Extend C416 to catch tuple unpacking
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/C416
created_at: 2023-09-13T19:08:24Z
updated_at: 2023-09-14T15:59:24Z
url: https://github.com/astral-sh/ruff/pull/7363
synced_at: 2026-01-12T02:39:09Z
```

# Extend C416 to catch tuple unpacking

---

_Pull request opened by @charliermarsh on 2023-09-13 19:08_

Closes https://github.com/astral-sh/ruff/issues/7307.

---

_Label `bug` added by @charliermarsh on 2023-09-13 19:08_

---

_@zanieb approved on 2023-09-13 23:30_

---

_Comment by @codspeed-hq[bot] on 2023-09-14 15:51_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/C416)

### Merging #7363 will **degrade performances by 2.48%**

<sub>Comparing <code>charlie/C416</code> (597c36b) with <code>main</code> (ec2f229)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/C416)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/C416` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[large/dataset.py]` | 161.1 ms | 165.2 ms | -2.48% |


---

_Merged by @charliermarsh on 2023-09-14 15:53_

---

_Closed by @charliermarsh on 2023-09-14 15:53_

---

_Branch deleted on 2023-09-14 15:53_

---

_Comment by @github-actions[bot] on 2023-09-14 15:59_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+4, -0, 0 error(s))

<details><summary>openpilot (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/6666413626d72b51759ce0330b9c02dc3ebf8fc9/selfdrive/car/tests/test_can_fingerprint.py#L11'>selfdrive/car/tests/test_can_fingerprint.py:11:25:</a> C416 [*] Unnecessary `list` comprehension (rewrite using `list()`)
</pre>

</p>
</details>
<details><summary>content (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/f50398cb0796460af4f7296f8b1e0d320c2a4a29/Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py#L34'>Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py:34:22:</a> C416 [*] Unnecessary `list` comprehension (rewrite using `list()`)
</pre>

</p>
</details>
<details><summary>zulip (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/0e73b5547c1000cc452b22fd1d75ef9cc0bc061f/zerver/forms.py#L167'>zerver/forms.py:167:17:</a> C416 [*] Unnecessary `list` comprehension (rewrite using `list()`)
+ <a href='https://github.com/zulip/zulip/blob/0e73b5547c1000cc452b22fd1d75ef9cc0bc061f/zerver/forms.py#L212'>zerver/forms.py:212:17:</a> C416 [*] Unnecessary `list` comprehension (rewrite using `list()`)
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| C416 | 4 | 4 | 0 |



---
