```yaml
number: 14236
title: Detect empty implicit namespace packages
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/nested
created_at: 2024-11-10T01:26:53Z
updated_at: 2024-11-12T11:07:47Z
url: https://github.com/astral-sh/ruff/pull/14236
synced_at: 2026-01-12T15:55:47Z
```

# Detect empty implicit namespace packages

---

_@charliermarsh_

## Summary

The implicit namespace package rule currently fails to detect cases like the following:

```text
foo/
├── __init__.py
└── bar/
    └── baz/
        └── __init__.py
```

The problem is that we detect a root at `foo`, and then an independent root at `baz`. We _would_ detect that `bar` is an implicit namespace package, but it doesn't contain any files! So we never check it, and have no place to raise the diagnostic.

This PR adds detection for these kinds of nested packages, and augments the `INP` rule to flag the `__init__.py` file above with a specialized message. As a side effect, I've introduced a dedicated `PackageRoot` struct which we can pass around in lieu of Yet Another `Path`.

For now, I'm only enabling this in preview (and the approach doesn't affect any other rules). It's a bug fix, but it may end up expanding the rule.

Closes https://github.com/astral-sh/ruff/issues/13519.


---

_Label `bug` added by @charliermarsh on 2024-11-10 01:29_

---

_Label `preview` added by @charliermarsh on 2024-11-10 01:29_

---

_Comment by @github-actions[bot] on 2024-11-10 01:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/embed/ext_package_no_main/__init__.py#L1'>tests/unit/bokeh/embed/ext_package_no_main/__init__.py:1:1:</a> INP001 File `tests/unit/bokeh/embed/ext_package_no_main/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `tests/unit/bokeh/embed`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/embed/latex_label/__init__.py#L1'>tests/unit/bokeh/embed/latex_label/__init__.py:1:1:</a> INP001 File `tests/unit/bokeh/embed/latex_label/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `tests/unit/bokeh/embed`.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| INP001 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-11-10 03:03_

---

_Closed by @charliermarsh on 2024-11-10 03:03_

---

_Branch deleted on 2024-11-10 03:03_

---

_@MichaReiser reviewed on 2024-11-12 11:07_

---

_Review comment by @MichaReiser on `bar/baz.py`:1 on 2024-11-12 11:07_

Is it intentional that this file is commited?

---
