---
number: 3878
title: Support custom tool entrypoints
type: issue
state: open
author: konstin
labels: []
assignees: []
created_at: 2024-05-28T13:29:22Z
updated_at: 2024-08-20T18:23:33Z
url: https://github.com/astral-sh/uv/issues/3878
synced_at: 2026-01-10T01:23:31Z
---

# Support custom tool entrypoints

---

_Issue opened by @konstin on 2024-05-28 13:29_

The package `build` doesn't install an entrypoint, you are supposed to call it as `pythom -m build` instead. `pipx run build .` while `uv tool run build .` fails. We should support whatever workaround pipx is using, too.

---

_Label `preview` added by @konstin on 2024-05-28 13:29_

---

_Comment by @charliermarsh on 2024-05-29 02:21_

I think `build` implements this special entrypoint thing for `pipx` exactly: https://github.com/pypa/build/blob/46337d95bcb6be84eb2b0416cff5bf51ba70d9c5/pyproject.toml#L84

---

_Comment by @njzjz on 2024-05-29 07:05_

Workaround:

```sh
uv tool run --with build --from build python -m build
```

One may also want to use `uv` in the `build`: (the command is pretty long!)

```sh
uv tool run --with build[uv] --from build python -m build --installer uv
```

---

_Renamed from "Run tool as module when there is no entrypoint" to "Support custom tool entrypoints" by @konstin on 2024-05-29 07:09_

---

_Comment by @T-256 on 2024-07-12 15:29_

Declaring custom entry points would be nice to support those projects didn't expose any entry point names and only allows to use them as module i.e `python -m <mod>`.
I think this could be allowed in cli (which is support multiple use):
```shell
# make `cmd` available which runs `mod.__main__`
$ uvx --from pkg --with-script "cmd='mod.__main__'" cmd
```
Also, for `build` it turns to:
```shell
$ uvx --from build --with-script "build='build.__main__:entrypoint'" build
```



---

_Comment by @charliermarsh on 2024-07-12 15:40_

You can already run `uvx --from build pyproject-build`

---

_Comment by @charliermarsh on 2024-07-12 15:43_

And in the next version, we'll surface that this executable is available, which seems good...

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---
