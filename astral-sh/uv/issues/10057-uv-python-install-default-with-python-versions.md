```yaml
number: 10057
title: uv python install --default with .python-versions misbehaves
type: issue
state: open
author: jooon
labels: []
assignees: []
created_at: 2024-12-20T15:16:19Z
updated_at: 2024-12-20T15:17:58Z
url: https://github.com/astral-sh/uv/issues/10057
synced_at: 2026-01-10T04:36:21Z
```

# uv python install --default with .python-versions misbehaves

---

_Issue opened by @jooon on 2024-12-20 15:16_

I know that uv does not allow you to specify multiple python versions on the command-line with --default. But specifying no version on the command-line and having multiple versions in .python-versions works. If you do that, then the symlinks are not consistent.

```
$ uv --version
uv 0.5.11
$ uv python uninstall --all
Searching for Python installations
No Python installations found
$ echo -e '3.12.6\n3.12.7\n3.12.5\n3.11.6\n3.11.7\n3.11.5' > .python-versions
$ uv python install --preview --default
Installed 6 versions in 3.41s
 + cpython-3.11.5-linux-x86_64-gnu
 + cpython-3.11.6-linux-x86_64-gnu
 + cpython-3.11.7-linux-x86_64-gnu (python3.11)
 + cpython-3.12.5-linux-x86_64-gnu (python3.12)
 + cpython-3.12.6-linux-x86_64-gnu (python, python3)
 + cpython-3.12.7-linux-x86_64-gnu
$ echo -e '3.12.6\n3.12.7\n3.12.5' > .python-versions
$ uv python install --preview --default
Installed 6 versions in 50ms
 + cpython-3.11.5-linux-x86_64-gnu (python3.11)
 + cpython-3.11.6-linux-x86_64-gnu
 + cpython-3.11.7-linux-x86_64-gnu
 + cpython-3.12.5-linux-x86_64-gnu (python3.12)
 + cpython-3.12.6-linux-x86_64-gnu
 + cpython-3.12.7-linux-x86_64-gnu
```

It seems to symlink python and python3 to the first version in the list and the last patch version to finish downloading as minor version. When you run it again and all versions are already downloaded it picks the lowest patch as minor version.

I am not sure what I want to happen. I assumed it would pick the highest version listed like it does without --default or maybe the first minor version in the list.

One way to fix this could be to not allow a bare --default without a single version.

I am also personally not sure why .python-versions exists or why one would have different patch versions of the same minor version, but I noticed this because I had accidentally changed directory to the uv repository where you have this file https://github.com/astral-sh/uv/blob/55502842c0117342c33266bad614c77d78ffd4bc/.python-versions when I just ran `uv python install --preview --default` and wondered why 3.8.12 was symlinked to python3.8 and not  3.8.18.

---

_Assigned to @zanieb by @zanieb on 2024-12-20 15:17_

---

_Comment by @zanieb on 2024-12-20 15:17_

I initially implemented this to take the first version in the list then later we decided we wouldn't allow it. I must have missed the `.python-versions` case — will look into it.

---
