```yaml
number: 1995
title: Added pylint formatter
type: pull_request
state: merged
author: damienallen
labels: []
assignees: []
merged: true
base: main
head: pylint-format
created_at: 2023-01-19T11:33:04Z
updated_at: 2023-01-19T14:15:09Z
url: https://github.com/astral-sh/ruff/pull/1995
synced_at: 2026-01-12T05:25:13Z
```

# Added pylint formatter

---

_Pull request opened by @damienallen on 2023-01-19 11:33_

Fixes: #1953

@charliermarsh thank you for the tips in the issue.

I'm not very familiar with Rust, so please excuse if my string formatting syntax is messy.

In terms of testing, I compared output of `flake8 --format=pylint ` and `cargo run --format=pylint` on the same code and the output syntax seems to check out.


---

_Review comment by @messense on `README.md`:2125 on 2023-01-19 12:32_

```suggestion
(GitLab CI code quality report) or `"pylint"` (Pylint text format).
```

I don't think the backslashes are needed.

<img width="395" alt="image" src="https://user-images.githubusercontent.com/1556054/213443817-02dfbe47-08e0-448d-90fc-72d3c61e4f9f.png">


---

_@messense reviewed on 2023-01-19 12:32_

---

_Merged by @charliermarsh on 2023-01-19 13:01_

---

_Closed by @charliermarsh on 2023-01-19 13:01_

---

_Comment by @charliermarsh on 2023-01-19 13:01_

Looks great, thank you for putting this together!

---

_@charliermarsh reviewed on 2023-01-19 13:02_

---

_Review comment by @charliermarsh on `README.md`:2125 on 2023-01-19 13:02_

Just for the future: all these references are auto-generated from the documentation, so this needed to be changed in `src/settings/options.rs` (followed by an invocation of `cargo dev generate-all`).

---

_@charliermarsh reviewed on 2023-01-19 13:02_

---

_Review comment by @charliermarsh on `ruff_cli/src/printer.rs`:320 on 2023-01-19 13:02_

Tweaked this to put some of the plain-text characters in the format string directly.

---

_Review comment by @damienallen on `README.md`:2125 on 2023-01-19 14:12_

Ah, good to know!

---

_@damienallen reviewed on 2023-01-19 14:13_

---

_Review comment by @damienallen on `ruff_cli/src/printer.rs`:320 on 2023-01-19 14:14_

I was unsure about this but this is more what I would expect.

---

_@damienallen reviewed on 2023-01-19 14:14_

---

_Comment by @damienallen on 2023-01-19 14:15_

> Looks great, thank you for putting this together!

Cheers, thanks for the merge!

---
