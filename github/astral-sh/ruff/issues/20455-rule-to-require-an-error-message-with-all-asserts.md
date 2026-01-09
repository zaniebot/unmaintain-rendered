---
number: 20455
title: Rule to require an error message with all asserts
type: issue
state: open
author: aiven-amartin
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-09-17T17:52:23Z
updated_at: 2025-09-18T06:58:31Z
url: https://github.com/astral-sh/ruff/issues/20455
synced_at: 2026-01-07T13:12:16-06:00
---

# Rule to require an error message with all asserts

---

_Issue opened by @aiven-amartin on 2025-09-17 17:52_

### Summary

Similar to PT016 requiring an error message for all pytest.fail, I would like a rule that requires all `assert <condition>` to be required to have the form `assert <condition>, <error message>"`.

---

_Comment by @ntBre on 2025-09-17 18:14_

This makes sense to me. I was surprised not to see any existing rule that covers this or even a previous request! Clippy also has a related rule called [`missing_assert_message`](https://rust-lang.github.io/rust-clippy/master/index.html#missing_assert_message).

This would somewhat conflict with [assert (S101)](https://docs.astral.sh/ruff/rules/assert/#assert-s101), which suggests removing `assert`s entirely, though.

---

_Label `rule` added by @ntBre on 2025-09-17 18:14_

---

_Label `needs-decision` added by @ntBre on 2025-09-17 18:14_

---

_Comment by @MichaReiser on 2025-09-18 06:57_

This would be a restriction lint (it disallowes perfectly valid code because of personal preferences but there's nothing wrong with the code itself) which is something we're currently holding back on adding until we solved https://github.com/astral-sh/ruff/issues/1774

---
