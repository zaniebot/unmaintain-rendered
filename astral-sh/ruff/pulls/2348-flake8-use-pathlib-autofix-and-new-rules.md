```yaml
number: 2348
title: "[`flake8-use-pathlib`] autofix and new rules"
type: pull_request
state: closed
author: sbrugman
labels:
  - fixes
assignees: []
draft: true
base: main
head: pathlib-extension
created_at: 2023-01-30T11:38:30Z
updated_at: 2023-12-10T21:01:57Z
url: https://github.com/astral-sh/ruff/pull/2348
synced_at: 2026-01-12T15:55:08Z
```

# [`flake8-use-pathlib`] autofix and new rules

---

_@sbrugman_

See also #2331 and #2090 

Kick-off pathlib autofix. The others could mostly be implemented in the same logic, but perhaps good to test with only this small subset to see what occurs in the wild.

PTH201-205:
- ~~Simplify path constructor `Path(".")` to `Path()`~~
- ~~Replace `os.path.getsize` with `Path.stat()`~~
- ~~Using PTH2XX rather than FURBXXX (rules originate from [`refurb`](https://github.com/charliermarsh/ruff/issues/1348)) to keep all `pathlib` logic together~~

Autofix for:
- ~~PTH201: `Path(".")` to `Path()`~~
- PTH109: `os.path.getcwd(x)` to `Path(x).cwd()`

Nested calls are unwrapped:
- `Path(os.path.getcwd(x))` => `Path(x).cwd()`

Promote `get_member_import_name_alias` from `pylint` to `ast.helpers`

Follow-up work items:
- Add (`from pathlib import Path`) or remove (`os`/`os.path`) imports (tracked by https://github.com/charliermarsh/ruff/issues/835)
- Check if the input type for `os.path.stats` is not `int` ([open file descriptors](https://docs.python.org/3/library/os.html#os.stat) are not supported in the pathlib version). My guess is that this feature is probably not widely used, and people could use `os.path.lstat` to prevent lints (next to #noqa).
- Alias PTH2XX with their corresponding FURB codes (after mapping is merged: https://github.com/charliermarsh/ruff/pull/2517)
- ~~Naming convention: rename from pathlib-* to os(-path)-*~~


---

_@charliermarsh reviewed on 2023-01-30 22:24_

---

_Review comment by @charliermarsh on `src/rules/flake8_use_pathlib/fixes.rs`:17 on 2023-01-30 22:24_

I think we should guard all of these to only autofix if `Path` is imported and available as `Path`. (Are we already doing that? I admittedly haven't read the PR through fully.)

---

_@Kilo59 reviewed on 2023-01-31 03:36_

---

_Review comment by @Kilo59 on `src/rules/flake8_use_pathlib/fixes.rs`:17 on 2023-01-31 03:36_

Just my 2 cents.
I think `Path` could be somewhat ambiguous. 
I tend always to use the fully qualified `pathlib.Path` version.

I've been bitten at least once before by seeing a `Path` object, assuming it's a `pathlib.Path` object only to find out it's something like a `glom.Path`.
https://glom.readthedocs.io/en/latest/api.html#glom.Path
There's a least one other popular library that uses a `Path` object but I'm having trouble remembering it. 😅 

Very excited about these auto-fixes BTW 🚀 ❤️ 

---

_@charliermarsh reviewed on 2023-01-31 03:54_

---

_Review comment by @charliermarsh on `src/rules/flake8_use_pathlib/fixes.rs`:17 on 2023-01-31 03:54_

Yeah we have the ability to ensure that whatever's referenced is indeed `pathlib.Path`, even in the following cases:

```py
import pathlib
pathlib.Path  # OK!

from pathlib import Path
Path  # OK!

from pathlib import Path as P
P  # OK!

Path = 1 + 2
Path  # Not OK!
```

So we _should_ be able to support and preserve whatever import style is used.

---

_@sbrugman reviewed on 2023-01-31 10:13_

---

_Review comment by @sbrugman on `src/rules/flake8_use_pathlib/fixes.rs`:17 on 2023-01-31 10:13_

Will introduce the guarding, great suggestion. Thank you. Same for the testing for import (we already check if the fully qualified name is pathlib.Path), the replacement should now also take that into account.

Related, but could also open a dedicated issue for them:
- How is the user notified of this behaviour? I suggest expanding the autofix documentation to include these kinds of directions.

- Say the user would like to autofix the code base, should they first do a manual pass through the code to add all imports? This seems suboptimal from a user perspective. There is a similar challenge with future annotations, with the difference that they become obsolete when the python target increases.

---

_Review comment by @charliermarsh on `src/rules/flake8_use_pathlib/fixes.rs`:17 on 2023-01-31 13:00_

(We have some logic for this in `src/rules/pylint/rules/use_sys_exit.rs`, hopefully it can be generalized.)

Hmm. I'm not sure I have great answers. The best long-term solution, probably, would be to fix #835 so we _can_ add the import as part of the fix.


---

_@charliermarsh reviewed on 2023-01-31 13:00_

---

_Label `autofix` added by @charliermarsh on 2023-02-10 04:13_

---

_Review comment by @sbrugman on `src/rules/flake8_use_pathlib/fixes.rs`:17 on 2023-02-11 00:55_

(Still working on this btw)

---

_@sbrugman reviewed on 2023-02-11 00:55_

---

_Converted to draft by @sbrugman on 2023-02-11 10:11_

---

_Renamed from "flake8-use-pathlib autofix and new rules" to "[`flake8-use-pathlib`] autofix and new rules" by @sbrugman on 2023-02-11 15:48_

---

_Comment by @charliermarsh on 2023-03-30 02:12_

We should finally be able to support this once #3787 lands.

---

_Comment by @aqeelat on 2023-06-06 09:15_

@sbrugman just to remind you that #3787 was merged and I think you can get the ball rolling again on this PR.

---

_Comment by @sbrugman on 2023-06-06 09:23_

Anyone interested, feel free to pick up where this PR left off (@aqeelat perhaps?). I'd like to work on it, but do not expect it to fit soon.

---

_Comment by @aqeelat on 2023-06-26 16:55_

> Anyone interested, feel free to pick up where this PR left off (@aqeelat perhaps?). I'd like to work on it, but do not expect it to fit soon.

I honestly have 0 rust experience and I'm not comfortable taking over.

---

_Comment by @charliermarsh on 2023-07-21 03:26_

@sbrugman - Should we close this out given that it's being ported over in chunks to new PRs?

---

_Closed by @sbrugman on 2023-07-21 03:28_

---

_Branch deleted on 2023-12-10 21:01_

---
