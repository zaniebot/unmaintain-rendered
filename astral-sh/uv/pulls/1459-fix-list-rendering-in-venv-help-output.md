```yaml
number: 1459
title: "Fix list rendering in `venv --help` output"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
assignees: []
merged: true
base: main
head: python-version-help-text
created_at: 2024-02-16T09:22:15Z
updated_at: 2024-02-16T14:36:38Z
url: https://github.com/astral-sh/uv/pull/1459
synced_at: 2026-01-12T16:04:37Z
```

# Fix list rendering in `venv --help` output

---

_@MichaReiser_

## Summary 

Use [`verbatim_doc_comment`](https://docs.rs/clap/latest/clap/_derive/index.html#arg-attributes) to preserve the formatting of the venv `python` command line option to avoid that the list formatting gets mangled. 

I changed the lists to use `-` which is what clap uses to render possible options (see color) and removed the `-p` in front of each format (again, to match claps possible options rendering)

Fixes https://github.com/astral-sh/uv/issues/1364

## Test Plan

```
Create a virtual environment

Usage: uv venv [OPTIONS] [NAME]

Arguments:
  [NAME]
          The path to the virtual environment to create
          
          [default: .venv]

Options:
  -p, --python <PYTHON>
          The Python interpreter to use for the virtual environment.
          
          Supported formats:
          - `3.10` searches for an installed Python 3.10 (`py --list-paths` on Windows, `python3.10` on Linux/Mac).
            Specifying a patch version is not supported.
          - `python3.10` or `python.exe` looks for a binary in `PATH`.
          - `/home/ferris/.local/bin/python3.10` uses this exact Python.
          
          Note that this is different from `--python-version` in `pip compile`, which takes `3.10` or `3.10.13` and
          doesn't look for a Python interpreter on disk.

      --seed
          Install seed packages (`pip`, `setuptools`, and `wheel`) into the virtual environment

  -i, --index-url <INDEX_URL>
          The URL of the Python Package Index
          
          [env: UV_INDEX_URL=]
          [default: https://pypi.org/simple]

      --extra-index-url <EXTRA_INDEX_URL>
          Extra URLs of package indexes to use, in addition to `--index-url`

  -q, --quiet
          Do not print any output

      --no-index
          Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those discovered via `--find-links`

  -v, --verbose
          Use verbose output

      --offline
          Run offline, i.e., without accessing the network

      --color <COLOR>
          Control colors in output
          
          [default: auto]

          Possible values:
          - auto:   Enables colored output only when the output is going to a terminal or TTY with support
          - always: Enables colored output regardless of the detected environment
          - never:  Disables colored output

  -n, --no-cache
          Avoid reading from or writing to the cache
          
          [env: UV_NO_CACHE=]

      --cache-dir <CACHE_DIR>
          Path to the cache directory
          
          [env: UV_CACHE_DIR=]

  -h, --help
          Print help (see a summary with '-h')

  -V, --version
          Print version

```

---

_Renamed from "Fix venv help text for python version option" to "Fix list rendering in `venv --help` output" by @MichaReiser on 2024-02-16 09:25_

---

_Label `bug` added by @MichaReiser on 2024-02-16 09:26_

---

_@dhruvmanila approved on 2024-02-16 09:30_

---

_@zanieb reviewed on 2024-02-16 14:26_

---

_Review comment by @zanieb on `crates/uv/src/main.rs`:638 on 2024-02-16 14:26_

Can we toggle the comment with cfg to have it be platform relevant? I think that'd make it a lot shorter and it wouldn't need to be listed like this?

---

_@zanieb reviewed on 2024-02-16 14:28_

---

_Review comment by @zanieb on `crates/uv/src/main.rs`:638 on 2024-02-16 14:28_

(Happy to track this in a follow-up too)

---

_@MichaReiser reviewed on 2024-02-16 14:36_

---

_Review comment by @MichaReiser on `crates/uv/src/main.rs`:638 on 2024-02-16 14:36_

Hmm, no if `cfg` applies to comments. We could define a custom struct for the `Path` and have two different definitions for each platform but that's a bit more involved (and requires copying the configuration)

---

_Merged by @MichaReiser on 2024-02-16 14:36_

---

_Closed by @MichaReiser on 2024-02-16 14:36_

---

_Branch deleted on 2024-02-16 14:36_

---
