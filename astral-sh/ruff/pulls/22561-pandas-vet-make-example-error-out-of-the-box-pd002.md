```yaml
number: 22561
title: "[`pandas-vet`] Make example error out-of-the-box (`PD002`)"
type: pull_request
state: open
author: MeGaGiGaGon
labels: []
assignees: []
base: main
head: patch-1
created_at: 2026-01-13T21:16:38Z
updated_at: 2026-01-13T21:16:38Z
url: https://github.com/astral-sh/ruff/pull/22561
synced_at: 2026-01-13T21:36:29Z
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
