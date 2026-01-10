```yaml
number: 19520
title: "[`flake8-blind-except`] Change `BLE001` to permit `logging.critical(..., exc_info=True)`."
type: pull_request
state: merged
author: clockback
labels:
  - rule
assignees: []
merged: true
base: main
head: logging-critical-bypass-blind-except
created_at: 2025-07-24T04:49:01Z
updated_at: 2025-07-25T22:08:50Z
url: https://github.com/astral-sh/ruff/pull/19520
synced_at: 2026-01-10T17:58:13Z
```

# [`flake8-blind-except`] Change `BLE001` to permit `logging.critical(..., exc_info=True)`.

---

_Pull request opened by @clockback on 2025-07-24 04:49_

## Summary

Changing `BLE001` (blind-except) so that it does not flag `except` clauses which include `logging.critical(..., exc_info=True)`.

## Test Plan

It passes the following (whereas the `main` branch does not):
```sh
$ cargo run -p ruff -- check somefile.py --no-cache --select=BLE001
```
```python
# somefile.py

import logging


try:
    print("Hello world!")
except Exception:
    logging.critical("Did not run.", exc_info=True)
```
Related: https://github.com/astral-sh/ruff/issues/19519

---

_Review requested from @ntBre by @ntBre on 2025-07-24 13:06_

---

_Label `rule` added by @ntBre on 2025-07-24 13:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_blind_except/rules/blind_except.rs`:51 on 2025-07-24 13:50_

```suggestion
/// Exceptions that are logged via `logging.exception()`,
/// `logging.error()`, or `logging.critical()` with `exc_info` enabled will
```

---

_@ntBre reviewed on 2025-07-24 13:55_

Thanks! This makes sense to me. @MichaReiser does this need to go out in preview? [blind-except (BLE001)](https://docs.astral.sh/ruff/rules/blind-except/#blind-except-ble001) is a stable rule.

I just had one minor docs nit, and could we add a test case or two? Maybe one example with `exc_info` without a lint and one without `exc_info` with a lint. These could go at the end of this file and basically be copies of the `error` case here:

https://github.com/astral-sh/ruff/blob/d13228ab856f8cce47b3031cb2b4f2a35401e7eb/crates/ruff_linter/resources/test/fixtures/flake8_blind_except/BLE.py#L91-L94



---

_Comment by @github-actions[bot] on 2025-07-24 14:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-07-24 14:25_

> Thanks! This makes sense to me. @MichaReiser does this need to go out in preview? [blind-except (BLE001)](https://docs.astral.sh/ruff/rules/blind-except/#blind-except-ble001) is a stable rule.

Our versioning policy is a bit vague here:

* The scope of a stable rule is significantly increased
* The intent of the rule changes

Given that this reduce violations, I'd say it's probably okay but it might be good to do a quick grep for `# noqa: BLE001` to see if you can find any suppressions specifically for this pattern. If there are many -> preview, otherwise go ahead

---

_@ntBre reviewed on 2025-07-24 14:39_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_blind_except/rules/blind_except.rs`:51 on 2025-07-24 14:39_

Oh, I take this back. My suggestion makes it sound like `exc_info` is needed for `logging.exception` too, my bad!

---

_Comment by @ntBre on 2025-07-24 14:46_

I went through two pages of search results on GitHub and didn't find any instances of this pattern. Along with the clean ecosystem report, I think this is good to go once we add tests.

---

_Review requested from @ntBre by @clockback on 2025-07-25 08:58_

---

_@ntBre approved on 2025-07-25 21:51_

Thank you!

---

_Renamed from "Change `BLE001` to permit `logging.critical(..., exc_info=True)`." to "[`flake8-blind-except`] Change `BLE001` to permit `logging.critical(..., exc_info=True)`." by @ntBre on 2025-07-25 21:52_

---

_Merged by @ntBre on 2025-07-25 21:52_

---

_Closed by @ntBre on 2025-07-25 21:52_

---

_Branch deleted on 2025-07-25 22:08_

---
