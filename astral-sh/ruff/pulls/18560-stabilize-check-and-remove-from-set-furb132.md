```yaml
number: 18560
title: "Stabilize `check-and-remove-from-set` (`FURB132`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-furb132
created_at: 2025-06-08T19:10:41Z
updated_at: 2025-06-09T19:03:55Z
url: https://github.com/astral-sh/ruff/pull/18560
synced_at: 2026-01-10T18:45:04Z
```

# Stabilize `check-and-remove-from-set` (`FURB132`)

---

_Pull request opened by @dylwil3 on 2025-06-08 19:10_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:10_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:10_

---

_Comment by @github-actions[bot] on 2025-06-08 19:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/models/helpers.py#L381'>superset/models/helpers.py:381:13:</a> FURB132 Use `export_fields.discard("id")` instead of check and `remove`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_gallery.py#L142'>src/bokeh/sphinxext/bokeh_gallery.py:142:9:</a> FURB132 Use `extras.discard(detail_file_path)` instead of check and `remove`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB132 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:29_

---

_@ntBre approved on 2025-06-09 18:30_

---

_Merged by @dylwil3 on 2025-06-09 19:03_

---

_Closed by @dylwil3 on 2025-06-09 19:03_

---

_Branch deleted on 2025-06-09 19:03_

---
