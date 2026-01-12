```yaml
number: 6889
title: Incorrect f-string application
type: issue
state: closed
author: hotpxl
labels:
  - bug
assignees: []
created_at: 2023-08-26T00:45:46Z
updated_at: 2024-03-01T11:41:25Z
url: https://github.com/astral-sh/ruff/issues/6889
synced_at: 2026-01-12T15:54:46Z
```

# Incorrect f-string application

---

_@hotpxl_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Python file
```
a = 1
b = ("{{hello}}" "{}").format(a)
```

Command
```
target/debug/ruff check --select UP032 --fix --diff a.py
```
Vanilla Ruff, no settings, built from master
```
commit f91bacbb94e1ba0dc56b15afe388fe1f6c92e3e8 (HEAD -> main, origin/main, origin/HEAD)
Author: Charlie Marsh <charlie.r.marsh@gmail.com>
Date:   Fri Aug 25 19:47:15 2023 -0400
```

The ruff output is
```
--- a.py
+++ a.py
@@ -1,2 +1,2 @@
 a = 1
-b = ("{{hello}}" "{}").format(a)
+b = ("{{hello}}" f"{a}")
```
which is incorrect. The old code, when calling `.format`, collapses the double braces in `{{hello}}`. But the Ruff version doesn't

---

_Comment by @zanieb on 2023-08-26 01:00_

Hi @hotpxl on what version did this used to work? Could you help us determine when this regression was introduced?

---

_Comment by @hotpxl on 2023-08-26 02:26_

~I don't know if any version used to work, but I can try and git bisect~

realized my original description is misleading, corrected the wording

---

_Comment by @dhruvmanila on 2023-08-26 11:43_

It seems like the case of implicit string concatenation and as there are no placeholder in the first string, it's not being considered. If you change the code to `b = ("{{hello}}{}" "{}").format(a, a)`, the fix is correct. I guess the fix could be to add the `f` prefix to all strings within `(...).format()` call?

---

_Comment by @charliermarsh on 2023-08-26 13:29_

Yeah, we probably need to add the f prefix to all segments in the implicit concat, is my guess.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-28 19:01_

---

_Label `bug` added by @charliermarsh on 2023-08-28 19:01_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-08-28 20:21_

---

_Comment by @raylu on 2023-09-28 20:31_

another example:
```py
print('{{ ' '{} {}'.format(1, 2))
```

```diff
$ ruff check --select UP032 test.py --diff
--- test.py
+++ test.py
@@ -1 +1 @@
-print('{{ ' '{} {}'.format(1, 2))
+print('{{ ' f'{1} {2}')
```
the correct version is `print('{ ' f'{1} {2}')`

```
$ ruff --version
ruff 0.0.291
```

---

_Comment by @MichaReiser on 2023-09-29 09:02_

> Yeah, we probably need to add the f prefix to all segments in the implicit concat, is my guess.

Won't that potentially trigger another violation because some parts now use f-strings unnecessarily? 

---

_Comment by @raylu on 2023-09-29 16:54_

surprisingly, no

```py
print(f'{{ ')
```
```
$ ruff check . --select F541
test.py:1:7: F541 [*] f-string without any placeholders
```
but
```py
print(f'{{ ' f'{1} {2}')
```
```
$ ruff check . --select F541
[no output]
```

but even if it did, F541 is autofixable too. just gotta autofix in the right order

---

_Comment by @charliermarsh on 2023-09-29 16:56_

No, we don't flag pieces of an implicit concatenation, if at least one segment contains a placeholder. (This is intentional.)

---

_Comment by @AlexWaygood on 2024-03-01 11:41_

Looks like this was fixed back in November in #8697 🥳

---

_Closed by @AlexWaygood on 2024-03-01 11:41_

---
