---
number: 12504
title: "Documentation mismatch between `ruff` and `docs` for `f-string-in-exception`"
type: issue
state: closed
author: UriyaHarpeness
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2024-07-25T08:26:11Z
updated_at: 2024-07-25T14:24:18Z
url: https://github.com/astral-sh/ruff/issues/12504
synced_at: 2026-01-10T01:22:52Z
---

# Documentation mismatch between `ruff` and `docs` for `f-string-in-exception`

---

_Issue opened by @UriyaHarpeness on 2024-07-25 08:26_

This is a documentation issue, not related to the logic.
This issue is relevant for the current version: 0.5.4.

In https://docs.astral.sh/ruff/rules/f-string-in-exception/ there's a section that shows the traceback before and after, in the after one it does not include the first line of the traceback.

I wanted to open a PR to add the missing line, but when opening the code docstrings it seemed to already include that line:
https://github.com/astral-sh/ruff/blob/2ce3e3ae60ba44700b5e98a61adc96d80b0e4bd3/crates/ruff_linter/src/rules/flake8_errmsg/rules/string_in_exception.rs#L43-L48

I went to the `docs` repo, and there the first line was missing:
https://github.com/astral-sh/docs/blob/main/site/ruff/rules/f-string-in-exception/index.html#L827-L830

I don't know what the source of this mismatch is, but seemed that just opening a manual change to the generated docs is not the way to go. So here I am opening an issue.

---

_Comment by @dhruvmanila on 2024-07-25 13:32_

I think it's the other rule (in the same file) which has a missing traceback header ;)

https://github.com/astral-sh/ruff/blob/2ce3e3ae60ba44700b5e98a61adc96d80b0e4bd3/crates/ruff_linter/src/rules/flake8_errmsg/rules/string_in_exception.rs#L98-L102

---

_Label `documentation` added by @dhruvmanila on 2024-07-25 13:32_

---

_Label `good first issue` added by @dhruvmanila on 2024-07-25 13:32_

---

_Comment by @UriyaHarpeness on 2024-07-25 13:58_

Sigh... my bad, I'll open a PR soon.
Thanks.

---

_Referenced in [astral-sh/ruff#12508](../../astral-sh/ruff/pulls/12508.md) on 2024-07-25 14:05_

---

_Comment by @charliermarsh on 2024-07-25 14:24_

Closed by https://github.com/astral-sh/ruff/pull/12508.

---

_Closed by @charliermarsh on 2024-07-25 14:24_

---
