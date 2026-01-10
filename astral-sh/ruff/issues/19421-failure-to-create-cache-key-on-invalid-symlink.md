```yaml
number: 19421
title: Failure to create cache key on invalid symlink
type: issue
state: open
author: kdeldycke
labels:
  - bug
  - cli
assignees: []
created_at: 2025-07-18T11:47:42Z
updated_at: 2025-07-18T12:01:14Z
url: https://github.com/astral-sh/ruff/issues/19421
synced_at: 2026-01-10T11:09:59Z
```

# Failure to create cache key on invalid symlink

---

_Issue opened by @kdeldycke on 2025-07-18 11:47_

### Summary

Tl;Dr: an invalid symlink makes `ruff` exits with code 1 due to cache key creation failure.

### How to reproduce

```shell-session
$ ln -s /blah.py

$ ls -lah
Permissions Size User Date Modified    Name
drwxr-xr-x@    - kde  2025-07-18 15:30 .
drwxr-xr-x     - kde  2025-07-18 15:30 ..
drwxr-xr-x@    - kde  2025-07-18 15:30 .ruff_cache
lrwxr-xr-x     - kde  2025-07-18 15:30 blah.py -> /blah.py

$ cat /blah.py                                                                
cat: /blah.py: No such file or directory (os error 2)

$ ruff check       
blah.py:1:1: E902 Failed to create cache key
  Cause: Failed to create cache key
  Cause: No such file or directory (os error 2)
Found 1 error.

$ echo $?        
1

$ rm ./blah.py

$ ruff check       
warning: No Python files found under the given path(s)
All checks passed!

$ echo $?      
0
```

### Expected behavior

I expect the `ruff` CLI to emit a warning and skip the invalid symlink altogether, and continue on checking other files.

An alternative would be to allow tweaking this behavior, i.e. either silently ignoring the broken symlink or erroring on purpose, behind an option.

### Version

ruff 0.12.3

---

_Comment by @kdeldycke on 2025-07-18 11:49_

Might be related to https://github.com/astral-sh/ruff/issues/19395

---

_Label `bug` added by @MichaReiser on 2025-07-18 12:01_

---

_Label `cli` added by @MichaReiser on 2025-07-18 12:01_

---
