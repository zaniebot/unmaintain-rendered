---
number: 9138
title: Failing to compile on Windows ARM64
type: issue
state: closed
author: dlech
labels:
  - windows
assignees: []
created_at: 2024-11-15T02:12:32Z
updated_at: 2024-11-18T01:47:26Z
url: https://github.com/astral-sh/uv/issues/9138
synced_at: 2026-01-10T01:24:36Z
---

# Failing to compile on Windows ARM64

---

_Issue opened by @dlech on 2024-11-15 02:12_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I'm trying to `pipx install cibuildwheel[uv]` on a Windows ARM64 machine. There are no binary wheels for `uv` on this platform, so it tries to compile the package from source (`uv-0.5.2.tar.gz`). However, it is failing with the following error.

```console
         Compiling uv-trampoline-builder v0.0.1 (C:\Users\extra\AppData\Local\Temp\pip-install-hv48jtb7\uv_8840a2c89e304464bb97267224937b3d\crates\uv-trampoline-builder)
      error: couldn't read `crates\uv-trampoline-builder\src\../../uv-trampoline/trampolines/uv-trampoline-aarch64-gui.exe`: The system cannot find the path specified. (os error 3)
        --> crates\uv-trampoline-builder\src\lib.rs:29:5
         |
      29 |     include_bytes!("../../uv-trampoline/trampolines/uv-trampoline-aarch64-gui.exe");
         |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
         |
         = note: this error originates in the macro `include_bytes` (in Nightly builds, run with -Z macro-backtrace for more info)
      
      error: couldn't read `crates\uv-trampoline-builder\src\../../uv-trampoline/trampolines/uv-trampoline-aarch64-console.exe`: The system cannot find the path specified. (os error 3)
        --> crates\uv-trampoline-builder\src\lib.rs:33:5
         |
      33 |     include_bytes!("../../uv-trampoline/trampolines/uv-trampoline-aarch64-console.exe");
         |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
         |
         = note: this error originates in the macro `include_bytes` (in Nightly builds, run with -Z macro-backtrace for more info)
      
      error: could not compile `uv-trampoline-builder` (lib) due to 2 previous errors
      warning: build failed, waiting for other jobs to finish...
      ðŸ’¥ maturin failed
        Caused by: Failed to build a native library through cargo
        Caused by: Cargo build finished with "exit code: 101": `"cargo" "rustc" "--message-format" "json-render-diagnostics" "--manifest-path" "C:\\Users\\extra\\AppData\\Local\\Temp\\pip-install-hv48jtb7\\uv_8840a2c89e304464bb97267224937b3d\\crates\\uv\\Cargo.toml" "--release" "--bin" "uv" "--" "-C" "strip=symbols"`
      Error: command ['maturin', 'pep517', 'build-wheel', '-i', 'C:\\Users\\extra\\pipx\\venvs\\cibuildwheel\\Scripts\\python.exe', '--compatibility', 'off'] returned non-zero exit status 1
      [end of output]
```

I don't really know anything about how crates/rust work so I'm not sure how to troubleshoot it. For example, is it sensitive about the unix path seperators (`/`) in the path? Is it a circular dependency issue where the `uv-trampoline` crate depends on `uv-trampoline-builder` but `uv-trampoline-builder` is using files from the `uv-trampoline` crate? Is it a version issue where it is depending on a version of the `uv-trampoline` from before the aarch64 binaries were added? Those are my best guesses.


---

_Comment by @charliermarsh on 2024-11-15 02:21_

I think this might be best discussed in https://github.com/astral-sh/uv/issues/1141.

---

_Comment by @dlech on 2024-11-15 02:40_

Sure. I wasn't entirely sure if it was actually related to Windows ARM64 specifically or if this was a more general "the sdist package is broken" kind of issue.

---

_Label `windows` added by @charliermarsh on 2024-11-17 16:39_

---

_Comment by @charliermarsh on 2024-11-17 16:39_

Looked into it and I think you're right that there's a real, separate problem with the sdist. Thanks.

---

_Referenced in [astral-sh/uv#9172](../../astral-sh/uv/pulls/9172.md) on 2024-11-17 16:39_

---

_Comment by @zanieb on 2024-11-17 16:50_

If someone could test the "fixed" sdist at https://github.com/astral-sh/uv/actions/runs/11880251223/artifacts/2198442674 that'd be great

---

_Closed by @charliermarsh on 2024-11-18 01:47_

---

_Closed by @charliermarsh on 2024-11-18 01:47_

---
