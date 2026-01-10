```yaml
number: 7435
title: "uv add `numpy` on legacy PyPI mirror does not respect Python version"
type: issue
state: closed
author: cambriancoder
labels:
  - needs-mre
assignees: []
created_at: 2024-09-16T17:49:52Z
updated_at: 2024-10-14T17:34:57Z
url: https://github.com/astral-sh/uv/issues/7435
synced_at: 2026-01-10T04:45:10Z
```

# uv add `numpy` on legacy PyPI mirror does not respect Python version

---

_Issue opened by @cambriancoder on 2024-09-16 17:49_

platform: `linux`
`uv` version: `0.4.10`
caveat: I'm using a private mirror (Nexus) for PyPI that only supports the "simple" repository API, this not an issue when using the public PyPI. 

Running `uv add numpy` when the Python version is set to `>=3.9` fails. 

`uv` will discover the latest version of `numpy` is `2.1.1` but then fail to find any wheels for py39 (`numpy` 2.1.* dropped py39 support), causing it to attempt to build from source, which then fails. 

Adding `--no-build-package numpy` also fails because it still resolves the version to `2.1.1` and then fails to find compatible wheels. Explicitly setting `numpy<2.1.0` does succeed in adding the package but requires I know ahead of time what python versions a package supports.

I would expect `uv add` to attempt to find the latest compatible version? For comparison, running the same command with `poetry` succeeds in adding numpy to the project with version 2.0.2 (albeit 100x slower). 

Is this is known issue? Is `poetry` doing something terribly inefficient, like downloading every version until it finds one that works?


---

_Comment by @charliermarsh on 2024-09-16 17:56_

I'm guessing that the API response from the mirror doesn't include `requires-python`? I don't quite know what Poetry would be doing there. Could you share the logs (redacted is fine) with `--verbose`?

---

_Label `needs-mre` added by @charliermarsh on 2024-09-16 17:56_

---

_Comment by @cambriancoder on 2024-09-17 21:24_

I can't provide the logs sorry, the network is air-gapped, but I did check the HTML output for the package and it doesn't provide a `requires-python` (or `data-requires-python`) tag. 


---

_Comment by @cambriancoder on 2024-09-25 21:15_

@charliermarsh Sorry for the delay on this.

I've managed to recreate the bug using a copy of the numpy index from PyPI with the `data-requires-python` tags removed, and serving that using FastAPI (with a redirect for all other packages). It's here if you want to take a look and try it yourself: https://github.com/cambriancoder/uv/tree/bug/issue-7435-numpy-install-py39/pypi-mock 

The only `uv` command that will install `numpy` successfully for python39 is `uv pip install numpy --only-binary=:all:`: 

```shell
uv add numpy
```
#### Fails
`uv` attempts to build from source

---

```shell
uv pip install numpy
```
#### Fails
`uv` attempts to build from source

---

```shell
uv add numpy --no-build-package numpy
```
#### Fails
`uv` will fail to find a compatible wheel for python3.9 and error out

---

```shell
uv pip install numpy --only-binary=:all:
```
#### Succeeds
`uv` will find and install the latest compatible wheel for python3.9 (`numpy@2.0.2`)

Can we have an `only-binary` equivalent for the `uv add` workflow, and/or is there a bug with the `--no-build-package` flow?

---

_Closed by @cambriancoder on 2024-10-14 17:34_

---
