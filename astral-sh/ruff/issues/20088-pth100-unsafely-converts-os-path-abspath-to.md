---
number: 20088
title: "PTH100 unsafely converts `os.path.abspath` to `pathlib.Path.resolve`"
type: issue
state: closed
author: dscorbett
labels:
  - fixes
  - preview
assignees: []
created_at: 2025-08-25T19:08:56Z
updated_at: 2025-08-26T18:59:14Z
url: https://github.com/astral-sh/ruff/issues/20088
synced_at: 2026-01-10T01:23:01Z
---

# PTH100 unsafely converts `os.path.abspath` to `pathlib.Path.resolve`

---

_Issue opened by @dscorbett on 2025-08-25 19:08_

### Summary

The fix for [`os-path-abspath` (PTH100)](https://docs.astral.sh/ruff/rules/os-path-abspath/) converts `os.path.abspath` to `pathlib.Path.resolve`, which does not do the same thing. Sometimes, as in [upstream flake8-use-pathlib issue #8](https://gitlab.com/RoPP/flake8-use-pathlib/-/issues/8), `pathlib.Path.absolute` is the preferred replacement, but there is no exact equivalent; see https://github.com/python/cpython/issues/69200. The documentation for PTH100 should explain this and the fix should be marked unsafe.

| Function | Removes `..`? | Resolves symlinks? |
| --- | --- | --- |
| `abspath` | ✅ | ❌ |
| `resolve` | ✅ | ✅ |
| `absolute` | ❌ | ❌ |

[Example](https://play.ruff.rs/3a148d9d-e0a8-4282-a692-cd8de79492f2):
```console
$ cat >pth100.py <<'# EOF'
import os
open("pth100_src", "w").close()
os.symlink("pth100_src", "pth100_dst")
os.mkdir("pth100_dir")
print(str(os.path.abspath("pth100_dir/../dst")).removeprefix(f"{os.getcwd()}/"))
# EOF

$ rm -fr pth100_{dir,dst,src}

$ python pth100.py
dst

$ ruff --isolated check pth100.py --select PTH100 --preview --fix
Found 1 error (1 fixed, 0 remaining).

$ cat pth100.py
import os
import pathlib
open("pth100_src", "w").close()
os.symlink("pth100_src", "pth100_dst")
os.mkdir("pth100_dir")
print(str(pathlib.Path("pth100_dir/../dst").resolve()).removeprefix(f"{os.getcwd()}/"))

$ rm -fr pth100_{dir,dst,src}

$ python pth100.py
src
```

### Version

ruff 0.12.10 (c68ff8d90 2025-08-21)

---

_Comment by @chirizxc on 2025-08-26 07:45_

I'm fix it

---

_Label `fixes` added by @ntBre on 2025-08-26 12:43_

---

_Label `preview` added by @ntBre on 2025-08-26 12:43_

---

_Referenced in [astral-sh/ruff#20100](../../astral-sh/ruff/pulls/20100.md) on 2025-08-26 16:24_

---

_Comment by @ntBre on 2025-08-26 17:07_

It looks like the [Corresponding tools](https://docs.python.org/3/library/pathlib.html#corresponding-tools) section of the `pathlib` docs now recommend `Path.absolute` as the replacement for `os.path.abspath` instead of `Path.resolve`. Related to that, the [Correspondence between os and pathlib](https://docs.python.org/3/library/pathlib.html#correspondence-to-tools-in-the-os-module) link appears to be broken in our PTH100 docs. This table used to equate `abspath` and `resolve` on [3.10](https://docs.python.org/3.10/library/pathlib.html#correspondence-to-tools-in-the-os-module) but pairs up `abspath` and `absolute` after that.

It seems like we could update our recommendation to `Path.absolute`, but marking the fix unsafe and extending the docs makes sense as a good start since neither case is really equivalent.

---

_Closed by @ntBre on 2025-08-26 18:59_

---
