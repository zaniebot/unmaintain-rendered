```yaml
number: 16869
title: "error: No interpreter found for Python 3.11.0 in managed installations or search path"
type: issue
state: open
author: kishaningithub
labels:
  - error messages
assignees: []
created_at: 2025-11-27T09:06:15Z
updated_at: 2025-11-28T05:40:51Z
url: https://github.com/astral-sh/uv/issues/16869
synced_at: 2026-01-12T16:02:39Z
```

# error: No interpreter found for Python 3.11.0 in managed installations or search path

---

_@kishaningithub_

### Summary

The command `uv venv --python 3.11.0` fails with the error

```bash
error: No interpreter found for Python 3.11.0 in managed installations or search path
```

Interestingly all the below are working as expected without any errors

```bash
uv venv --python 3.9.0
uv venv --python 3.10.0
uv venv --python 3.12.0
uv venv --python 3.13.0
uv venv --python 3.14.0
```

### Platform

all

### Version

0.9.13

### Python version

3.11.0

---

_Label `bug` added by @kishaningithub on 2025-11-27 09:06_

---

_Comment by @FishAlchemist on 2025-11-27 09:25_

**python-build-standalone** did not build `cpython-3.11.0` and `cpython-3.11.2`, but I am not sure why they were not built.
`uv python list cpython-3.11 --all-versions --only-downloads`
```
cpython-3.11.14-windows-x86_64-none    <download available>
cpython-3.11.13-windows-x86_64-none    <download available>
cpython-3.11.12-windows-x86_64-none    <download available>
cpython-3.11.11-windows-x86_64-none    <download available>
cpython-3.11.10-windows-x86_64-none    <download available>
cpython-3.11.9-windows-x86_64-none     <download available>
cpython-3.11.8-windows-x86_64-none     <download available>
cpython-3.11.7-windows-x86_64-none     <download available>
cpython-3.11.6-windows-x86_64-none     <download available>
cpython-3.11.5-windows-x86_64-none     <download available>
cpython-3.11.4-windows-x86_64-none     <download available>
cpython-3.11.3-windows-x86_64-none     <download available>
cpython-3.11.1-windows-x86_64-none     <download available>
```
I didn't realize I had contributed to answering a similar question before. Simply put, that version was skipped and there are no plans to specially support it.
ref: https://github.com/astral-sh/uv/issues/9394#issuecomment-2496486042

---

_Comment by @FishAlchemist on 2025-11-27 11:30_

However, since it usually isn't supported, have uv considered hardcoding a list of definitely unsupported version to detect them and provide an enhanced error message?
After all, a message like that really looks more like a bug than the expected behavior.

---

_Comment by @zanieb on 2025-11-27 13:30_

I'd be fine with special-casing that or adding a hint. The heuristic is probably just that there's a newer version available with the same minor version?


---

_Label `bug` removed by @zanieb on 2025-11-27 13:30_

---

_Label `error messages` added by @zanieb on 2025-11-27 13:30_

---

_Comment by @FishAlchemist on 2025-11-27 14:02_

> The heuristic is probably just that there's a newer version available with the same minor version?

I believe that's correct. To be more complete, the heuristic is: if a newer version with the same minor version and same implementation exists, inform the user that the selected version is unsupported and suggest they use a specific version (the newest version closest to the selected one), a newer version, or the latest version.

---

_Comment by @konstin on 2025-11-27 15:55_

Are you sure you want 3.11.0 and not 3.11.14? Patch releases are highly compatible, and requesting a `.0` patch release usually only happens through a mistaken specifier, such as `==3.11`, which means `3.11.0`, instead of `==3.11.*`.

---

_Comment by @kishaningithub on 2025-11-28 05:39_

@konstin Ran into a case where that dependency file was not in my control (if it was mine would have bumped it to 3.14 itself)

It was part of a legacy system and cant really update that `.python-version` file of that project even with a minor version bump due to non-technical reasons.

---
