```yaml
number: 16510
title: Multi stage docker builds fails for binary libraries
type: issue
state: closed
author: erikvanoosten
labels:
  - needs-mre
assignees: []
created_at: 2025-10-30T09:28:30Z
updated_at: 2025-11-03T12:18:50Z
url: https://github.com/astral-sh/uv/issues/16510
synced_at: 2026-01-12T16:02:33Z
```

# Multi stage docker builds fails for binary libraries

---

_@erikvanoosten_

### Summary

The Docker integration documentation suggests to use a multi stage docker process, where we copy the python installation from the first image into the final image. See https://docs.astral.sh/uv/guides/integration/docker/#non-editable-installs

However, this process does not work when there is a dependency on a binary library. In my tests with `bookworm-slim`, these `so` files land in `/usr/lib/aarch64-linux-gnu` (which also contains a ton of other files). Since they don't land in the directory given by `UV_PYTHON_INSTALL_DIR`, they won't be copied into the final image.

For example, the python `shapely` package depends on the `geos` library. Even though `UV_PYTHON_INSTALL_DIR` is set to `/python`, that library is downloaded/created in the first stage as file `/usr/lib/aarch64-linux-gnu/libgeos_c.so`.

### Minimized example application

The following ZIP file contains a minimal project that demonstrates the issue.

[example-app.zip](https://github.com/user-attachments/files/23230070/example-app.zip)

After unzipping: build and run the multi-stage project:

```shell
uv lock
docker build --tag example-app:multi .
docker run --rm -ti example-app:multi
```

Results in:
```
OSError: Could not find lib geos_c or load any of its variants ['libgeos_c.so.1', 'libgeos_c.so'].
```

Build and run the single-stage project:

```shell
uv lock
docker build -f Dockerfile-single-stage --tag example-app:single .
docker run --rm -ti example-app:single
```

Results in:
```
PI is roughly 3.1365484905459398 
```


### Platform

macOS 15 host, colima, debian (bookworm-slim) docker images

### Version

uv 0.8.17

### Python version

Python 3.11.13

---

_Label `bug` added by @erikvanoosten on 2025-10-30 09:28_

---

_Comment by @erikvanoosten on 2025-10-30 13:17_

**UPDATE:** see addition at the end of this comment

-- old text --

Here is a workaround for the exampleapp in the zip file:

In `Dockerfile` after the lines:

```
# Install only the dependencies
RUN ...elided... uv sync --locked --no-install-project --no-editable --no-dev
```

add:

```
# Install binary libraries:
RUN cp /usr/lib/*/libgeos* /python/*/lib/
```

It is a bit crappy because we need to explicitly add every additional library. Also, it is only tested for ARM64 environments, not sure if it works on AMD64 machines.

-- end of old text --

**Update:** the proposed workaround here is not very reliable. It is better to use `apt` and install the library again in the second stage.

---

_Comment by @samypr100 on 2025-11-01 03:41_

Are these system libraries installed via `apt`, or some other mechanism? In case its a system library I'd typically suggest reinstall your system libraries (usually without dev headers) on your final stage rather than copying so files as links could easily get broken and cause unintended side-effects. 

To expand, if these .so files are not packaged by a python wheel (e.g. shapely), you would still need to install them yourself. Wheels can decide to package these for you using tools like [auditwheel](https://github.com/pypa/auditwheel), but may not do so possibly due to license restrictions or other limitations.

`UV_PYTHON_INSTALL_DIR` does not dictate where system libraries are installed, but if a wheel does package so files, they'll be in the expected site-packages location in most cases.

---

_Label `bug` removed by @samypr100 on 2025-11-01 03:41_

---

_Label `external` added by @samypr100 on 2025-11-01 03:41_

---

_Comment by @erikvanoosten on 2025-11-01 09:17_

Thanks for replying!

Does this mean that support from uv is not something that we can expect (not even in the future)?

> Are these system libraries installed via apt, or some other mechanism?

As far as I can tell the lib_geos library somehow comes with the shapely package.

> UV_PYTHON_INSTALL_DIR does not dictate where system libraries are installed

I understand. But they _could_ be installed in UV_PYTHON_INSTALL_DIR/*/lib (where * is the name of the specific python version), at least this is true for cpython.

> I'd typically suggest reinstall your system libraries... on your final stage

I was hoping we'd not have to pollute the final stage with transient apt files. But I guess this is the best solution.


---

_Comment by @samypr100 on 2025-11-01 18:37_

> As far as I can tell the lib_geos library somehow comes with the shapely package.

I see, in that case it would be great if you could share more about your build process instead to understand why its failing and how we can help more efficiently. A minimal reproducible example would be great.

I tried a quick check and I see no problems with installing shapely and it does seem to package it's own .so files and runs correctly.

```console
$ docker run -it --rm --name build-test ghcr.io/astral-sh/uv:python3.14-trixie bash -c 'cd /root && uv init && uv add shapely && find .venv -iname libgeos* && uv run python -c "import shapely; print(shapely.__version__)"'
Initialized project `root`
Using CPython 3.14.0 interpreter at: /usr/local/bin/python3.14
Creating virtual environment at: .venv
Resolved 3 packages in 333ms
Prepared 2 packages in 1.21s
Installed 2 packages in 45ms
 + numpy==2.3.4
 + shapely==2.1.2
.venv/lib/python3.14/site-packages/shapely.libs/libgeos_c-abcdd5fa.so.1.19.2
.venv/lib/python3.14/site-packages/shapely.libs/libgeos-3ef06f11.so.3.13.1
2.1.2
```

---

_Label `external` removed by @samypr100 on 2025-11-01 18:37_

---

_Label `needs-mre` added by @samypr100 on 2025-11-01 18:37_

---

_Comment by @erikvanoosten on 2025-11-01 18:40_

> A minimal reproducible example would be great.

I added one to the description of this issue.

---

_Comment by @samypr100 on 2025-11-01 19:25_

> > A minimal reproducible example would be great.
> 
> I added one to the description of this issue.

Thanks, I can't reproduce `OSError: Could not find lib geos_c or load any of its variants ['libgeos_c.so.1', 'libgeos_c.so'].` from the attached example either.

I see libgeos is correctly added.
```
$ ls -la /app/.venv/lib/python3.11/site-packages/shapely.libs/*
-rwxr-xr-x 1 root root 5315353 Nov  1 19:10 /app/.venv/lib/python3.11/site-packages/shapely.libs/libgeos-3ef06f11.so.3.13.1
-rwxr-xr-x 1 root root  514225 Nov  1 19:10 /app/.venv/lib/python3.11/site-packages/shapely.libs/libgeos_c-abcdd5fa.so.1.19.2
```

```
$ ls -l /app/.venv/lib/python3.11/site-packages
total 48
drwxr-xr-x  2 root root 4096 Nov  1 19:10 __pycache__
-rw-r--r--  1 root root   18 Nov  1 19:10 _virtualenv.pth
-rw-r--r--  1 root root 4342 Nov  1 19:10 _virtualenv.py
drwxr-xr-x  3 root root 4096 Nov  1 19:11 example_app
drwxr-xr-x  2 root root 4096 Nov  1 19:11 example_app-0.1.0.dist-info
drwxr-xr-x 25 root root 4096 Nov  1 19:10 numpy
drwxr-xr-x  2 root root 4096 Nov  1 19:10 numpy-2.3.4.dist-info
drwxr-xr-x  2 root root 4096 Nov  1 19:10 numpy.libs
drwxr-xr-x  7 root root 4096 Nov  1 19:10 shapely
drwxr-xr-x  3 root root 4096 Nov  1 19:10 shapely-2.1.2.dist-info
drwxr-xr-x  2 root root 4096 Nov  1 19:10 shapely.libs
```

I tried both x86_64 and arm64 variants with the same result. Are you using a different shapely version than 2.1.2? Can you also attach your lock file?

---

_Comment by @samypr100 on 2025-11-01 19:30_

I also removed `RUN apt update && apt install -y libgeos-dev build-essential` as it shouldn't be needed, and the application still ran sucessfully.

---

_Comment by @erikvanoosten on 2025-11-03 08:11_

With shapely 2.1.2 everything is okay indeed! My apologies, I didn't test the minimizer properly.

The application that the example-app minimizer is based on, uses shapely 1.8.5. Changing `pyproject.toml` as follows:

```
dependencies = [
    "shapely==1.8.5", # <-- add version
]
```

will make the problematic behavior appear.

Here is my lock file after making the above change: 
[uv.lock.zip](https://github.com/user-attachments/files/23297974/uv.lock.zip)

---

_Comment by @erikvanoosten on 2025-11-03 12:18_

Closing this issue. The underlying problem is that shapely 1.8.5 does not install the library, also not in the first stage. This is not a uv problem. This was not immediately clear due to a lot of confusing factors.

---

_Closed by @erikvanoosten on 2025-11-03 12:18_

---
