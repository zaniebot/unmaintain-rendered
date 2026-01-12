```yaml
number: 11213
title: "new docker image bookworm-slim makes `uv build` fail"
type: issue
state: closed
author: Morgadoooo
labels:
  - bug
assignees: []
created_at: 2025-02-04T12:11:01Z
updated_at: 2025-02-04T17:38:24Z
url: https://github.com/astral-sh/uv/issues/11213
synced_at: 2026-01-12T16:00:31Z
```

# new docker image bookworm-slim makes `uv build` fail

---

_@Morgadoooo_

### Summary

Hi, 

I'm running CI on gitlab using their saas runners (linux amd64)

I don't know if i'm the only one here but I noticed that since latest docker images have dropped my CI are failing to `uv build`.

Tried to run without cache / with different python version (bookworm-slim 3.8 / 3.9 / 3.12) 

```bash
> uv build
Building source distribution...
  × Failed to build `/builds/test/a_package`
  ├─▶ failed to unpack
  │   `/builds/test/a_package/.uv-cache/sdists-v7/.tmp1n3mUq/test/a_package-1.0.0/.uv-cache/archive-v0/xnIKTmXwjjeTGFKmNQ04g/lib/python3.12/site-packages/typing_extensions.py`
  ╰─▶ No such file or directory (os error 2) while canonicalizing
      /builds/test/a_package/.uv-cache/sdists-v7/.tmp1n3mUq/test/a_package-1.0.0/.uv-cache/archive-v0/xnIKTmXwjjeTGFKmNQ04g/lib/python3.12/site-packages/a_package-1.0.0/.uv-cache/archive-v0/fQx_zMdWQeLktREBUY0-Z/typing_extensions.py
```

Using the ghcr.io/astral-sh/uv:0.5.26-python3.12-bookworm-slim does work for me


### Platform

linux amd64

### Version

uv 0.5.27

### Python version

3.12

---

_Label `bug` added by @Morgadoooo on 2025-02-04 12:11_

---

_Renamed from "new bookworm make my `uv build` failed pipelines" to "new docker image bookworm-slim makes `uv build` fail" by @Morgadoooo on 2025-02-04 12:11_

---

_Comment by @konstin on 2025-02-04 12:50_

What build system and build system settings are you using (the `[build-system]` table)? Can your (redacted) pyproject.toml?

---

_Comment by @Morgadoooo on 2025-02-04 12:52_

Yes sorry forgot to forward it with the initial message
```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

---

_Comment by @konstin on 2025-02-04 13:02_

Unfortunately i can't reproduce this:

```
$ docker run --rm -it ghcr.io/astral-sh/uv:0.5.27-python3.12-bookworm-slim bash
root@tempid:/# mkdir -p /builds/test
root@tempid:/# cd /builds/test
root@tempid:/builds/test# uv init --lib a_package
Initialized project `a-package` at `/builds/test/a_package`
root@tempid:/builds/test# cd a_package/
root@tempid:/builds/test/a_package# uv build
Building source distribution...
Building wheel from source distribution...
Successfully built dist/a_package-0.1.0.tar.gz
Successfully built dist/a_package-0.1.0-py3-none-any.whl
root@tempid:/builds/test/a_package# cat pyproject.toml 
[project]
name = "a-package"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

---

_Label `needs-mre` added by @konstin on 2025-02-04 13:02_

---

_Comment by @Morgadoooo on 2025-02-04 13:33_

The problem might be on our end but just to be sure
```bash
docker run --rm -it ghcr.io/astral-sh/uv:0.5.27-python3.12-bookworm-slim bash
Unable to find image 'ghcr.io/astral-sh/uv:0.5.27-python3.12-bookworm-slim' locally
0.5.27-python3.12-bookworm-slim: Pulling from astral-sh/uv
root@3d4d403b95e6:/# mkdir -p test
root@3d4d403b95e6:/# cd test/
root@3d4d403b95e6:/test# uv init --lib a_package
Initialized project `a-package` at `/test/a_package`
root@3d4d403b95e6:/test# cd a_package/
root@3d4d403b95e6:/test/a_package# export UV_CACHE_DIR=.uv-cache
root@3d4d403b95e6:/test/a_package# uv build
Building source distribution...
  × Failed to build `/test/a_package`
  ├─▶ failed to unpack `/test/a_package/.uv-cache/sdists-v7/.tmppBrVL8/a_package-0.1.0/.uv-cache/builds-v0/.tmpY0p9D8/lib/python3.12/site-packages/hatchling/__about__.py`
  ╰─▶ No such file or directory (os error 2) while canonicalizing
      /test/a_package/.uv-cache/sdists-v7/.tmppBrVL8/a_package-0.1.0/.uv-cache/builds-v0/.tmpY0p9D8/lib/python3.12/site-packages/hatchling/a_package-0.1.0/.uv-cache/archive-v0/_2EI4Ur_6bHqMSVqQDuEx/hatchling/__about__.py
```

Seems like using `export UV_CACHE_DIR=.uv-cache` makes the issue appear can reproduce this using the same steps ? 

Am I doing something wrong here ? 

And FYI
```bash
 docker run --rm -it ghcr.io/astral-sh/uv:0.5.26-python3.12-bookworm-slim bash
root@7f52a30394c4:/# mkdir -p test
root@7f52a30394c4:/# cd test/
root@7f52a30394c4:/test# uv init --lib a_package
Initialized project `a-package` at `/test/a_package`
root@7f52a30394c4:/test# cd a_package/
root@7f52a30394c4:/test/a_package# export UV_CACHE_DIR=.uv-cache
root@7f52a30394c4:/test/a_package# uv build
Building source distribution...
Building wheel from source distribution...
Successfully built dist/a_package-0.1.0.tar.gz
Successfully built dist/a_package-0.1.0-py3-none-any.whl
```

---

_Comment by @charliermarsh on 2025-02-04 13:36_

Are you able to share a minimal package that reproduces this problem? It seems to be something specific to the package. E.g., does it contain any hardlinks or symlinks?

---

_Comment by @charliermarsh on 2025-02-04 13:37_

Sorry, nevermind -- it's just `uv init --lib`? Ok, thanks.

---

_Comment by @Morgadoooo on 2025-02-04 13:39_

If you need more details or info on my side (regarding my specific package) I'd be happy to give more details

---

_Label `needs-mre` removed by @konstin on 2025-02-04 13:41_

---

_Comment by @konstin on 2025-02-04 13:41_

Thanks, `UV_CACHE_DIR=.uv-cache` was the relevant detail, i can reproduce this now

---

_Comment by @charliermarsh on 2025-02-04 15:36_

I assume you can workaround this for now by setting `UV_CACHE_DIR` to an absolute path? Like `UV_CACHE_DIR=$(pwd)/.uv-cache`?

---

_Comment by @konstin on 2025-02-04 16:16_

The problem is that the uv is cache is inside the project, so it gets packaged into the source dist including the hard links in the cache, and then there's the regression I'll fix that we don't support hardlinks in source distributions anymore.

---

_Comment by @Morgadoooo on 2025-02-04 17:12_

I can totally do a workaround for now like you said.

For more context in case it can be of some help, using previous uv version we noticed that sometimes the cache `.uv-cache` caused this issue but "cleaning" the runner cache always solved the problem but not for the new version. That is why I opened the issue. 

Now that I understand it better i'll change my cache logic to prevent this. 

Thanks for the help guys really appreciate it. 

---

_Closed by @konstin on 2025-02-04 17:38_

---

_Closed by @konstin on 2025-02-04 17:38_

---
