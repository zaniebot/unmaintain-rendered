---
number: 4242
title: Inspect system to determine if musl toolchains should be used
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - musl
  - uv python
assignees: []
created_at: 2024-06-11T17:52:51Z
updated_at: 2024-08-27T00:06:55Z
url: https://github.com/astral-sh/uv/issues/4242
synced_at: 2026-01-07T13:12:17-06:00
---

# Inspect system to determine if musl toolchains should be used

---

_Issue opened by @zanieb on 2024-06-11 17:52_

As discussed in https://github.com/astral-sh/uv/pull/4160#issuecomment-2157850192

---

_Label `preview` added by @zanieb on 2024-06-11 17:53_

---

_Referenced in [astral-sh/uv#4236](../../astral-sh/uv/pulls/4236.md) on 2024-06-12 14:12_

---

_Referenced in [astral-sh/uv#5036](../../astral-sh/uv/pulls/5036.md) on 2024-07-13 15:00_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---

_Referenced in [astral-sh/uv#6392](../../astral-sh/uv/issues/6392.md) on 2024-08-22 00:09_

---

_Label `enhancement` added by @zanieb on 2024-08-22 00:14_

---

_Label `uv python` added by @zanieb on 2024-08-22 00:14_

---

_Comment by @SamuelMarks on 2024-08-22 02:58_

Hmm I'm trying to figure it out by `grep`ping about, `hexdump`ing, `nm`, `objdump` and friends.

This is the closest I can do `docker run --rm -it --entrypoint /bin/sh alpine`:
```
/ # grep -F musl "$(which ldd)"
exec /lib/ld-musl-x86_64.so.1 --list "$@"
```

(you could also look at what files are in `/lib` and determine from that)

---

Comparing to regular debian `docker run --rm -it --entrypoint /bin/sh debian:stable-slim`:
```cmd
# grep -F musl "$(which ldd)"
# grep -F libc "$(which ldd)"
TEXTDOMAIN=libc
```

---

Naturally you don't need `grep` and can just use pure Rust to open the file and look for a single string.

---

_Comment by @charliermarsh on 2024-08-22 03:01_

I wonder if there are other Rust projects that get this right, could we look at how they handle it?

---

_Comment by @SamuelMarks on 2024-08-22 03:05_

:man_shrugging: this https://github.com/rust-lang/rust/issues/71564 links here https://github.com/rust-lang/rust/blob/master/src/ci/docker/scripts/musl-toolchain.sh which could just as easily be checked by file presence | absence in the `/lib` directory.

My little `grep` seems pretty clean. Also keep in mind this would just be a heuristic, you can have multiple `libc`s on the one system. It would make sense though that whichever `ldd` you've put in your `PATH` is the one you want considered by this script [in this case, which Python variant you want to install].

---

_Comment by @charliermarsh on 2024-08-22 03:07_

Impressive sleuthing

---

_Comment by @zanieb on 2024-08-22 03:35_

We had a relatively comprehensive way of doing this before: https://github.com/astral-sh/uv/blob/e0ac5b4e84d2544c915b83bfb50776b468cdb38d/crates/platform-host/src/linux.rs

I'm not sure there are problems with it — mostly I think the plan was to make it simpler since we don't actually need to parse version numbers.

---

_Comment by @charliermarsh on 2024-08-22 03:36_

Why was it removed? I can't remember.

---

_Comment by @zanieb on 2024-08-22 03:39_

The comment at https://github.com/astral-sh/uv/pull/4160#issuecomment-2157850192 provides important context on the current state of things. It was removed in https://github.com/astral-sh/uv/pull/2381 when we moved to relying on Python's platform detection — we of course can't do this to procure Python itself, but that was before we supported downloads.

---

_Comment by @zanieb on 2024-08-22 03:41_

Basically, we should totally add support back and we can probably mostly copy the old implementation from the `platform-host` crate. However, there are significant problems with using python-build-standalone on Alpine and we'll need to invest in fixing those before it is really production ready — it's been low priority because that's such a hard problem.

---

_Comment by @SamuelMarks on 2024-08-22 04:00_

Happy to tackle that. What error were you getting?

---

_Comment by @zanieb on 2024-08-22 04:08_

So there's two parts

1. Restoring our previous musl detection code: [source](https://github.com/astral-sh/uv/blob/e0ac5b4e84d2544c915b83bfb50776b468cdb38d/crates/platform-host/src/linux.rs)
2. Improving python-build-standalone on Alpine: e.g. fixing https://github.com/astral-sh/uv/pull/2382

For (1) we'll probably want to simplify the code some, if feasible. It'd be applied [here](https://github.com/astral-sh/uv/blob/bcb2568f47941ccf7f8349a58eb5ad56701ec8ce/crates/uv-python/src/platform.rs#L34).



---

_Comment by @zanieb on 2024-08-22 04:08_

@konstin could give far more context on (2)

---

_Comment by @SamuelMarks on 2024-08-22 04:37_

Hmm not sure we need all that proper parsing of ELF headers and such. Is there a special reason you need to know which specific version of a libc is available? - Otherwise a simple function that reads a chunk of bytes from a file, searches for magic string, read next chunk, if present return `true` otherwise look in next location. A few heuristic filepaths later and you'd have like 99.99% accurate libc detection.

E.g., I sent the MonetDB guys this little C change https://github.com/SamuelMarks/MonetDB/commit/4496ba9caa1ba1e35aa3afcc39cbf85ba30d6f31#diff-245eb970cc6ec49fc0821f3bf591bcbeb0667b94e30e6296aa24de18c80d35d0R85 but that would require a whole toolchain whereas literally checking filesystem locations or the bytes your `ldd` is made of has basically no overhead and doesn't assume working C buildsystem.

So we could I guess parse the header file musl https://github.com/rofl0r/musl/blob/master/include/features.h glibc https://sourceware.org/git/?p=glibc.git;a=blob;f=include/features.h;h=093de6f44cd168fe1e8727851af1baf666ae57fc;hb=HEAD and infer from there. Or a simple line count / file size :laughing: 

---

_Comment by @konstin on 2024-08-22 09:07_

The major problem is that python-build-standalone currently has statically linked musl builds, which means they can't load any extension modules. We need to fix this before we can ship this toolchain to users.

uv itself may be linked to glibc or musl, which shouldn't make a difference (we could ship statically musl linked uv for all linuxes - uv itself should look dependency-less to the user). For downloading python, we need to determine if should prefer musl or glibc, some heuristic like the `/bin/sh`/`/bin/ls` check we used to do.

We don't need to determine glibc or musl version to determine the python download, but it would be helpful to enable things like #6212.

---

_Comment by @charliermarsh on 2024-08-22 12:51_

@konstin -- Personally it seems worthwhile to get this right in uv even if the musl builds are limited.

---

_Comment by @zanieb on 2024-08-22 14:32_

Yeah if the current ones (libc) can't be used at all, I think it's an important improvement to download the correct musl distribution.

> Hmm not sure we need all that proper parsing of ELF headers and such. Is there a special reason you need to know which specific version of a libc is available? - Otherwise a simple function that reads a chunk of bytes from a file, searches for magic string, read next chunk, if present return true otherwise look in next location. 

Yeah I don't think we need to know specific versions anymore — maybe it'd be nice have later but if you're willing to contribute a "simple" version I'd love that.

---

_Comment by @konstin on 2024-08-22 15:41_

I believe we should detect musl targets, but not download musl python-build-standalone.

Without being able to load extension-modules, this interpreter will fail on any non-trivial project. Using `fastapi` will fail because it depends on pydantic, `django` with `mysqlclient` won't work, `packse` won't work due to `msgspec`, `nh3` and `cryptography`, `github-wikidata-bot` depends `msgpack`; `numpy`, `pandas`, `torch` or any other part of the scientific stack won't work. I don't know any non-trivial project that could be installed without using an extension module.

If we treat static python as a `linux` only, no `manylinux` or `muslllinux` python, we get to the real problem is though that you don't get a non-supported failure, but a cryptic build error (output from alpine with main uv and cpython-3.12.5-linux-x86_64-musl):

```
$ uv sync
Resolved 31 packages in 1ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: msgpack==1.0.8
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running build_py
creating build
creating build/lib.linux-x86_64-cpython-312
creating build/lib.linux-x86_64-cpython-312/msgpack
copying msgpack/fallback.py -> build/lib.linux-x86_64-cpython-312/msgpack
copying msgpack/ext.py -> build/lib.linux-x86_64-cpython-312/msgpack
copying msgpack/exceptions.py -> build/lib.linux-x86_64-cpython-312/msgpack
copying msgpack/__init__.py -> build/lib.linux-x86_64-cpython-312/msgpack
running build_ext
building 'msgpack._cmsgpack' extension
creating build/temp.linux-x86_64-cpython-312
creating build/temp.linux-x86_64-cpython-312/msgpack
-fno-strict-overflow -fno-strict-aliasing -DNDEBUG -g -O3 -Wall -fPIC -static -fPIC -I. -I/root/.cache/uv/builds-v0/.tmp4ubtc7/include -I/io/home/konsti/.local/share/uv/python/cpython-3.12.5-linux-x86_64-musl/include/python3.12 -c msgpack/_cmsgpack.cpp -o build/temp.linux-x86_64-cpython-312/msgpack/_cmsgpack.o
--- stderr:
error: command '-fno-strict-overflow' failed: No such file or directory
---
```

If i instead hack in installation, i get an equally unhelpful runtime error (my native host with the musl python-build-standalone hacked in):

```
/home/konsti/projects/github-wikidata-bot/.venv/lib/python3.12/site-packages/requests/__init__.py:86: RequestsDependencyWarning: Unable to find acceptable character detection dependency (chardet or charset_normalizer).
  warnings.warn(
Traceback (most recent call last):
  File "/home/konsti/projects/github-wikidata-bot/main.py", line 3, in <module>
    from github_wikidata_bot.main import main
  File "/home/konsti/projects/github-wikidata-bot/src/github_wikidata_bot/main.py", line 12, in <module>
    from .github import Project, get_data_from_github
  File "/home/konsti/projects/github-wikidata-bot/src/github_wikidata_bot/github.py", line 14, in <module>
    from .sparql import WikidataProject
  File "/home/konsti/projects/github-wikidata-bot/src/github_wikidata_bot/sparql.py", line 4, in <module>
    from pydantic import TypeAdapter, BaseModel
  File "/home/konsti/projects/github-wikidata-bot/.venv/lib/python3.12/site-packages/pydantic/__init__.py", line 404, in __getattr__
    module = import_module(module_name, package=package)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/konsti/.local/share/uv/python/cpython-3.12.1-linux-x86_64-gnu/lib/python3.12/importlib/__init__.py", line 90, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/konsti/projects/github-wikidata-bot/.venv/lib/python3.12/site-packages/pydantic/type_adapter.py", line 26, in <module>
    from pydantic_core import CoreSchema, SchemaSerializer, SchemaValidator, Some
  File "/home/konsti/projects/github-wikidata-bot/.venv/lib/python3.12/site-packages/pydantic_core/__init__.py", line 6, in <module>
    from ._pydantic_core import (
ImportError: Dynamic loading not supported
CRITICAL: Exiting due to uncaught exception ImportError: Dynamic loading not supported
```

I agree that the current situation of downloading glibc python on musl platforms such as alpine is also untenable. I suggest restoring `crates/platform-host/src/linux.rs` from https://github.com/astral-sh/uv/pull/2381/files, but never automatically downloading musl builds. When you are on alpine and don't have a system python installed, we instead tell the user that we currently don't support musl targets in python-build-standalone and ask them to install python using their system package manager (`apk add python3=~3.11` for all the musl users that are in alpine docker images).

It would obviously be great to fix the musl builds to be dynamically linked, but i lack the build system to do that kind of change. 

---

_Comment by @SamuelMarks on 2024-08-22 16:09_

Hacking around with whatever is in Alpine:
```
$ docker run --rm -it --entrypoint /bin/sh alpine
/ # apk add python3-dev
fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/main/x86_64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/community/x86_64/APKINDEX.tar.gz
(1/19) Installing pkgconf (2.2.0-r0)
(2/19) Installing libbz2 (1.0.8-r6)
(3/19) Installing libexpat (2.6.2-r0)
(4/19) Installing libffi (3.4.6-r0)
(5/19) Installing gdbm (1.23-r1)
(6/19) Installing xz-libs (5.6.2-r0)
(7/19) Installing libgcc (13.2.1_git20240309-r0)
(8/19) Installing libstdc++ (13.2.1_git20240309-r0)
(9/19) Installing mpdecimal (4.0.0-r0)
(10/19) Installing ncurses-terminfo-base (6.4_p20240420-r0)
(11/19) Installing libncursesw (6.4_p20240420-r0)
(12/19) Installing libpanelw (6.4_p20240420-r0)
(13/19) Installing readline (8.2.10-r0)
(14/19) Installing sqlite-libs (3.45.3-r1)
(15/19) Installing python3 (3.12.3-r1)
(16/19) Installing python3-pycache-pyc0 (3.12.3-r1)
(17/19) Installing pyc (3.12.3-r1)
(18/19) Installing python3-pyc (3.12.3-r1)
(19/19) Installing python3-dev (3.12.3-r1)
Executing busybox-1.36.1-r29.trigger
OK: 142 MiB in 33 packages
/ # python3 -m venv venv
/ # . /venv/bin/activate
(venv) / # python -m pip install numpy
Collecting numpy
  Downloading numpy-2.1.0-cp312-cp312-musllinux_1_1_x86_64.whl.metadata (60 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 60.9/60.9 kB 1.1 MB/s eta 0:00:00
Downloading numpy-2.1.0-cp312-cp312-musllinux_1_1_x86_64.whl (16.4 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 16.4/16.4 MB 13.1 MB/s eta 0:00:00
Installing collected packages: numpy
Successfully installed numpy-2.1.0

[notice] A new release of pip is available: 24.0 -> 24.2
[notice] To update, run: pip install --upgrade pip
(venv) / # python -m pip install fastapi
Collecting fastapi
  Downloading fastapi-0.112.1-py3-none-any.whl.metadata (27 kB)
Collecting starlette<0.39.0,>=0.37.2 (from fastapi)
  Downloading starlette-0.38.2-py3-none-any.whl.metadata (5.9 kB)
Collecting pydantic!=1.8,!=1.8.1,!=2.0.0,!=2.0.1,!=2.1.0,<3.0.0,>=1.7.4 (from fastapi)
  Downloading pydantic-2.8.2-py3-none-any.whl.metadata (125 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 125.2/125.2 kB 2.2 MB/s eta 0:00:00
Collecting typing-extensions>=4.8.0 (from fastapi)
  Downloading typing_extensions-4.12.2-py3-none-any.whl.metadata (3.0 kB)
Collecting annotated-types>=0.4.0 (from pydantic!=1.8,!=1.8.1,!=2.0.0,!=2.0.1,!=2.1.0,<3.0.0,>=1.7.4->fastapi)
  Downloading annotated_types-0.7.0-py3-none-any.whl.metadata (15 kB)
Collecting pydantic-core==2.20.1 (from pydantic!=1.8,!=1.8.1,!=2.0.0,!=2.0.1,!=2.1.0,<3.0.0,>=1.7.4->fastapi)
  Downloading pydantic_core-2.20.1-cp312-cp312-musllinux_1_1_x86_64.whl.metadata (6.6 kB)
Collecting anyio<5,>=3.4.0 (from starlette<0.39.0,>=0.37.2->fastapi)
  Downloading anyio-4.4.0-py3-none-any.whl.metadata (4.6 kB)
Collecting idna>=2.8 (from anyio<5,>=3.4.0->starlette<0.39.0,>=0.37.2->fastapi)
  Downloading idna-3.7-py3-none-any.whl.metadata (9.9 kB)
Collecting sniffio>=1.1 (from anyio<5,>=3.4.0->starlette<0.39.0,>=0.37.2->fastapi)
  Downloading sniffio-1.3.1-py3-none-any.whl.metadata (3.9 kB)
Downloading fastapi-0.112.1-py3-none-any.whl (93 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 93.2/93.2 kB 2.4 MB/s eta 0:00:00
Downloading pydantic-2.8.2-py3-none-any.whl (423 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 423.9/423.9 kB 3.3 MB/s eta 0:00:00
Downloading pydantic_core-2.20.1-cp312-cp312-musllinux_1_1_x86_64.whl (2.1 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 MB 6.3 MB/s eta 0:00:00
Downloading starlette-0.38.2-py3-none-any.whl (72 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 72.0/72.0 kB 2.7 MB/s eta 0:00:00
Downloading typing_extensions-4.12.2-py3-none-any.whl (37 kB)
Downloading annotated_types-0.7.0-py3-none-any.whl (13 kB)
Downloading anyio-4.4.0-py3-none-any.whl (86 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 86.8/86.8 kB 2.9 MB/s eta 0:00:00
Downloading idna-3.7-py3-none-any.whl (66 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 66.8/66.8 kB 2.4 MB/s eta 0:00:00
Downloading sniffio-1.3.1-py3-none-any.whl (10 kB)
Installing collected packages: typing-extensions, sniffio, idna, annotated-types, pydantic-core, anyio, starlette, pydantic, fastapi
Successfully installed annotated-types-0.7.0 anyio-4.4.0 fastapi-0.112.1 idna-3.7 pydantic-2.8.2 pydantic-core-2.20.1 sniffio-1.3.1 starlette-0.38.2 typing-extensions-4.12.2

[notice] A new release of pip is available: 24.0 -> 24.2
[notice] To update, run: pip install --upgrade pip
```

Looks like it works just fine with the packages you referenced.

---

Ok so let's look:
https://pkgs.alpinelinux.org/package/edge/main/x86_64/python3-dev

Two deps:
- [python3](https://pkgs.alpinelinux.org/package/edge/main/x86_64/python3)
- [pkgconf](https://pkgs.alpinelinux.org/package/edge/main/x86_64/pkgconf)

`python3` in turn depends on:
- [gdbm](https://pkgs.alpinelinux.org/package/edge/main/x86_64/gdbm)
- [libbz2](https://pkgs.alpinelinux.org/package/edge/main/x86_64/libbz2)
- [libcrypto3](https://pkgs.alpinelinux.org/package/edge/main/x86_64/libcrypto3)
- [libexpat](https://pkgs.alpinelinux.org/package/edge/main/x86_64/libexpat)
- [libffi](https://pkgs.alpinelinux.org/package/edge/main/x86_64/libffi)
- [libncursesw](https://pkgs.alpinelinux.org/package/edge/main/x86_64/libncursesw)
- [libpanelw](https://pkgs.alpinelinux.org/package/edge/main/x86_64/libpanelw)
- [libssl3](https://pkgs.alpinelinux.org/package/edge/main/x86_64/libssl3)
- [mpdecimal](https://pkgs.alpinelinux.org/package/edge/main/x86_64/mpdecimal)
- [musl](https://pkgs.alpinelinux.org/package/edge/main/x86_64/musl)
- [readline](https://pkgs.alpinelinux.org/package/edge/main/x86_64/readline)
- [sqlite-libs](https://pkgs.alpinelinux.org/package/edge/main/x86_64/sqlite-libs)
- [xz-libs](https://pkgs.alpinelinux.org/package/edge/main/x86_64/xz-libs)
- [zlib](https://pkgs.alpinelinux.org/package/edge/main/x86_64/zlib)

`pkgconf` depends on:
- [musl](https://pkgs.alpinelinux.org/package/edge/main/x86_64/musl)

Now let's consider how to solve this problem. Having a bunch of linkable shared libraries doesn't seem horrible, and it can still be basically free-standing by putting each installation in its own self-contained directory (I think Nix does this).

Shouldn't be that hard, just a bit of grunt work, there's only like 17 archives to consider here and we can use alpine's mirrors. We don't need to use their package manager even, their archive format is pretty simple can stick to Rust at the source-code rather than process-execution level if you like.

---

_Comment by @ptrcnull on 2024-08-23 01:55_

> We don't need to use their package manager even, their archive format is pretty simple

FWIW, Alpine is switching to a custom package format with the (upcoming, still TBA) release of apk-tools v3; the better option here would be to just compile dynamic musl builds under a musl-based distribution ( whether it would be done by pbs or astral ) and copy the dynamic libraries in the same version it was compiled against, rather than fetch the dynamic libraries from distro packages manually ( that is, if i'm even understanding this correctly )

---

_Comment by @ptrcnull on 2024-08-23 04:14_

> I suggest restoring `crates/platform-host/src/linux.rs` from https://github.com/astral-sh/uv/pull/2381/files

one little nitpick for when that happens: using `/usr/bin/env` to get interpreter path should be more reliable, as it's one of the few things that are consistently an executable across most Linux distributions, even the more odd ones like NixOS

---

_Comment by @ptrcnull on 2024-08-23 04:20_

also, i was able to build somewhat cursed CPython environments with dynamic linking by... extracting Docker containers:
<details>
<summary>build-standalone-musl-python.sh</summary>

```sh
#!/bin/sh

set -eux

alpine_version="$1"
arch="$2"

case "$arch" in
	x86) docker_arch="i386" ;;
	x86_64) docker_arch="library" ;;
	aarch64) docker_arch="arm64v8" ;;
	*) echo "unknown arch: $arch" && exit 1 ;;
esac

container_id="$(docker create docker.io/$docker_arch/alpine:"$alpine_version" apk add --no-cache python3)"
docker start -ai "$container_id"
docker export "$container_id" | tar x usr lib
docker rm "$container_id"
rm -r lib/apk usr/share usr/sbin
mv lib/* usr/lib/
rmdir lib
patchelf --add-rpath '$ORIGIN/../lib' usr/bin/python3.*

python_version="$(./usr/lib/ld-musl-*.so.1 ./usr/bin/python3 -V | cut -d' ' -f2)"
archive_name="cpython-$python_version-linux-$arch-musl"

mv usr "$archive_name"
tar czf "$archive_name.tar.gz" "$archive_name"
rm -rf "$archive_name"
```

</details>

which then seems to work just fine with uv ( after patching `Libc::from_env` to always return musl ):
```console
$ uv python list --only-installed
cpython-3.12.5-linux-x86_64-musl     /usr/bin/python3.12
cpython-3.12.5-linux-x86_64-musl     /usr/bin/python3 -> python3.12
cpython-3.12.5-linux-x86_64-musl     /usr/bin/python -> python3
cpython-3.10.14-linux-x86_64-musl    /home/patrycja/.local/share/uv/python/cpython-3.10.2-linux-x86_64-musl/bin/python3 -> python3.10
cpython-3.9.17-linux-x86_64-musl     /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/bin/python3 -> python3.9
$ cat test.py    
# /// script
# dependencies = [
#   "numpy"
# ]
# ///

import sys
import numpy as np
a = np.arange(15).reshape(3, 5)
print(a)
print(sys.version_info)
$ uv run --python-preference=system -- test.py
Reading inline script metadata from: test.py
[[ 0  1  2  3  4]
 [ 5  6  7  8  9]
 [10 11 12 13 14]]
sys.version_info(major=3, minor=12, micro=5, releaselevel='final', serial=0)
$ uv run --python=3.10 -- test.py           
Reading inline script metadata from: test.py
[[ 0  1  2  3  4]
 [ 5  6  7  8  9]
 [10 11 12 13 14]]
sys.version_info(major=3, minor=10, micro=14, releaselevel='final', serial=0)
```

---

_Comment by @konstin on 2024-08-23 08:02_

@ptrcnull Is that apk or docker cpython build relocatable, i.e. does it work no matter where we unpack the archive to? One of the main motivations for python-build-standalone was that the upstream cpython builds aren't, see e.g. https://discuss.python.org/t/creating-a-standalone-cpython-distribution/19402.

---

_Comment by @SamuelMarks on 2024-08-23 16:08_

@konstin Yep you should be fine, see https://github.com/NixOS/patchelf/blob/a0f5433/src/patchelf.cc#L1483

@ptrcnull Cool cool. Also you can do that without `apk` and just extract out https://pkgs.alpinelinux.org/package/edge/main/x86_64/python3 -

PS: In terms of the new apk package format I can guarantee it'll be easily reverse-engineerable!

---

_Comment by @ptrcnull on 2024-08-23 16:31_

@konstin looks like it; importing numpy and zlib ( with numpy using its own native libraries and zlib using "system" libz.so ) shows both of them loading libraries from the correct places, with only the system-wide dynamic loader being used:

<details>
<summary>really long terminal output</summary>

```
~ ❯ ps aux | grep python              
patrycja  3696  0.1  0.4 155872 100332 pts/5   Sl+  18:27   0:00 uv run --python=3.9 --with=numpy python
patrycja  3724  2.9  0.1 560532 23012 pts/5    Sl+  18:27   0:02 /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/bin/python3
patrycja  4082  0.0  0.0   1864   640 pts/2    S+   18:29   0:00 grep --color=auto python

~ ❯ cat /proc/3724/maps | grep -F '.so'
7f5d91c7c000-7f5d91c7f000 r--p 00000000 fd:00 9803839                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libz.so.1.2.12
7f5d91c7f000-7f5d91c8d000 r-xp 00003000 fd:00 9803839                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libz.so.1.2.12
7f5d91c8d000-7f5d91c94000 r--p 00011000 fd:00 9803839                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libz.so.1.2.12
7f5d91c94000-7f5d91c95000 r--p 00017000 fd:00 9803839                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libz.so.1.2.12
7f5d91c95000-7f5d91c96000 rw-p 00018000 fd:00 9803839                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libz.so.1.2.12
7f5d91c96000-7f5d91c98000 r--p 00000000 fd:00 10113786                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/zlib.cpython-39-x86_64-linux-musl.so
7f5d91c98000-7f5d91c9b000 r-xp 00002000 fd:00 10113786                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/zlib.cpython-39-x86_64-linux-musl.so
7f5d91c9b000-7f5d91c9e000 r--p 00005000 fd:00 10113786                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/zlib.cpython-39-x86_64-linux-musl.so
7f5d91c9e000-7f5d91c9f000 r--p 00007000 fd:00 10113786                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/zlib.cpython-39-x86_64-linux-musl.so
7f5d91c9f000-7f5d91ca0000 rw-p 00008000 fd:00 10113786                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/zlib.cpython-39-x86_64-linux-musl.so
7f5d91dfc000-7f5d91e03000 r--p 00000000 fd:00 9451075                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/linalg/_umath_linalg.cpython-39-x86_64-linux-gnu.so
7f5d91e03000-7f5d91e25000 r-xp 00007000 fd:00 9451075                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/linalg/_umath_linalg.cpython-39-x86_64-linux-gnu.so
7f5d91e25000-7f5d91e2b000 r--p 00029000 fd:00 9451075                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/linalg/_umath_linalg.cpython-39-x86_64-linux-gnu.so
7f5d91e2b000-7f5d91e2c000 r--p 0002e000 fd:00 9451075                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/linalg/_umath_linalg.cpython-39-x86_64-linux-gnu.so
7f5d91e2c000-7f5d91e2d000 rw-p 0002f000 fd:00 9451075                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/linalg/_umath_linalg.cpython-39-x86_64-linux-gnu.so
7f5d91e2d000-7f5d91e31000 rw-p 00036000 fd:00 9451075                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/linalg/_umath_linalg.cpython-39-x86_64-linux-gnu.so
7f5d91fa3000-7f5d91fa5000 r--p 00000000 fd:00 10113771                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/select.cpython-39-x86_64-linux-musl.so
7f5d91fa5000-7f5d91fa8000 r-xp 00002000 fd:00 10113771                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/select.cpython-39-x86_64-linux-musl.so
7f5d91fa8000-7f5d91fab000 r--p 00005000 fd:00 10113771                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/select.cpython-39-x86_64-linux-musl.so
7f5d91fab000-7f5d91fac000 r--p 00007000 fd:00 10113771                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/select.cpython-39-x86_64-linux-musl.so
7f5d91fac000-7f5d91fad000 rw-p 00008000 fd:00 10113771                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/select.cpython-39-x86_64-linux-musl.so
7f5d91fad000-7f5d91faf000 r--p 00000000 fd:00 10113737                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_posixsubprocess.cpython-39-x86_64-linux-musl.so
7f5d91faf000-7f5d91fb2000 r-xp 00002000 fd:00 10113737                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_posixsubprocess.cpython-39-x86_64-linux-musl.so
7f5d91fb2000-7f5d91fb3000 r--p 00005000 fd:00 10113737                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_posixsubprocess.cpython-39-x86_64-linux-musl.so
7f5d91fb3000-7f5d91fb4000 r--p 00005000 fd:00 10113737                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_posixsubprocess.cpython-39-x86_64-linux-musl.so
7f5d91fb4000-7f5d91fb5000 rw-p 00006000 fd:00 10113737                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_posixsubprocess.cpython-39-x86_64-linux-musl.so
7f5d92006000-7f5d92008000 r--p 00000000 fd:00 10113763                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/grp.cpython-39-x86_64-linux-musl.so
7f5d92008000-7f5d92009000 r-xp 00002000 fd:00 10113763                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/grp.cpython-39-x86_64-linux-musl.so
7f5d92009000-7f5d9200a000 r--p 00003000 fd:00 10113763                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/grp.cpython-39-x86_64-linux-musl.so
7f5d9200a000-7f5d9200b000 r--p 00003000 fd:00 10113763                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/grp.cpython-39-x86_64-linux-musl.so
7f5d9200b000-7f5d9200c000 rw-p 00004000 fd:00 10113763                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/grp.cpython-39-x86_64-linux-musl.so
7f5d92040000-7f5d92046000 r--p 00000000 fd:00 10113717                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_ctypes.cpython-39-x86_64-linux-musl.so
7f5d92046000-7f5d92056000 r-xp 00006000 fd:00 10113717                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_ctypes.cpython-39-x86_64-linux-musl.so
7f5d92056000-7f5d9205d000 r--p 00016000 fd:00 10113717                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_ctypes.cpython-39-x86_64-linux-musl.so
7f5d9205d000-7f5d9205e000 r--p 0001c000 fd:00 10113717                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_ctypes.cpython-39-x86_64-linux-musl.so
7f5d9205e000-7f5d92062000 rw-p 0001d000 fd:00 10113717                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_ctypes.cpython-39-x86_64-linux-musl.so
7f5d920aa000-7f5d920b1000 r--p 00000000 fd:00 9451451                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/_core/_multiarray_tests.cpython-39-x86_64-linux-gnu.so
7f5d920b1000-7f5d920c8000 r-xp 00007000 fd:00 9451451                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/_core/_multiarray_tests.cpython-39-x86_64-linux-gnu.so
7f5d920c8000-7f5d920ce000 r--p 0001e000 fd:00 9451451                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/_core/_multiarray_tests.cpython-39-x86_64-linux-gnu.so
7f5d920ce000-7f5d920cf000 r--p 00023000 fd:00 9451451                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/_core/_multiarray_tests.cpython-39-x86_64-linux-gnu.so
7f5d920cf000-7f5d920d0000 rw-p 00024000 fd:00 9451451                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/_core/_multiarray_tests.cpython-39-x86_64-linux-gnu.so
7f5d920d0000-7f5d920d4000 rw-p 0002b000 fd:00 9451451                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/_core/_multiarray_tests.cpython-39-x86_64-linux-gnu.so
7f5d92142000-7f5d92144000 r--p 00000000 fd:00 9803803                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libffi.so.7.1.0
7f5d92144000-7f5d92149000 r-xp 00002000 fd:00 9803803                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libffi.so.7.1.0
7f5d92149000-7f5d9214b000 r--p 00007000 fd:00 9803803                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libffi.so.7.1.0
7f5d9214b000-7f5d9214c000 r--p 00008000 fd:00 9803803                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libffi.so.7.1.0
7f5d9214c000-7f5d9214d000 rw-p 00009000 fd:00 9803803                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libffi.so.7.1.0
7f5d921f2000-7f5d921f3000 r--p 00000000 fd:00 10113714                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_contextvars.cpython-39-x86_64-linux-musl.so
7f5d921f3000-7f5d921f4000 r-xp 00001000 fd:00 10113714                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_contextvars.cpython-39-x86_64-linux-musl.so
7f5d921f4000-7f5d921f5000 r--p 00002000 fd:00 10113714                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_contextvars.cpython-39-x86_64-linux-musl.so
7f5d921f5000-7f5d921f6000 r--p 00002000 fd:00 10113714                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_contextvars.cpython-39-x86_64-linux-musl.so
7f5d921f6000-7f5d921f7000 rw-p 00003000 fd:00 10113714                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_contextvars.cpython-39-x86_64-linux-musl.so
7f5d921f7000-7f5d921fc000 r--p 00000000 fd:00 10113735                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_pickle.cpython-39-x86_64-linux-musl.so
7f5d921fc000-7f5d92210000 r-xp 00005000 fd:00 10113735                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_pickle.cpython-39-x86_64-linux-musl.so
7f5d92210000-7f5d92217000 r--p 00019000 fd:00 10113735                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_pickle.cpython-39-x86_64-linux-musl.so
7f5d92217000-7f5d92218000 r--p 0001f000 fd:00 10113735                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_pickle.cpython-39-x86_64-linux-musl.so
7f5d92218000-7f5d9221a000 rw-p 00020000 fd:00 10113735                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_pickle.cpython-39-x86_64-linux-musl.so
7f5d92316000-7f5d92319000 r--p 00000000 fd:00 10113748                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_struct.cpython-39-x86_64-linux-musl.so
7f5d92319000-7f5d92320000 r-xp 00003000 fd:00 10113748                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_struct.cpython-39-x86_64-linux-musl.so
7f5d92320000-7f5d92324000 r--p 0000a000 fd:00 10113748                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_struct.cpython-39-x86_64-linux-musl.so
7f5d92324000-7f5d92325000 r--p 0000d000 fd:00 10113748                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_struct.cpython-39-x86_64-linux-musl.so
7f5d92325000-7f5d92326000 rw-p 0000e000 fd:00 10113748                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_struct.cpython-39-x86_64-linux-musl.so
7f5d94464000-7f5d94469000 r--p 00000000 fd:00 10113721                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_datetime.cpython-39-x86_64-linux-musl.so
7f5d94469000-7f5d9447b000 r-xp 00005000 fd:00 10113721                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_datetime.cpython-39-x86_64-linux-musl.so
7f5d9447b000-7f5d94481000 r--p 00017000 fd:00 10113721                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_datetime.cpython-39-x86_64-linux-musl.so
7f5d94481000-7f5d94482000 r--p 0001c000 fd:00 10113721                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_datetime.cpython-39-x86_64-linux-musl.so
7f5d94482000-7f5d94485000 rw-p 0001d000 fd:00 10113721                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_datetime.cpython-39-x86_64-linux-musl.so
7f5d964d1000-7f5d964d4000 r--p 00000000 fd:00 10113764                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/math.cpython-39-x86_64-linux-musl.so
7f5d964d4000-7f5d964dc000 r-xp 00003000 fd:00 10113764                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/math.cpython-39-x86_64-linux-musl.so
7f5d964dc000-7f5d964e1000 r--p 0000b000 fd:00 10113764                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/math.cpython-39-x86_64-linux-musl.so
7f5d964e1000-7f5d964e2000 r--p 0000f000 fd:00 10113764                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/math.cpython-39-x86_64-linux-musl.so
7f5d964e2000-7f5d964e3000 rw-p 00010000 fd:00 10113764                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/math.cpython-39-x86_64-linux-musl.so
7f5db060f000-7f5db062c000 r--p 00000000 fd:00 9450939                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgfortran-c4ef5775-00112feb.so.5.0.0
7f5db062c000-7f5db078c000 r-xp 0001d000 fd:00 9450939                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgfortran-c4ef5775-00112feb.so.5.0.0
7f5db078c000-7f5db07b8000 r--p 0017d000 fd:00 9450939                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgfortran-c4ef5775-00112feb.so.5.0.0
7f5db07b8000-7f5db07ba000 r--p 001a8000 fd:00 9450939                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgfortran-c4ef5775-00112feb.so.5.0.0
7f5db07ba000-7f5db0800000 rw-p 001aa000 fd:00 9450939                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgfortran-c4ef5775-00112feb.so.5.0.0
7f5db0800000-7f5db08b7000 r--p 00000000 fd:00 9450938                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libstdc++-a9383cce.so.6.0.28
7f5db08b7000-7f5db0942000 r-xp 000b7000 fd:00 9450938                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libstdc++-a9383cce.so.6.0.28
7f5db0942000-7f5db0986000 r--p 00142000 fd:00 9450938                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libstdc++-a9383cce.so.6.0.28
7f5db0986000-7f5db0995000 r--p 00185000 fd:00 9450938                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libstdc++-a9383cce.so.6.0.28
7f5db0995000-7f5db0996000 rw-p 00194000 fd:00 9450938                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libstdc++-a9383cce.so.6.0.28
7f5db0999000-7f5db0a5a000 rw-p 00195000 fd:00 9450938                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libstdc++-a9383cce.so.6.0.28
7f5db0c00000-7f5db0d0a000 r--p 00000000 fd:00 9450935                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libscipy_openblas64_-81ba8edc.so
7f5db0d0a000-7f5db0d12000 r-xp 0010a000 fd:00 9450935                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libscipy_openblas64_-81ba8edc.so
7f5db0d12000-7f5db0d18000 r--p 00112000 fd:00 9450935                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libscipy_openblas64_-81ba8edc.so
7f5db0d18000-7f5db2a20000 r-xp 00118000 fd:00 9450935                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libscipy_openblas64_-81ba8edc.so
7f5db2a20000-7f5db2c1d000 r--p 01e20000 fd:00 9450935                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libscipy_openblas64_-81ba8edc.so
7f5db2c1d000-7f5db2c27000 r--p 0201c000 fd:00 9450935                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libscipy_openblas64_-81ba8edc.so
7f5db2c27000-7f5db2c3a000 rw-p 02026000 fd:00 9450935                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libscipy_openblas64_-81ba8edc.so
7f5db2c47000-7f5db2df1000 rw-p 0214f000 fd:00 9450935                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libscipy_openblas64_-81ba8edc.so
7f5db2e00000-7f5db2e2a000 r--p 00000000 fd:00 9451462                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/_core/_multiarray_umath.cpython-39-x86_64-linux-gnu.so
7f5db2e2a000-7f5db3585000 r-xp 0002a000 fd:00 9451462                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/_core/_multiarray_umath.cpython-39-x86_64-linux-gnu.so
7f5db3585000-7f5db3708000 r--p 00785000 fd:00 9451462                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/_core/_multiarray_umath.cpython-39-x86_64-linux-gnu.so
7f5db3708000-7f5db370c000 r--p 00907000 fd:00 9451462                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/_core/_multiarray_umath.cpython-39-x86_64-linux-gnu.so
7f5db370c000-7f5db3730000 rw-p 0090b000 fd:00 9451462                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/_core/_multiarray_umath.cpython-39-x86_64-linux-gnu.so
7f5db3750000-7f5db375b000 rw-p 009a6000 fd:00 9451462                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy/_core/_multiarray_umath.cpython-39-x86_64-linux-gnu.so
7f5db378e000-7f5db3791000 r--p 00000000 fd:00 9450937                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgcc_s-a04fdf82-631a8935.so.1
7f5db3791000-7f5db379d000 r-xp 00003000 fd:00 9450937                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgcc_s-a04fdf82-631a8935.so.1
7f5db379d000-7f5db37a0000 r--p 0000f000 fd:00 9450937                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgcc_s-a04fdf82-631a8935.so.1
7f5db37a0000-7f5db37a1000 r--p 00011000 fd:00 9450937                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgcc_s-a04fdf82-631a8935.so.1
7f5db37a1000-7f5db37a4000 rw-p 00012000 fd:00 9450937                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgcc_s-a04fdf82-631a8935.so.1
7f5db37a4000-7f5db37a7000 r--p 00000000 fd:00 9450940                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libquadmath-1b17c16b-ac517e76.so.0.0.0
7f5db37a7000-7f5db37c5000 r-xp 00003000 fd:00 9450940                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libquadmath-1b17c16b-ac517e76.so.0.0.0
7f5db37c5000-7f5db37dc000 r--p 00021000 fd:00 9450940                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libquadmath-1b17c16b-ac517e76.so.0.0.0
7f5db37dc000-7f5db37dd000 r--p 00037000 fd:00 9450940                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libquadmath-1b17c16b-ac517e76.so.0.0.0
7f5db37dd000-7f5db37e0000 rw-p 00038000 fd:00 9450940                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libquadmath-1b17c16b-ac517e76.so.0.0.0
7f5db37e0000-7f5db37e3000 r--p 00000000 fd:00 9450936                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgcc_s-a04fdf82.so.1
7f5db37e3000-7f5db37ef000 r-xp 00003000 fd:00 9450936                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgcc_s-a04fdf82.so.1
7f5db37ef000-7f5db37f2000 r--p 0000f000 fd:00 9450936                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgcc_s-a04fdf82.so.1
7f5db37f2000-7f5db37f3000 r--p 00011000 fd:00 9450936                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgcc_s-a04fdf82.so.1
7f5db37f3000-7f5db37f5000 rw-p 00012000 fd:00 9450936                    /home/patrycja/.cache/uv/archive-v0/sHoDg4UqjPDq2p3oK2OQT/lib/python3.9/site-packages/numpy.libs/libgcc_s-a04fdf82.so.1
7f5db38b3000-7f5db38b4000 r--p 00000000 fd:00 10113727                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_heapq.cpython-39-x86_64-linux-musl.so
7f5db38b4000-7f5db38b7000 r-xp 00001000 fd:00 10113727                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_heapq.cpython-39-x86_64-linux-musl.so
7f5db38b7000-7f5db38ba000 r--p 00004000 fd:00 10113727                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_heapq.cpython-39-x86_64-linux-musl.so
7f5db38ba000-7f5db38bb000 r--p 00006000 fd:00 10113727                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_heapq.cpython-39-x86_64-linux-musl.so
7f5db38bb000-7f5db38bc000 rw-p 00007000 fd:00 10113727                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/_heapq.cpython-39-x86_64-linux-musl.so
7f5db394f000-7f5db3965000 r--p 00000000 fd:00 9803820                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libncursesw.so.6.2
7f5db3965000-7f5db398e000 r-xp 00016000 fd:00 9803820                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libncursesw.so.6.2
7f5db398e000-7f5db39a5000 r--p 0003f000 fd:00 9803820                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libncursesw.so.6.2
7f5db39a5000-7f5db39aa000 r--p 00055000 fd:00 9803820                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libncursesw.so.6.2
7f5db39aa000-7f5db39ab000 rw-p 0005a000 fd:00 9803820                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libncursesw.so.6.2
7f5db39ab000-7f5db39c1000 r--p 00000000 fd:00 9803826                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libreadline.so.8.1
7f5db39c1000-7f5db39e1000 r-xp 00016000 fd:00 9803826                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libreadline.so.8.1
7f5db39e1000-7f5db39eb000 r--p 00036000 fd:00 9803826                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libreadline.so.8.1
7f5db39eb000-7f5db39ee000 r--p 0003f000 fd:00 9803826                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libreadline.so.8.1
7f5db39ee000-7f5db39f4000 rw-p 00042000 fd:00 9803826                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libreadline.so.8.1
7f5db39f5000-7f5db39f8000 r--p 00000000 fd:00 10113769                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/readline.cpython-39-x86_64-linux-musl.so
7f5db39f8000-7f5db39fb000 r-xp 00003000 fd:00 10113769                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/readline.cpython-39-x86_64-linux-musl.so
7f5db39fb000-7f5db39fe000 r--p 00006000 fd:00 10113769                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/readline.cpython-39-x86_64-linux-musl.so
7f5db39fe000-7f5db39ff000 r--p 00008000 fd:00 10113769                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/readline.cpython-39-x86_64-linux-musl.so
7f5db39ff000-7f5db3a00000 rw-p 00009000 fd:00 10113769                   /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/python3.9/lib-dynload/readline.cpython-39-x86_64-linux-musl.so
7f5db3c00000-7f5db3c5a000 r--p 00000000 fd:00 9803823                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libpython3.9.so.1.0
7f5db3c5a000-7f5db3d9d000 r-xp 0005a000 fd:00 9803823                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libpython3.9.so.1.0
7f5db3d9d000-7f5db3e76000 r--p 0019d000 fd:00 9803823                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libpython3.9.so.1.0
7f5db3e76000-7f5db3e7c000 r--p 00275000 fd:00 9803823                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libpython3.9.so.1.0
7f5db3e7c000-7f5db3eb4000 rw-p 0027b000 fd:00 9803823                    /home/patrycja/.local/share/uv/python/cpython-3.9.17-linux-x86_64-musl/lib/libpython3.9.so.1.0
7f5db3f20000-7f5db3f34000 r--p 00000000 fd:00 1179672                    /lib/ld-musl-x86_64.so.1
7f5db3f34000-7f5db3f8b000 r-xp 00014000 fd:00 1179672                    /lib/ld-musl-x86_64.so.1
7f5db3f8b000-7f5db3fc1000 r--p 0006b000 fd:00 1179672                    /lib/ld-musl-x86_64.so.1
7f5db3fc1000-7f5db3fc2000 r--p 000a0000 fd:00 1179672                    /lib/ld-musl-x86_64.so.1
7f5db3fc2000-7f5db3fc3000 rw-p 000a1000 fd:00 1179672                    /lib/ld-musl-x86_64.so.1
```
</details>

rpath is doing the heavy lifting here when it comes to "system" libraries like ncurses and zlib, the rest is just Python

---

_Comment by @ptrcnull on 2024-08-23 16:38_

> @ptrcnull Cool cool. Also you can do that without apk and just extract out https://pkgs.alpinelinux.org/package/edge/main/x86_64/python3 -

@SamuelMarks yeah, but then you have to resolve the dependency tree yourself and unpack the correct packages, why do that when a Docker container can do that for you? :P

still, this is a bodge more than anything, and i don't think this should be used as an _actual_ solution, rather just a proof-of-concept for loading dynamic Python installations under musl

> PS: In terms of the new apk package format I can guarantee it'll be easily reverse-engineerable!

no need for reverse-engineering, it's mostly documented [in the repo][1], and i'm pretty sure a standalone tool for unpacking apk3 packages will appear at some point as well

[1]: https://gitlab.alpinelinux.org/alpine/apk-tools/-/blob/40670c68/doc/apk-v3.5.scd

---

_Label `musl` added by @konstin on 2024-08-26 11:52_

---

_Referenced in [astral-sh/uv#6643](../../astral-sh/uv/pulls/6643.md) on 2024-08-26 12:47_

---

_Closed by @charliermarsh on 2024-08-27 00:06_

---

_Referenced in [astral-sh/uv#6890](../../astral-sh/uv/issues/6890.md) on 2024-09-03 14:20_

---
