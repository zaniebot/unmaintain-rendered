```yaml
number: 8298
title: "[pylint] Implement import-outside-toplevel (C0415)"
type: pull_request
state: closed
author: dankleeman
labels:
  - rule
assignees: []
base: main
head: import-outside-toplevel
created_at: 2023-10-28T08:34:40Z
updated_at: 2023-10-29T19:20:12Z
url: https://github.com/astral-sh/ruff/pull/8298
synced_at: 2026-01-12T02:11:58Z
```

# [pylint] Implement import-outside-toplevel (C0415)

---

_Pull request opened by @dankleeman on 2023-10-28 08:34_

## Summary
Per #970 :
Implement pylint rule C0415: import-outside-toplevel

https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/import-outside-toplevel.html

## Test Plan

With cargo test following the contribution instructions.


---

_Comment by @github-actions[bot] on 2023-10-28 08:48_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/47e1e4bbc9d65c4587a833889756b2e459c2129d/tools/pyinstaller_hooks/pre_find_module_path/hook-distutils.py#L40'>tools/pyinstaller_hooks/pre_find_module_path/hook-distutils.py:40:5:</a> PLC0415 Import outside toplevel (opcode)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

</p>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0415 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @charliermarsh by @charliermarsh on 2023-10-28 22:44_

---

_Label `rule` added by @charliermarsh on 2023-10-28 22:45_

---

_Comment by @charliermarsh on 2023-10-29 15:55_

Ugh, so sorry -- thank you for getting involved but unfortunately there's already an open PR for this rule, which I've just been slow to merge (https://github.com/astral-sh/ruff/pull/5180). Is there any way I can help you pick out another rule to work on?

---

_Closed by @charliermarsh on 2023-10-29 15:55_

---

_Comment by @dankleeman on 2023-10-29 18:08_

> Ugh, so sorry -- thank you for getting involved but unfortunately there's already an open PR for this rule, which I've just been slow to merge (#5180). Is there any way I can help you pick out another rule to work on?

No worries! I'd love to work on other rules. I'd happily take a shot at any rule you'd suggest.

I think my only concern picking another one on my own is the discussion on #970 that mentions not wanting to implement pylint rules that are covered by mypy: https://github.com/astral-sh/ruff/issues/970#issuecomment-1783147905



---

_Comment by @charliermarsh on 2023-10-29 19:20_

Awesome! Here are a few that look reasonable to me:

- `bad-staticmethod-argument`
- `unnecessary-dunder-call`
- `too-many-locals`

Separately: @zanieb was actually planning to go through and move all the Mypy-overlapping rules to a separate section to avoid this issue in the future.


---
