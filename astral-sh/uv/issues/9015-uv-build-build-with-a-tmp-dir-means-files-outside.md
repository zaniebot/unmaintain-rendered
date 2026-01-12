```yaml
number: 9015
title: "uv build: build with a tmp dir means files outside python project can't be used for build script"
type: issue
state: closed
author: BaxHugh
labels:
  - question
assignees: []
created_at: 2024-11-11T13:08:38Z
updated_at: 2024-12-26T16:58:22Z
url: https://github.com/astral-sh/uv/issues/9015
synced_at: 2026-01-12T15:59:40Z
```

# uv build: build with a tmp dir means files outside python project can't be used for build script

---

_@BaxHugh_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

My specific problem is that Maturin projects in a uv workspace where `build.rs` requires other files outside of the python subproject (but inside the uv workspace / monorepo) (i.e. protobuf files / .cargo/config.toml) cannot be built properly with `uv build`.

Here is a minimal example, described here, and attached as a zip file.
TLDR: this project contains a protos directory in the monorepo, that a package in the mono repo uses in the build.rs script. a .cargo/config.toml is also required. The problem, is that these files don't get copied to the uv cache temp directory which uv build creates and uses to compile the wheel (I assume because they are outside the directory containing the package's pyproject.toml).

### Zip of the example
[uv-build-debugging.zip](https://github.com/user-attachments/files/17718488/uv-build-debugging.zip)

## Monorepo project structure
Note: Files needed to build the my_pyo3_project package directory:
- The `proto` directory at the monorepo root is needed to build the package.
- `.cargo/config.toml` contains env vars to tell the `build.rs` script where to find the proto files.

### File tree and explanations
```sh
├── Cargo.toml              # Cargo workspace defined
├── .cargo/config.toml      # Env vars required for the build.rs script to find the proto files
├── packages
│   └── my_pyo3_project     # The directory containing the mixed Rust/Python project using PyO3 & maturin
│       ├── build.rs        # Build script which generates rust and native python from proto files
│       ├── Cargo.toml
│       ├── my_pyo3_project
│       │   ├── __init__.py
│       │   ├── my_pyo3_project.pyi   # Stubs for the pyo3 bindings
│       │   └── [api_pb2.py]          # Generated python class from the proto file
│       ├── pyproject.toml            # Defined for building the python package (Maturin & UV)
│       ├── src
│       │   └── lib.rs                # Python bindings for the Rust code
│       └── tests
│           └── test_sum_as_string.py # Example usage of the simple function, using the proto generated python class + pyo3 binding
├── proto                             # Proto files defining shared apis which the build.rs script uses to python & rust code
│   └── mypackageapis
│       └── api.proto
├── pyproject.toml                    # UV workspace defined
└── README.md
```

**Content of `.cargo/config.toml`**
Note: the path is relative to the directory
```toml
[env]
MYPACKAGEAPIS = { value = "proto/mypackageapis", relative = true }
```

## Building the project's package:
**Prerequisites**
Need to have protoc installed (can't be installed with pip install sadly)

**Commands expected to work:**
```sh
uv build --package my_pyo3_project
```
```sh
uv build --all-packages
```
### The issue causing the build to fail
uv build, copies the package's directory to a tmp cache directory, but it doesn't copy the `.cargo/config.toml` or the `proto` directory to the cache directory. This causes the build.rs script to fail when it tries to find the proto files (`error: environment variable 'MYPACKAGEAPIS' not defined at compile time`).
i.e. running the following command while the above build command is running shows:
```sh
ls -A ~/.cache/uv/sdists-v6/.tmp*/my_pyo3_project-0.1.0:
```
`build.rs  Cargo.lock  Cargo.toml  my_pyo3_project  PKG-INFO  pyproject.toml  src  target  tests`
i.e. this doesn't contain `.cargo` or `proto` directories.

Note: if the `.cargo` was copied, then the build.rs script would still fail, since the path to the proto files is relative to the monorepo root,
and within the cache build directory, those files don't exist.
If .cargo/config.toml was evaluated at it's original location, then the absolute path of the proto path env var would point to the developer's source directory, the build would pass, but then the build wouldn't be isolated from the source directory.

## Current workaround
The workaround is to `export MYPACKAGEAPIS=$PWD/proto/mypackageapis` before running the uv build command, so that the build.rs script uses the dev source.
Or have this in a .env file and source that before calling `uv build`.

## Other ideal workarounds
Run uv build in the dev directory, i.e., not in a cache temp, or temp directory, such that all the files are available and the file environment is the same as with cargo build.
**I have not worked out if this is supported**

## Possible solutions
In the uv config, there could be additional fields to specify files that need to be copied to the tmp directory on build. i.e.
```toml
[tool.uv]
build-include = [".cargo/config.toml", "proto"].
```

---

_Comment by @BaxHugh on 2024-11-12 11:12_

UPDATE:
I'm using this command (run from the monorepo root)
```
uv run --env-file .env.uv.build uv build --all-packages
```
with .env.uv.build defining:
```
MYPACKAGEAPIS=${PWD}/proto/mypackageapis
```

So the only con here really is the preamble before uv build

---

_Comment by @BaxHugh on 2024-11-12 11:32_

Relates to #1384 for the env file workaround

---

_Comment by @konstin on 2024-11-12 11:42_

I'll give this a more detailed read later; what i can say already: it's maturin's reponsibility which files get included (https://www.maturin.rs/config). You can also try `python -m build` to see if this is something specific to uv or a general build thing.

In Python, there is a split build setup: You start with a source tree (everything next to pyproject.toml on disk) which gets turned into a source distribution (`.tar.gz`). This source distribution needs to contain all files to build, because you need to be able to unpack that archive on a different machine, and build a wheel (`.whl`) from it.

---

_Comment by @BaxHugh on 2024-11-12 15:12_

Note: in part of my original comment, I got confused about build-requires vs development dependencies. I've removed that part of the comment and updated the zip file to have the correct build-requires

---

_Comment by @BaxHugh on 2024-11-12 15:14_

Thanks @konstin. That could be a lead. So ultimately, it looks like my problem is that the build depends on files above and outside the pyproject.toml's directory, i.e. `<MEMBER_PACKAGE_ROOT>/../../proto` so that doesn't make it into the sdist?
But when compiling with cargo, the paths are relative to the original cargo files.

---

_Comment by @BaxHugh on 2024-11-12 15:30_

Thanks again for your help @konstin. I tried updating the include tool.maturin.include to include the proto dir.

Initially: include ../../proto, however Maturin doesn't like that:
`Caused by: paths in archives must not have `..` when setting path for ...`

So I've made symbolic links in the member package directory  `(proto -> ../../proto)` and `(.cargo -> ../../.cargo)`  and this works now that maturin includes them in the sdist

---

_Comment by @BaxHugh on 2024-11-12 15:33_

So I think the issue itself is closed, however would it be worth considering the ability to build in place as an option?
i.e., if making symbolic links to shared source directories outside of the python project's directory is not an option, then being able to build in place could work? 
I don't know enough really to suggest yes or no, my experience of building in place, is just running `maturin build` in the directory and things work.

---

_Comment by @konstin on 2024-11-13 12:32_

Rust is bit of a special case: Cargo has its own assumptions about workspace structure and packaging that don't mix well with python's assumptions, e.g., maturin often needs to package multiple rust crates into a single python source distribution since they are part of a workspace and python packages are built in isolation. This unfortunately can lead to confusing errors like the one you had.

In python, we need to support source distributions, which are by design isolated, so building in place would mean breaking source distributions. If it was possible, i'd rather make `maturin build` behave more isolated and more like source distributions.

---

_Label `question` added by @konstin on 2024-11-13 12:32_

---

_Comment by @BaxHugh on 2024-11-13 12:37_

Thanks @konstin. Yes, that makes sense. Like you say, since it's maturin bringing cargo / rust to a python domain, and this is inherent to python building, it makes sense for this to be solved within maturin.

---

_Closed by @charliermarsh on 2024-12-26 16:58_

---
