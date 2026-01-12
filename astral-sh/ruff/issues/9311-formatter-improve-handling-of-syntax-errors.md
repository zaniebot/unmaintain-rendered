```yaml
number: 9311
title: "Formatter: Improve handling of syntax errors"
type: issue
state: closed
author: zanieb
labels:
  - formatter
assignees: []
created_at: 2023-12-29T17:26:12Z
updated_at: 2023-12-31T12:10:47Z
url: https://github.com/astral-sh/ruff/issues/9311
synced_at: 2026-01-12T15:54:49Z
```

# Formatter: Improve handling of syntax errors

---

_@zanieb_

The current user-facing messaging for syntax errors encountered during formatting are not friendly and are significantly worse than the linter's. The error should be normalized into a message instead of using the debug representation of `ParseError`

e.g.

Line number not included ([existing issue](https://github.com/astral-sh/ruff/issues/8338)):

`error: Failed to format commands/liberrors.py: source contains syntax errors: ParseError { error: UnrecognizedToken(Name { name: "THE_INVALID_FILE" }, None), offset: 208, source_path: "<filename>" } 157 files left unchanged`

Cell number not included in notebook errors

`error: Failed to format course/videos/domain_adaptation.ipynb: source contains syntax errors: ParseError { error: UnrecognizedToken(Rpar, None), offset: 70, source_path: "<filename>" }`

The previous error is much nicer via the linter

`error: Failed to parse course/videos/domain_adaptation.ipynb:cell 9:1:71: Unexpected token ')'`


---

_Label `formatter` added by @zanieb on 2023-12-29 17:26_

---

_Added to milestone `Formatter: Stable` by @zanieb on 2023-12-29 17:26_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-29 20:23_

---

_Comment by @charliermarsh on 2023-12-29 20:23_

I can fix these.

---

_Comment by @zanieb on 2023-12-29 20:28_

It'd also be nice to exit with a specific code if we only fail due to syntax errors rather than some sort of other failure :) that way we can report it nicely in the ecosystem checks instead of treating it as a failed run of Ruff.

---

_Closed by @charliermarsh on 2023-12-31 12:10_

---
