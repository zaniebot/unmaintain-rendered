```yaml
number: 6485
title: Respect output-file symlinks
type: issue
state: closed
author: dpoznik
labels:
  - bug
assignees: []
created_at: 2024-08-23T02:25:32Z
updated_at: 2024-08-23T03:09:47Z
url: https://github.com/astral-sh/uv/issues/6485
synced_at: 2026-01-12T15:59:04Z
```

# Respect output-file symlinks

---

_@dpoznik_

`uv 0.2.36` respected output-file symlinks, whereas `uv 0.3.1` does not.

Let's say we have the following symlink:
```sh
macos.txt  ->  macos.312.txt
```
With `uv 0.2.36`, invoking the following command would update `macos.312.txt`, leaving the symlink in place, as expected.
```sh
uv pip compile macos.in --output-file macos.txt
```

With `uv 0.3.1`, invoking the command removes the symlink and creates a new regular file in its place, `macos.txt`, contrary to expectation.

I did not see a note about this behavior change in the release notes. Was it intentional?
If not, would it be possible to revert this behavior?

Thanks!

---

_Comment by @charliermarsh on 2024-08-23 02:27_

Ah ok, this was _not_ intentional but I see why it changed.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-23 02:27_

---

_Label `bug` added by @charliermarsh on 2024-08-23 02:27_

---

_Comment by @charliermarsh on 2024-08-23 02:27_

I'll mark it as a bug.

---

_Closed by @charliermarsh on 2024-08-23 02:54_

---

_Comment by @dpoznik on 2024-08-23 03:09_

Thanks @charliermarsh!

---
