```yaml
number: 4906
title: "Implement `uv help` manually instead of using Clap default"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/cli-help-cmd
created_at: 2024-07-08T19:46:36Z
updated_at: 2024-07-09T17:43:15Z
url: https://github.com/astral-sh/uv/pull/4906
synced_at: 2026-01-12T16:06:31Z
```

# Implement `uv help` manually instead of using Clap default

---

_@zanieb_

Extends #4772 

Implements `uv help` ourselves so we can do things like #4909 
Adds hints to use `uv help` for more details during short help display.

---

_Label `cli` added by @zanieb on 2024-07-08 19:46_

---

_@zanieb reviewed on 2024-07-08 19:47_

---

_Review comment by @zanieb on `crates/uv/tests/help.rs`:25 on 2024-07-08 19:47_

Should move this to the bottom again

---

_Review requested from @charliermarsh by @zanieb on 2024-07-09 00:28_

---

_Review requested from @konstin by @zanieb on 2024-07-09 00:28_

---

_Marked ready for review by @zanieb on 2024-07-09 00:28_

---

_@konstin reviewed on 2024-07-09 08:56_

---

_Review comment by @konstin on `crates/uv/tests/help.rs`:237 on 2024-07-09 08:56_

This should keep the uv prefix

---

_@konstin approved on 2024-07-09 08:58_

Found a way to make it panic, but i do like the more concise help a lot.


```
$ cargo run -q -- help pip install
thread 'main' panicked at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.8/src/builder/debug_asserts.rs:214:13:
Command install: Argument or group 'offline' specified in 'conflicts_with*' for 'refresh' does not exist
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_@zanieb reviewed on 2024-07-09 14:35_

---

_Review comment by @zanieb on `crates/uv/tests/help.rs`:237 on 2024-07-09 14:35_

Will investigate. I don't know how Clap renders these things in a parent-aware style.

---

_Review comment by @zanieb on `crates/uv/tests/help.rs`:237 on 2024-07-09 17:14_

Fixed.

---

_@zanieb reviewed on 2024-07-09 17:14_

---

_Comment by @zanieb on 2024-07-09 17:14_

Fixed the panic :)

---

_Comment by @charliermarsh on 2024-07-09 17:28_

Should these be different?

```
puffin on  zb/cli-help-cmd [$] is 📦 v0.2.23 via 🐍 v3.12.3 via 🦀 v1.79.0
❯ cargo run -- python --help
   Compiling uv-cli v0.0.1 (/Users/crmarsh/workspace/puffin/crates/uv-cli)
   Compiling uv v0.2.23 (/Users/crmarsh/workspace/puffin/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 2.81s
     Running `target/debug/uv python --help`
Manage Python installations

Usage: uv python [OPTIONS] <COMMAND>

Commands:
  list       List the available Python installations
  install    Download and install Python versions
  find       Search for a Python installation
  dir        Show the uv Python installation directory
  uninstall  Uninstall Python versions

Options:
  -q, --quiet                                  Do not print any output
  -v, --verbose...                             Use verbose output
      --color <COLOR_CHOICE>                   Control colors in output [default: auto] [possible values: auto, always, never]
      --native-tls                             Whether to load TLS certificates from the platform's native certificate store [env: UV_NATIVE_TLS=]
      --offline                                Disable network access, relying only on locally cached data and locally available files
      --python-preference <PYTHON_PREFERENCE>  Whether to prefer using Python from uv or on the system [possible values: only-managed, installed, managed, system, only-system]
      --python-fetch <PYTHON_FETCH>            Whether to automatically download Python when required [possible values: automatic, manual]
  -n, --no-cache                               Avoid reading from or writing to the cache [env: UV_NO_CACHE=]
      --cache-dir <CACHE_DIR>                  Path to the cache directory [env: UV_CACHE_DIR=]
      --config-file <CONFIG_FILE>              The path to a `uv.toml` file to use for configuration [env: UV_CONFIG_FILE=]
  -h, --help                                   Print help
  -V, --version                                Print version

Use `uv help python` for more details.
puffin on  zb/cli-help-cmd [$] is 📦 v0.2.23 via 🐍 v3.12.3 via 🦀 v1.79.0 took 3s
❯ cargo run -- help python
   Compiling uv-cli v0.0.1 (/Users/crmarsh/workspace/puffin/crates/uv-cli)
   Compiling uv v0.2.23 (/Users/crmarsh/workspace/puffin/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 2.83s
     Running `target/debug/uv help python`
Manage Python installations

Usage: python <COMMAND>

Commands:
  list       List the available Python installations
  install    Download and install Python versions
  find       Search for a Python installation
  dir        Show the uv Python installation directory
  uninstall  Uninstall Python versions
  help       Print this message or the help of the given subcommand(s)

Options:
  -h, --help
          Print help (see a summary with '-h')
```

---

_Comment by @zanieb on 2024-07-09 17:32_

Yes they should be different but we should be displaying the long-form documentation for all those options in the second invocation. Will investigate that...

---

_Comment by @zanieb on 2024-07-09 17:33_

Ah I pushed a fix to #4909 but not here by accident.

---

_@charliermarsh approved on 2024-07-09 17:37_

---

_Merged by @zanieb on 2024-07-09 17:43_

---

_Closed by @zanieb on 2024-07-09 17:43_

---

_Branch deleted on 2024-07-09 17:43_

---
