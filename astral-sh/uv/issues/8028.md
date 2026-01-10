```yaml
number: 8028
title: "FR: List and repair broken `uv tool` venvs (e.g. after a python upgrade)."
type: issue
state: open
author: ilyagr
labels:
  - enhancement
  - uv tool
assignees: []
created_at: 2024-10-09T00:48:52Z
updated_at: 2024-10-09T08:44:15Z
url: https://github.com/astral-sh/uv/issues/8028
synced_at: 2026-01-10T04:45:10Z
```

# FR: List and repair broken `uv tool` venvs (e.g. after a python upgrade).

---

_Issue opened by @ilyagr on 2024-10-09 00:48_

In the transcript below, I upgrade from Python 3.12.6 to 3.12.7 and all my `uv tool install`-ed tools break because the symlinks to python are broken. The problem is that `uv` can't handle these broken environments very well.

Currently, `uv tool upgrade` does not work as seen in the transcript. `uv tool install` works, but does not remember how the tool was installed originally, which is a problem if the tool needs a non-trivial command line to be installed correctly, e.g.:

```shell
# Commands like these are needed to work around `uv tool upgrade`
# not working.
uv tool install xonsh[full]
uv tool install marimo[sql] --with polars --with pandas  --with pyarrow \
                            --with matplotlib --with altair --with ruff
uv tool install --from \
   https://github.com/jjlee/git-meld-index/archive/release.zip git-meld-index
```

I would like either a `uv tool repair` command to repair such tools or for `uv tool upgrade` to be able to repair them. I'd also like `uv tool list` to work better for such tools.

Fortunately, the `uv-receipt.toml` file in the tool's venv already has all the needed info, so I'm guessing some feature like this was planned. (Let me know if there's already a way to do it that I missed!)

Related: https://github.com/astral-sh/uv/issues/1640, #7287. I feel like, even if those issues are solved, it'd still be nice if `uv` handled broken tool venvs better.

Also related: #7892 

### Transcript illustrating the problem

```console
$ uv --version  # I'm on Mac OS on ARM
uv 0.4.20 (0e1b25a53 2024-10-08)
$ uv tool list
git-meld-index v0.2.4
- git-meld-index
- git-meld-index-run-merge-tool
marimo v0.9.4
- marimo
poetry v1.8.3
- poetry
pxpx v3.6.5
- ptop
- px
- pxtree
xonsh v0.18.3
- xonsh
- xonsh-cat
- xonsh-uname
- xonsh-uptime
$ uv python install 3.12
Searching for Python versions matching: Python 3.12
Found existing installation for Python 3.12: cpython-3.12.6-macos-aarch64-none
$ uv python install 3.12 --reinstall
Searching for Python versions matching: Python 3.12
Found existing installation for Python 3.12: cpython-3.12.6-macos-aarch64-none
Installed Python 3.12.7 in 1.38s
 - cpython-3.12.6-macos-aarch64-none
 + cpython-3.12.7-macos-aarch64-none
$ marimo
exec: Failed to execute process '/Users/ilyagr/.local/bin/marimo': The file specified the interpreter '/Users/ilyagr/.local/share/uv/tools/marimo/bin/python', which is not an executable command.
$ uv tool upgrade marimo --python 3.12
`marimo` is not installed; run `uv tool install marimo` to install
$ uv tool list
Python interpreter not found at `/Users/ilyagr/.local/share/uv/tools/git-meld-index/bin/python3`
Python interpreter not found at `/Users/ilyagr/.local/share/uv/tools/marimo/bin/python3`
Python interpreter not found at `/Users/ilyagr/.local/share/uv/tools/poetry/bin/python3`
Python interpreter not found at `/Users/ilyagr/.local/share/uv/tools/pxpx/bin/python3`
Python interpreter not found at `/Users/ilyagr/.local/share/uv/tools/xonsh/bin/python3`
$ uv tool upgrade --all --python 3.12
Failed to upgrade `git-meld-index`: `git-meld-index` is not installed; run `uv tool install git-meld-index` to install
Failed to upgrade `marimo`: `marimo` is not installed; run `uv tool install marimo` to install
Failed to upgrade `poetry`: `poetry` is not installed; run `uv tool install poetry` to install
Failed to upgrade `pxpx`: `pxpx` is not installed; run `uv tool install pxpx` to install
Failed to upgrade `xonsh`: `xonsh` is not installed; run `uv tool install xonsh` to install
```




---

_Renamed from "FR: Better handling for broken `uv tool` venvs (e.g. after a python upgrade)." to "FR: List and repair broken `uv tool` venvs (e.g. after a python upgrade)." by @ilyagr on 2024-10-09 00:51_

---

_Comment by @zanieb on 2024-10-09 01:22_

Related:

- https://github.com/astral-sh/uv/issues/4718
- https://github.com/astral-sh/uv/issues/7287

Yeah there's room for improvement here in general.

---

_Comment by @zanieb on 2024-10-09 01:25_

`uv tool repair` sounds kind of cool. 

---

_Label `enhancement` added by @zanieb on 2024-10-09 01:25_

---

_Label `uv tool` added by @zanieb on 2024-10-09 01:25_

---
