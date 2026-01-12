```yaml
number: 3376
title: Update activation scripts from virtualenv
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/activate
created_at: 2024-05-04T21:48:30Z
updated_at: 2024-05-04T23:30:01Z
url: https://github.com/astral-sh/uv/pull/3376
synced_at: 2026-01-12T16:05:36Z
```

# Update activation scripts from virtualenv

---

_@charliermarsh_

## Summary

Refreshes some of the activation scripts, and fixes some bugs in `activate_this.py` that were likely the rest of some erroneous copy-pasting.

Closes https://github.com/astral-sh/uv/issues/3346.

## Test Plan

```
❯ python
Python 3.12.0 (main, Feb 28 2024, 09:44:16) [Clang 15.0.0 (clang-1500.1.0.2.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import httpx
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'httpx'
>>> activator = '.venv/bin/activate_this.py'
>>> with open(activator) as f:
...     exec(f.read(), {'__file__': activator})
...
>>> import httpx
```


---

_Review requested from @konstin by @charliermarsh on 2024-05-04 21:48_

---

_Label `bug` added by @charliermarsh on 2024-05-04 21:48_

---

_Marked ready for review by @charliermarsh on 2024-05-04 21:48_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/bare.rs`:205 on 2024-05-04 21:48_

Technically need to include both, though we `dedup` just below.

---

_@charliermarsh reviewed on 2024-05-04 21:48_

---

_Review comment by @AlexWaygood on `crates/uv-virtualenv/src/activator/activate_this.py`:55 on 2024-05-04 21:58_

(Ignorable nit, but since we're here)

```suggestion
sys.path[:] = sys.path[prev_length:] + sys.path[:prev_length]
```

---

_Review comment by @AlexWaygood on `crates/uv-virtualenv/src/bare.rs`:1 on 2024-05-04 21:59_

```suggestion
//! Create a bare virtualenv without any packages installed.
```

---

_@AlexWaygood reviewed on 2024-05-04 22:00_

---

_@AlexWaygood reviewed on 2024-05-04 22:10_

---

_Review comment by @AlexWaygood on `crates/uv-virtualenv/src/activator/activate_this.py`:57 on 2024-05-04 22:10_

I know this is what virtualenv has done for a long time, and that this script is largely vendored from virtualenv, but it's very strange to me that this script monkeypatches a variable onto the `sys` module that wouldn't exist otherwise. I don't think the stdlib venv module does this, right?

---

_@charliermarsh reviewed on 2024-05-04 22:48_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/activator/activate_this.py`:55 on 2024-05-04 22:48_

Thanks, although unlikely to change this since every deviation makes it a bit more difficult to keep these in-sync going forward.

---

_@charliermarsh reviewed on 2024-05-04 22:51_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/activator/activate_this.py`:57 on 2024-05-04 22:51_

I haven't dug into the history deeply, but I think it's a legacy virtualenv thing, possibly from before various evolutions to the `sys` module came about? We don't rely on this anywhere (though `pip` and others have code to detect it). I'm inclined to just leave it as-is to avoid deviations, it doesn't really cost us anything.

---

_@AlexWaygood reviewed on 2024-05-04 22:56_

---

_Review comment by @AlexWaygood on `crates/uv-virtualenv/src/activator/activate_this.py`:57 on 2024-05-04 22:56_

Yeah. While it's still very weird to me that a script of ours would do this, I don't mind it _too_ much given that virtualenv has been doing it since forever. 

---

_Merged by @charliermarsh on 2024-05-04 23:30_

---

_Closed by @charliermarsh on 2024-05-04 23:30_

---

_Branch deleted on 2024-05-04 23:30_

---
