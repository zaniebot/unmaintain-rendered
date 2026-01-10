```yaml
number: 853
title: "per-file-ignores glob **.py crash"
type: issue
state: closed
author: JonathanPlasse
labels:
  - bug
assignees: []
created_at: 2022-11-21T14:29:52Z
updated_at: 2022-11-21T18:39:20Z
url: https://github.com/astral-sh/ruff/issues/853
synced_at: 2026-01-10T12:09:58Z
```

# per-file-ignores glob **.py crash

---

_Issue opened by @JonathanPlasse on 2022-11-21 14:29_

Running the command below with double star crash Ruff.

```
ruff --per-file-ignores="**.py:B" .
```

with this error code:

```
thread 'main' panicked at 'Invalid pattern.: PatternError { pos: 37, msg: "recursive wildcards must form a single path component" }', src/settings/types.rs:61:71
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
[1]    135779 abort (core dumped)  ruff --per-file-ignores="**.py:B" .
```

ruff version: 0.0.132
python version: 3.11.0
os: Ubuntu 20.04

---

_Label `bug` added by @charliermarsh on 2022-11-21 14:49_

---

_Closed by @charliermarsh on 2022-11-21 18:39_

---
