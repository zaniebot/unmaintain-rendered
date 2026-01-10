---
number: 11451
title: Ruff fails to build from source on Windows with Rust and Visual Studio installed
type: issue
state: closed
author: jaraco
labels:
  - windows
assignees: []
created_at: 2024-05-16T15:59:16Z
updated_at: 2024-05-30T17:19:02Z
url: https://github.com/astral-sh/ruff/issues/11451
synced_at: 2026-01-10T01:22:51Z
---

# Ruff fails to build from source on Windows with Rust and Visual Studio installed

---

_Issue opened by @jaraco on 2024-05-16 15:59_

In https://github.com/astral-sh/ruff/pull/11450#issue-2300728386, I'm attempting to build ruff from source on Windows (to validate the behavior).

When I do, it fails with the following output:

<details>

```
 ~ # pip install --no-deps --no-binary ruff ruff
Collecting ruff
  Using cached ruff-0.4.4.tar.gz (2.5 MB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: ruff
  Building wheel for ruff (pyproject.toml) ... error
  error: subprocess-exited-with-error

  × Building wheel for ruff (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> [250 lines of output]
      Running `maturin pep517 build-wheel -i C:\Python311\python.exe --compatibility off`
      ðŸ\x8d¹ Building a mixed python/rust project
      ðŸ”— Found bin bindings
      ðŸ“¡ Using build options bindings from pyproject.toml
         Compiling unicode-ident v1.0.12
         Compiling proc-macro2 v1.0.81
         Compiling serde v1.0.200
         Compiling memchr v2.7.2
         Compiling windows_x86_64_gnu v0.52.5
         Compiling once_cell v1.19.0
         Compiling windows_x86_64_gnu v0.48.5
         Compiling cfg-if v1.0.0
         Compiling windows-targets v0.52.5
         Compiling aho-corasick v1.1.3
         Compiling windows-sys v0.52.0
         Compiling getrandom v0.2.14
         Compiling regex-syntax v0.8.3
         Compiling rand_core v0.6.4
         Compiling quote v1.0.36
         Compiling syn v2.0.60
         Compiling windows-targets v0.48.5
         Compiling log v0.4.21
         Compiling ppv-lite86 v0.2.17
         Compiling windows-sys v0.48.0
         Compiling rand_chacha v0.3.1
         Compiling either v1.11.0
         Compiling regex-automata v0.4.6
         Compiling siphasher v0.3.11
         Compiling phf_shared v0.11.2
         Compiling rand v0.8.5
         Compiling phf_generator v0.11.2
         Compiling phf_codegen v0.11.2
         Compiling winapi-x86_64-pc-windows-gnu v0.4.0
         Compiling utf8parse v0.2.1
         Compiling itertools v0.12.1
         Compiling tinyvec_macros v0.1.1
         Compiling winapi v0.3.9
         Compiling crossbeam-utils v0.8.19
         Compiling tinyvec v1.6.0
         Compiling serde_derive v1.0.200
         Compiling Inflector v0.11.4
         Compiling unicode-normalization v0.1.23
         Compiling is-macro v0.3.5
         Compiling regex v1.10.4
         Compiling unicode-width v0.1.11
         Compiling thiserror v1.0.59
         Compiling anstyle v1.0.6
         Compiling static_assertions v1.1.0
         Compiling autocfg v1.2.0
         Compiling anstyle-wincon v3.0.2
         Compiling getopts v0.2.21
         Compiling phf v0.11.2
         Compiling thiserror-impl v1.0.59
         Compiling bstr v1.9.1
         Compiling anstyle-parse v0.2.3
         Compiling anstyle-query v1.0.2
         Compiling tracing-core v0.1.32
         Compiling rustc-hash v1.1.0
         Compiling colorchoice v1.0.0
         Compiling anyhow v1.0.82
         Compiling anstream v0.6.13
         Compiling unicode_names2_generator v1.2.2
         Compiling tracing-attributes v0.1.27
         Compiling terminal_size v0.3.0
         Compiling heck v0.5.0
         Compiling ruff_text_size v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_text_size)
         Compiling pin-project-lite v0.2.14
         Compiling clap_lex v0.7.0
         Compiling bitflags v2.5.0
         Compiling strsim v0.11.1
         Compiling ruff_source_file v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_source_file)
         Compiling tracing v0.1.40
         Compiling clap_builder v4.5.2
         Compiling ruff_python_trivia v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_python_trivia)
         Compiling unicode_names2 v1.2.2
         Compiling ruff_python_ast v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_python_ast)
         Compiling clap_derive v4.5.4
         Compiling fnv v1.0.7
         Compiling strsim v0.10.0
         Compiling ident_case v1.0.1
         Compiling darling_core v0.20.8
         Compiling clap v4.5.4
         Compiling equivalent v1.0.1
         Compiling percent-encoding v2.3.1
         Compiling hashbrown v0.14.5
         Compiling serde_json v1.0.116
         Compiling unicode-bidi v0.3.15
         Compiling idna v0.5.0
         Compiling indexmap v2.2.6
         Compiling darling_macro v0.20.8
         Compiling form_urlencoded v1.2.1
         Compiling ruff_macros v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_macros)
         Compiling globset v0.4.14
         Compiling num-traits v0.2.18
         Compiling lexical-util v0.8.5
         Compiling crossbeam-epoch v0.9.18
         Compiling uuid-macro-internal v1.8.0
         Compiling filetime v0.2.23
         Compiling rustversion v1.0.15
         Compiling glob v0.3.1
         Compiling lazy_static v1.4.0
         Compiling itoa v1.0.11
         Compiling ryu v1.0.17
         Compiling unic-char-range v0.9.0
         Compiling unic-common v0.9.0
         Compiling unic-char-property v0.9.0
         Compiling unic-ucd-version v0.9.0
         Compiling lexical-parse-integer v0.8.6
         Compiling uuid v1.8.0
         Compiling crossbeam-deque v0.8.5
         Compiling ruff_python_parser v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_python_parser)
         Compiling url v2.5.0
         Compiling darling v0.20.8
         Compiling toml_datetime v0.6.5
         Compiling serde_spanned v0.6.5
         Compiling winapi-util v0.1.8
         Compiling vte_generate_state_changes v0.1.1
         Compiling winnow v0.6.6
         Compiling matches v0.1.10
         Compiling peg-runtime v0.8.2
         Compiling smallvec v1.13.2
         Compiling paste v1.0.14
         Compiling seahash v4.1.0
         Compiling peg-macros v0.8.2
         Compiling ruff_cache v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_cache)
         Compiling unic-ucd-category v0.9.0
         Compiling vte v0.11.1
         Compiling toml_edit v0.22.12
         Compiling same-file v1.0.6
         Compiling serde_with_macros v3.8.1
         Compiling lexical-parse-float v0.8.5
         Compiling pep440_rs v0.4.0
         Compiling pmutil v0.6.1
         Compiling libc v0.2.154
         Compiling hexf-parse v0.2.1
         Compiling heck v0.4.1
         Compiling annotate-snippets v0.6.1
         Compiling option-ext v0.2.0
         Compiling strum_macros v0.26.2
         Compiling peg v0.8.2
         Compiling chic v1.2.2
         Compiling dirs-sys v0.4.1
         Compiling toml v0.8.12
         Compiling serde_with v3.8.1
         Compiling pep508_rs v0.3.0
         Compiling ruff_python_literal v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_python_literal)
         Compiling result-like-derive v0.5.0
         Compiling walkdir v2.5.0
         Compiling strip-ansi-escapes v0.2.0
         Compiling chrono v0.4.38
         Compiling ruff_python_index v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_python_index)
         Compiling newtype-uuid v1.1.0
         Compiling ruff_index v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_index)
         Compiling ruff_diagnostics v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_diagnostics)
         Compiling rust-stemmers v1.2.0
         Compiling yansi-term v0.1.2
         Compiling ruff_python_stdlib v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_python_stdlib)
         Compiling crossbeam-channel v0.5.12
         Compiling libcst_derive v1.3.1
         Compiling quick-xml v0.31.0
         Compiling path-dedot v3.1.1
         Compiling is-docker v0.2.0
         Compiling regex-syntax v0.6.29
         Compiling drop_bomb v0.1.5
         Compiling cc v1.0.95
         Compiling unscanny v0.1.0
         Compiling ruff_formatter v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_formatter)
         Compiling pep440_rs v0.6.0
         Compiling libcst v1.3.1
         Compiling is-wsl v0.4.0
         Compiling path-absolutize v3.1.1
         Compiling imperative v1.0.5
         Compiling regex-automata v0.1.10
         Compiling libmimalloc-sys v0.1.37
         Compiling quick-junit v0.4.0
         Compiling ruff_notebook v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_notebook)
         Compiling ruff_python_semantic v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_python_semantic)
         Compiling annotate-snippets v0.9.2
         Compiling pyproject-toml v0.9.0
         Compiling result-like v0.5.0
         Compiling strum v0.26.2
         Compiling ruff_python_codegen v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_python_codegen)
         Compiling dirs v5.0.1
         Compiling colored v2.1.0
         Compiling clap_complete v4.5.2
         Compiling dirs-sys v0.3.7
         Compiling fs-err v2.11.0
         Compiling terminfo v0.8.0
         Compiling fern v0.6.2
         Compiling pathdiff v0.2.1
         Compiling rayon-core v1.12.1
         Compiling minimal-lexical v0.2.1
         Compiling overload v0.1.1
         Compiling typed-arena v2.0.2
         Compiling countme v3.0.1
         Compiling natord v1.0.9
         Compiling similar v2.5.0
         Compiling ruff_python_formatter v0.0.0 (C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\crates\ruff_python_formatter)
         Compiling nom v7.1.3
         Compiling nu-ansi-term v0.46.0
         Compiling dirs v4.0.0
         Compiling shellexpand v3.1.0
      The following warnings were emitted during compilation:

      warning: libmimalloc-sys@0.1.37: Compiler family detection failed due to error: ToolNotFound: Failed to find tool. Is `gcc.exe` installed? (see https://github.com/rust-lang/cc-rs#compile-time-requirements for help)
      warning: libmimalloc-sys@0.1.37: Compiler family detection failed due to error: ToolNotFound: Failed to find tool. Is `gcc.exe` installed? (see https://github.com/rust-lang/cc-rs#compile-time-requirements for help)

      error: failed to run custom build command for `libmimalloc-sys v0.1.37`

      Caused by:
        process didn't exit successfully: `C:\Users\jaraco\AppData\Local\Temp\pip-install-n29gkh77\ruff_6132d3aaffa145deaf230e6b7c831794\target\release\build\libmimalloc-sys-655cb3ab60955642\build-script-build` (exit code: 1)
        --- stdout
        OPT_LEVEL = Some("3")
        TARGET = Some("x86_64-pc-windows-gnu")
        HOST = Some("x86_64-pc-windows-gnu")
        cargo:rerun-if-env-changed=CC_x86_64-pc-windows-gnu
        CC_x86_64-pc-windows-gnu = None
        cargo:rerun-if-env-changed=CC_x86_64_pc_windows_gnu
        CC_x86_64_pc_windows_gnu = None
        cargo:rerun-if-env-changed=HOST_CC
        HOST_CC = None
        cargo:rerun-if-env-changed=CC
        CC = None
        cargo:rerun-if-env-changed=CC_ENABLE_DEBUG_OUTPUT
        cargo:warning=Compiler family detection failed due to error: ToolNotFound: Failed to find tool. Is `gcc.exe` installed? (see https://github.com/rust-lang/cc-rs#compile-time-requirements for help)
        cargo:rerun-if-env-changed=CRATE_CC_NO_DEFAULTS
        CRATE_CC_NO_DEFAULTS = None
        DEBUG = Some("false")
        CARGO_CFG_TARGET_FEATURE = Some("cmpxchg16b,fxsr,sse,sse2,sse3")
        cargo:rerun-if-env-changed=CFLAGS_x86_64-pc-windows-gnu
        CFLAGS_x86_64-pc-windows-gnu = None
        cargo:rerun-if-env-changed=CFLAGS_x86_64_pc_windows_gnu
        CFLAGS_x86_64_pc_windows_gnu = None
        cargo:rerun-if-env-changed=HOST_CFLAGS
        HOST_CFLAGS = None
        cargo:rerun-if-env-changed=CFLAGS
        CFLAGS = None
        cargo:warning=Compiler family detection failed due to error: ToolNotFound: Failed to find tool. Is `gcc.exe` installed? (see https://github.com/rust-lang/cc-rs#compile-time-requirements for help)

        --- stderr


        error occurred: Failed to find tool. Is `gcc.exe` installed? (see https://github.com/rust-lang/cc-rs#compile-time-requirements for help)


      warning: build failed, waiting for other jobs to finish...
      ðŸ’¥ maturin failed
        Caused by: Failed to build a native library through cargo
        Caused by: Cargo build finished with "exit code: 101": `"cargo" "rustc" "--message-format" "json-render-diagnostics" "--manifest-path" "C:\\Users\\jaraco\\AppData\\Local\\Temp\\pip-install-n29gkh77\\ruff_6132d3aaffa145deaf230e6b7c831794\\crates\\ruff\\Cargo.toml" "--release" "--bin" "ruff" "--" "-C" "link-arg=-s"`
      Error: command ['maturin', 'pep517', 'build-wheel', '-i', 'C:\\Python311\\python.exe', '--compatibility', 'off'] returned non-zero exit status 1
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for ruff
Failed to build ruff
ERROR: Could not build wheels for ruff, which is required to install pyproject.toml-based projects
```
</details>


I've used Chocolatey to install rust (`choco install rust`) and installed Visual Studio and proven that other C extension modules will build.

I believe the primary error message is:

> cargo:warning=Compiler family detection failed due to error: ToolNotFound: Failed to find tool. Is `gcc.exe` installed? (see https://github.com/rust-lang/cc-rs#compile-time-requirements for help)

Following that link fails to resolve the anchor, but following that readme into the documentation does provide [some guidance with that header](https://docs.rs/cc/latest/cc/#compile-time-requirements).

It seems that it's not `gcc` that's needed but MSVC, which is in fact installed. Apparently, `cc-rs` attempts to locate it, but doesn't do a very good job, because it failed to locate it when it's installed.

I suspect the issue here is with the rust ecosystem, probably cc-rs.

Since I'm not plugged into the Rust ecosystem, would someone be willing to solve the root issue on behalf of ruff users?

Do we know if the default installation of ruff or the chocolatey installation of rust is targeting MSVC or MinGW?

---

_Comment by @charliermarsh on 2024-05-17 14:05_

Unfortunately I'm not sure and don't have my Windows machine with me right now.

---

_Comment by @jborbely on 2024-05-23 20:17_

> Do we know if the default installation of ruff or the chocolatey installation of rust is targeting MSVC or MinGW?

From https://github.com/astral-sh/ruff/issues/11503#issuecomment-2126642384

> Currently Windows builds uses `msvc` runtime.

@jaraco hopefully that helps to clarify the current build runtime for `ruff`, but the same comment in #11503 also suggests that a move to target MinGW could be one way to remove the external VCRUNTIME dependency issue.

---

_Label `windows` added by @MichaReiser on 2024-05-27 15:58_

---

_Comment by @MichaReiser on 2024-05-27 16:06_

@jaraco any chance you're building inside of WSL? I'm asking because I'm surprised that it tries to build `x86_64-pc-windows-gnu` instead of the msvc target. 

---

_Comment by @jaraco on 2024-05-27 17:45_

> @jaraco any chance you're building inside of WSL? I'm asking because I'm surprised that it tries to build `x86_64-pc-windows-gnu` instead of the msvc target.

Definitely not. I'm using a mostly vanilla Windows 11 install (using UTM on macOS).

I am running (and building on) Windows for ARM, so that could very well be a factor. I didn't think to mention that before.

I also was using a non-standard terminal (Hyper.js) and shell (xonsh), so that also could be a factor. I've since re-run the command using Windows Terminal and Powershell, but the build fails with the same error, so those factors are probably not a relevant concern.

---

_Comment by @T-256 on 2024-05-28 01:11_

@jaraco Visual Studio uses msvcrt, then you setup rust with `windows-gnu` (which mainly should be used with mingw).

Use proper rustup toolchain to be compatible to Visual Studio:
```shell
> rustup toolchain install stable-x86_64-pc-windows-msvc
> rustup default stable-x86_64-pc-windows-msvc
```

---

_Comment by @T-256 on 2024-05-28 01:16_

> Do we know if the default installation of ruff or the chocolatey installation of rust is targeting MSVC or MinGW?

Current binaries in Github releases are MSVC, but I verify that I can compile ruff from source via MinGW (rustup's `stable-x86_64-pc-windows-gnu` target).

---

_Comment by @jaraco on 2024-05-30 17:03_

> then you setup rust with `windows-gnu`

It sounds like maybe Chocolatey's default install of rust configures for `windows-gnu`. I'm slightly surprised that choice is made; I've always considered MinGW to be a fallback and for MSVC to be preferred, at least in the Python ecosystem.

> Use proper rustup toolchain to be compatible to Visual Studio:

Interestingly, rustup isn't included with chocolatey's install of rust, so I can't even query the toolchains. It sounds like maybe I should abondon chocolatey and follow the install instructions on rust-lang.org.

After a boondoggle and stumbling on two bugs in httpie (https://github.com/httpie/cli/issues/1580, https://github.com/httpie/cli/issues/1581), I've managed to download rust-init.exe and uninstalled rust using chocolatey, but it unfortunately refuses to run:

```
 ~ # https static.rust-lang.org/rustup/dist/i686-pc-windows-gnu/rustup-init.exe --download -q
 ~ # ./rustup-init
error: unknown proxy name: 'RUSTUP-INIT'; valid proxy names are 'rustc', 'rustdoc', 'cargo', 'rust-lldb', 'rust-gdb', 'rust-gdbgui', 'rls', 'cargo-clippy', 'clippy-driver', 'cargo-miri', 'rust-analyzer', 'rustfmt', 'cargo-fmt'
 ~ # https static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe --download -q
 ~ # ./rustup-init
error: unknown proxy name: 'RUSTUP-INIT'; valid proxy names are 'rustc', 'rustdoc', 'cargo', 'rust-lldb', 'rust-gdb', 'rust-gdbgui', 'rls', 'cargo-clippy', 'clippy-driver', 'cargo-miri', 'rust-analyzer', 'rustfmt', 'cargo-fmt'
```

It seems there's a bug in the installer that it can't run from my shell. When I run it from Powershell, it seems to run, so that's weird, but okay.

When I use the rustup-init installer, it does default to the default host triple of `aarch64-pc-windows-msvc`, so it does seem that it's using msvc by default. I'm unsure why the chocolatey installer did something different (or what it did do).

And with that change, ruff now installs from source in my environment:

```
 ~ # pip-run git+https://github.com/jaraco/ruff@feature/honor-install-target-bin -- -m ruff --version
ruff 0.4.4
```

\o/

Oh, interesting, chocolatey has [two different packages for rust](https://community.chocolatey.org/packages?q=rust), one with the GNU ABI and one with the MSVC ABI:

![image](https://github.com/astral-sh/ruff/assets/308610/7221ba24-a797-4e58-a026-d9cb17a6a41b)

I suspect that means I could have done `chocolatey install rust-ms` and had a better experience, but only because I had Visual Studio installed. No idea what would have happened if I'd not had MSVC installed.

It's kind of a shame that the GNU ABI is default, alas, and that it doesn't provide compiler support. I'll file an [issue with that project](https://gitlab.com/notriddle/notriddle-chocolatey-packages/-/issues) [notriddle/notriddle-chocolatey-packages#3](https://gitlab.com/notriddle/notriddle-chocolatey-packages/-/issues/3).

Thanks for the help and the tips and getting me to the root of the issue.

---

_Closed by @jaraco on 2024-05-30 17:03_

---
