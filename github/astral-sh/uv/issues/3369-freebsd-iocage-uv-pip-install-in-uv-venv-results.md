---
number: 3369
title: "FreeBSD iocage `uv pip install` in `uv venv` results in \"invalid peer certificate\""
type: issue
state: closed
author: cmpadden
labels:
  - question
assignees: []
created_at: 2024-05-04T04:12:54Z
updated_at: 2024-05-07T05:05:18Z
url: https://github.com/astral-sh/uv/issues/3369
synced_at: 2026-01-07T13:12:17-06:00
---

# FreeBSD iocage `uv pip install` in `uv venv` results in "invalid peer certificate"

---

_Issue opened by @cmpadden on 2024-05-04 04:12_

**Preface / Versions**

- `uv` was installed with `pkg install uv`
- venv was created with `uv venv --python 3.11`

```
(root) [root@cage ~]# uv --version
uv 0.1.15

(root) [root@cage ~]# python --version
Python 3.11.9

(root) [root@cage ~]# python -m pip --version
pip 24.0 from /root/.venv/lib/python3.11/site-packages/pip (python 3.11)

(root) [root@cage ~]# uname -a
FreeBSD cage 13.1-RELEASE-p9 FreeBSD 13.1-RELEASE-p9 n245429-296d095698e TRUENAS amd64
```

**Description**

When running `uv pip install` in a `uv venv` from a FreeBSD iocage the following error is thrown; this does _not_ occur when using `pip` in the same virtual environment:

```
(root) [root@cage ~]# uv pip install cowsay
error: error sending request for url (https://pypi.org/simple/cowsay/): error trying to connect: invalid peer certificate: UnknownIssuer
  Caused by: error trying to connect: invalid peer certificate: UnknownIssuer
  Caused by: invalid peer certificate: UnknownIssuer
```

Here is the same installation in the same virtual environment working with `pip`:

```
(root) [root@cage ~]# which python
/root/.venv/bin/python
(root) [root@cage ~]# python -m pip --no-cache-dir install cowsay
Collecting cowsay
  Downloading cowsay-6.1-py3-none-any.whl.metadata (5.6 kB)
Downloading cowsay-6.1-py3-none-any.whl (25 kB)
Installing collected packages: cowsay
Successfully installed cowsay-6.1
```

---

_Renamed from "FreeBSD iocage `uv pip install` results in "invalid peer certificate"" to "FreeBSD iocage `uv pip install` in `uv venv` results in "invalid peer certificate"" by @cmpadden on 2024-05-04 04:14_

---

_Comment by @zanieb on 2024-05-04 04:49_

Hi! It looks like you're on an older version of uv which uses [system certificates](https://github.com/astral-sh/uv?tab=readme-ov-file#custom-ca-certificates) by default. If you upgrade, we switched to using bundled certificates (like pip) by default in [0.1.18](https://github.com/astral-sh/uv/releases/tag/0.1.18) (https://github.com/astral-sh/uv/pull/2362).

---

_Label `question` added by @zanieb on 2024-05-04 04:49_

---

_Comment by @cmpadden on 2024-05-04 05:18_

> Hi! It looks like you're on an older version of uv which uses [system certificates](https://github.com/astral-sh/uv?tab=readme-ov-file#custom-ca-certificates) by default. If we you upgrade, we switched to using bundled certificates (like pip) by default in [0.1.18](https://github.com/astral-sh/uv/releases/tag/0.1.18) (#2362).

Hi @zanieb, thanks for such a quick response! I'll need to tinker a bit to see how FreeBSD packages their binaries, as I'm unable to install it via the curl script, or via pip. Will update here once I get the latest version running.

```
[root@cage ~]# curl -LsSf https://astral.sh/uv/install.sh | sh
ERROR: there isn't a package for x86_64-unknown-freebsd
```

```
[root@cage ~]# python3 -m pip install uv

<snip>

        Error configuring OpenSSL build:
            Command: cd "/tmp/pip-install-ch8k_j4t/uv_d766d0d998ad417691e85338c13f452d/target/release/build/openssl-sys-f012f36da6f58d09/out/openssl-build/build/src" && env -u CROSS_COMPILE AR="ar" CC="cc" RANLIB="ranlib" "perl" "./Configure" "--prefix=/tmp/pip-install-ch8k_j4t/uv_d766d0d998ad417691e85338c13f452d/target/release/build/openssl-sys-f012f36da6f58d09/out/openssl-build/install" "--openssldir=/usr/local/ssl" "no-dso" "no-shared" "no-ssl3" "no-tests" "no-comp" "no-zlib" "no-zlib-dynamic" "--libdir=lib" "no-md2" "no-rc5" "no-weak-ssl-ciphers" "no-camellia" "no-idea" "no-seed" "BSD-x86_64" "-O2" "-ffunction-sections" "-fdata-sections" "-fPIC" "-m64" "--target=x86_64-unknown-freebsd"
            Failed to execute: No such file or directory (os error 2)



        note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
      warning: build failed, waiting for other jobs to finish...
      💥 maturin failed
        Caused by: Failed to build a native library through cargo
        Caused by: Cargo build finished with "exit status: 101": `env -u CARGO "cargo" "rustc" "--message-format" "json-render-diagnostics" "--manifest-path" "/tmp/pip-install-ch8k_j4t/uv_d766d0d998ad417691e85338c13f452d/crates/uv/Cargo.toml" "--release" "--bin" "uv" "--" "-C" "link-arg=-s"`
      Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/root/.venv/bin/python3', '--compatibility', 'off'] returned non-zero exit status 1
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for uv
Failed to build uv
ERROR: Could not build wheels for uv, which is required to install pyproject.toml-based projects
```


---

_Comment by @zanieb on 2024-05-04 06:08_

Hm sorry we don't have a pre-built version of uv for FreeBSD, I'm not sure how hard that would be for us to add. We can track that in https://github.com/astral-sh/uv/issues/3370.

You'll need our development prerequisites to build from source. There are some more details on that in our [contributing guide](https://github.com/astral-sh/uv/blob/main/CONTRIBUTING.md#setup).

---

_Comment by @cmpadden on 2024-05-04 17:49_

The FreeBSD maintainer @yurivict has updated their port to be v0.1.39, and it looks like we're back in business.

https://github.com/freebsd/freebsd-ports/commit/6a6dca9fdc456632e9690a9cfc63626a91666f3a

Thank you both for the assistance!

---

_Closed by @cmpadden on 2024-05-04 17:49_

---

_Comment by @charliermarsh on 2024-05-04 17:50_

Right on, thanks for following up!

---

_Referenced in [rust-lang/rustup#3810](../../rust-lang/rustup/pulls/3810.md) on 2024-05-07 02:37_

---

_Comment by @rami3l on 2024-05-07 02:41_

@zanieb @konstin Just to share the experience: at Rustup we [use `vmactions/freebsd-vm`](https://github.com/rust-lang/rustup/blob/f26d16b3b940bfb7b5e5aad7ee8f7121569f1d60/ci/actions-templates/freebsd-builds-template.yaml) to test and ship for FreeBSD without leaving GitHub Actions.

Also, facing this very issue, `pkg install -y ca_root_nss` has fixed the CI for us (https://github.com/rust-lang/rustup/pull/3810), without having to change the CA certs to bundled.

---

_Comment by @charliermarsh on 2024-05-07 02:46_

You rock thanks for sharing @rami3l.

---
