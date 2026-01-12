```yaml
number: 1475
title: Simplified code for unicode fix
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: fixunicodefix
created_at: 2022-12-30T12:38:56Z
updated_at: 2022-12-30T16:18:52Z
url: https://github.com/astral-sh/ruff/pull/1475
synced_at: 2026-01-12T15:55:06Z
```

# Simplified code for unicode fix

---

_@colin99d_

This morning I realized that all the unicode checker is doing is removing the first character. So I went ahead and simplified the code.

---

_@charliermarsh reviewed on 2022-12-30 12:40_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/rewrite_unicode_literal.rs`:19 on 2022-12-30 12:40_

You could even make this `Fix::deletion`, for a span of that single character.

---

_@charliermarsh reviewed on 2022-12-30 12:42_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/rewrite_unicode_literal.rs`:18 on 2022-12-30 12:42_

Maybe also link to https://docs.python.org/3/reference/lexical_analysis.html#string-and-bytes-literals to show what the valid prefixes are.

---

_Review comment by @colin99d on `src/pyupgrade/plugins/rewrite_unicode_literal.rs`:19 on 2022-12-30 15:09_

Good idea!

---

_@colin99d reviewed on 2022-12-30 15:09_

---

_Merged by @charliermarsh on 2022-12-30 16:18_

---

_Closed by @charliermarsh on 2022-12-30 16:18_

---
