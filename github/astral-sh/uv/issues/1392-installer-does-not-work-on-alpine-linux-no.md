---
number: 1392
title: "Installer does not work on Alpine Linux: no package for `aarch64-unknown-linux-musl-dynamic`"
type: issue
state: closed
author: mgasner
labels:
  - bug
  - musl
assignees: []
created_at: 2024-02-15T23:54:45Z
updated_at: 2024-02-23T20:05:58Z
url: https://github.com/astral-sh/uv/issues/1392
synced_at: 2026-01-07T13:12:16-06:00
---

# Installer does not work on Alpine Linux: no package for `aarch64-unknown-linux-musl-dynamic`

---

_Issue opened by @mgasner on 2024-02-15 23:54_

Are there plans to support Alpine?

---

_Comment by @zanieb on 2024-02-15 23:57_

Hi. I believe we do! What isn't working?

---

_Label `question` added by @zanieb on 2024-02-15 23:57_

---

_Comment by @mgasner on 2024-02-16 00:09_

It may be an architecture issue -- I am on an M1 and with the following minimal dockerfile:

```
FROM python:3.10.11-alpine

RUN apk update && apk add --no-cache \
    build-base \
    curl \
    rust \
    cargo

RUN curl -LsSf https://astral.sh/uv/install.sh | sh
```

Get this error:

```
 > [4/8] RUN curl -LsSf https://astral.sh/uv/install.sh | sh:
#8 1.702 ERROR: there isn't a package for aarch64-unknown-linux-musl-dynamic```

---

_Comment by @zanieb on 2024-02-16 00:20_

Thanks for the details! I wonder if you can pick the correct `musl` build from our [releases page](https://github.com/astral-sh/uv/releases)?

---

_Comment by @mgasner on 2024-02-16 00:29_

Thanks, that works!

```
FROM python:3.12.2-alpine

RUN apk update && apk add --no-cache \
    build-base \
    curl \ 
    rust \ 
    cargo

RUN curl --proto '=https' --tlsv1.2 -LsSfO https://github.com/astral-sh/uv/releases/download/0.1.1/uv-aarch64-unknown-linux-musl.tar.gz && \
    tar -xzvf uv-aarch64-unknown-linux-musl.tar.gz && \
    mv uv-aarch64-unknown-linux-musl/uv /bin/ && \
    rm -rfv uv-aarch64-unknown-linux-musl*
```

---

_Comment by @zanieb on 2024-02-16 00:33_

We'll get the installer fixed. Thanks again!

---

_Comment by @mgasner on 2024-02-16 00:36_

There may be some further issues -- running `uv` works fine, but then attempting to run actual commands leads to more errors, e.g.:

```
/ # uv venv
  × Failed to detect the operating system version: Couldn't detect either glibc
  │ version nor musl libc version, at least one of which is required
```

---

_Comment by @mgasner on 2024-02-16 00:45_

Fwiw, this is the output of `ldd` within the container (and unlike #1395, `/bin/ls` does exist and contains the appropriate reference)

```
/ # ldd
musl libc (aarch64)
Version 1.2.4
Dynamic Program Loader
Usage: /lib/ld-musl-aarch64.so.1 [options] [--] pathname
```

---

_Renamed from "Support alpine linux" to "Support alpine linux (on Apple Silicon)" by @mgasner on 2024-02-16 00:50_

---

_Referenced in [astral-sh/uv#1427](../../astral-sh/uv/issues/1427.md) on 2024-02-16 04:36_

---

_Renamed from "Support alpine linux (on Apple Silicon)" to "Installer does not work on Alpine Linux: no package for `aarch64-unknown-linux-musl-dynamic`" by @zanieb on 2024-02-16 04:43_

---

_Comment by @zanieb on 2024-02-16 04:43_

We'll track the installer here and the OS detection issue over in https://github.com/astral-sh/uv/issues/1427

---

_Comment by @MichaReiser on 2024-02-16 08:50_

I'm looking at the installer script and the `get_architecture` function has a branch to detect "dynamic-musl"

```sh
    if [ "$_ostype" = Linux ]; then
        if [ "$(uname -o)" = Android ]; then
            _ostype=Android
        fi
        if ldd --version 2>&1 | grep -q 'musl'; then
            _clibtype="musl-dynamic"
        # glibc, but is it a compatible glibc?
        else
            # Parsing version out from line 1 like:
            # ldd (Ubuntu GLIBC 2.35-0ubuntu3.1) 2.35
            _local_glibc="$(ldd --version | awk -F' ' '{ if (FNR<=1) print $NF }')"

            if [ "$(echo "${_local_glibc}" | awk -F. '{ print $1 }')" = $BUILDER_GLIBC_MAJOR ] && [ "$(echo "${_local_glibc}" | awk -F. '{ print $2 }')" -ge $BUILDER_GLIBC_SERIES ]; then
                _clibtype="gnu"
            else
                say "System glibc version (\`${_local_glibc}') is too old; using musl" >&2
                _clibtype="musl-static"
            fi
        fi
    fi
```

The script then matches over the architecture and fails to match because there's no branch for aarm64 (there is for x86) musl-dynamic:

```sh
# Lookup what to download/unpack based on platform
    case "$_arch" in 
        "aarch64-apple-darwin")
            _artifact_name="uv-aarch64-apple-darwin.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        "aarch64-unknown-linux-gnu")
            _artifact_name="uv-aarch64-unknown-linux-gnu.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        "aarch64-unknown-linux-musl")
            _artifact_name="uv-aarch64-unknown-linux-musl.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        "armv7-unknown-linux-gnueabihf")
            _artifact_name="uv-armv7-unknown-linux-gnueabihf.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        "i686-unknown-linux-gnu")
            _artifact_name="uv-i686-unknown-linux-gnu.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        "i686-unknown-linux-musl")
            _artifact_name="uv-i686-unknown-linux-musl.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        "powerpc64-unknown-linux-gnu")
            _artifact_name="uv-powerpc64-unknown-linux-gnu.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        "powerpc64le-unknown-linux-gnu")
            _artifact_name="uv-powerpc64le-unknown-linux-gnu.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        "s390x-unknown-linux-gnu")
            _artifact_name="uv-s390x-unknown-linux-gnu.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        "x86_64-apple-darwin")
            _artifact_name="uv-x86_64-apple-darwin.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        "x86_64-unknown-linux-gnu")
            _artifact_name="uv-x86_64-unknown-linux-gnu.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        "x86_64-unknown-linux-musl-dynamic")
            _artifact_name="uv-x86_64-unknown-linux-musl.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        "x86_64-unknown-linux-musl-static")
            _artifact_name="uv-x86_64-unknown-linux-musl.tar.gz"
            _zip_ext=".tar.gz"
            _bins="uv"
            ;;
        *)
            err "there isn't a package for $_arch"
            ;;
    esac
```

It's unclear to me why cargo-dist only generates a branch for aarm64 but not x86. @Gankra is musl-dynamic not yet supported by cargo-dist or is there a mistake in our configuration? 

---

_Comment by @MichaReiser on 2024-02-16 09:08_

Okay it seems that `cargo-dist` has an explicit fallback for x86 but not for aarm64:

https://github.com/axodotdev/cargo-dist/blob/475794eadae3f6c52b45a61b47d996692ecc20c8/cargo-dist/src/tasks.rs#L1649-L1654

Might be related to https://github.com/axodotdev/cargo-dist/issues/518


---

_Label `question` removed by @MichaReiser on 2024-02-16 12:10_

---

_Label `bug` added by @MichaReiser on 2024-02-16 12:10_

---

_Label `question` added by @MichaReiser on 2024-02-16 12:10_

---

_Label `question` removed by @MichaReiser on 2024-02-16 12:10_

---

_Referenced in [astral-sh/uv#1635](../../astral-sh/uv/issues/1635.md) on 2024-02-19 08:52_

---

_Label `musl` added by @konstin on 2024-02-20 13:55_

---

_Referenced in [astral-sh/uv#1927](../../astral-sh/uv/issues/1927.md) on 2024-02-23 20:04_

---

_Comment by @MichaReiser on 2024-02-23 20:04_

This should be fixed by https://github.com/astral-sh/uv/pull/1929 that will be part of the next release

---

_Closed by @MichaReiser on 2024-02-23 20:04_

---

_Closed by @MichaReiser on 2024-02-23 20:05_

---

_Referenced in [astral-sh/uv#2732](../../astral-sh/uv/issues/2732.md) on 2024-03-30 14:06_

---

_Referenced in [acts-project/acts#4104](../../acts-project/acts/pulls/4104.md) on 2025-02-20 13:54_

---
