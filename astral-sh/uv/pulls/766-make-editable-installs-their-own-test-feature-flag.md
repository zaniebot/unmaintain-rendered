```yaml
number: 766
title: Make editable installs their own test feature flag
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/editables
created_at: 2024-01-04T01:26:21Z
updated_at: 2024-01-05T13:35:42Z
url: https://github.com/astral-sh/uv/pull/766
synced_at: 2026-01-12T16:04:10Z
```

# Make editable installs their own test feature flag

---

_@charliermarsh_

For whatever reason these fail for me with mold, and it's not worth it to me to disable the linker.


---

_Label `internal` added by @charliermarsh on 2024-01-04 01:26_

---

_Merged by @charliermarsh on 2024-01-04 01:33_

---

_Closed by @charliermarsh on 2024-01-04 01:33_

---

_Branch deleted on 2024-01-04 01:33_

---

_Comment by @konstin on 2024-01-04 11:02_

Can you post the error you get?

---

_Comment by @charliermarsh on 2024-01-04 13:53_

```
----- stderr -----
error: Failed to build editables
  Caused by: Failed to build editable: file://[WORKSPACE_DIR]/scripts/editable-installs/maturin_editable/
  Caused by: Failed to build: file://[WORKSPACE_DIR]/scripts/editable-installs/maturin_editable/
  Caused by: Build backend failed to build wheel through `build_editable()`:
--- stdout:
Running `maturin pep517 build-wheel -i /private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpQwDZR9/.venv/bin/python --compatibility off --editable`
--- stderr:
🍹 Building a mixed python/rust project
🔗 Found pyo3 bindings
🐍 Found CPython 3.12 at /private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpQwDZR9/.venv/bin/python
📡 Using build options features from pyproject.toml
   Compiling pyo3-build-config v0.20.0
   Compiling pyo3-ffi v0.20.0
   Compiling pyo3 v0.20.0
   Compiling maturin_editable v0.1.0 ([WORKSPACE_DIR]/scripts/editable-installs/maturin_editable)
error: linking with `clang` failed: exit status: 1
  |
  = note: env -u IPHONEOS_DEPLOYMENT_TARGET -u TVOS_DEPLOYMENT_TARGET LC_ALL="C" PATH="/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/bin:/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpQwDZR9/.venv/bin:/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/bin:[WORKSPACE_DIR]/target/release:/Users/crmarsh/.bun/bin:/opt/homebrew/sbin:/opt/homebrew/bin:/Library/Frameworks/Python.framework/Versions/3.7/bin:/usr/local/bin:/System/Cryptexes/App/usr/bin:/usr/bin:/bin:/usr/sbin:/sbin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/local/bin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/bin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/appleinternal/bin:/Users/crmarsh/.cargo/bin:/Users/crmarsh/.deno/bin:/Users/crmarsh/.poetry/bin:/Users/crmarsh/.pyenv/bin:/Users/crmarsh/bin:/opt/homebrew/bin:/Users/crmarsh/.local/bin:/Users/crmarsh/.antigen/bundles/agkozak/zsh-z:/Users/crmarsh/.antigen/bundles/zsh-users/zsh-syntax-highlighting:/Users/crmarsh/.antigen/bundles/zsh-users/zsh-autosuggestions:/Users/crmarsh/.local/bin" VSLANG="1033" ZERO_AR_DATE="1" "clang" "-Wl,-exported_symbols_list,/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/rustcHZzMB2/list" "-arch" "arm64" "/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/rustcHZzMB2/symbols.o" "[WORKSPACE_DIR]/scripts/editable-installs/maturin_editable/../../../target/target_install_editable/release/deps/maturin_editable.maturin_editable.ee7895962565d733-cgu.0.rcgu.o" "[WORKSPACE_DIR]/scripts/editable-installs/maturin_editable/../../../target/target_install_editable/release/deps/maturin_editable.maturin_editable.ee7895962565d733-cgu.1.rcgu.o" "[WORKSPACE_DIR]/scripts/editable-installs/maturin_editable/../../../target/target_install_editable/release/deps/maturin_editable.maturin_editable.ee7895962565d733-cgu.2.rcgu.o" "[WORKSPACE_DIR]/scripts/editable-installs/maturin_editable/../../../target/target_install_editable/release/deps/maturin_editable.1pghlbsr7hrzdldh.rcgu.o" "-L" "[WORKSPACE_DIR]/scripts/editable-installs/maturin_editable/../../../target/target_install_editable/release/deps" "-L" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib" "[WORKSPACE_DIR]/target/target_install_editable/release/deps/libpyo3-fabb9ec73102c1e4.rlib" "[WORKSPACE_DIR]/target/target_install_editable/release/deps/libmemoffset-0be0e7ae85cfad40.rlib" "[WORKSPACE_DIR]/target/target_install_editable/release/deps/libparking_lot-a019aaa12f2dbbdb.rlib" "[WORKSPACE_DIR]/target/target_install_editable/release/deps/libparking_lot_core-402ee42bf33faf36.rlib" "[WORKSPACE_DIR]/target/target_install_editable/release/deps/libcfg_if-ebd62bc4c95f9afe.rlib" "[WORKSPACE_DIR]/target/target_install_editable/release/deps/libsmallvec-2399b34ac1bdbdc7.rlib" "[WORKSPACE_DIR]/target/target_install_editable/release/deps/liblock_api-c829434f75dfc0e1.rlib" "[WORKSPACE_DIR]/target/target_install_editable/release/deps/libscopeguard-56416d264ff4d2a6.rlib" "[WORKSPACE_DIR]/target/target_install_editable/release/deps/libpyo3_ffi-0b0a088518af85fc.rlib" "[WORKSPACE_DIR]/target/target_install_editable/release/deps/liblibc-2a600e5bbb3fd567.rlib" "[WORKSPACE_DIR]/target/target_install_editable/release/deps/libunindent-8d530f6225ed1e81.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libstd-b0083070c892a1db.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libpanic_unwind-8282820217d7b362.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libobject-f163e9d1987a8318.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libmemchr-350512940f04084a.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libaddr2line-e19e4ea986b9addc.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libgimli-363744fff3c4e7ba.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/librustc_demangle-e1d006f163566466.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libstd_detect-dea09910a3b22702.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libhashbrown-4802352fcc77de56.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/librustc_std_workspace_alloc-e71c86d9086176a7.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libminiz_oxide-6d82e7a8c3f5e2c7.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libadler-e66d24d044cc2029.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libunwind-46eaa7bd445cb528.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libcfg_if-3c8a48285a1e7255.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/liblibc-50e20e60add24734.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/liballoc-edb678dd3e28691a.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/librustc_std_workspace_core-3b6c10a2acaa607f.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libcore-2447397acf63b01e.rlib" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libcompiler_builtins-5816c590a0da89c2.rlib" "-liconv" "-lSystem" "-lc" "-lm" "-L" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib" "-o" "[WORKSPACE_DIR]/scripts/editable-installs/maturin_editable/../../../target/target_install_editable/release/deps/libmaturin_editable.dylib" "-Wl,-dead_strip" "-dynamiclib" "-Wl,-dylib" "-nodefaultlibs" "-undefined" "dynamic_lookup" "-Wl,-install_name,@rpath/maturin_editable.cpython-312-darwin.so" "-fuse-ld=/Users/crmarsh/workspace/sold/build/ld64"
  = note: mold: fatal: unknown command line option: -undefined
          clang: error: linker command failed with exit code 1 (use -v to see invocation)
          

error: could not compile `maturin_editable` (lib) due to previous error
💥 maturin failed
  Caused by: Failed to build a native library through cargo
  Caused by: Cargo build finished with "exit status: 101": `env -u CARGO CARGO_ENCODED_RUSTFLAGS="-C\u{1f}link-arg=-fuse-ld=/Users/crmarsh/workspace/sold/build/ld64" PYO3_ENVIRONMENT_SIGNATURE="cpython-3.12-64bit" PYO3_PYTHON="/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpQwDZR9/.venv/bin/python" PYTHON_SYS_EXECUTABLE="/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpQwDZR9/.venv/bin/python" "/Users/crmarsh/.rustup/toolchains/1.75-aarch64-apple-darwin/bin/cargo" "rustc" "--features" "pyo3/extension-module" "--message-format" "json-render-diagnostics" "--manifest-path" "[WORKSPACE_DIR]/scripts/editable-installs/maturin_editable/Cargo.toml" "--release" "--lib" "--" "-C" "link-arg=-undefined" "-C" "link-arg=dynamic_lookup" "-C" "link-args=-Wl,-install_name,@rpath/maturin_editable.cpython-312-darwin.so"`
Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpQwDZR9/.venv/bin/python', '--compatibility', 'off', '--editable'] returned non-zero exit status 1
---
```

---

_Comment by @konstin on 2024-01-04 14:00_

That's strange `-undefined dynamic_lookup` is required to build pyo3 native modules because we can't link libpython and the macos toolchain otherwise complains. I'm surprised nobody reported this upstream yet.

---

_Comment by @konstin on 2024-01-04 14:07_

Could you try the latest version of sold, this has been fixed upstream: https://github.com/bluewhalesystems/sold/issues/13


---

_Comment by @charliermarsh on 2024-01-04 16:39_

@konstin - Will try!

---

_Comment by @charliermarsh on 2024-01-04 21:57_

@konstin - I now get:

```
thread 'sync_editable' panicked at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/ops/function.rs:250:5:
Unexpected failure.
code=1
stderr=``````
Traceback (most recent call last):
  File \"<string>\", line 1, in <module>
  File \"/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/python/maturin_editable/__init__.py\", line 1, in <module>
    from .maturin_editable import *
ImportError: dlopen(/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/python/maturin_editable/maturin_editable.cpython-312-darwin.so, 0x0002): weak-def symbol not found \'_PyBytes_AsString\'
```
```
command=`cd "/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpPeRL51" && "/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpPeRL51/.venv/bin/python" "-B" "-c" "from maturin_editable import sum_as_string, version\n\nassert version == 1, version\nassert sum_as_string(1, 2) == \"3\", sum_as_string(1, 2)\n"`
code=1
stdout=""
stderr=```
Traceback (most recent call last):
  File \"<string>\", line 1, in <module>
  File \"/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/python/maturin_editable/__init__.py\", line 1, in <module>
    from .maturin_editable import *
ImportError: dlopen(/Users/crmarsh/workspace/puffin/scripts/editable-installs/maturin_editable/python/maturin_editable/maturin_editable.cpython-312-darwin.so, 0x0002): weak-def symbol not found \'_PyBytes_AsString\'
```

---

_Comment by @konstin on 2024-01-05 13:35_

That's also odd, does this python version work e.g. with `orjson`? I've heard things mentioned about framework builds with brew, but i don't have a mac to test myself.

---
