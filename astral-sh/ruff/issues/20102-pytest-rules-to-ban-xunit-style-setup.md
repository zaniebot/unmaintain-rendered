```yaml
number: 20102
title: pytest rules to ban xunit-style setup
type: issue
state: open
author: rouge8
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-08-26T18:22:08Z
updated_at: 2025-08-26T18:46:08Z
url: https://github.com/astral-sh/ruff/issues/20102
synced_at: 2026-01-10T11:09:59Z
```

# pytest rules to ban xunit-style setup

---

_Issue opened by @rouge8 on 2025-08-26 18:22_

### Summary

It would be nice if Ruff had (optional) rules to ban pytest's xunit-style setup methods and encourage fixtures instead: https://docs.pytest.org/en/stable/how-to/xunit_setup.html

- setup_module
- teardown_module
- setup_class
- teardown_class
- setup_method
- teardown_method
- setup_function
- teardown_function

---

_Label `rule` added by @ntBre on 2025-08-26 18:46_

---

_Label `needs-decision` added by @ntBre on 2025-08-26 18:46_

---
