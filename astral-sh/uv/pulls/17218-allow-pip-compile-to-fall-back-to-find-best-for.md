```yaml
number: 17218
title: "Allow `pip compile` to fall back to `find_best` for `--python`"
type: pull_request
state: open
author: EliteTK
labels:
  - enhancement
assignees: []
draft: true
base: main
head: tk/pip-compile-missing-py-2
created_at: 2025-12-22T19:16:52Z
updated_at: 2025-12-29T13:16:22Z
url: https://github.com/astral-sh/uv/pull/17218
synced_at: 2026-01-12T16:12:39Z
```

# Allow `pip compile` to fall back to `find_best` for `--python`

---

_@EliteTK_

## Summary

Partially address #16709.

For cases when `uv pip compile` is used with `--python <ver>` and this flag is being honoured, allow falling back to `find_best` in cases where the python version cannot be found or downloaded.

## Test Plan

A number of existing testcases already hit this and have been updated.


---

_Review requested from @zanieb by @EliteTK on 2025-12-22 19:16_

---

_Label `enhancement` added by @EliteTK on 2025-12-22 19:16_

---

_@EliteTK reviewed on 2025-12-22 19:18_

---

_Review comment by @EliteTK on `crates/uv/src/commands/pip/compile.rs`:369 on 2025-12-22 19:18_

There's an argument to be made here that this should print the original error too... Let me know.

---

_@EliteTK reviewed on 2025-12-22 19:19_

---

_Review comment by @EliteTK on `crates/uv/src/commands/pip/compile.rs`:327 on 2025-12-22 19:19_

These are taken from `find_or_download`. Maybe this is an argument for some kind of predicate function for `uv_python::Error` so this case can be tested in one place? 

---

_@EliteTK reviewed on 2025-12-22 19:22_

---

_Review comment by @EliteTK on `crates/uv/src/commands/pip/compile.rs`:307 on 2025-12-22 19:22_

The reason why `python_missing_download` is an `Option` instead of a bool is to avoid needing to check a bool _and_ `let Some(python) = python.as_ref()` a second time.

The reason why this exists at all is to detect the case when the attempt to download failed in the first place, I couldn't find some way of testing if an `interpreter` satisfied a specific version. If there is a way to do that, let me know, then this code can simplified.

---

_Comment by @EliteTK on 2025-12-22 19:24_

Might be a candidate for the breaking label?

I guess the previous PR was too...

---

_@EliteTK reviewed on 2025-12-22 19:46_

---

_Review comment by @EliteTK on `crates/uv/src/commands/pip/compile.rs`:364 on 2025-12-22 19:46_

Maybe this should be `is not available and could not be downloaded` or it could just be `is not available` like the other message?

---

_Comment by @EliteTK on 2025-12-23 00:01_

I actually don't think this is right. We don't fall back to `find_best` for other things when `--python` is specified. e.g.

```
$ uv sync --no-python-downloads --python 3.13
warning: Ignoring existing virtual environment linked to non-existent Python interpreter: .venv/bin/python3 -> python
error: No interpreter found for Python 3.13 in managed installations or search path

hint: A managed Python download is available for Python 3.13, but Python downloads are set to 'never'
```

---

_Converted to draft by @EliteTK on 2025-12-29 13:16_

---
