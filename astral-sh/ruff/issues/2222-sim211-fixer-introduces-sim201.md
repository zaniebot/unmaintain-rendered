```yaml
number: 2222
title: SIM211 fixer introduces SIM201
type: issue
state: closed
author: spaceone
labels:
  - fixes
assignees: []
created_at: 2023-01-26T20:53:29Z
updated_at: 2023-01-26T23:23:49Z
url: https://github.com/astral-sh/ruff/issues/2222
synced_at: 2026-01-12T15:54:42Z
```

# SIM211 fixer introduces SIM201

---

_@spaceone_




```
$ cat foo.py
setuid = False if os.geteuid() == 0 else True
$ ruff --select SIM211 --fix foo.py
Found 1 error (1 fixed, 0 remaining).
$ ruff --select SIM foo.py
foo.py:1:10: SIM201 Use `os.geteuid() != 0` instead of `not os.geteuid() == 0`
Found 1 error.
1 potentially fixable with the --fix option.
```

---

_Label `autofix` added by @charliermarsh on 2023-01-26 20:56_

---

_Comment by @charliermarsh on 2023-01-26 23:23_

I think this is acceptable, though, because it fixes it in the next pass?

```
❯ cargo run foo.py --select SIM --fix -n
   Compiling ruff v0.0.235 (/Users/crmarsh/workspace/ruff)
   Compiling ruff_cli v0.0.235 (/Users/crmarsh/workspace/ruff/ruff_cli)
    Finished dev [unoptimized + debuginfo] target(s) in 7.33s
     Running `target/debug/ruff foo.py --select SIM --fix -n`
Found 2 errors (2 fixed, 0 remaining).
```

Gives me: 

```py
setuid = os.geteuid() != 0
```

---

_Closed by @charliermarsh on 2023-01-26 23:23_

---
