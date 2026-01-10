```yaml
number: 3071
title: False positive in RET503 since ruff 0.0.248
type: issue
state: closed
author: link2xt
labels:
  - bug
assignees: []
created_at: 2023-02-20T21:44:32Z
updated_at: 2023-02-21T00:35:56Z
url: https://github.com/astral-sh/ruff/issues/3071
synced_at: 2026-01-10T11:09:46Z
```

# False positive in RET503 since ruff 0.0.248

---

_Issue opened by @link2xt on 2023-02-20 21:44_

Since version 0.0.248 ruff flags this code as having an error:
```python
def foo():
    while True:
        return True
```

I have `git bisect`ed this down to commit 059601d9685098a071bac6149ef5f321b3d845b8, PR #2962.

Running `$ cargo run -p ruff_cli -- check ./x.py --no-cache --select RET503` produces this error:
```
x.py:2:5: RET503 [*] Missing explicit `return` at the end of function able to return non-`None` value
```

This `while True` is a common pattern in Python to make infinite loops, and it clearly never returns unless there is a `break` inside.
I do not want to `return None` in the end, because I annotate my functions with types, and having a `return None` at the end of a function that is supposed to return an object (in this case I can add `return False` in the end, but not all objects are as easily constructable) would be flagged by mypy/pyright.

---

_Renamed from "False positive in PEP503 since ruff 0.0.248" to "False positive in RET503 since ruff 0.0.248" by @link2xt on 2023-02-20 21:44_

---

_Comment by @charliermarsh on 2023-02-20 21:59_

I guess we could treat `while True:` differently than other `while` loops. The issue is that, in general, we can't be certain that the loop will actually execute, so even if it ends in a `return`, we treat it as if it's non-returning. (The previous version just omitted `while` loops entirely, which is also a valid option and probably easier.)


---

_Label `question` added by @charliermarsh on 2023-02-20 21:59_

---

_Comment by @link2xt on 2023-02-20 22:05_

The loop also should not contain `break`, and it may be possible to break out to the function body with an exception.

If nothing else, I think it is ok to treat `while True` as a special case even if it introduces false negatives. This is what pyright does as far as I understand, it does not recognize `while 1` but recognizes `while True`.

---

_Comment by @charliermarsh on 2023-02-20 22:33_

I'll make sure it's fixed for the next release.

---

_Label `question` removed by @charliermarsh on 2023-02-20 22:34_

---

_Label `bug` added by @charliermarsh on 2023-02-20 22:34_

---

_Closed by @charliermarsh on 2023-02-20 23:22_

---

_Comment by @link2xt on 2023-02-20 23:47_

Thanks!

---

_Comment by @charliermarsh on 2023-02-21 00:00_

For now I just omitted `while True` entirely. We can do better of course but hopefully this avoids these false positives for you. (Previously, I think we ignored _all_ `while` loops.)

---

_Comment by @link2xt on 2023-02-21 00:35_

I only have one false positive currently at
https://github.com/deltachat/deltachat-core-rust/blob/f07206bd6cb6e4e783a197eeb88b6367f0de9afd/deltachat-rpc-client/src/deltachat_rpc_client/client.py#L105

As long as ruff does not report it, it's good enough for me to update.

---
