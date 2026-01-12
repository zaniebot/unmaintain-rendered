```yaml
number: 4920
title: git submodule files not being excluded
type: issue
state: closed
author: mckib2
labels:
  - question
assignees: []
created_at: 2023-06-07T05:12:54Z
updated_at: 2023-06-08T00:40:01Z
url: https://github.com/astral-sh/ruff/issues/4920
synced_at: 2026-01-12T15:54:45Z
```

# git submodule files not being excluded

---

_@mckib2_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Working on https://github.com/scipy/scipy/pull/18642, I'm having trouble getting the ruff `lint.toml` file to ignore python files found in 3rd party git submodules.  It's reproducible locally and in CI.  I've tried the following locally:
- add additional `exclude` item `"scipy/_lib/highs/**/*.py"`
- added additional `per-file-ignore`:
    - `"scipy/_lib/highs/highspy/tests/test_highspy.py" = ["ALL"]`

Both result in the unhelpful message for 3rd-party code found in a git submodule:
```
scipy/_lib/highs/highspy/tests/test_highspy.py:57:4 found test function 'get_infeasible_model' which does not start with 'test'
```

Command I'm running: `python dev.py lint`, which I understand is running:
```
ruff --config=lint.toml [list of files that notably does not include scipy/_lib/highs/highspy/tests/test_highspy.py]
```

Version info:
```
$  python --version
Python 3.10.6
$ ruff --version
ruff 0.0.271
```

---

_Comment by @charliermarsh on 2023-06-07 21:27_

Adding this to `lint.toml` does seem to resolve the issue for me:

```diff
❯ git diff HEAD
diff --git a/tools/lint.toml b/tools/lint.toml
index 67b996939..6f6756372 100644
--- a/tools/lint.toml
+++ b/tools/lint.toml
@@ -25,6 +25,7 @@ target-version = "py39"
 "scipy/optimize/_linprog.py" = ["F401"]
 "scipy/optimize/_linprog_ip.py" = ["F401"]
 "setup.py" = ["ALL"]
+"scipy/_lib/highs/**/*.py" = ["ALL"]

 [mccabe]
 # Unlike Flake8, default to a complexity level of 10.
```

Running `ruff --config=tools/lint.toml .` before and after suppresses an error in `test_highspy.py`. Do you mind giving that a try?

---

_Label `question` added by @charliermarsh on 2023-06-07 21:27_

---

_Comment by @mckib2 on 2023-06-07 22:55_

Thanks @charliermarsh!  This is actually working, it's another utility printing out that message that's causing me grief.  Ruff is (and was) working as expected, operator error :)

---

_Closed by @mckib2 on 2023-06-07 22:55_

---

_Comment by @charliermarsh on 2023-06-08 00:40_

No worries, glad to hear it :)

---
