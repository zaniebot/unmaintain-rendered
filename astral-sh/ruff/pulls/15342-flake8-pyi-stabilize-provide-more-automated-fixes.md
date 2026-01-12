```yaml
number: 15342
title: "[`flake8-pyi`]: Stabilize: Provide more automated fixes for `duplicate-union-members` (`PYI016`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - fixes
assignees: []
merged: true
base: ruff-0.9
head: micha/pyi016-fix
created_at: 2025-01-08T09:41:50Z
updated_at: 2025-01-08T12:13:26Z
url: https://github.com/astral-sh/ruff/pull/15342
synced_at: 2026-01-12T15:55:51Z
```

# [`flake8-pyi`]: Stabilize: Provide more automated fixes for `duplicate-union-members` (`PYI016`)

---

_@MichaReiser_

## Summary

Stabilizes the fix extension introduced in https://github.com/astral-sh/ruff/pull/14270



## Test Plan

There are no open issues related to `PYI016` fixes. The most recent issue was fixed on Nov 25. There are also no open issues with the rule itself.

I reviewed the ecosystem changes and they look good.


---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-08 09:41_

---

_Label `rule` added by @MichaReiser on 2025-01-08 09:41_

---

_Label `rule` removed by @MichaReiser on 2025-01-08 09:42_

---

_Label `fixes` added by @MichaReiser on 2025-01-08 09:42_

---

_Renamed from "[`flake8-pyi`]: Stabilize: Provide more automated fixes for `duplicate-union-members` (PYI016)" to "[`flake8-pyi`]: Stabilize: Provide more automated fixes for `duplicate-union-members` (`PYI016`)" by @MichaReiser on 2025-01-08 09:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:96 on 2025-01-08 09:45_

It seems like https://github.com/astral-sh/ruff/pull/14270 incorrectly removed the fix for non-preview 🤷 So, technically, this PR makes the rule fixable again

---

_@MichaReiser reviewed on 2025-01-08 09:45_

---

_Comment by @github-actions[bot] on 2025-01-08 09:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +10 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/providers/src/airflow/providers/microsoft/azure/operators/container_instances.py#L254'>providers/src/airflow/providers/microsoft/azure/operators/container_instances.py:254:43:</a> PYI016 Duplicate union member `VolumeMount`
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/providers/src/airflow/providers/microsoft/azure/operators/container_instances.py#L254'>providers/src/airflow/providers/microsoft/azure/operators/container_instances.py:254:43:</a> PYI016 [*] Duplicate union member `VolumeMount`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/models/helpers.py#L905'>superset/models/helpers.py:905:39:</a> PYI016 Duplicate union member `str`
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/models/helpers.py#L905'>superset/models/helpers.py:905:39:</a> PYI016 [*] Duplicate union member `str`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L51'>src/bokeh/core/property/factors.py:51:56:</a> PYI016 Duplicate union member `tuple[str, str]`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L51'>src/bokeh/core/property/factors.py:51:56:</a> PYI016 [*] Duplicate union member `tuple[str, str]`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L52'>src/bokeh/core/property/factors.py:52:85:</a> PYI016 Duplicate union member `tp.Sequence[tuple[str, str]]`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L52'>src/bokeh/core/property/factors.py:52:85:</a> PYI016 [*] Duplicate union member `tp.Sequence[tuple[str, str]]`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L43'>src/bokeh/plotting/contour.py:43:88:</a> PYI016 Duplicate union member `ContourColor`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L43'>src/bokeh/plotting/contour.py:43:88:</a> PYI016 [*] Duplicate union member `ContourColor`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI016 | 10 | 0 | 0 | 10 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Added to milestone `v0.9` by @MichaReiser on 2025-01-08 10:29_

---

_@AlexWaygood reviewed on 2025-01-08 10:43_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:96 on 2025-01-08 10:43_

hehe, onwards and upwards!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI016.py`:116 on 2025-01-08 10:48_

I'm not sure I'd want to extend the rule to detect this, since:
- It seems like an unlikely edge case
- It will fail at runtime with `TypeError` unless you have `from __future__ import annotations` at the top of the file (and if you do, there's no need to quote any annotations anyway)
- In a stub file it won't fail, but you shouldn't ever use quoted annotations in a stub file anyway, and we have PYI020 to tell you off about that if you do

So I don't think it would be worth the added complexity

---

_@AlexWaygood reviewed on 2025-01-08 10:48_

---

_@AlexWaygood approved on 2025-01-08 10:54_

All LGTM other than https://github.com/astral-sh/ruff/pull/15342/files#r1906988995.

Thanks so much @sbrugman for this improvement. Changes like this make me really happy. It's still so cool that Ruff can provide sophisticated autofixes like this 😃

---

_Merged by @MichaReiser on 2025-01-08 12:13_

---

_Closed by @MichaReiser on 2025-01-08 12:13_

---

_Branch deleted on 2025-01-08 12:13_

---
