---
number: 10598
title: "[bug] Can't build Rust extensions via `pyo3` if using a `uv`-managed Python interpreter"
type: issue
state: closed
author: LukeMathWalker
labels:
  - bug
  - uv python
assignees: []
created_at: 2025-01-14T14:19:18Z
updated_at: 2025-01-15T20:11:56Z
url: https://github.com/astral-sh/uv/issues/10598
synced_at: 2026-01-10T01:24:55Z
---

# [bug] Can't build Rust extensions via `pyo3` if using a `uv`-managed Python interpreter

---

_Issue opened by @LukeMathWalker on 2025-01-14 14:19_

When building the extension, the following error occurs (omitting the prefixes of some paths):

```
dyld[90807]: Library not loaded: /install/lib/libpython3.13.dylib
  Referenced from: <A73153CF-70A5-379B-B9D3-526153D8322F> repro/target/debug/deps/setup-29f3c899bfb488ef
  Reason: tried: '/install/lib/libpython3.13.dylib' (no such file), 'repro/target/debug/deps/libpython3.13.dylib' (no such file), 'repro/target/debug/libpython3.13.dylib' (no such file), '.rustup/toolchains/stable-aarch64-
apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libpython3.13.dylib' (no such file), '.rustup/toolchains/stable-aarch64-apple-darwin/lib/libpython3.13.dylib' (no such file), '/Users/luca/lib/libpyt
hon3.13.dylib' (no such file), '/usr/local/lib/libpython3.13.dylib' (no such file), '/usr/lib/libpython3.13.dylib' (no such file, not in dyld cache)
error: test failed, to rerun pass `--lib
```

This is on macOS, using the latest Rust toolchain and `uv` build. I uploaded a [reproduction repository](https://github.com/LukeMathWalker/uv-repro-install-lib) to help with the investigation.

I originally suspected this to be an issue with `sysconfig`, but it appears to be configured correctly:

<img width="868" alt="Image" src="https://github.com/user-attachments/assets/e79936ec-d22d-4775-8be3-d9b6761d6884" />

---

_Comment by @charliermarsh on 2025-01-14 14:20_

Just to confirm: have you tried reinstalling Python (like `uv python install 3.13 --reinstall`)?

---

_Comment by @LukeMathWalker on 2025-01-14 14:20_

This came up while following up on [a user report](https://github.com/mainmatter/rust-python-interoperability/issues/12) after we ported our Rust-Python interoperability workshop from `rye` to `uv` (and for `rye` we were indeed patching `sysconfig` for a similar problem).

---

_Comment by @LukeMathWalker on 2025-01-14 14:21_

> Just to confirm: have you tried reinstalling Python (like `uv python install 3.13 --reinstall`)?

I had not!
But it doesn't seem to change the outcome (reinstall => `uv clean` => same steps => error).

---

_Comment by @charliermarsh on 2025-01-14 14:30_

@zanieb -- Any idea where this reference is?

---

_Comment by @LukeMathWalker on 2025-01-14 14:35_

If it helps, this is the configuration outputted by `pyo3-build-config` via `PYO3_PYTHON="${PWD}/.venv/bin/python3" PYO3_PRINT_CONFIG=1 cargo t`:

```
implementation=CPython
version=3.13
shared=true
abi3=false
lib_name=python3.13
lib_dir=/Users/luca/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib
executable=/Users/luca/code/scratch/repro/.venv/bin/python3
pointer_width=64
build_flags=
python_framework_prefix=
suppress_build_script_link_lines=false
```



---

_Comment by @LukeMathWalker on 2025-01-14 14:36_

I don't spot any smoking gun there since `lib_dir` is indeed pointing to the right place.
It somehow feels like the issue is the lack of that `lib_dir` in the list of paths to be searched for when looking for the `dylib` at runtime.

---

_Comment by @LukeMathWalker on 2025-01-14 14:55_

Well, it turns out that it is added to that list.
Setting `-vvv` on `cargo build`, we get the following output from `pyo3-ffi`:

```
[pyo3-ffi 0.23.4] cargo:rustc-link-search=native=/Users/luca/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib
```

which then shows up correctly as an `-L` option in the invocation of `rustc` to compile the final binary:

```
 -L native=/Users/luca/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib`
```

but [Cargo's documentations](https://doc.rust-lang.org/cargo/reference/environment-variables.html#dynamic-library-paths) states that:

> Search paths included from any build script with the [rustc-link-search instruction](https://doc.rust-lang.org/cargo/reference/build-scripts.html#rustc-link-search). Paths outside of the target directory are removed. It is the responsibility of the user running Cargo to properly set the environment if additional libraries on the system are needed in the search path.

So I do wonder if that's the root cause here? 
Namely, _something_ must copy the `dylib` into the `target` directory (or a directory on macOS's fallback list for dynamic library lookups) otherwise the binary won't work.

---

_Comment by @zanieb on 2025-01-14 15:20_

Interesting.

I wonder where the `/install/lib/libpython3.13.dylib` path is coming from?

The `rustc-link-search` caveat seems worth pursing. I tried using the `target` directory though — no dice. Maybe it needs to be in a specific spot?

```
❯ uv python install --install-dir ./target/python 3.12
Installed Python 3.12.8 in 1.45s
 + cpython-3.12.8-macos-aarch64-none
❯ PYO3_PYTHON="${PWD}/target/python/cpython-3.12.8-macos-aarch64-none/bin/python cargo t
   Compiling pyo3-build-config v0.23.4
   Compiling pyo3-macros-backend v0.23.4
   Compiling pyo3-ffi v0.23.4
   Compiling pyo3 v0.23.4
   Compiling pyo3-macros v0.23.4
   Compiling setup v0.1.0 (/Users/zb/workspace/uv-repro-install-lib)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 2.72s
     Running unittests src/lib.rs (target/debug/deps/setup-29f3c899bfb488ef)
dyld[37377]: Library not loaded: /install/lib/libpython3.12.dylib
  Referenced from: <594B5A77-99FB-3942-8F15-0A487FE66DDF> /Users/zb/workspace/uv-repro-install-lib/target/debug/deps/setup-29f3c899bfb488ef
  Reason: tried: '/install/lib/libpython3.12.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/install/lib/libpython3.12.dylib' (no such file), '/install/lib/libpython3.12.dylib' (no such file), '/Users/zb/workspace/uv-repro-install-lib/target/debug/deps/libpython3.12.dylib' (no such file), '/Users/zb/workspace/uv-repro-install-lib/target/debug/libpython3.12.dylib' (no such file), '/Users/zb/.rustup/toolchains/stable-aarch64-apple-darwin/lib/rustlib/aarch64-apple-darwin/lib/libpython3.12.dylib' (no such file), '/Users/zb/.rustup/toolchains/stable-aarch64-apple-darwin/lib/libpython3.12.dylib' (no such file), '/Users/zb/lib/libpython3.12.dylib' (no such file), '/usr/local/lib/libpython3.12.dylib' (no such file), '/usr/lib/libpython3.12.dylib' (no such file, not in dyld cache)
error: test failed, to rerun pass `--lib`

Caused by:
  process didn't exit successfully: `/Users/zb/workspace/uv-repro-install-lib/target/debug/deps/setup-29f3c899bfb488ef` (signal: 6, SIGABRT: process abort signal)
```

---

_Comment by @zanieb on 2025-01-14 15:21_

Oh hello

```
❯ uv python install --install-dir ./target/debug/deps/python 3.12
Installed Python 3.12.8 in 1.40s
 + cpython-3.12.8-macos-aarch64-none
❯ PYO3_PYTHON=/Users/zb/workspace/uv-repro-install-lib/target/debug/deps/python/cpython-3.12.8-macos-aarch64-none/bin/python cargo t
   Compiling pyo3-build-config v0.23.4
   Compiling pyo3-ffi v0.23.4
   Compiling pyo3-macros-backend v0.23.4
   Compiling pyo3 v0.23.4
   Compiling pyo3-macros v0.23.4
   Compiling setup v0.1.0 (/Users/zb/workspace/uv-repro-install-lib)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 2.84s
     Running unittests src/lib.rs (target/debug/deps/setup-29f3c899bfb488ef)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```

---

_Comment by @LukeMathWalker on 2025-01-14 15:24_

Using this build script for the same purpose seems to do the trick too:

```rust
use std::env::consts::{DLL_PREFIX, DLL_SUFFIX};
use std::path::PathBuf;

fn main() {
    let interpreter_config = pyo3_build_config::get();
    let lib_dir = interpreter_config
        .lib_dir
        .as_ref()
        .expect("lib_dir is not set for the Python interpreter detected by pyo3-build-config");
    let lib_name = interpreter_config
        .lib_name
        .as_ref()
        .expect("lib_name is not set for the Python interpreter detected by pyo3-build-config");
    let dll_name = format!("{DLL_PREFIX}{lib_name}{DLL_SUFFIX}");
    let dll_path = PathBuf::from(lib_dir).join(&dll_name);

    let out_dir = PathBuf::from(std::env::var("OUT_DIR").expect("OUT_DIR is not set"));
    std::fs::copy(dll_path, out_dir.join(dll_name)).expect("Failed to copy Python DLL");
    println!("cargo:rustc-link-search=native={}", out_dir.display());
}
```

Although it'd be nice if it wasn't needed 🤔 

---

_Comment by @zanieb on 2025-01-14 15:25_

I'm not sure what we could do to fix that; it seems like a pyo3 issue, right?

---

_Comment by @LukeMathWalker on 2025-01-14 15:25_

> The `rustc-link-search` caveat seems worth pursing. I tried using the `target` directory though — no dice. Maybe it needs to be in a specific spot?

I'd have to be `target/debug` (for a `debug` build). The top-level `target` directory it's indeed not in the search path by default.

---

_Comment by @LukeMathWalker on 2025-01-14 15:28_

> I'm not sure what we could do to fix that; it seems like a pyo3 issue, right?

It's unclear to me to what extent it's a `pyo3` issue, since it doesn't happen with "stock" Python.
If I don't force the uv-managed Python interpreter, the build works and the final binary knows where to look for the dylib:

```
otool -L target/debug/deps/setup-a1fe82e135076eae
target/debug/deps/setup-a1fe82e135076eae:
        /opt/homebrew/opt/python@3.13/Frameworks/Python.framework/Versions/3.13/Python (compatibility version 3.13.0, current version 3.13.0)
        /usr/lib/libiconv.2.dylib (compatibility version 7.0.0, current version 7.0.0)
        /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1345.120.2)
```

---

_Comment by @LukeMathWalker on 2025-01-14 15:31_

And I honestly have no idea why it works in that scenario, since the instruction emitted by `pyo3-ffi` looks formally identical:

```
[pyo3-ffi 0.23.4] cargo:rustc-link-search=native=/opt/homebrew/opt/python@3.13/Frameworks/Python.framework/Versions/3.13/lib
```

---

_Comment by @zanieb on 2025-01-14 15:31_

Fair! The crux seems to be that one of our build paths `/install/...` is being picked up from somewhere outside of `sysconfig`. Presumably this link is present in a Python binary or library and we should patch that on install too?

---

_Comment by @LukeMathWalker on 2025-01-14 15:34_

> Fair! The crux seems to be that one of our build paths `/install/...` is being picked up from somewhere outside of `sysconfig`. Presumably this link is present in a Python binary or library and we should patch that on install too?

That's my guess, since `otool` on a `uv`-backed build outputs:

```
otool -L target/debug/deps/setup-a1fe82e135076eae
target/debug/deps/setup-a1fe82e135076eae:
        /install/lib/libpython3.13.dylib (compatibility version 3.13.0, current version 3.13.0)
        /usr/lib/libiconv.2.dylib (compatibility version 7.0.0, current version 7.0.0)
        /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1345.120.2)
```

and that's a mystery to me at the moment.

---

_Comment by @LukeMathWalker on 2025-01-14 15:43_

I found something interesting.
For a stock Python build, `pyo3-ffi` emits:
```
[pyo3 0.23.4] cargo:rustc-link-arg=-Wl,-rpath,/opt/homebrew/opt/python@3.13/Frameworks
```

while for a `uv`-Python build, we get:

```
[pyo3 0.23.4] cargo:rustc-link-arg=-Wl,-rpath,
```

And that stinks. 
Tracking this down into `pyo3-build-config`, it points here:

```rust
#[cfg(feature = "resolve-config")]
fn _add_python_framework_link_args(
    interpreter_config: &InterpreterConfig,
    triple: &Triple,
    link_libpython: bool,
    mut writer: impl std::io::Write,
) {
    if matches!(triple.operating_system, OperatingSystem::Darwin(_)) && link_libpython {
        if let Some(framework_prefix) = interpreter_config.python_framework_prefix.as_ref() {
            writeln!(
                writer,
                "cargo:rustc-link-arg=-Wl,-rpath,{}",
                framework_prefix
            )
            .unwrap();
        }
    }
}
```

and `python_framework_prefix` is documented as "macOS Python3.framework requires special rpath handling". Looking further now 🔎 

---

_Comment by @LukeMathWalker on 2025-01-14 15:46_

OK, I think I might have found it:

```python
from sysconfig import get_config_var

FRAMEWORK_PREFIX = get_config_var("PYTHONFRAMEWORKPREFIX")
```

This outputs an empty string for a `uv` build, while it's set to "/opt/homebrew/opt/python@3.13/Frameworks" for a "stock" build.


---

_Comment by @LukeMathWalker on 2025-01-14 15:48_

This now begs the question: should `uv`-Python set that value? I'm not deep enough into `sysconfig`'s lore to know if the fix should be on the `pyo3` side or on the `uv` side.

---

_Comment by @zanieb on 2025-01-14 16:01_

We're not doing a framework build of Python (it's... different), so I think that should be empty.

---

_Comment by @LukeMathWalker on 2025-01-14 16:09_

Cool, then it's a red herring. Even more so since trying to emit that `"cargo:rustc-link-arg=-Wl,-rpath,{}"` instructions with various parent directories of the `uv` build doesn't seem to help our case.
This brings us back to the root question: where is `/install/lib/libpython3.13.dylib` coming from?

---

_Comment by @zanieb on 2025-01-14 16:13_

```
❯ otool -L  bin/python
bin/python:
	/System/Library/Frameworks/CoreFoundation.framework/Versions/A/CoreFoundation (compatibility version 150.0.0, current version 2202.0.0)
	@executable_path/../lib/libpython3.12.dylib (compatibility version 3.12.0, current version 3.12.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1336.61.1)

❯ otool -L lib/libpython3.12.dylib
lib/libpython3.12.dylib:
	/install/lib/libpython3.12.dylib (compatibility version 3.12.0, current version 3.12.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1336.61.1)
	/usr/lib/libncurses.5.4.dylib (compatibility version 5.4.0, current version 5.4.0)
	/usr/lib/libpanel.5.4.dylib (compatibility version 5.4.0, current version 5.4.0)
	/System/Library/Frameworks/CoreFoundation.framework/Versions/A/CoreFoundation (compatibility version 150.0.0, current version 2202.0.0)
	/usr/lib/libobjc.A.dylib (compatibility version 1.0.0, current version 228.0.0)
	/System/Library/Frameworks/SystemConfiguration.framework/Versions/A/SystemConfiguration (compatibility version 1.0.0, current version 1296.60.3)
	/System/Library/Frameworks/AppKit.framework/Versions/C/AppKit (compatibility version 45.0.0, current version 2487.30.104)
	/System/Library/Frameworks/ApplicationServices.framework/Versions/A/ApplicationServices (compatibility version 1.0.0, current version 64.0.0)
	/System/Library/Frameworks/CoreGraphics.framework/Versions/A/CoreGraphics (compatibility version 64.0.0, current version 1774.2.3)
	/System/Library/Frameworks/CoreServices.framework/Versions/A/CoreServices (compatibility version 1.0.0, current version 1226.0.0)
	/System/Library/Frameworks/CoreText.framework/Versions/A/CoreText (compatibility version 1.0.0, current version 1.0.0)
	/System/Library/Frameworks/Foundation.framework/Versions/C/Foundation (compatibility version 300.0.0, current version 2202.0.0)
	/System/Library/Frameworks/Carbon.framework/Versions/A/Carbon (compatibility version 2.0.0, current version 170.0.0)
	/System/Library/Frameworks/Cocoa.framework/Versions/A/Cocoa (compatibility version 1.0.0, current version 24.0.0)
	/System/Library/Frameworks/IOKit.framework/Versions/A/IOKit (compatibility version 1.0.0, current version 275.0.0)
	/System/Library/Frameworks/QuartzCore.framework/Versions/A/QuartzCore (compatibility version 1.2.0, current version 1.11.0)
	/usr/lib/libedit.3.dylib (compatibility version 2.0.0, current version 3.0.0)
	/usr/lib/libz.1.dylib (compatibility version 1.0.0, current version 1.2.12)
```

Maybe from `lib/libpython3.12.dylib` itself? kind of weird.

---

_Comment by @LukeMathWalker on 2025-01-14 16:32_

That'd be quite weird, but it's interesting.
System Python seems to link to itself there?

```
otool -L /opt/homebrew/opt/python@3.13/Frameworks/Python.framework/Versions/3.13/Python
/opt/homebrew/opt/python@3.13/Frameworks/Python.framework/Versions/3.13/Python:
        /opt/homebrew/opt/python@3.13/Frameworks/Python.framework/Versions/3.13/Python (compatibility version 3.13.0, current version 3.13.0)
        /System/Library/Frameworks/CoreFoundation.framework/Versions/A/CoreFoundation (compatibility version 150.0.0, current version 2503.1.0)
        /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1345.120.2)
```

---

_Comment by @zanieb on 2025-01-14 16:32_

tada

```
❯ install_name_tool -id /Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/lib/libpython3.12.dylib /Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/lib/libpython3.12.dylib

❯ otool -L /Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/lib/libpython3.12.dylib
/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/lib/libpython3.12.dylib:
	/Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/lib/libpython3.12.dylib (compatibility version 3.12.0, current version 3.12.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1336.61.1)
	/usr/lib/libncurses.5.4.dylib (compatibility version 5.4.0, current version 5.4.0)
	/usr/lib/libpanel.5.4.dylib (compatibility version 5.4.0, current version 5.4.0)
	/System/Library/Frameworks/CoreFoundation.framework/Versions/A/CoreFoundation (compatibility version 150.0.0, current version 2202.0.0)
	/usr/lib/libobjc.A.dylib (compatibility version 1.0.0, current version 228.0.0)
	/System/Library/Frameworks/SystemConfiguration.framework/Versions/A/SystemConfiguration (compatibility version 1.0.0, current version 1296.60.3)
	/System/Library/Frameworks/AppKit.framework/Versions/C/AppKit (compatibility version 45.0.0, current version 2487.30.104)
	/System/Library/Frameworks/ApplicationServices.framework/Versions/A/ApplicationServices (compatibility version 1.0.0, current version 64.0.0)
	/System/Library/Frameworks/CoreGraphics.framework/Versions/A/CoreGraphics (compatibility version 64.0.0, current version 1774.2.3)
	/System/Library/Frameworks/CoreServices.framework/Versions/A/CoreServices (compatibility version 1.0.0, current version 1226.0.0)
	/System/Library/Frameworks/CoreText.framework/Versions/A/CoreText (compatibility version 1.0.0, current version 1.0.0)
	/System/Library/Frameworks/Foundation.framework/Versions/C/Foundation (compatibility version 300.0.0, current version 2202.0.0)
	/System/Library/Frameworks/Carbon.framework/Versions/A/Carbon (compatibility version 2.0.0, current version 170.0.0)
	/System/Library/Frameworks/Cocoa.framework/Versions/A/Cocoa (compatibility version 1.0.0, current version 24.0.0)
	/System/Library/Frameworks/IOKit.framework/Versions/A/IOKit (compatibility version 1.0.0, current version 275.0.0)
	/System/Library/Frameworks/QuartzCore.framework/Versions/A/QuartzCore (compatibility version 1.2.0, current version 1.11.0)
	/usr/lib/libedit.3.dylib (compatibility version 2.0.0, current version 3.0.0)
	/usr/lib/libz.1.dylib (compatibility version 1.0.0, current version 1.2.12)

❯ cargo clean
     Removed 313 files, 72.4MiB total
❯ PYO3_PYTHON="$(uv python find 3.12)" cargo t
   Compiling target-lexicon v0.12.16
   Compiling once_cell v1.20.2
   Compiling proc-macro2 v1.0.93
   Compiling unicode-ident v1.0.14
   Compiling autocfg v1.4.0
   Compiling libc v0.2.169
   Compiling heck v0.5.0
   Compiling indoc v2.0.5
   Compiling cfg-if v1.0.0
   Compiling unindent v0.2.3
   Compiling memoffset v0.9.1
   Compiling pyo3-build-config v0.23.4
   Compiling quote v1.0.38
   Compiling syn v2.0.96
   Compiling pyo3-ffi v0.23.4
   Compiling pyo3-macros-backend v0.23.4
   Compiling pyo3 v0.23.4
   Compiling pyo3-macros v0.23.4
   Compiling setup v0.1.0 (/Users/zb/workspace/uv-repro-install-lib)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 3.77s
     Running unittests src/lib.rs (target/debug/deps/setup-29f3c899bfb488ef)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```

I guess we should try to do that at install time.

---

_Comment by @zanieb on 2025-01-14 16:32_

AI tells me

> On macOS, dynamic libraries contain an installation path that tells the dynamic linker where to look for the library at runtime. This path is embedded in the library itself and can be different from its current location on disk. When the library is built, it's given this install path which represents where it expects to be installed in the final system.

---

_Comment by @LukeMathWalker on 2025-01-14 16:33_

That checks out with what we are seeing I'd say.

---

_Comment by @zanieb on 2025-01-14 16:34_

I'd review a PR if you post it; otherwise, I'll get to it when I have a chance.

I think this is a macOS-only issue, but haven't checked.

---

_Label `bug` added by @zanieb on 2025-01-14 16:34_

---

_Label `uv python` added by @zanieb on 2025-01-14 16:34_

---

_Comment by @LukeMathWalker on 2025-01-14 16:42_

Happy to give it a try—any pointer as to where I should be looking at for the installation phase? 

---

_Comment by @zanieb on 2025-01-14 16:44_

It'd be a new one of these https://github.com/astral-sh/uv/blob/d2fb4c585d7882e3323082e0c80132a33a472bbf/crates/uv/src/commands/python/install.rs#L313

https://github.com/astral-sh/uv/blob/bec8468183c7cc1697ad1d34a5eb6087ec5c8a90/crates/uv-python/src/managed.rs#L496-L509

---

_Referenced in [astral-sh/uv#10629](../../astral-sh/uv/pulls/10629.md) on 2025-01-15 11:02_

---

_Comment by @LukeMathWalker on 2025-01-15 11:02_

PR is up: https://github.com/astral-sh/uv/pull/10629

---

_Closed by @zanieb on 2025-01-15 20:11_

---

_Closed by @zanieb on 2025-01-15 20:11_

---

_Referenced in [astral-sh/uv#11234](../../astral-sh/uv/issues/11234.md) on 2025-02-05 06:45_

---

_Referenced in [astral-sh/uv#12343](../../astral-sh/uv/issues/12343.md) on 2025-03-20 16:34_

---

_Referenced in [astral-sh/uv#10766](../../astral-sh/uv/issues/10766.md) on 2025-04-21 20:03_

---
