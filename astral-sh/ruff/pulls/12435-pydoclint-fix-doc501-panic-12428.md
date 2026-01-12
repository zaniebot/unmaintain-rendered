```yaml
number: 12435
title: "[`pydoclint`] Fix `DOC501` panic #12428"
type: pull_request
state: merged
author: augustelalande
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: doc501
created_at: 2024-07-21T18:05:05Z
updated_at: 2024-07-21T19:52:29Z
url: https://github.com/astral-sh/ruff/pull/12435
synced_at: 2026-01-12T15:55:41Z
```

# [`pydoclint`] Fix `DOC501` panic #12428

---

_@augustelalande_

## Summary

Fix panic reported in #12428. Where a string would sometimes get split within a character boundary. This bypasses the need to split the string.

This does not guarantee the correct formatting of the docstring, but neither did the previous implementation.

Resolves #12428 

## Test Plan

Test case added to fixture


---

_@charliermarsh reviewed on 2024-07-21 18:12_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:210 on 2024-07-21 18:12_

While we're here, can we use `content.lines()` above instead of `content.split`? I'd hope `content.lines()` will also handle `\r`.

---

_@charliermarsh reviewed on 2024-07-21 18:15_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:211 on 2024-07-21 18:15_

Could we use `strip_prefix` here with the indentation instead of using `chars`?

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:211 on 2024-07-21 19:06_

Ok I reimplemented using `strip_prefix` but I still needed to call `chars` to check if the first char `is_whitepace`

---

_@augustelalande reviewed on 2024-07-21 19:06_

---

_@charliermarsh reviewed on 2024-07-21 19:13_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:211 on 2024-07-21 19:13_

Makes sense!

---

_@charliermarsh reviewed on 2024-07-21 19:26_

Thx!

---

_Label `bug` added by @charliermarsh on 2024-07-21 19:26_

---

_Label `fuzzer` added by @charliermarsh on 2024-07-21 19:26_

---

_Merged by @charliermarsh on 2024-07-21 19:30_

---

_Closed by @charliermarsh on 2024-07-21 19:30_

---

_Branch deleted on 2024-07-21 19:52_

---
