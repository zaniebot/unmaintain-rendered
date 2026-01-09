---
number: 9623
title: "virtualenv detection skipped when running via `python -m uv`"
type: issue
state: closed
author: tjni
labels:
  - needs-decision
  - breaking
assignees: []
created_at: 2024-12-03T22:35:21Z
updated_at: 2025-06-20T13:51:10Z
url: https://github.com/astral-sh/uv/issues/9623
synced_at: 2026-01-07T13:12:18-06:00
---

# virtualenv detection skipped when running via `python -m uv`

---

_Issue opened by @tjni on 2024-12-03 22:35_

```
/path/to/system/python -m uv python find
```
When run in a project with a virtual environment, this returns the invoking interpreter. I would expect it to find the virtual environment's Python interpreter. This is a consequence of https://github.com/astral-sh/uv/pull/3736. I think it's reasonable that the invoking interpreter is discoverable but would expect it to still check for a virtual environment.

---

_Comment by @zanieb on 2024-12-03 22:44_

Can you explain the use-case? As you noted, it's intentional that if you invoke uv from a parent interpreter we'll use that environment. I don't think we should check for a `VIRTUAL_ENV` in this case, the parent interpreter takes precedence. As an example scenario, the following behavior seems "correct":

```
❯ uv venv foo
Using CPython 3.12.7
Creating virtual environment at: foo
Activate with: source foo/bin/activate
❯ uv venv bar
Using CPython 3.12.7
Creating virtual environment at: bar
Activate with: source bar/bin/activate
❯ uv pip install --python foo uv
Using Python 3.12.7 environment at foo
Resolved 1 package in 196ms
Prepared 1 package in 1.05s
Installed 1 package in 6ms
 + uv==0.5.6
❯ source bar/bin/activate
❯ ./foo/bin/python -m uv python find
/Users/zb/workspace/uv/foo/bin/python
❯ ./foo/bin/uv python find
/Users/zb/workspace/uv/bar/bin/python3
❯ uv python find
/Users/zb/workspace/uv/bar/bin/python3
```

Perhaps you want `/path/to/system/python/bin/uv python find` instead?

---

_Comment by @tjni on 2024-12-03 23:17_

My use case is a bit more complicated: I am trying to invoke uv from a separate Python script by reusing the `__main__` entrypoint, and this is the reduced reproducer.

I wanted to discuss it because, after thinking about it, I feel that the current behavior is unexpected. I view `python -m uv` as primarily a way to launch uv and not a way to forcefully override the interpreter. The default behavior of uv to adapt its behavior based on what the current virtual environment is seems desirable even when launched this way.

The resolved issues point out that it's surprising that uv doesn't prefer the invoking interpreter over other interpreters on the PATH, especially during venv creation, and I do like that uv defaults to what it's invoked with in that case.



---

_Label `needs-decision` added by @zanieb on 2024-12-04 00:16_

---

_Label `breaking` added by @zanieb on 2024-12-04 00:16_

---

_Comment by @zanieb on 2024-12-04 00:18_

> I view python -m uv as primarily a way to launch uv and not a way to forcefully override the interpreter.

Hm. Fair.

I'm not sure if we should change this or not, but it's breaking regardless so we can wait to see if we get additional feedback.

> I am trying to invoke uv from a separate Python script by reusing the `__main__` entrypoint, and this is the reduced reproducer.

Could you share a minimal example of this?

---

_Comment by @tjni on 2024-12-04 00:32_

In a project that has a dependency on `uv`, it would look like:

```
from uv.__main__ import _run

def main():
    # Do some stuff like setting environment variables
    _run()

if __name__ == "__main__":
    main()
```

The actual setup makes a little more sense: we build a [shiv](https://github.com/linkedin/shiv) and at the top add a shebang, so it is just invoked directly instead of through an explicit Python interpreter.

---

_Comment by @zanieb on 2025-06-20 13:51_

I think given the lack of feedback from other users, I'm going to close this for now.

---

_Closed by @zanieb on 2025-06-20 13:51_

---
