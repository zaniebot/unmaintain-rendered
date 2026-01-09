---
number: 2442
title: Unable to install on FreeBSD
type: issue
state: closed
author: hhartzer
labels: []
assignees: []
created_at: 2024-03-14T02:09:28Z
updated_at: 2025-07-07T08:51:09Z
url: https://github.com/astral-sh/uv/issues/2442
synced_at: 2026-01-07T13:12:17-06:00
---

# Unable to install on FreeBSD

---

_Issue opened by @hhartzer on 2024-03-14 02:09_

This looks like an interesting project! I was excited to try it out, but alas I cannot use it on FreeBSD.

```
# pipx install uv
Fatal error from pip prevented installation. Full pip output in file:
    /root/.local/pipx/logs/cmd_2024-03-14_01.48.57_pip_errors.log

pip failed to build package:
    uv

Some possibly relevant errors from pip install:
    error: subprocess-exited-with-error
    error: could not compile `pubgrub` (lib) due to 2 previous errors
    Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/root/.local/pipx/venvs/uv/bin/python', '--compatibility', 'off'] returned non-zero exit status 1

Error installing uv.
```

The log:

```
PIP STDOUT
----------
Collecting uv
  Downloading uv-0.1.19.tar.gz (560 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 560.4/560.4 kB 3.2 MB/s eta 0:00:00
  Installing build dependencies: started
  Installing build dependencies: finished with status 'done'
  Getting requirements to build wheel: started
  Getting requirements to build wheel: finished with status 'done'
  Preparing metadata (pyproject.toml): started
  Preparing metadata (pyproject.toml): finished with status 'done'
Building wheels for collected packages: uv
  Building wheel for uv (pyproject.toml): started
  Building wheel for uv (pyproject.toml): still running...
  Building wheel for uv (pyproject.toml): still running...
  Building wheel for uv (pyproject.toml): still running...
  Building wheel for uv (pyproject.toml): still running...
  Building wheel for uv (pyproject.toml): finished with status 'error'
Failed to build uv

PIP STDERR
----------
  error: subprocess-exited-with-error

  × Building wheel for uv (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> [178 lines of output]
      Running `maturin pep517 build-wheel -i /root/.local/pipx/venvs/uv/bin/python --compatibility off`
      🍹 Building a mixed python/rust project
      🔗 Found bin bindings
      📡 Using build options bindings from pyproject.toml
         Compiling proc-macro2 v1.0.79
         Compiling unicode-ident v1.0.12
         Compiling libc v0.2.153
         Compiling cfg-if v1.0.0
         Compiling autocfg v1.1.0
         Compiling version_check v0.9.4
         Compiling quote v1.0.35
         Compiling jobserver v0.1.28
         Compiling syn v2.0.52
         Compiling cc v1.0.90
         Compiling once_cell v1.19.0
         Compiling serde v1.0.197
         Compiling pin-project-lite v0.2.13
         Compiling memchr v2.7.1
         Compiling getrandom v0.2.12
<snip>
         Compiling crypto-common v0.1.6
         Compiling block-buffer v0.10.4
         Compiling crossbeam-epoch v0.9.18
         Compiling instant v0.1.12
         Compiling subtle v2.5.0
         Compiling mime v0.3.17
         Compiling owo-colors v4.0.0
         Compiling uv-warnings v0.0.1 (/tmp/pip-install-xjzv6y7u/uv_6b8aa8085de24aea9bc57d1db6ee026e/crates/uv-warnings)
         Compiling digest v0.10.7
         Compiling backoff v0.4.0
         Compiling crossbeam-deque v0.8.5
         Compiling pubgrub v0.2.1 (https://github.com/astral-sh/pubgrub?rev=addbaf184891d66a2dfd93d241a66d13bfe5de86#addbaf18)
         Compiling hyper-rustls v0.24.2
      error[E0562]: `impl Trait` only allowed in function and inherent method return types, not in trait method return types
         --> /root/.cargo/git/checkouts/pubgrub-4eebf9758cb3b340/addbaf1/src/solver.rs:264:30
          |
      264 |     ) -> Result<Dependencies<impl IntoIterator<Item = (P, VS)> + Clone>, Self::Err>;
          |                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
          |
          = note: see issue #91611 <https://github.com/rust-lang/rust/issues/91611> for more information

      error[E0562]: `impl Trait` only allowed in function and inherent method return types, not in `impl` method return types
         --> /root/.cargo/git/checkouts/pubgrub-4eebf9758cb3b340/addbaf1/src/solver.rs:372:30
          |
      372 |     ) -> Result<Dependencies<impl IntoIterator<Item = (P, VS)> + Clone>, Self::Err> {
          |                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
          |
          = note: see issue #91611 <https://github.com/rust-lang/rust/issues/91611> for more information

         Compiling rustls-native-certs v0.6.3
      For more information about this error, try `rustc --explain E0562`.
      error: could not compile `pubgrub` (lib) due to 2 previous errors
      warning: build failed, waiting for other jobs to finish...
      💥 maturin failed
        Caused by: Failed to build a native library through cargo
        Caused by: Cargo build finished with "exit status: 101": `env -u CARGO "cargo" "rustc" "--message-format" "json-render-diagnostics" "--manifest-path" "/tmp/pip-install-xjzv6y7u/uv_6b8aa8085de24aea9bc57d1db6ee026e/crates/uv/Cargo.toml" "--release" "--bin" "uv" "--" "-C" "link-arg=-s"`
      Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/root/.local/pipx/venvs/uv/bin/python', '--compatibility', 'off'] returned non-zero exit status 1
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for uv
ERROR: Could not build wheels for uv, which is required to install pyproject.toml-based projects
```

I assume this pertains to: https://github.com/rustls/hyper-rustls

Which as no issues regarding FreeBSD.

I'm no rust expert, but I've seen similar errors before when there's a new rust version that the code requires, and the package doesn't update the minimum rust required version. However, the actual issue could be totally different.

If the issue does seem to be with hyper-rustls, it seems like I should be able to reproduce it with `cargo`. If I can do that, I should be able to open an issue over there and link it to here.

Does this all sound good? Would I need to run something like `cargo install hyper-rustls`?

Thank you!

---

_Comment by @charliermarsh on 2024-03-14 02:12_

I think your Rust version is too low -- `impl Trait` is now allowed there, but wasn't until recently. Can you upgrade to `rustc 1.76.0`?

---

_Comment by @hhartzer on 2024-03-14 02:21_

Gotcha, that does make sense. I'll see if I can update.

---

_Referenced in [rustls/hyper-rustls#258](../../rustls/hyper-rustls/issues/258.md) on 2024-03-14 02:25_

---

_Comment by @hhartzer on 2024-03-14 02:53_

I may end up waiting till April to get on 1.76. Happy to close this out till then if it does take me that long. I'll have to build from source if not, which will probably take some hours.

That said, I think I misread the logs and it's actually pubgrub that has rust-version set incorrectly.

Your pubgrub is a fork and I can't make issues there. No idea what your long term plans for the project are, but it seems like you can test/set rust-version accordingly with this:

 * https://crates.io/crates/cargo-msrv
 * https://doc.rust-lang.org/cargo/guide/continuous-integration.html#verifying-rust-version

(You may already be aware of this. I had no idea and just found out about it.)

---

_Comment by @zanieb on 2024-03-15 21:18_

I could be missing this but it doesn't look like PubGrub _has_ a minimum Rust version declared. Perhaps we should add one but regardless we require a newer version of Rust.

Hopefully it doesn't take hours to build from source, it's generally pretty quick.

---

_Comment by @hhartzer on 2024-03-16 17:28_

I do think having PubGrub state the version would be nice, although for most users it may not be an issue.

I meant building Rust from source, although it could also be my machine.

---

_Referenced in [pubgrub-rs/pubgrub#197](../../pubgrub-rs/pubgrub/issues/197.md) on 2024-03-22 16:24_

---

_Comment by @Eh2406 on 2024-03-22 17:02_

> I meant building Rust from source, although it could also be my machine.

I believe you can get the official prebuilt Rust binaries for FreeBSD using RustUp. https://www.rust-lang.org/tools/install

> I do think having PubGrub state the version would be nice, although for most users it may not be an issue.

If pubgrub set its MSRV, you would have gotten a better error message. The error message would have made it clear that the project would not work in less you upgraded your Rust. So the core of the problem would still be the same.

---

_Referenced in [astral-sh/uv#2618](../../astral-sh/uv/pulls/2618.md) on 2024-03-22 18:35_

---

_Comment by @zanieb on 2024-04-12 20:10_

I don't think this is actionable for us.

---

_Closed by @zanieb on 2024-04-12 20:10_

---

_Comment by @cmpadden on 2024-05-04 03:40_

For those looking for alternative solutions to this, `uv` can now be installed directly through `pkg`.
```
# pkg install uv
Updating FreeBSD repository catalogue...
FreeBSD repository is up to date.
All repositories are up to date.
The following 1 package(s) will be affected (of 0 checked):

New packages to be INSTALLED:
        uv: 0.1.15_1

Number of packages to be installed: 1

The process will require 36 MiB more space.
11 MiB to be downloaded.

Proceed with this action? [y/N]: y
[user] [1/1] Fetching uv-0.1.15_1.pkg: 100%   11 MiB  11.1MB/s    00:01
Checking integrity... done (0 conflicting)
[user] [1/1] Installing uv-0.1.15_1...
[user] [1/1] Extracting uv-0.1.15_1: 100%
```

---

_Referenced in [astral-sh/uv#3370](../../astral-sh/uv/issues/3370.md) on 2024-05-04 06:08_

---

_Comment by @rcghpge on 2025-07-05 11:32_

Hey group, I am building out a project for Python R&D on the FreeBSD operating system. I am reading through the uv documentation. Is there a set `sys_platform` that we need to set our uv project enviroments to specifically for FreeBSD. Keeping the conversation going for FreeBSD development [uv Settings environments](https://docs.astral.sh/uv/reference/settings/#environments)

---

_Comment by @konstin on 2025-07-07 08:29_

`sys_platform` is queried from the Python interpreter, there's nothing that needs to be set manually.

---

_Comment by @rcghpge on 2025-07-07 08:51_

> `sys_platform` is queried from the Python interpreter, there's nothing that needs to be set manually.

Hey konstin I just pushed a repo to test venvs on FreeBSD. Feel free to poke around [annum-tab](https://github.com/rcghpge/annum-tab)


---
