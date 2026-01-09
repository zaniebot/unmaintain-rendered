---
number: 7441
title: "Can't build uv 4.10 on Archlinux"
type: issue
state: closed
author: mardiros
labels:
  - bug
  - releases
assignees: []
created_at: 2024-09-16T21:05:37Z
updated_at: 2024-09-18T12:14:08Z
url: https://github.com/astral-sh/uv/issues/7441
synced_at: 2026-01-07T13:12:17-06:00
---

# Can't build uv 4.10 on Archlinux

---

_Issue opened by @mardiros on 2024-09-16 21:05_

Hi, 

I am trying to update (uv package for archlinux users)[https://gitlab.archlinux.org/archlinux/packaging/packages/uv/-/merge_requests/1] but I have an invalid dynamic links bound to the uv binary.

* A minimal code snippet that reproduces the bug.

Using archlinux with the last rustc stable compiler (1.18.1) and maturin 1.7.1 

```bash
$ git clone git@github.com:astral-sh/uv.git
$ cd uv
$ maturin build
```

Afterwhat the `uv` binary is linked to non existing library `libbz2-3265f064.so.1.0`

```bash
$ ldd target/debug/uv
        linux-vdso.so.1 (0x000076ffea2db000)
        libbz2-3265f064.so.1.0 => not found
        libgcc_s.so.1 => /usr/lib/libgcc_s.so.1 (0x000076ffea280000)
        libm.so.6 => /usr/lib/libm.so.6 (0x000076ffea191000)
        libc.so.6 => /usr/lib/libc.so.6 (0x000076ffe620f000)
        /lib64/ld-linux-x86-64.so.2 => /usr/lib64/ld-linux-x86-64.so.2 (0x000076ffea2dd000)
```

Afterwhat it cabbot be repaired.

```
~/workspace/git/uv on  main [!] is 📦 v0.4.10 via 🐍 v3.12.5 via 🦀 v1.81.0
$  maturin build
📦 Including license file "/home/guillaume/workspace/git/uv/LICENSE-APACHE"
📦 Including license file "/home/guillaume/workspace/git/uv/LICENSE-MIT"
🍹 Building a mixed python/rust project
🔗 Found bin bindings
📡 Using build options bindings from pyproject.toml
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.21s
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.21s
📦 Including files matching "rust-toolchain.toml"
💥 maturin failed
  Caused by: Cannot repair wheel, because required library libbz2-3265f064.so.1.0 could not be located.
```

I currently use patchelf to fix the binding after the build.


Note that while using `cargo build`, the uv binary build does not have the wrong binding.

```bash
$ ldd target/debug/uv
        linux-vdso.so.1 (0x00007516d5679000)
        libbz2.so.1.0 => /usr/lib/libbz2.so.1.0 (0x00007516d5639000)
        libgcc_s.so.1 => /usr/lib/libgcc_s.so.1 (0x00007516d560b000)
        libm.so.6 => /usr/lib/libm.so.6 (0x00007516d1711000)
        libc.so.6 => /usr/lib/libc.so.6 (0x00007516d1520000)
        /lib64/ld-linux-x86-64.so.2 => /usr/lib64/ld-linux-x86-64.so.2 (0x00007516d567b000)
```

At the moment, I did not succeed at isolate the bug between maturin and bzip2.

---

_Comment by @zanieb on 2024-09-16 21:13_

cc @konstin 

Thanks for the report!

---

_Label `bug` added by @zanieb on 2024-09-16 21:13_

---

_Label `releases` added by @zanieb on 2024-09-16 21:13_

---

_Comment by @charliermarsh on 2024-09-16 21:27_

Were you able to build previous versions of uv?

---

_Comment by @zanieb on 2024-09-16 21:29_

This patch wasn't required for the previous version, but that was `0.3.1` — it'd be great to have more clarity about when this regressed.

---

_Comment by @mardiros on 2024-09-17 06:50_

> Were you able to build previous versions of uv?

Good question, because I did not try before you ask (I am not the maintainer of the package).

So at the moment, building tags `0.3.1`, I am having the same issue.

As I see, maturin is using patchelf so I suppose the issue should be in [maturin itself](https://github.com/PyO3/maturin/blob/main/src/build_context.rs#L359)


---

_Comment by @mardiros on 2024-09-17 07:01_

Ok so i've started from the first commit that introduce libbz2

`* d2551bb2 - (HEAD) Add support for .tar.bz2 source distributions (#3069) (5 months ago) <Sergey Kolosov>`

And its where the bug come from.

```
~/workspace/git/uv on  HEAD (d2551bb) is 📦 v0.1.32 via 🐍 v3.12.5 via 🦀 v1.77.2 took 2s
❯ ldd target/debug/uv
        linux-vdso.so.1 (0x000073ec50566000)
        libbz2-3265f064.so.1.0 => not found
        libz.so.1 => /usr/lib/libz.so.1 (0x000073ec5051f000)
        libgcc_s.so.1 => /usr/lib/libgcc_s.so.1 (0x000073ec504f1000)
        libm.so.6 => /usr/lib/libm.so.6 (0x000073ec50402000)
        libc.so.6 => /usr/lib/libc.so.6 (0x000073ec4ca0f000)
        /lib64/ld-linux-x86-64.so.2 => /usr/lib64/ld-linux-x86-64.so.2 (0x000073ec50568000)
```

I suppose that you can close the issue, because it is probably not related on uv itself and now, I should probably open
an issue at maturin.

---

_Comment by @konstin on 2024-09-17 08:55_

maturin uses patchelf to bundle shared libraries into the wheel so it can be uploaded to pypi without external dependencies.

Unfortunately, i can't reproduce your problem on my machine (f679987fe1908c8a815dbcbc6a2a112ae49858ba):

```console
$ uvx maturin build
📦 Including license file "/home/konsti/projects/uv/LICENSE-APACHE"
📦 Including license file "/home/konsti/projects/uv/LICENSE-MIT"
🍹 Building a mixed python/rust project
🔗 Found bin bindings
📡 Using build options bindings from pyproject.toml
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.10s
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.10s
⚠️  Warning: No compatible platform tag found, using the linux tag instead. You won't be able to upload those wheels to PyPI.
📦 Including files matching "rust-toolchain.toml"
📦 Built wheel to /home/konsti/projects/uv/target/wheels/uv-0.4.10-py3-none-linux_x86_64.whl
$ ldd target/debug/uv
	linux-vdso.so.1 (0x00007fffbc57e000)
	libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007d2ef9dad000)
	libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007d2ef5f17000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007d2ef5c00000)
	/lib64/ld-linux-x86-64.so.2 (0x00007d2ef9dfa000)
```

fwiw you don't critically need maturin, for uv it essentially calls `cargo build` and bundles the resulting binaries (`uv` and `uvx`) with the python entrypoint (the `python` directory), so if pacman uses a custom format anyway you can also bundle those yourself.

---

_Comment by @zanieb on 2024-09-17 13:34_

@mardiros Would you be able to ping one of the uv package maintainers on this issue? We're happy to help — it's a bummer to see such an old version on Arch.

---

_Comment by @kahojyun on 2024-09-17 16:22_

Do we need to repair the wheel with `patchelf` when packaging for a specific distro? This process can be skipped by passing `--compatibility linux` or `--auditwheel skip` to `maturin`.

---

_Comment by @mardiros on 2024-09-17 21:01_

I have good news.

The proposal from @kahojyun  to pass ` --compatibility linux` works.


@zanieb 

> @mardiros Would you be able to ping one of the uv package maintainers on this issue? We're happy to help — it's a bummer to see such an old version on Arch.

Both issues are cross referenced. I agree that Arch user deserve an up to date version of uv, this is why I starts my contribution.

Thanks everyone for the help and support. 🙏🏿 

---

_Closed by @mardiros on 2024-09-17 21:01_

---

_Comment by @orhun on 2024-09-18 12:13_

uv 4.11 is now packaged & available for Arch Linux!

---
