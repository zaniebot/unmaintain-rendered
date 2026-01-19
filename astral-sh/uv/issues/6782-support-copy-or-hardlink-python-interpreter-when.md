```yaml
number: 6782
title: Support copy or hardlink python interpreter when creating venv
type: issue
state: open
author: amartani
labels:
  - enhancement
  - cli
assignees: []
created_at: 2024-08-29T00:08:45Z
updated_at: 2026-01-19T07:08:16Z
url: https://github.com/astral-sh/uv/issues/6782
synced_at: 2026-01-19T07:23:49Z
```

# Support copy or hardlink python interpreter when creating venv

---

_@amartani_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Currently, when creating a venv, `uv` always symlinks the python interpreter. Example:

```
$ uv venv --python=3.12 /venv
$ ls -l /venv/bin
...
lrwxrwxrwx 1 root root   25 Aug 28 23:53 python -> /root/.local/share/uv/python/cpython-3.12.5-linux-aarch64-gnu/bin/python3.12
lrwxrwxrwx 1 root root    6 Aug 28 23:53 python3 -> python
lrwxrwxrwx 1 root root    6 Aug 28 23:53 python3.12 -> python
```

The original `python -m venv` supports either symlink or copy (with `--symlinks` / `--copies` flag). It would be great if `uv` also supported the same, similar to how `--link-mode` exists for `uv pip`.

My main use case is for using a docker multi-stage build where one of the stages builds the venv and then other stages can just copy the entire venv folder; example Dockerfile:

```
FROM base as venv-builder

RUN uv venv --python=3.12 /venv
RUN uv pip install --python /venv/bin/python ...

FROM base as final-image

# Do other things here

COPY --from=venv-builder /venv /venv
```

This use case currently breaks as the symlink ends up pointing to a python binary that does not exist in the final image. For this use case, the ideal behavior would be that the main `/venv/bin/python` binary was copied or hardlinked from the external binary and `python3`/`python3.12` continued to be relative symlinks.

Tested on uv version 0.4.0.

---

_Comment by @charliermarsh on 2024-08-29 00:14_

This makes sense to me.

---

_Label `enhancement` added by @charliermarsh on 2024-08-29 01:36_

---

_Label `cli` added by @charliermarsh on 2024-08-29 01:36_

---

_Comment by @zanieb on 2024-08-29 03:28_

Minor note, this is included as `--bundle-python` in @konstin's document about features we should add for Docker.

---

_Comment by @SethWen on 2024-09-12 09:49_

Sometimes, I want to package the Python source code and the virtual environment (venv) together for direct distribution. In this case, the interpreter should not be a symbolic link, but a full copy of the original interpreter. and I hope to have an option for this.
Currently, I am using a mix of conda and pip. After this feature is added in the future, I plan to replace them with uv.

---

_Comment by @mweislley on 2024-10-25 17:47_

Running into this also - basically the docker case but the applications current make use of `python -m venv .bundle --copies` and I'm not seeing any alternative apart from this issue.

---

_Comment by @charliermarsh on 2024-10-25 17:52_

I think the alternative is you just copy over the interpreter along with the environment.

---

_Comment by @RayyanRiaz on 2024-11-15 17:28_

any updates here? Would really like to have this functionality!

---

_Comment by @MarijkeStein on 2025-01-07 09:45_

Me as well, please 💟 

We use the official Docker images to setup Python environments with 'uv'.

But later on, we need to run the actual scripts under a different Docker image.

The way with having a combined image (donor 'uv' executable) does not really scale, as you would need to build one image per combination of Python interpreter version and Linux distro 🙈 .

---

_Comment by @bennyweise on 2025-01-12 05:37_

+1 for this, for a different use case. Using uv virtual environments with Cursor, Cursor appears to follow the symlink and give me the wrong environment. I'm also following up to see if it's something I can fix within Cursor, but I've had no luck so far.

---

_Comment by @kaytwo on 2025-01-24 18:27_

> +1 for this, for a different use case. Using uv virtual environments with Cursor, Cursor appears to follow the symlink and give me the wrong environment. I'm also following up to see if it's something I can fix within Cursor, but I've had no luck so far.

Same in vscode, at least for me.

---

_Comment by @Vigilans on 2025-02-14 12:55_

Now with `uv venv --relocatable`, the last obstacle to a copiable virtual environment is base python's installation dir: `python` executable symlink's path and `home` field in `pyvenv.cfg`.

I attempted to address it in commit https://github.com/astral-sh/uv/commit/b79ab015e40f3812ee138f5180a22d4873cb6449, to use relative path for `python` symlink and `home` field, when `uv venv` is provided with:
* Relocatable mode: `--relocatable`
* A relative path to python installation: `--python <../relative/path/to/python>`

The `python` executable does not bring issue, however `home` in `pyvenv.cfg` does: when `home` path is relative, it is relative to `python` process's working directory, instead of the location of `pyvenv.cfg`, see https://github.com/python/cpython/issues/83650. 

This is essentially a deal breaker. One workaround proposed in [Stack Overflow](https://stackoverflow.com/questions/51989490/venv-home-key-in-pyvenv-cfg) suggests to modify `bin/activate` script to update `home` field when activating the environment. This may work in most circumstances, but cannot be proposed as a general solution for `uv`.  I wonder whether `home` field here will still be an issue when implementing the feature in this issue.

---

_Comment by @Vigilans on 2025-07-28 15:56_

Support for relative home path in `pyvenv.cfg` is now being discussed in PEP 796: 
* https://github.com/python/peps/pull/4476
* https://github.com/python/cpython/issues/136051

Once it gets accepted, I can make a PR for https://github.com/Vigilans/uv/commit/b79ab015e40f3812ee138f5180a22d4873cb6449 to make uv venv fully portable when provided with relative python path in relocatable mode.

(As for the original request in this issue, [`python -m venv --copies` seems to be a no-op in non-windows platform](https://stackoverflow.com/questions/60193899/venv-not-respecting-copies-argument#comment106469051_60193899), and there seems to be more things than hardlinking/copying python binary alone in a python installation (e.g. include, lib, and share folder in python-build-standalone)

---

_Comment by @raylu on 2025-10-07 05:02_

noting that this issue covers the same ground as #2103

---

I got what I wanted with some jank: I installed a python and pretended it was a venv
```sh
$ uv python install 3 -i .
$ mv cpython-3.13.7-linux-aarch64-gnu .venv
$ uv pip install jinja2
error: Broken virtual environment `/home/raylu/test/.venv`: `pyvenv.cfg` is missing
$ uv sync
 + jinja2==3.1.6
$ ls -1 .venv/lib/python3.13/site-packages
jinja2
jinja2-3.1.6.dist-info
```
so `uv pip install` thinks it's not a functional venv but `uv sync` is still happy to install my `pyproject.toml`'s dependencies. and there are no symlinks out of the directory so it works as a chroot

---

_Comment by @staticglobal on 2025-10-14 16:10_

> (As for the original request in this issue, [`python -m venv --copies` seems to be a no-op in non-windows platform](https://stackoverflow.com/questions/60193899/venv-not-respecting-copies-argument#comment106469051_60193899), and there seems to be more things than hardlinking/copying python binary alone in a python installation (e.g. include, lib, and share folder in python-build-standalone)

This is false. On my Linux system if I run without `--copies` I get this in `bin`
```sh
lrwxrwxrwx 1 490 490   38 Oct 14 16:04 python -> /usr/lib/python-exec/python3.13/python
lrwxrwxrwx 1 490 490    6 Oct 14 16:04 python3 -> python
lrwxrwxrwx 1 490 490    6 Oct 14 16:04 python3.13 -> python
```

But with `--copies` I get this:
```sh
-rwxr-xr-x 1 490 490 14344 Oct 14 16:05 python
-rwxr-xr-x 1 490 490 14344 Oct 14 16:05 python3
-rwxr-xr-x 1 490 490 14344 Oct 14 16:05 python3.13
```

I'm not sure what OP's issue was in that SO post, but seems like it might have been related to `lib` vs `lib64` on his machine, not `bin`. Then commenter un-helpfully misinterpreted the python source code. A closer read will show that `--copies` is a no-op _on Windows platforms_ rather than _non-Windows platforms_.

There is still great merit in the original request.

---

_Comment by @cyber-barrista on 2025-11-16 01:55_

For anyone looking for a workaround...

Running
```
uv venv --clear && find .venv -type l -exec sh -c 'target=$(readlink -f "$1"); rm "$1"; cp -a "$target" "$1"' _ {} \;
```
instead of just
```
uv venv --clear
```
does the trick

---

_Comment by @theis188 on 2026-01-05 16:10_

+1 to this request. I'm managing a persistent venv system for ML engineers, and broken symlinks are a problem. It would be awesome to have functionality that replicates --copies from venv so that venvs don't rely on central Python interpreters.

@cyber-barrista, tried the above and looks like `lib/libpython3.11.dylib` was missing.

---

_Comment by @bakkiaraj on 2026-01-19 07:08_

+1 , I do need a real , redistributeable VENV 

---
