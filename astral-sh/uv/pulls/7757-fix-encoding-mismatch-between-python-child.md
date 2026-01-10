```yaml
number: 7757
title: Fix encoding mismatch between python child process and uv
type: pull_request
state: merged
author: kahojyun
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: fix-utf8
created_at: 2024-09-28T12:32:33Z
updated_at: 2024-10-11T04:27:43Z
url: https://github.com/astral-sh/uv/pull/7757
synced_at: 2026-01-10T12:53:55Z
```

# Fix encoding mismatch between python child process and uv

---

_Pull request opened by @kahojyun on 2024-09-28 12:32_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR fixes #7733. According to [CPython documentation on `sys.stdout`](https://docs.python.org/3.12/library/sys.html#sys.stdout), when `stdout`/`stderr` is non-character device like pipe, the encoding will be set to system locale on windows. However, on the Rust side `stdout_reader` and `stderr_reader` expect them to be encoded in UTF-8 and will fail when child process write non-ASCII character to stdout/stderr, e.g., build directory name containing non-ASCII character.

Both [CPython3](https://docs.python.org/3.12/using/cmdline.html#envvar-PYTHONIOENCODING) and [PyPy](https://doc.pypy.org/en/default/man/pypy3.1.html#environment) support environment variable `PYTHONIOENCODING`. When it is set to `utf-8`, python will use UTF-8 encoding for `stdin`/`stdout`/`stderr`. Since `stdin` is not used by the spawned python process and we expect `stdout`/`stderr` to use UTF-8, this fix should work as expected.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

I only tested it on my computer with CPython 3.12 and 3.7. With the fix applied I confirmed that [the case I described](https://github.com/astral-sh/uv/issues/7733#issuecomment-2380416093) is fixed.

I'm using Windows 11 with system locale set to code page 936.
<!-- How was it tested? -->


---

_Label `bug` added by @charliermarsh on 2024-09-28 12:34_

---

_Label `windows` added by @charliermarsh on 2024-09-28 12:34_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-28 12:34_

---

_Comment by @charliermarsh on 2024-09-28 12:34_

\cc @BurntSushi 

---

_@BurntSushi approved on 2024-09-28 12:35_

This sounds right to me. Nice catch.

---

_Comment by @charliermarsh on 2024-09-28 12:38_

Great find, thanks!

---

_Merged by @charliermarsh on 2024-09-28 12:39_

---

_Closed by @charliermarsh on 2024-09-28 12:39_

---

_Branch deleted on 2024-10-11 04:27_

---
