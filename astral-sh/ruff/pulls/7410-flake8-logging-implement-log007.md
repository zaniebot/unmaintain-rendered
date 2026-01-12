```yaml
number: 7410
title: "[`flake8-logging`] Implement `LOG007`: `ExceptionWithoutExcInfo`"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: rule/LOG007
created_at: 2023-09-15T16:03:21Z
updated_at: 2023-09-20T09:55:02Z
url: https://github.com/astral-sh/ruff/pull/7410
synced_at: 2026-01-12T02:39:10Z
```

# [`flake8-logging`] Implement `LOG007`: `ExceptionWithoutExcInfo`

---

_Pull request opened by @qdegraaf on 2023-09-15 16:03_

## Summary

This PR implements a new rule for `flake8-logging` plugin that checks for uses of `logging.exception()` with `exc_info` set to `False` or a falsy value. It suggests using `logging.error` in these cases instead. 

I am unsure about the name. Open to suggestions there, went with the most explicit name I could think of in the meantime.

Refer https://github.com/astral-sh/ruff/issues/7248

## Test Plan

Added a new fixture cases and ran `cargo test`


---

_Renamed from "[`flake8-logging`] Implement LOG007: `ExcInfoFalseInException`" to "[`flake8-logging`] Implement `LOG007`: `ExcInfoFalseInException`" by @qdegraaf on 2023-09-15 16:04_

---

_Comment by @github-actions[bot] on 2023-09-15 16:19_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@qdegraaf reviewed on 2023-09-15 16:28_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_logging/rules/exc_info_false_in_exception.rs`:43 on 2023-09-15 16:28_

Would it be worth it to check the name of the call first before calling `resolve_call_path()`for performance reasons? Or would that just add unnecessary clutter? 

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_logging/rules/exc_info_false_in_exception.rs`:16 on 2023-09-18 08:20_

```suggestion
/// `exc_info=False` is the same as using `error()`, which is clearer and doesn't need the
/// `exc_info` argument. This rule detects `exception()` calls with an exc_info argument that is
```

---

_Review comment by @konstin on `crates/ruff/resources/test/fixtures/flake8_logging/LOG007.py`:1 on 2023-09-18 08:21_

Can you add a test case with `logger.exception`? (https://github.com/adamchainz/flake8-logging/blob/8cd23181647d4c68391af1db71588e8b3dc2bcfe/tests/test_flake8_logging.py#L783-L793)

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_logging/rules/exc_info_false_in_exception.rs`:40 on 2023-09-18 08:26_

This needs to catch all log messages.

More common than
```python
logging.exception("...")
```
is doing
```python
logger = logging.getLogger(__name__)

logger.exception("...")
```
ruff has utils to detect logging calls, see `is_logger_candidate` and e.g. the G001 implementation.

---

_@konstin reviewed on 2023-09-18 08:26_

---

_@qdegraaf reviewed on 2023-09-18 13:09_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_logging/rules/exc_info_false_in_exception.rs`:40 on 2023-09-18 13:09_

Rewrote to utilise `is_logger_candidate`. It flags `logger.exception` and `logging.exception` now but does not flag `from logging import exception; exception()` Is that a limitation of `is_logger_candidate`? And if so, do we want to create another branch of logic to catch those cases? 

---

_Renamed from "[`flake8-logging`] Implement `LOG007`: `ExcInfoFalseInException`" to "[`flake8-logging`] Implement `LOG007`: `ExceptionWithoutExcInfo`" by @charliermarsh on 2023-09-18 17:28_

---

_Label `rule` added by @charliermarsh on 2023-09-18 17:29_

---

_@charliermarsh approved on 2023-09-18 17:29_

---

_Merged by @charliermarsh on 2023-09-18 17:46_

---

_Closed by @charliermarsh on 2023-09-18 17:46_

---

_@konstin reviewed on 2023-09-19 06:58_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_logging/rules/exc_info_false_in_exception.rs`:40 on 2023-09-19 06:58_

Moved this to https://github.com/astral-sh/ruff/issues/7502

---

_Branch deleted on 2023-09-20 09:55_

---
