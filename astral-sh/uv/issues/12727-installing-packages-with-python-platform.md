```yaml
number: 12727
title: installing packages with --python-platform sometimes fails to choose wheel file
type: issue
state: closed
author: falkben
labels:
  - bug
assignees: []
created_at: 2025-04-07T19:17:29Z
updated_at: 2025-04-08T00:15:23Z
url: https://github.com/astral-sh/uv/issues/12727
synced_at: 2026-01-12T16:01:11Z
```

# installing packages with --python-platform sometimes fails to choose wheel file

---

_@falkben_

### Summary

Some packages installed using the `uv pip install` interface, alongside the `python-platform` option, cause uv to not choose the wheel file and instead attempt to install that package from source.

Example inside a docker container, where uv attempts to build from source the `confluent-kafka` package.

```sh
docker run --platform=linux/amd64 --rm -it ghcr.io/astral-sh/uv:python3.12-bookworm-slim /bin/bash
uv venv
uv pip install confluent-kafka -v --python-platform linux
```

If you leave off `--python-platform linux` uv will install the wheel file.

```sh
docker run --platform=linux/amd64 --rm -it ghcr.io/astral-sh/uv:python3.12-bookworm-slim /bin/bash
uv venv
uv pip install confluent-kafka
```

Typically we aren't calling with the option `--python-platform` in this way, but we have it defined in our `uv.toml` config. My understanding from the docs was that this is used for resolution, and we indeed are using it to create `requirements.txt` lock files from arm or linux based max for linux x86_x64. I've also tried setting python-platform to `x86_64-unknown-linux-gnu` and it results in the same behavior.

Why does `uv` prefer source distribution in the above case, and is there a way to get it to use the wheel file instead? Most packages I think get installed from wheels, but another package where this happens is with `numexpr`.

Is there something strange about the wheel files these projects have uploaded?

- https://pypi.org/project/numexpr/2.10.1/#files
- https://pypi.org/project/confluent-kafka/#files

Should the `--python-platform` even be available during install? I would have assumed when installing you would want to resolve the platform independently.

### Platform

Darwin 24.3.0 arm64

### Version

uv 0.6.12

### Python version

Python 3.12.9

---

_Label `bug` added by @falkben on 2025-04-07 19:17_

---

_Comment by @charliermarsh on 2025-04-07 19:25_

I think you should drop `--python-platform`. You very rarely want to override that in `uv pip install`. You typically want it for `uv pip compile --python-platform windows` or something so that you can output a resolution for another platform.

---

_Comment by @charliermarsh on 2025-04-07 19:27_

The reason it's building from source here is that `--python-platform linux` is an alias for `x86_64-manylinux_2_17`, but `confluent-kafka` only contains wheels for `manylinux_2_28` and later. So our default minimum glibc version is lower than that package supports. You could use `--python-platform x86_64-manylinux_2_28` instead.

---

_Comment by @falkben on 2025-04-07 21:22_

Thanks for the suggestion to try `x86_64-manylinux_2_28`. That does indeed work.

> I think you should drop --python-platform. You very rarely want to override that in uv pip install. You typically want it for uv pip compile --python-platform windows or something so that you can output a resolution for another platform.

Yes, we normally aren't including it in our uv pip install commands, but have the python-platform in our `uv.toml`. My intention was to have it in our `uv.toml` files so that it would be present when we compile our lock files, we'd always compile to that platform. I was surprised that it's getting used when we do `uv pip install` as well and that it's so "literal" and won't take the newer wheel files.

Is there config that would only use python-platform for dependency resolution and not for uv pip install?

---

_Comment by @charliermarsh on 2025-04-07 23:06_

Unfortunately no. This has come up before though.

---

_Comment by @falkben on 2025-04-08 00:15_

Okay, thanks. For now I'll take it out of our uv.toml files and only pass it in on the command line when we compile our dependencies.

---

_Closed by @falkben on 2025-04-08 00:15_

---
