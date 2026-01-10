```yaml
number: 14355
title: Unused local variable reassignment with the same name is not reported (F841?)
type: issue
state: closed
author: MiroPsota
labels:
  - type-inference
assignees: []
created_at: 2024-11-15T09:03:25Z
updated_at: 2024-11-18T08:15:31Z
url: https://github.com/astral-sh/ruff/issues/14355
synced_at: 2026-01-10T11:09:56Z
```

# Unused local variable reassignment with the same name is not reported (F841?)

---

_Issue opened by @MiroPsota on 2024-11-15 09:03_

In the following code snippet, the first assignment to `a` is never used, but is not reported by Ruff 0.7.3. PyCharm reports it and suggests to delete assignment target.
```
def main():
    a = 1
    a = 2
    print(a)
```

---

_Label `bug` added by @dylwil3 on 2024-11-15 17:22_

---

_Label `bug` removed by @dylwil3 on 2024-11-15 17:29_

---

_Comment by @dylwil3 on 2024-11-15 17:34_

Thanks, this is interesting! Unless I'm misusing the tool, it doesn't seem like Pyflakes flags this either. I'm wondering if `F841` is restricted in scope to literally not using the _name_ of some variable.

I also tried it in `pyright` and `mypy` to see if a type checker would complain and didn't see any issues.

```console
➜  cat tmp.py
def main():
    a = 1
    a = 2
    print(a)%                                                                           
➜  uvx pyflakes tmp.py
➜  
```
Is the suggestion that we add a lint rule for this? Do you happen to know of a lint rule in one of the (partially) supported plugins that catches this, or is it just something Pycharm does?

---

_Comment by @charliermarsh on 2024-11-18 02:11_

Yeah this matches the Pyflakes behavior, which is intentional IIRC.

---

_Comment by @MiroPsota on 2024-11-18 08:09_

If it's intentional, then I suggest making a new rule, as it's useful IMO.

---

_Label `type-inference` added by @MichaReiser on 2024-11-18 08:13_

---

_Comment by @MichaReiser on 2024-11-18 08:15_

The relevant upstream issue is https://github.com/PyCQA/pyflakes/issues/498. So this is more of a technical limitation than it isn't in the rule's intent. Ruff might be able to detect this very trivial case (if all assignments are all in the same branch).

This is the same underlying issue as https://github.com/astral-sh/ruff/issues/6704

---

_Closed by @MichaReiser on 2024-11-18 08:15_

---
