```yaml
number: 8692
title: "\"Error: No such image: mcr.microsoft.com/devcontainers/rust:0-1-bullseye\" error when attempting to open devcontainer in vscode"
type: issue
state: open
author: DetachHead
labels:
  - bug
assignees: []
created_at: 2023-11-15T11:28:10Z
updated_at: 2024-01-16T23:11:19Z
url: https://github.com/astral-sh/ruff/issues/8692
synced_at: 2026-01-12T15:54:48Z
```

# "Error: No such image: mcr.microsoft.com/devcontainers/rust:0-1-bullseye" error when attempting to open devcontainer in vscode

---

_@DetachHead_

to reproduce:

1. open the repo in vscode
2. hit f1 and type "Dev Containers: Reopen in Container" then hit enter


logs:

```
[81043260 ms] Start: Run: docker pull mcr.microsoft.com/devcontainers/rust:0-1-bullseye
0-1-bullseye: Pulling from devcontainers/rust
bd73737482dd: Pull complete
6710592d62aa: Pull complete
75256935197e: Pull complete
c1e5026c6457: Retrying in 1 second
b6e9298ad07c: Downloading  51.17MB/51.17MB
465cf57c99a0: Download complete
e6352427f489: Download complete
36b1de8d22ed: Download complete
41e032ec4792: Download complete
782e668b207b: Download complete
d8f8c78328ca: Download complete
7f4473b57e81: Retrying in 1 second
7d6b70440e46: Waiting
open /var/lib/docker/tmp/GetImageBlob521495043: read-only file system
[81206501 ms] []
[81206501 ms] Error: No such image: mcr.microsoft.com/devcontainers/rust:0-1-bullseye

[81206501 ms] Command failed: docker inspect --type image mcr.microsoft.com/devcontainers/rust:0-1-bullseye
[81206519 ms] Exit code 1
[81224380 ms] Start: Run: C:\Program Files\Microsoft VS Code\Code.exe --ms-enable-electron-run-as-node c:\Users\userr\.vscode\extensions\ms-vscode-remote.remote-containers-0.315.1\dist\spec-node\devContainersSpecCLI.js outdated --workspace-folder C:\Program Files\Microsoft VS Code --config c:\Users\userr\ruff\.devcontainer\devcontainer.json --output-format json --log-level debug --log-format json --terminal-columns 202 --terminal-rows 35
[81225883 ms] @devcontainers/cli 0.51.3. Node.js v18.15.0. win32 10.0.19045 x64.
[81225883 ms] Start: Run: git rev-parse --show-cdup
[81226342 ms] Start: Run: docker-credential-desktop get
```

---

_Label `question` added by @charliermarsh on 2023-11-15 16:41_

---

_Comment by @charliermarsh on 2023-11-15 16:41_

I know basically nothing about this :joy: Does anyone with VS Code experience have an idea of what could be happening here?

---

_Comment by @zanieb on 2023-11-15 17:07_

Perhaps you need to set a Rust version? i.e. https://github.com/microsoft/vscode-remote-release/issues/7753#issuecomment-1384319208

edit: I guess actually there the manifest is missing and here you cans ee the image pull successfully. Weird.

This line looks suspicious:

> open /var/lib/docker/tmp/GetImageBlob521495043: read-only file system

---

_Label `question` removed by @charliermarsh on 2024-01-08 03:48_

---

_Label `bug` added by @charliermarsh on 2024-01-08 03:48_

---

_Comment by @charliermarsh on 2024-01-16 00:32_

Is this still an issue @DetachHead?

---

_Comment by @DetachHead on 2024-01-16 23:11_

that specific error seems to no longer occur, but now i see these errors after it pulls the image:
```
[102080 ms] Port forwarding 64799 > 45691 > 45691 terminated with code 0 and signal null.
info: installing component 'rustc'
info: rolling back changes
error: failed to install component: 'rustc-aarch64-unknown-linux-gnu', detected conflict: 'bin/rust-gdb'
warning: removing stray hash found at '/usr/local/rustup/update-hashes/1.75-aarch64-unknown-linux-gnu' in order to continue
info: syncing channel updates for '1.75-aarch64-unknown-linux-gnu'
info: latest update on 2023-12-28, rust version 1.75.0 (82e1608df 2023-12-21)
info: downloading component 'cargo'
info: downloading component 'rust-std'
info: downloading component 'rustc'
[128015 ms] Start: Run in container: mkdir -p '/vscode/vscode-server/extensionsCache' && cd '/home/vscode/.vscode-server/extensionsCache' && cp 'charliermarsh.ruff-2024.2.0-linux-arm64' 'hbenl.vscode-test-explorer-2.21.1' 'ms-python.python-2023.22.1' 'ms-python.vscode-pylance-2023.12.1' 'ms-vscode.test-adapter-converter-0.1.9' 'mutantdino.resourcemonitor-1.0.7' 'rust-lang.rust-analyzer-0.3.1807-linux-arm64' 'serayuzgur.crates-0.6.5' 'swellaby.vscode-rust-test-adapter-0.11.0' 'tamasfe.even-better-toml-0.19.2' 'vadimcn.vscode-lldb-1.10.0' '/vscode/vscode-server/extensionsCache'
[128579 ms] 
[128581 ms] 
[128582 ms] Start: Run in container: cd '/vscode/vscode-server/extensionsCache' && ls -t | tail -n +50 | xargs rm -f
[128685 ms] 
[128689 ms] 
info: installing component 'cargo'
info: installing component 'rust-std'
info: installing component 'rustc'
info: rolling back changes
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/manifest-rust-std-aarch64-unknown-linux-gnu'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libunwind-c821617a29de84b3.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libunicode_width-9da7922e9da3f010.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libtest-bb85a582f3b58524.so'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libtest-bb85a582f3b58524.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libsysroot-73c80a3100d24095.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libstd_detect-828e977783680240.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libstd-ff9e2f4daa5f5044.so'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libstd-ff9e2f4daa5f5044.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/librustc_std_workspace_std-4fd664ebf34ba79b.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/librustc_std_workspace_core-fc61da4cf34b1cd0.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/librustc_std_workspace_alloc-07044d868b568254.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/librustc_demangle-770a536d7b802f6e.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/librustc-stable_rt.tsan.a'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/librustc-stable_rt.msan.a'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/librustc-stable_rt.lsan.a'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/librustc-stable_rt.hwasan.a'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/librustc-stable_rt.asan.a'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libprofiler_builtins-8239992b5efbdef2.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libproc_macro-0e8e08b5ae90502b.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libpanic_unwind-4c069e5be14e089e.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libpanic_abort-3be5ddefa2998d41.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libobject-5dad8c6832fe5ee1.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libminiz_oxide-b9447ffb1c9d5ec5.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libmemchr-08fa975fabbb7847.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/liblibc-27cd7f142906ea75.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libhashbrown-c2f157ffd03af249.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libgimli-29e75079cbe951ff.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libgetopts-4e3cb9b91285b783.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libcore-ead1225e03eb63b1.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libcompiler_builtins-3f410048cea510b6.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libcfg_if-077e039e6f0bd75d.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/liballoc-55c809a1b2d51fd2.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libadler-287e4034a78ea916.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/aarch64-unknown-linux-gnu/lib/libaddr2line-6fa5c572933bd83c.rlib'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/lib/rustlib/manifest-cargo-aarch64-unknown-linux-gnu'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/zsh/site-functions/_cargo'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-yank.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-version.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-verify-project.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-vendor.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-update.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-uninstall.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-tree.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-test.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-search.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-rustdoc.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-rustc.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-run.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-report.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-remove.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-publish.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-pkgid.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-package.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-owner.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-new.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-metadata.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-logout.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-login.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-locate-project.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-install.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-init.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-help.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-generate-lockfile.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-fix.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-fetch.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-doc.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-clean.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-check.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-build.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-bench.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/man/man1/cargo-add.1'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/doc/cargo/README.md'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/doc/cargo/LICENSE-THIRD-PARTY'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/doc/cargo/LICENSE-MIT'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/share/doc/cargo/LICENSE-APACHE'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/etc/bash_completion.d/cargo'
error: could not remove 'component' file: '/usr/local/rustup/toolchains/1.75-aarch64-unknown-linux-gnu/bin/cargo'
[169176 ms] Port forwarding 64799 > 45691 > 45691: Local close
error: failed to install component: 'rustc-aarch64-unknown-linux-gnu', detected conflict: 'lib/libdarling_macro-4fe028f65800e304.so'
```

i also can't seem to run anything with cargo:
```shell
vscode ➜ /workspaces/ruff (main) $ cargo run -p ruff -- --version
error: the 'cargo' binary, normally provided by the 'cargo' component, is not applicable to the '1.75-aarch64-unknown-linux-gnu' toolchain
```

---
