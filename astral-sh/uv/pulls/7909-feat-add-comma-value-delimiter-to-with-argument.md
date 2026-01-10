```yaml
number: 7909
title: "feat: add comma value-delimiter to with argument in tool run args to allow for multiple arguments in with flag"
type: pull_request
state: merged
author: noamteyssier
labels:
  - cli
assignees: []
merged: true
base: main
head: main
created_at: 2024-10-03T22:21:22Z
updated_at: 2024-10-11T09:19:57Z
url: https://github.com/astral-sh/uv/pull/7909
synced_at: 2026-01-10T12:53:59Z
```

# feat: add comma value-delimiter to with argument in tool run args to allow for multiple arguments in with flag

---

_Pull request opened by @noamteyssier on 2024-10-03 22:21_

This is to address my own issue #7908 

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This change makes use of the `clap` value_delimiter parser to populate the `with` `Vec<String>` which currently can either only be empty or with 1 value for each `--with` flag.

This makes use of the current code structure but allows for multiple arguments with a single `--with` flag.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Can be tested with the following CLI:

```bash
target/debug/uv tool run --with numpy,polars,matplotlib ipython -c "import numpy;import polars;import matplotlib;"
```

And former behavior of multiple `--with` flags are kept

```bash
target/debug/uv tool run --with numpy --with polars --with matplotlib ipython -c "import numpy;import polars;import matplotlib;"
```

<!-- How was it tested? -->


---

_Comment by @noamteyssier on 2024-10-03 22:46_

Also added testing of CSV `--with` flag (i.e. `--with mod1,mod2`) and repeated `--with` flag (i.e. `--with mod1 --with mod2`) to keep current behavior

Would be happy to adjust testing scheme to minimize the elapsed time if you have ideas. 

---

_@charliermarsh approved on 2024-10-11 09:12_

Thanks, this seems reasonable to me.

---

_Label `cli` added by @charliermarsh on 2024-10-11 09:12_

---

_Merged by @charliermarsh on 2024-10-11 09:19_

---

_Closed by @charliermarsh on 2024-10-11 09:19_

---
