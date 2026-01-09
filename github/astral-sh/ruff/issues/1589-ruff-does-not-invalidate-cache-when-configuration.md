---
number: 1589
title: Ruff does not invalidate cache when configuration changes (PL001)
type: issue
state: closed
author: notypecheck
labels: []
assignees: []
created_at: 2023-01-03T08:27:59Z
updated_at: 2023-01-03T12:13:24Z
url: https://github.com/astral-sh/ruff/issues/1589
synced_at: 2026-01-07T13:12:14-06:00
---

# Ruff does not invalidate cache when configuration changes (PL001)

---

_Issue opened by @notypecheck on 2023-01-03 08:27_

I ran into weird behavior with ruff cache and PT001 check, but other checks potentially could be affected too.

## Steps to reproduce:
1. Add `fixture-parenthesis` option
```toml
[tool.ruff.flake8-pytest-style]
fixture-parentheses = true
```
2. Run `ruff` so cache directory is generated:
```py
import pytest

@pytest.fixture
def my_fixture() -> int:
    return 42
```
3. Change `fixture-parentheses` to opposite value
4. Run `ruff` again, same output should be generated

I made a repository with reproducible example: https://gitlab.com/ThirVondukr/ruff-issues/-/tree/f231fe4bb3752fab0c866925c9eba241e24c609e
Here's a failed CI run: https://gitlab.com/ThirVondukr/ruff-issues/-/jobs/3545437297

---

_Renamed from "Ruff does not invalidate cache wheen configuration changes (PL001)" to "Ruff does not invalidate cache when configuration changes (PL001)" by @notypecheck on 2023-01-03 08:44_

---

_Referenced in [astral-sh/ruff#1595](../../astral-sh/ruff/pulls/1595.md) on 2023-01-03 12:12_

---

_Closed by @charliermarsh on 2023-01-03 12:12_

---

_Comment by @charliermarsh on 2023-01-03 12:13_

Thank you! Fixed. Just an oversight.

---
