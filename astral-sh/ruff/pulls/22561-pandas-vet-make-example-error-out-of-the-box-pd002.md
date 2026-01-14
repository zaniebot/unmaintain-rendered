```yaml
number: 22561
title: "[`pandas-vet`] Make example error out-of-the-box (`PD002`)"
type: pull_request
state: open
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
base: main
head: patch-1
created_at: 2026-01-13T21:16:38Z
updated_at: 2026-01-14T08:50:07Z
url: https://github.com/astral-sh/ruff/pull/22561
synced_at: 2026-01-14T09:35:09Z
```

# [`pandas-vet`] Make example error out-of-the-box (`PD002`)

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [pandas-use-of-inplace-argument (PD002)](https://docs.astral.sh/ruff/rules/pandas-use-of-inplace-argument/#pandas-use-of-inplace-argument-pd002)'s example error out-of-the-box.

[Old example](https://play.ruff.rs/1f2afc4b-3b6e-4bbc-8292-20f457ff2280)
```py
df.sort_values("col1", inplace=True)
```

[New example](https://play.ruff.rs/badb2eb4-aa23-4e07-9ea4-18bbc58a2edb)
```py
import pandas as pd

students_df = pd.read_csv("students.csv")
students_df.sort_values("name", inplace=True)
```

The "Use instead" section was also updated to emphasize the benefits gained from being able to use the method chaining style.

```py
import pandas as pd

students = pd.read_csv("students.csv").sort_values("name")
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @astral-sh-bot[bot] on 2026-01-13 21:32_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:30 on 2026-01-13 22:53_

I'd prefer names that are more generic

```suggestion
/// data = pd.read_csv("data.csv")
/// data.sort_values("column", inplace=True)
```

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:37 on 2026-01-13 22:53_

```suggestion
/// data = pd.read_csv("data.csv").sort_values("column")
```

---

_@amyreese reviewed on 2026-01-13 22:53_

---

_@MeGaGiGaGon reviewed on 2026-01-13 23:54_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:30 on 2026-01-13 23:54_

I chose to leave the name since that matches how some of the other examples in pandas-vet are:
https://docs.astral.sh/ruff/rules/pandas-use-of-dot-is-null/
```py
import pandas as pd

animals_df = pd.read_csv("animals.csv")
pd.isnull(animals_df)
```
https://docs.astral.sh/ruff/rules/pandas-use-of-dot-ix/ (The one I copied from)
```py
import pandas as pd

students_df = pd.read_csv("students.csv")
students_df.ix[0]  # 0th row or row with label 0?
```

---

_Label `documentation` added by @MichaReiser on 2026-01-14 08:50_

---
