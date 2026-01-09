---
number: 6029
title: F823 (undefined local) triggered by a double import
type: issue
state: closed
author: ajkerrigan
labels: []
assignees: []
created_at: 2023-07-24T13:15:31Z
updated_at: 2023-07-24T14:42:50Z
url: https://github.com/astral-sh/ruff/issues/6029
synced_at: 2026-01-07T13:12:15-06:00
---

# F823 (undefined local) triggered by a double import

---

_Issue opened by @ajkerrigan on 2023-07-24 13:15_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
First, thanks for ruff! It rocks and I love it :heart_eyes: .

I ran across a minor issue recently and wanted to document it here in case other folks do too. I'm not sure if it's an intentional change or not...

Moving from version `0.0.278` to `0.0.279` I noticed `F823 Local variable 'sys' referenced before assignment` occurring in a new situation. If you import a module twice, this rule flags references in between the two imports. For example:

> double_import.py
```python
import sys


def main():
    print(sys.argv)

    try:
        3 / 0
    except ZeroDivisionError:
        import sys

        sys.exit(1)
```

```console
❯ ruff double_import.py
double_import.py:5:11: F823 Local variable `sys` referenced before assignment
Found 1 error.
```

It does feel right to call out the double `import sys` as unnecessary here, but F823 seems misleading. Is this change intentional?

_I didn't see an obvious answer in the `0.0.279` changelog though there are a couple changes that seem possibly relevant. Will update here if I find anything useful. The change that seems _most_ relevant at a glance (to my untrained eye anyway) is bcec2f0c4ce626c22c0727d02195ec077572ea98._

---

_Referenced in [cloud-custodian/cloud-custodian#8755](../../cloud-custodian/cloud-custodian/pulls/8755.md) on 2023-07-24 13:22_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-24 13:46_

---

_Comment by @charliermarsh on 2023-07-24 13:48_

Thanks! Looks like a bug.

---

_Renamed from "F823 triggered by a double import" to "F823 (undefined local) triggered by a double import" by @ajkerrigan on 2023-07-24 14:10_

---

_Comment by @charliermarsh on 2023-07-24 14:20_

Or, this may actually be correct? I'm not 100% sure. Python does error with an early unbound access:

```
Traceback (most recent call last):
  File "/Users/crmarsh/workspace/ruff/foo.py", line 22, in <module>
    main()
  File "/Users/crmarsh/workspace/ruff/foo.py", line 5, in main
    print(sys.argv)
          ^^^
UnboundLocalError: cannot access local variable 'sys' where it is not associated with a value
```

So in that case, F823 feels correct to me.

---

_Comment by @ajkerrigan on 2023-07-24 14:35_

Ha, you're right :sweat_smile: !  I could have seen it either way since it does feel wrong and worth yelling about one way or the other.

My first thought was looking at `flake8` which runs clean on this file. But `pylint` yells all around the issue :grin: 

```console
double_import.py:11:8: W0621: Redefining name 'sys' from outer scope (line 1) (redefined-outer-name)
double_import.py:6:10: E0601: Using variable 'sys' before assignment (used-before-assignment)
double_import.py:11:8: W0404: Reimport 'sys' (imported line 1) (reimported)
double_import.py:11:8: C0415: Import outside toplevel (sys) (import-outside-toplevel)
double_import.py:1:0: W0611: Unused import sys (unused-import)
```

This is also a good reminder to not just lint your minimal example but also try, you know... _running_ it :laughing: .

Thanks for the follow-up!

---

_Closed by @ajkerrigan on 2023-07-24 14:35_

---

_Comment by @charliermarsh on 2023-07-24 14:42_

I had the same reaction as you, I actually thought that code was fine! I'm gonna add some test cases so that we're at least explicit about this.

---

_Referenced in [astral-sh/ruff#6036](../../astral-sh/ruff/pulls/6036.md) on 2023-07-24 15:40_

---
