---
number: 10390
title: File restructuring needs a manual sync --reinstall?
type: issue
state: closed
author: cgravill
labels:
  - question
assignees: []
created_at: 2025-01-08T12:25:57Z
updated_at: 2025-01-29T16:38:19Z
url: https://github.com/astral-sh/uv/issues/10390
synced_at: 2026-01-10T01:24:53Z
---

# File restructuring needs a manual sync --reinstall?

---

_Issue opened by @cgravill on 2025-01-08 12:25_

We're restructuring some of our packages with a `src/` layout and we've had some friction with moving modules not being found unless we do a manual `uv sync --resinstall`. I wondered if that's expected or might be something that can be handled?

Minimal reproduction:

`uv init --lib`

Then add some files

`one.py`
```python
def function_in_one():
    print("I am in one.py")
```

`two.py`
```python
from moving_module.one import function_in_one

function_in_one()
```

(this shows the opposite move, out of `src/` but the same happens either way)

```console
$ uv run src/moving_module/two.py
I am in one.py
$ mv src/moving_module moving_module
$ rm -r src
$ uv run moving_module/two.py
Traceback (most recent call last):
  File "/home//BLAH/BLAh/moving_module/moving_module/two.py", line 1, in <module>
    from moving_module.one import function_in_one
ModuleNotFoundError: No module named 'moving_module'
$ uv sync
Resolved 1 package in 1ms
Audited 1 package in 0.09ms
$ uv run moving_module/two.py
Traceback (most recent call last):
  File "/home//BLAH/BLAh/moving_module/moving_module/two.py", line 1, in <module>
    from moving_module.one import function_in_one
ModuleNotFoundError: No module named 'moving_module'
$ uv sync --reinstall
Resolved 1 package in 0.86ms
   Built moving-module @ file:///home//BLAH/BLAh/moving_module
Prepared 1 package in 282ms
Uninstalled 1 package in 0.33ms
Installed 1 package in 0.46ms
 ~ moving-module==0.1.0 (from file:///home//BLAH/BLAh/moving_module)
$ uv run moving_module/two.py
I am in one.py
$ uv --version
uv 0.5.15
```

It's easily fixed, just causes confusion when multiplied over lots of people and repositories being updated at different times.

---

_Comment by @roteiro on 2025-01-08 12:44_

You could add
```
[tool.uv]
reinstall-package = ["moving_module"]
```
to your `pyproject.toml` to enforce reinstallation before every `uv run` invocation.

---

_Comment by @charliermarsh on 2025-01-08 14:44_

I think this is "working as intended" even though it's somewhat confusing... We don't re-build the package on every invocation, only when certain paths change (see, e.g., https://docs.astral.sh/uv/concepts/cache/). In this case, the package is installed as editable, so that mostly "just works" -- like, if you change the print in `function_in_one`, you should see that reflected immediately when you `uv run`.

However, in this case, when you built the first time, `hatchling` considered `src/moving_module` to be the top-level module, so it points there when it builds the editable. If you move files out of that directory, it won't see them anymore, and you have to re-build. When you re-build, `hatchling` presumedly identifies that `moving_module` is now the top-level module, and changes the included `.pth` files in the environment to point to that directory.

In short, you need to tell uv to re-build the module, because we don't "always rebuild", and you've changed something that we don't detect.

---

_Label `question` added by @charliermarsh on 2025-01-08 14:44_

---

_Comment by @cgravill on 2025-01-08 21:31_

That totally makes sense, and avoiding rebuilds is great. I hoped you might have enough information to cheaply know the cache needs invalidating.

Is this something that the uv build system (#3957) might be able to catch? We were waiting while that stabilises a little but could try that out too?

No problem if not, it's just a bit messy for folks on my team less into `uv`, but I can tell them to `uv sync --reinstall` when they get pull the changes on the different repos.

---

_Comment by @cgravill on 2025-01-08 21:36_

Actually looking at your link on caching more (thanks for that), we're likely to change the `pyproject.toml` anyway but I can ensure any migration PR has a change to that file which will invalidate and force a rebuild for other folks. I've checked and even a `touch pyproject.toml` is enough.

---

_Comment by @charliermarsh on 2025-01-29 16:24_

Yeah `touch pyproject.toml` should be fine, since the caching is based on the file's ctime. I think it's plausible that the uv build backend could do a better job here... It's worth thinking about. (Though it likely doesn't do a better job _today_.)

---

_Closed by @charliermarsh on 2025-01-29 16:24_

---

_Comment by @cgravill on 2025-01-29 16:38_

Excellent. The workaround is good enough for our purposes. Thanks for pointers.

---

_Referenced in [astral-sh/uv#12047](../../astral-sh/uv/issues/12047.md) on 2025-03-14 23:50_

---
