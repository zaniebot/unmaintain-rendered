```yaml
number: 1596
title: "[plugin] flake8_pytest_style without config section (pyproject.toml) crashes"
type: issue
state: closed
author: marscher
labels: []
assignees: []
created_at: 2023-01-03T12:31:03Z
updated_at: 2023-01-03T12:44:40Z
url: https://github.com/astral-sh/ruff/issues/1596
synced_at: 2026-01-10T12:05:31Z
```

# [plugin] flake8_pytest_style without config section (pyproject.toml) crashes

---

_Issue opened by @marscher on 2023-01-03 12:31_

minimal reproducing config:
```toml
select = ["PT"]
```

output:
```
[2023-01-03][13:28:56][ruff::cache][ERROR] Failed to deserialize encoded cache entry: Io(Kind(UnexpectedEof))
[2023-01-03][13:28:56][ruff::cache][ERROR] Failed to deserialize encoded cache entry: Io(Kind(UnexpectedEof))
thread '<unnamed>' panicked at 'called `Option::unwrap()` on a `None` value', src/flake8_pytest_style/plugins/parametrize.rs:196:29
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Comment by @charliermarsh on 2023-01-03 12:33_

Fixing now.

---

_Closed by @charliermarsh on 2023-01-03 12:39_

---

_Comment by @marscher on 2023-01-03 12:44_

This was insanely fast, just like your software :D

---
