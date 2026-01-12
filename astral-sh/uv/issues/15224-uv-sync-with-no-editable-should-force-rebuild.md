```yaml
number: 15224
title: "`uv sync` with `--no-editable` should force rebuild/install packages"
type: issue
state: open
author: jackrosenthal
labels:
  - question
assignees: []
created_at: 2025-08-11T17:41:40Z
updated_at: 2025-10-27T06:36:28Z
url: https://github.com/astral-sh/uv/issues/15224
synced_at: 2026-01-12T16:02:06Z
```

# `uv sync` with `--no-editable` should force rebuild/install packages

---

_@jackrosenthal_

### Summary

When running `uv sync` with `--no-editable`, it seems like we'd expect that we re-install packages that would've been installed as editable (for example, the main package).  Here's a simple example:

```shellsession
jrosenth ~/example-project % uv init --build-backend uv
Initialized project `example-project`
jrosenth ~/example-project % uv sync --no-editable
Using CPython 3.13.0
Creating virtual environment at: .venv
Resolved 1 package in 2ms
      Built example-project @ file:///home/jrosenth/example-project
Prepared 1 package in 15ms
Installed 1 package in 1ms
 + example-project==0.1.0 (from file:///home/jrosenth/example-project)
jrosenth ~/example-project % echo 'print("changed!")' >> src/example_project/__init__.py 
jrosenth ~/example-project % uv sync --no-editable                                      
Resolved 1 package in 0.88ms
Audited 1 package in 0.04ms
jrosenth ~/example-project % tail .venv/lib/python3.13/site-packages/example_project/__init__.py 
def main() -> None:
    print("Hello from example-project!")
jrosenth ~/example-project % uv sync --no-editable --reinstall-package example-project           
Resolved 1 package in 0.94ms
      Built example-project @ file:///home/jrosenth/example-project
Prepared 1 package in 15ms
Uninstalled 1 package in 0.40ms
Installed 1 package in 1ms
 ~ example-project==0.1.0 (from file:///home/jrosenth/example-project)
jrosenth ~/example-project % tail .venv/lib/python3.13/site-packages/example_project/__init__.py 
def main() -> None:
    print("Hello from example-project!")
print("changed!")
```

In this example, we notice the `example-project` package didn't sync local changes until `--reinstall-package` was manually used.

With packages that have `path` dependencies, this could be a little bit more fishy, we'd expect each of those dependencies to re-build and re-install when `--no-editable` is applied.

### Platform

Linux 6.15.8-arch1-2 x86_64 GNU/Linux

### Version

uv 0.8.8 (9a54754b0 2025-08-08)

### Python version

Python 3.13

---

_Label `bug` added by @jackrosenthal on 2025-08-11 17:41_

---

_Comment by @zanieb on 2025-08-11 18:08_

This seems the same as https://github.com/astral-sh/uv/issues/15190

---

_Comment by @ghost on 2025-08-13 10:39_

This is some sort of caching problem. I guess uv reuses the already built wheel and ignores if there are changes in the project. This issue is still reproducible, if you delete the venv before `uv sync`! 

There are two other workarounds:

- change project version
- delete venv and use `--no-cache`



---

_Comment by @charliermarsh on 2025-08-13 10:57_

Isn't this just consistent with our documented caching behavior?

---

_Comment by @charliermarsh on 2025-08-13 10:57_

https://docs.astral.sh/uv/concepts/cache/#dependency-caching

---

_Comment by @charliermarsh on 2025-08-13 10:58_

We don't rebuild on arbitrary file changes. As an example, you can `touch pyproject.toml` to force a rebuild, because we _do_ rebuild if the `pyproject.toml` is modified.

---

_Label `bug` removed by @charliermarsh on 2025-08-13 10:58_

---

_Label `question` added by @charliermarsh on 2025-08-13 10:58_

---

_Comment by @ghost on 2025-08-13 11:22_

Ah interesting. I guess the `{ git = { commit = true }` cache-keys setting would help for my use case.

However this still behaves pretty unexpected on a dirty repository state. In this case nothing gets updated.

Maybe the correct behavior in case of a dirty repository would be to skip caching for this build?!

---

_Comment by @jackrosenthal on 2025-08-14 04:13_

It seems pretty unintuitive that using `uv sync --no-editable` could result in an older version of your package in the venv, however.  Is it worth special-casing `--no-editable` to force a rebuild of would-be editable packages?

---

_Comment by @mttbernardini on 2025-10-27 06:35_

> It seems pretty unintuitive that using uv sync --no-editable could result in an older version of your package in the venv, however.

I can relate to this as it's been affecting my deployment workflow.

I recently switched from `pdm` to `uv` to manage a workspace project with different deployable members. I then use `shiv` to create a deployable zip-app from a workspace member via a dedicated virtual environment (i.e. not the default `.venv`, but for the sake of this issue it doesn't really matter), installed via `uv sync --no-editable --package <package_to_deploy>`.

I found it really surprising that changing the source code (of either the package or any of its in-worspace dependencies) and then re-triggering my deployment pipeline ends up with a stale artifacts that includes none of the changes made, because of the aggressive cache behaviour of `uv sync --no-editable`.

I can understand `pyproject.toml`-based caching makes sense for editable installs since changes in `src/` would be picked up automatically. But that's not the case for non-editable installs, and having `uv sync --no-editable` not rebuild the sources feels like a broken design to me (or at least it doesn't follow the principle of least surprise).

>  Is it worth special-casing --no-editable to force a rebuild of would-be editable packages?

Or perhaps hash the source files (e.g. by default search for a `src/` directory and hash its contents), possibly with a configuration option orthogonal to `tool.uv.cache-keys` for alternative non-default scenarios? (e.g. flat layouts, where `src/` is not used). In this way you still retain caching for sources that didn't change (which is convenient for a workspace with multiple members).

---
