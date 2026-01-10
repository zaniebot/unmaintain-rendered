```yaml
number: 21881
title: "`return-in-generator` (`B901`) - should not be reported on pytest hook wrappers"
type: issue
state: open
author: DetachHead
labels:
  - rule
  - help wanted
  - preview
assignees: []
created_at: 2025-12-09T23:08:50Z
updated_at: 2025-12-12T18:26:49Z
url: https://github.com/astral-sh/ruff/issues/21881
synced_at: 2026-01-10T11:10:00Z
```

# `return-in-generator` (`B901`) - should not be reported on pytest hook wrappers

---

_Issue opened by @DetachHead on 2025-12-09 23:08_

### Summary

```py
from collections.abc import Generator

from pytest import TestReport, hookimpl


@hookimpl(wrapper=True)
def pytest_runtest_makereport() -> Generator[None, TestReport, TestReport]:
    result = yield
    result.outcome = "passed"
    return result # error: return-in-generator (B901)
```

this is the intended usage of pytest hook wrappers. see https://docs.pytest.org/en/stable/how-to/writing_hook_functions.html#hook-wrappers-executing-around-other-hooks

this behavior was introduced in 0.14.8 (#21200)

### Version

0.14.8

---

_Comment by @ntBre on 2025-12-09 23:41_

Ah thanks for the link to the docs! I guess that explains the ecosystem hits on pytest from the PR. It makes sense to me to special case functions decorated with `pytest.hookimpl`.

---

_Label `rule` added by @ntBre on 2025-12-09 23:41_

---

_Label `preview` added by @ntBre on 2025-12-09 23:41_

---

_Label `help wanted` added by @MichaReiser on 2025-12-10 07:00_

---

_Comment by @assadyousuf on 2025-12-12 04:01_

Have a fix for this [here](https://github.com/astral-sh/ruff/pull/21931). Specifically looking for pytest hook wrappers before reporting `return-in-generator` @MichaReiser @ntBre 

---

_Assigned to @assadyousuf by @ntBre on 2025-12-12 18:26_

---
