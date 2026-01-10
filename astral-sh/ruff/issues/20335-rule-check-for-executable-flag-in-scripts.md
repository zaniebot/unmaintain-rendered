---
number: 20335
title: "Rule: check for executable flag in scripts"
type: issue
state: closed
author: spaceone
labels:
  - question
assignees: []
created_at: 2025-09-10T15:18:27Z
updated_at: 2025-09-10T17:07:14Z
url: https://github.com/astral-sh/ruff/issues/20335
synced_at: 2026-01-10T01:23:01Z
---

# Rule: check for executable flag in scripts

---

_Issue opened by @spaceone on 2025-09-10 15:18_

### Summary

When a file doesn't have a `.py` suffix it's a script (or if it contains a `#!`-python-hashbang).

These files should have a `+x` executable file flag.
It's a common mistake to add new scripts to git, forgetting the executable rights and then in the best case the CI pipelines fail.

Therefor it would be nice to have a rule, which detects this.

---

_Comment by @ntBre on 2025-09-10 16:19_

This sounds like our [`flake8-executable` ](https://docs.astral.sh/ruff/rules/#flake8-executable-exe) rules. Do any of those cover your use case?

I can get EXE001 to fire on this script without the executable bit set, but I had to pass the filename to ruff directly since it won't check files without a Python extension by default:

filename: `try`
```py
#!/usr/bin/python

print("Hello world")
```

```shell
$ chmod -x try
$ ruff check try --select EXE
EXE001 Shebang is present but file is not executable
 --> try:1:1
  |
1 | #!/usr/bin/python
  | ^^^^^^^^^^^^^^^^^
2 |
3 | print("Hello world")
  |

Found 1 error.
```

I think you can use [include](https://docs.astral.sh/ruff/settings/#include) to configure files in a script directory to be checked too.

---

_Label `question` added by @ntBre on 2025-09-10 16:19_

---

_Comment by @spaceone on 2025-09-10 17:07_

hm, you are right. I was missing the hashbang in the file and therefor the check wasn't executed.

---

_Closed by @spaceone on 2025-09-10 17:07_

---
