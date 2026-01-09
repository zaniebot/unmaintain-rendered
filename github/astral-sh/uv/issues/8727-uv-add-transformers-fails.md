---
number: 8727
title: uv add transformers fails
type: issue
state: closed
author: JohannesMessnerAA
labels: []
assignees: []
created_at: 2024-10-31T15:08:34Z
updated_at: 2024-10-31T15:32:27Z
url: https://github.com/astral-sh/uv/issues/8727
synced_at: 2026-01-07T13:12:18-06:00
---

# uv add transformers fails

---

_Issue opened by @JohannesMessnerAA on 2024-10-31 15:08_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hi all!

Trying uv for the first time and loving it so far, were it not for the following:

```console
johannes_messner@dev-01:~/test-uv-project$ uv add transformers
Using CPython 3.13.0
Creating virtual environment at: .venv
Resolved 19 packages in 26ms
error: Failed to prepare distributions
  Caused by: Failed to download and build `tokenizers==0.20.1`
  Caused by: Build backend failed to build wheel through `build_wheel` (exit status: 1)

[stdout]
Running `maturin pep517 build-wheel -i /home/johannes.messner/.cache/uv/builds-v0/.tmpplqTLO/bin/python --compatibility off`

[stderr]
💥 maturin failed
  Caused by: Cargo metadata failed. Do you have cargo in your PATH?
  Caused by: No such file or directory (os error 2)
Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/home/johannes.messner/.cache/uv/builds-v0/.tmpplqTLO/bin/python', '--compatibility', 'off'] returned non-zero exit status 1
```

My PATH does contain cargo:

```console
johannes_messner@dev-01:~/test-uv-project$ echo $PATH
[...]:/home/johannes.messner/.cargo/bin:[...]
```

`uv pip install` fails in the same way, however `pip install` works without issues:

```console
(transforers-env) johannes_messner@dev-01:~$ pip install transformers
Collecting transformers
  Using cached transformers-4.46.1-py3-none-any.whl.metadata (44 kB)
Requirement already satisfied: filelock in ./transforers-env/lib/python3.12/site-packages (from transformers) (3.16.1)
Requirement already satisfied: huggingface-hub<1.0,>=0.23.2 in ./transforers-env/lib/python3.12/site-packages (from transformers) (0.26.2)
Requirement already satisfied: numpy>=1.17 in ./transforers-env/lib/python3.12/site-packages (from transformers) (2.1.2)
Requirement already satisfied: packaging>=20.0 in ./transforers-env/lib/python3.12/site-packages (from transformers) (24.1)
Requirement already satisfied: pyyaml>=5.1 in ./transforers-env/lib/python3.12/site-packages (from transformers) (6.0.2)
Requirement already satisfied: regex!=2019.12.17 in ./transforers-env/lib/python3.12/site-packages (from transformers) (2024.9.11)
Requirement already satisfied: requests in ./transforers-env/lib/python3.12/site-packages (from transformers) (2.32.3)
Requirement already satisfied: safetensors>=0.4.1 in ./transforers-env/lib/python3.12/site-packages (from transformers) (0.4.5)
Collecting tokenizers<0.21,>=0.20 (from transformers)
  Using cached tokenizers-0.20.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (6.7 kB)
Requirement already satisfied: tqdm>=4.27 in ./transforers-env/lib/python3.12/site-packages (from transformers) (4.66.6)
Requirement already satisfied: fsspec>=2023.5.0 in ./transforers-env/lib/python3.12/site-packages (from huggingface-hub<1.0,>=0.23.2->transformers) (2024.10.0)
Requirement already satisfied: typing-extensions>=3.7.4.3 in ./transforers-env/lib/python3.12/site-packages (from huggingface-hub<1.0,>=0.23.2->transformers) (4.12.2)
Requirement already satisfied: charset-normalizer<4,>=2 in ./transforers-env/lib/python3.12/site-packages (from requests->transformers) (3.4.0)
Requirement already satisfied: idna<4,>=2.5 in ./transforers-env/lib/python3.12/site-packages (from requests->transformers) (3.10)
Requirement already satisfied: urllib3<3,>=1.21.1 in ./transforers-env/lib/python3.12/site-packages (from requests->transformers) (2.2.3)
Requirement already satisfied: certifi>=2017.4.17 in ./transforers-env/lib/python3.12/site-packages (from requests->transformers) (2024.8.30)
Using cached transformers-4.46.1-py3-none-any.whl (10.0 MB)
Using cached tokenizers-0.20.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (3.0 MB)
Installing collected packages: tokenizers, transformers
Successfully installed tokenizers-0.20.1 transformers-4.46.1

[notice] A new release of pip is available: 24.0 -> 24.3.1
[notice] To update, run: pip install --upgrade pip
```

uv version:

```
uv 0.4.28
```

Thanks for your help!


---

_Comment by @konstin on 2024-10-31 15:14_

What operating system and architecture are you on? Can you share the output of `cargo -vV`?

---

_Comment by @JohannesMessnerAA on 2024-10-31 15:32_

Thanks for the quick reply!

Running `cargo -vV`didn't work, which lead me to find the 2-setp solution that solved my problem:
1. Re-install cargo
2. Downgrade to Python 3.12 from 3.13

For the benefit of future users that might stumble over this issue, this is the error message I saw after re-installing cargo, and that made me downgrade the python version:

```console
ohannes_messner@dev-01:~/iterated-learning$ uv add transformers
Resolved 73 packages in 311ms
error: Failed to prepare distributions
  Caused by: Failed to download and build `tokenizers==0.20.1`
  Caused by: Build backend failed to build wheel through `build_wheel` (exit status: 1)

[stdout]
Running `maturin pep517 build-wheel -i /home/johannes.messner/.cache/uv/builds-v0/.tmpky4u9r/bin/python --compatibility off`

[stderr]
    Updating crates.io index
 Downloading crates ...
  Downloaded anstyle-query v1.1.1
  Downloaded bitflags v2.6.0
  Downloaded strsim v0.11.1
  Downloaded rustc-hash v1.1.0
  Downloaded smallvec v1.13.2
  Downloaded serde_derive v1.0.210
  Downloaded windows-targets v0.52.6
  Downloaded regex v1.11.0
  Downloaded unicode-normalization-alignments v0.1.12
  Downloaded unindent v0.2.3
  Downloaded wasi v0.11.0+wasi-snapshot-preview1
  Downloaded tempfile v3.13.0
  Downloaded shlex v1.3.0
  Downloaded redox_syscall v0.5.7
  Downloaded thiserror-impl v1.0.64
  Downloaded serde v1.0.210
  Downloaded utf8parse v0.2.2
  Downloaded rayon-core v1.12.1
  Downloaded unicode-ident v1.0.13
  Downloaded thiserror v1.0.64
  Downloaded portable-atomic v1.9.0
  Downloaded serde_json v1.0.128
  Downloaded unicode_categories v0.1.1
  Downloaded rayon v1.10.0
  Downloaded target-lexicon v0.12.16
  Downloaded unicode-segmentation v1.12.0
  Downloaded zerocopy-derive v0.7.35
  Downloaded zerocopy v0.7.35
  Downloaded nom v7.1.3
  Downloaded pyo3-macros-backend v0.21.2
  Downloaded regex-syntax v0.8.5
  Downloaded rustix v0.38.37
  Downloaded pyo3-ffi v0.21.2
  Downloaded parking_lot v0.12.3
  Downloaded unicode-width v0.1.14
  Downloaded proc-macro2 v1.0.86
  Downloaded num-traits v0.2.19
  Downloaded rand_chacha v0.3.1
  Downloaded quote v1.0.37
  Downloaded pyo3-build-config v0.21.2
  Downloaded pkg-config v0.3.31
  Downloaded spm_precompiled v0.1.4
  Downloaded parking_lot_core v0.9.10
  Downloaded onig v6.4.0
  Downloaded regex-automata v0.4.8
  Downloaded windows_x86_64_gnullvm v0.52.6
  Downloaded windows_aarch64_gnullvm v0.52.6
  Downloaded windows_i686_gnullvm v0.52.6
  Downloaded libc v0.2.159
  Downloaded once_cell v1.20.1
  Downloaded num-complex v0.4.6
  Downloaded scopeguard v1.2.0
  Downloaded esaxx-rs v0.1.10
  Downloaded rawpointer v0.2.1
  Downloaded number_prefix v0.4.0
  Downloaded itertools v0.11.0
  Downloaded monostate v0.1.13
  Downloaded matrixmultiply v0.3.9
  Downloaded macro_rules_attribute-proc_macro v0.2.0
  Downloaded log v0.4.22
  Downloaded itertools v0.12.1
  Downloaded instant v0.1.13
  Downloaded linux-raw-sys v0.4.14
  Downloaded indoc v2.0.5
  Downloaded fastrand v2.1.1
  Downloaded windows_x86_64_gnu v0.52.6
  Downloaded windows_x86_64_msvc v0.52.6
  Downloaded windows_i686_gnu v0.52.6
  Downloaded windows_aarch64_msvc v0.52.6
  Downloaded env_filter v0.1.2
  Downloaded windows_i686_msvc v0.52.6
  Downloaded darling_macro v0.20.10
  Downloaded darling v0.20.10
  Downloaded pyo3 v0.21.2
  Downloaded onig_sys v69.8.1
  Downloaded monostate-impl v0.1.13
  Downloaded memoffset v0.9.1
  Downloaded getrandom v0.2.15
  Downloaded env_logger v0.11.5
  Downloaded console v0.15.8
  Downloaded syn v2.0.79
  Downloaded lock_api v0.4.12
  Downloaded lazy_static v1.5.0
  Downloaded either v1.13.0
  Downloaded crossbeam-utils v0.8.20
  Downloaded crossbeam-deque v0.8.5
  Downloaded ndarray v0.15.6
  Downloaded ident_case v1.0.1
  Downloaded humantime v2.1.0
  Downloaded derive_builder_macro v0.20.1
  Downloaded derive_builder v0.20.1
  Downloaded aho-corasick v1.1.3
  Downloaded crossbeam-epoch v0.9.18
  Downloaded cc v1.1.22
  Downloaded base64 v0.13.1
  Downloaded ryu v1.0.18
  Downloaded rand v0.8.5
  Downloaded numpy v0.21.0
  Downloaded minimal-lexical v0.2.1
  Downloaded memchr v2.7.4
  Downloaded indicatif v0.17.8
  Downloaded encode_unicode v0.3.6
  Downloaded darling_core v0.20.10
  Downloaded byteorder v1.5.0
  Downloaded num-integer v0.1.46
  Downloaded derive_builder_core v0.20.1
  Downloaded bitflags v1.3.2
  Downloaded anstream v0.6.15
  Downloaded rand_core v0.6.4
  Downloaded macro_rules_attribute v0.2.0
  Downloaded colorchoice v1.0.2
  Downloaded cfg-if v1.0.0
  Downloaded autocfg v1.4.0
  Downloaded anstyle-wincon v3.0.4
  Downloaded anstyle-parse v0.2.5
  Downloaded anstyle v1.0.8
  Downloaded rayon-cond v0.3.0
  Downloaded pyo3-macros v0.21.2
  Downloaded ppv-lite86 v0.2.20
  Downloaded paste v1.0.15
  Downloaded itoa v1.0.11
  Downloaded is_terminal_polyfill v1.70.1
  Downloaded heck v0.4.1
  Downloaded fnv v1.0.7
  Downloaded errno v0.3.9
  Downloaded windows-sys v0.59.0
  Downloaded windows-sys v0.52.0
🍹 Building a mixed python/rust project
🔗 Found pyo3 bindings
🐍 Found CPython 3.13 at /home/johannes.messner/.cache/uv/builds-v0/.tmpky4u9r/bin/python
📡 Using build options features, bindings from pyproject.toml
   Compiling proc-macro2 v1.0.86
   Compiling unicode-ident v1.0.13
   Compiling autocfg v1.4.0
   Compiling target-lexicon v0.12.16
   Compiling libc v0.2.159
   Compiling once_cell v1.20.1
   Compiling shlex v1.3.0
   Compiling cfg-if v1.0.0
   Compiling memchr v2.7.4
   Compiling crossbeam-utils v0.8.20
   Compiling fnv v1.0.7
   Compiling ident_case v1.0.1
   Compiling strsim v0.11.1
   Compiling serde v1.0.210
   Compiling portable-atomic v1.9.0
   Compiling smallvec v1.13.2
   Compiling either v1.13.0
   Compiling byteorder v1.5.0
   Compiling pkg-config v0.3.31
   Compiling cc v1.1.22
   Compiling rayon-core v1.12.1
   Compiling regex-syntax v0.8.5
   Compiling parking_lot_core v0.9.10
   Compiling scopeguard v1.2.0
   Compiling heck v0.4.1
   Compiling paste v1.0.15
   Compiling rawpointer v0.2.1
   Compiling thiserror v1.0.64
   Compiling lazy_static v1.5.0
   Compiling num-traits v0.2.19
   Compiling lock_api v0.4.12
   Compiling memoffset v0.9.1
   Compiling matrixmultiply v0.3.9
   Compiling utf8parse v0.2.2
   Compiling aho-corasick v1.1.3
   Compiling minimal-lexical v0.2.1
   Compiling log v0.4.22
   Compiling unicode-width v0.1.14
   Compiling serde_json v1.0.128
   Compiling pyo3-build-config v0.21.2
   Compiling crossbeam-epoch v0.9.18
   Compiling quote v1.0.37
   Compiling nom v7.1.3
   Compiling getrandom v0.2.15
   Compiling syn v2.0.79
   Compiling crossbeam-deque v0.8.5
   Compiling rand_core v0.6.4
   Compiling parking_lot v0.12.3
   Compiling console v0.15.8
   Compiling anstyle-parse v0.2.5
   Compiling itertools v0.11.0
   Compiling itoa v1.0.11
   Compiling bitflags v1.3.2
   Compiling colorchoice v1.0.2
   Compiling onig_sys v69.8.1
   Compiling esaxx-rs v0.1.10
   Compiling num-integer v0.1.46
   Compiling num-complex v0.4.6
   Compiling anstyle v1.0.8
   Compiling rayon v1.10.0
   Compiling anstyle-query v1.1.1
   Compiling macro_rules_attribute-proc_macro v0.2.0
   Compiling regex-automata v0.4.8
   Compiling unicode-segmentation v1.12.0
   Compiling unindent v0.2.3
   Compiling ryu v1.0.18
   Compiling number_prefix v0.4.0
   Compiling pyo3-ffi v0.21.2
   Compiling pyo3 v0.21.2
   Compiling base64 v0.13.1
   Compiling indoc v2.0.5
   Compiling is_terminal_polyfill v1.70.1
   Compiling indicatif v0.17.8
   Compiling anstream v0.6.15
error: failed to run custom build command for `pyo3-ffi v0.21.2`

Caused by:
  process didn't exit successfully: `/home/johannes_messner/.cache/uv/sdists-v5/pypi/tokenizers/0.20.1/x7RjY9ILTNYuIpqRYwJ8I/tokenizers-0.20.1.tar.gz/bindings/python/target/release/build/pyo3-ffi-f520a0951f03389d/build-script-build` (exit status: 1)
  --- stdout
  cargo:rerun-if-env-changed=PYO3_CROSS
  cargo:rerun-if-env-changed=PYO3_CROSS_LIB_DIR
  cargo:rerun-if-env-changed=PYO3_CROSS_PYTHON_VERSION
  cargo:rerun-if-env-changed=PYO3_CROSS_PYTHON_IMPLEMENTATION
  cargo:rerun-if-env-changed=PYO3_PRINT_CONFIG
  cargo:rerun-if-env-changed=PYO3_USE_ABI3_FORWARD_COMPATIBILITY

  --- stderr
  error: the configured Python interpreter version (3.13) is newer than PyO3's maximum supported version (3.12)
  = help: please check if an updated version of PyO3 is available. Current version: 0.21.2
  = help: set PYO3_USE_ABI3_FORWARD_COMPATIBILITY=1 to suppress this check and build anyway using the stable ABI
warning: build failed, waiting for other jobs to finish...
💥 maturin failed
  Caused by: Failed to build a native library through cargo
  Caused by: Cargo build finished with "exit status: 101": `env -u CARGO PYO3_ENVIRONMENT_SIGNATURE="cpython-3.13-64bit" PYO3_PYTHON="/home/johannes.messner/.cache/uv/builds-v0/.tmpky4u9r/bin/python" PYTHON_SYS_EXECUTABLE="/home/johannes.messner/.cache/uv/builds-v0/.tmpky4u9r/bin/python" "cargo" "rustc" "--features" "pyo3/extension-module" "--message-format" "json-render-diagnostics" "--manifest-path" "/home/johannes_messner/.cache/uv/sdists-v5/pypi/tokenizers/0.20.1/x7RjY9ILTNYuIpqRYwJ8I/tokenizers-0.20.1.tar.gz/bindings/python/Cargo.toml" "--release" "--lib"`
Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/home/johannes.messner/.cache/uv/builds-v0/.tmpky4u9r/bin/python', '--compatibility', 'off'] returned non-zero exit status 1
```

To downgrade the python version i had to change the `requires-python = ">=3.13"` line in `pyproject.toml`, as well as the `.python-version` file.

Thanks, problem solved!

---

_Closed by @JohannesMessnerAA on 2024-10-31 15:32_

---
