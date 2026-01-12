```yaml
number: 2075
title: "`INP001` failing even after adding `__init__.py` to the relevant folder"
type: issue
state: closed
author: StrohMatanya
labels:
  - bug
assignees: []
created_at: 2023-01-21T23:08:10Z
updated_at: 2025-03-24T05:26:01Z
url: https://github.com/astral-sh/ruff/issues/2075
synced_at: 2026-01-12T15:54:42Z
```

# `INP001` failing even after adding `__init__.py` to the relevant folder

---

_@StrohMatanya_

I updated to ruff v0.0.229, and added `INP` to the list of linters I would like to run.
After running the new version it failed with the below error (which was correct);
```
src/xxx.py:1:1: INP001 File `src/xxx.py` is part of an implicit namespace package. Add an `__init__.py`.
```
I added `__init__.py` to the src folder, and yet.. I'm seeing the same error.

I did an experiment, and I added in a different branch `__init__.py` to the src folder before running `ruff`, and indeed there is no `INP001` failure, I then deleted the `__init__.py` from the src file, and this time when I ran `ruff` I didn't encounter any `INP001` failure.
If I'm not missing anything (which usually I do :-)), it seems that something is cached and not being cleared correctly 

---

_Renamed from "`INP001` failing after adding `__init__.py` to the failing folder" to "`INP001` failing even after adding `__init__.py` to the relevant folder" by @StrohMatanya on 2023-01-21 23:09_

---

_Comment by @charliermarsh on 2023-01-21 23:19_

Mmm yeah, good call, this definitely breaks our caching model, since it's a per-file check that depends on _other_ files.

You can run Ruff with `-n` or `--no-cache` for now. It shouldn't be much of a noticeable difference. Let me think on how to fix this.


---

_Label `bug` added by @charliermarsh on 2023-01-21 23:19_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-21 23:24_

---

_Closed by @charliermarsh on 2023-01-21 23:33_

---

_Comment by @charliermarsh on 2023-01-21 23:56_

(This was a bit premature -- #2077 is a good change, but this issue is still possible with that alone.)

---

_Reopened by @charliermarsh on 2023-01-21 23:56_

---

_Closed by @charliermarsh on 2023-01-22 00:49_

---

_Comment by @matthias-roussel-soyhuce on 2023-06-29 08:23_

This seems to be still happening in 0.0.275. I have to run ruff with `--no-cache` or delete the cache manually for the solved INP001 warnings. Has the bug regressed or am I missing something ?

---

_Comment by @charliermarsh on 2023-06-29 08:26_

Probably a regression, we made some changes to the cache recently.

---

_Comment by @charliermarsh on 2023-06-30 02:19_

(Created a new issue: https://github.com/astral-sh/ruff/issues/5449.)

---

_Comment by @rwarren on 2025-03-24 05:26_

This appears like it might have resurfaced again?  In `v0.11.2` I am seeing this:

```
$ ruff check .
profiling/profile_scan.py:1:1: INP001 File `profiling/profile_scan.py` is part of an implicit namespace package. Add an `__init__.py`.
Found 1 error.
$ ruff check . --no-cache
All checks passed!
$ ruff --version
ruff 0.11.2
```

`profiling/__init__.py` legitimately did not exist when I first saw this happen. When I added it it did not get rid of the error.  I tried removing it and replacing it, and adding contents to it, and I don't know what else.  Then I found this bug report and `--no-cache` fixes it.

Since it seems like `--no-cache` will just make Ruff run a tiny bit slower (which is hilarious, since it is SO fast) I'm just going to force `--no-cache` in all usage for now.

---
