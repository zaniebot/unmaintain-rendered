```yaml
number: 6559
title: "Formatter ecosystem checks: Use twine instead of build"
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: twine-instead-of-build
created_at: 2023-08-14T13:19:28Z
updated_at: 2023-08-14T20:40:58Z
url: https://github.com/astral-sh/ruff/pull/6559
synced_at: 2026-01-12T15:55:21Z
```

# Formatter ecosystem checks: Use twine instead of build

---

_@konstin_

`build` uses `skip-string-normalization: true` which we don't support. Instead, we replace it with twine, which is also a small util library.

tokei stats for build:
```
===============================================================================
 Language            Files        Lines         Code     Comments       Blanks
===============================================================================
 INI                     1          106           98            0            8
 Markdown                1           38            0           25           13
 Python                 25         3214         2437          134          643
 ReStructuredText        8          541          375            0          166
 Plain Text              3           10            0           10            0
 TOML                   16          231          211            1           19
 YAML                    1            6            6            0            0
===============================================================================
 Total                  55         4146         3127          170          849
===============================================================================
```
tokei stats for twine:
```
===============================================================================
 Language            Files        Lines         Code     Comments       Blanks
===============================================================================
 INI                     3          166          139           11           16
 Python                 32         5649         4046          592         1011
 ReStructuredText       20         1298          888            0          410
 Plain Text              1            5            0            5            0
 TOML                    1           13           10            1            2
===============================================================================
 Total                  57         7131         5083          609         1439
===============================================================================
```

---

_Label `internal` added by @konstin on 2023-08-14 13:19_

---

_Label `formatter` added by @konstin on 2023-08-14 13:19_

---

_@charliermarsh approved on 2023-08-14 13:20_

---

_Comment by @github-actions[bot] on 2023-08-14 13:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Assigned to @MichaReiser by @konstin on 2023-08-14 18:27_

---

_Comment by @konstin on 2023-08-14 18:27_

@MichaReiser feel free to merge

---

_Review requested from @MichaReiser by @konstin on 2023-08-14 18:27_

---

_Merged by @MichaReiser on 2023-08-14 20:40_

---

_Closed by @MichaReiser on 2023-08-14 20:40_

---

_Branch deleted on 2023-08-14 20:40_

---
