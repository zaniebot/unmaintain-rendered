---
number: 3163
title: problem with cache in combination with EXE001
type: issue
state: closed
author: xmatthias
labels: []
assignees: []
created_at: 2023-02-23T05:47:40Z
updated_at: 2023-02-23T10:39:50Z
url: https://github.com/astral-sh/ruff/issues/3163
synced_at: 2026-01-07T13:12:14-06:00
---

# problem with cache in combination with EXE001

---

_Issue opened by @xmatthias on 2023-02-23 05:47_

ruff version `ruff 0.0.251`

The premise  is that a file is present in a project with the EXE ruleset enabled (the file violates this ruleset by not being executable).



``` bash
touch random_file.py

echo "#!/usr/bin/env python" > random_file.py
# The file is now violating the EXE rule - as it's not executable.
```

Running ruff on this correctly identifies the problem. but after fixing it, ruff is not checking the file again properly, still reporting it as violation.
Running without cache (`-n`) shows correct results - but the error "returns from cache" after the run.

```
$ ruff . 
random_file.py:1:3: EXE001 Shebang is present but file is not executable
Found 1 error.
$ chmod +x random_file.py
$ ruff . 
random_file.py:1:3: EXE001 Shebang is present but file is not executable
Found 1 error.
$ ruff . -n
$ ruff . 
random_file.py:1:3: EXE001 Shebang is present but file is not executable
```

the only way i found to permanently fix this after fixing the violation is by removing cache (`rm -rf .ruff_cache`) - which i'm sure is not the indended way.

---

_Comment by @ngnpope on 2023-02-23 08:47_

Duplicate of #3086, fixed in [v0.0.252](https://github.com/charliermarsh/ruff/releases/tag/v0.0.252) by #3104. 

---

_Comment by @xmatthias on 2023-02-23 10:39_

Thanks - didn't see that.
as i updated yesterday evening, i was assuming i was using the latest version ...

---

_Closed by @xmatthias on 2023-02-23 10:39_

---
