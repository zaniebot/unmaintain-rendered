```yaml
number: 5665
title: Add help sections for global options
type: pull_request
state: merged
author: eth3lbert
labels:
  - cli
assignees: []
merged: true
base: main
head: global-help
created_at: 2024-07-31T17:49:09Z
updated_at: 2024-08-01T16:21:39Z
url: https://github.com/astral-sh/uv/pull/5665
synced_at: 2026-01-10T13:37:23Z
```

# Add help sections for global options

---

_Pull request opened by @eth3lbert on 2024-07-31 17:49_

## Summary

Part of #4454 .

## Test Plan

test cases included.


---

_Comment by @eth3lbert on 2024-07-31 17:53_

Currently with proposed:

```shell-session
(devshell) :)  uv help
An extremely fast Python package manager.

Usage: uv [OPTIONS] <COMMAND>

Commands:
  pip      Resolve and install Python packages
  tool     Run and manage executable Python packages
  python   Manage Python installations
  venv     Create a virtual environment
  cache    Manage the cache
  version  Display uv's version
  help     Display documentation for a command

Options:
  -n, --no-cache                   Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation [env: UV_NO_CACHE=]
      --cache-dir <CACHE_DIR>      Path to the cache directory [env: UV_CACHE_DIR=]
      --config-file <CONFIG_FILE>  The path to a `uv.toml` file to use for configuration [env: UV_CONFIG_FILE=]
      --no-config                  Avoid discovering configuration files (`pyproject.toml`, `uv.toml`) in the current directory, parent directories, or user configuration directories [env:
                                   UV_NO_CONFIG=]
  -h, --help                       Print help
  -V, --version                    Print version

Global Options:
  -q, --quiet                                  Do not print any output
  -v, --verbose...                             Use verbose output
      --color <COLOR_CHOICE>                   Control colors in output [default: auto] [possible values: auto, always, never]
      --native-tls                             Whether to load TLS certificates from the platform's native certificate store [env: UV_NATIVE_TLS=]
      --offline                                Disable network access, relying only on locally cached data and locally available files
      --python-preference <PYTHON_PREFERENCE>  Whether to prefer using Python installations that are already present on the system, or those that are downloaded and installed by uv [possible values:
                                               only-managed, managed, system, only-system]
      --python-fetch <PYTHON_FETCH>            Whether to automatically download Python when required [possible values: automatic, manual]
      --no-progress                            Hides all progress outputs when set

Use `uv help <command>` for more information on a specific command.
```

@zanieb Do we have any expected output or a specific order?

---

_@zanieb reviewed on 2024-07-31 18:02_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:90 on 2024-07-31 18:02_

Nit: I don't think we use title case

```suggestion
#[command(mut_args(|arg| arg.help_heading("Global options")))]
```

---

_Comment by @zanieb on 2024-07-31 18:04_

Hm are those other ones not global options too?

Ordering within the group seems okay to leave as-is for now. I think I want to group everything e.g. a "Resolver options" section then deal with within-group ordering afterwards _if needed_.

---

_Comment by @eth3lbert on 2024-07-31 18:10_

> Hm are those other ones not global options too?

They are, but I'm not sure if we want e.g. `CacheArgs` to be placed under a dedicated heading or shared with the `Global options` heading.


> Ordering within the group seems okay to leave as-is for now. I think I want to group everything e.g. a "Resolver options" section then deal with within-group ordering afterwards _if needed_.

So, do you want a dedicated heading for each separate flatten command?


---

_Comment by @zanieb on 2024-07-31 18:13_

>They are, but I'm not sure if we want e.g. CacheArgs to be placed under a dedicated heading or shared with the Global options heading.

Fair, but `--help` and `--version` probably should be?

> So, do you want a dedicated heading for each separate flatten command?

Not quite. Like... we don't want a "Resolver installer options" group or a "Refresh options" group, instead we want "Resolver options", "Installer options", and "Cache options" groups :) I was planning on looking at _every_ option and figuring out categories. If you're interested in it, you're definitely welcome to and I can help.


---

_Comment by @eth3lbert on 2024-07-31 18:19_

> Fair, but `--help` and `--version` probably should be?

`mut_args` does not affect the built-in `--help` or `--version` arguments.
Ref: https://docs.rs/clap/4.5.11/clap/struct.Command.html#method.mut_args

I'll how to make this work later.

Another question, If we use `Cache options` will it cause ambiguity when running `uv help cache`?

---

_Comment by @eth3lbert on 2024-07-31 18:22_

> > So, do you want a dedicated heading for each separate flatten command?
> 
> Not quite. Like... we don't want a "Resolver installer options" group or a "Refresh options" group, instead we want "Resolver options", "Installer options", and "Cache options" groups :) I was planning on looking at _every_ option and figuring out categories. If you're interested in it, you're definitely welcome to and I can help.

Okay, then I suppose we could separate this part into follow-up PRs and focus this PR solely on the global options.

---

_Comment by @zanieb on 2024-07-31 18:27_

> Another question, If we use Cache options will it cause ambiguity when running uv help cache?

I don't think so (without trying it)

---

_Comment by @eth3lbert on 2024-07-31 19:55_

Back :)

Are these what you expected?

``` shell-session
(devshell) :)  uv -h
An extremely fast Python package manager.

Usage: uv [OPTIONS] <COMMAND>

Commands:
  pip      Resolve and install Python packages
  tool     Run and manage executable Python packages
  python   Manage Python installations
  venv     Create a virtual environment
  cache    Manage the cache
  version  Display uv's version
  help     Display documentation for a command

Global options:
  -q, --quiet                                  Do not print any output
  -v, --verbose...                             Use verbose output
      --color <COLOR_CHOICE>                   Control colors in output [default: auto] [possible values: auto, always, never]
      --native-tls                             Whether to load TLS certificates from the platform's native certificate store [env: UV_NATIVE_TLS=]
      --offline                                Disable network access, relying only on locally cached data and locally available files
      --python-preference <PYTHON_PREFERENCE>  Whether to prefer using Python installations that are already present on the system, or those that are downloaded and installed by uv [possible values:
                                               only-managed, managed, system, only-system]
      --python-fetch <PYTHON_FETCH>            Whether to automatically download Python when required [possible values: automatic, manual]
      --no-progress                            Hides all progress outputs when set
      --config-file <CONFIG_FILE>              The path to a `uv.toml` file to use for configuration [env: UV_CONFIG_FILE=]
      --no-config                              Avoid discovering configuration files (`pyproject.toml`, `uv.toml`) in the current directory, parent directories, or user configuration directories [env:
                                               UV_NO_CONFIG=]
  -h, --help                                   Print help
  -V, --version                                Print version

Cache options:
  -n, --no-cache               Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation [env: UV_NO_CACHE=]
      --cache-dir <CACHE_DIR>  Path to the cache directory [env: UV_CACHE_DIR=]

Use `uv help` for more details.
```

``` shell-session
(devshell) :)  uv cache prune -h
Prune all unreachable objects from the cache

Usage: uv cache prune [OPTIONS]

Options:
      --ci  Optimize the cache for persistence in a continuous integration environment, like GitHub Actions

Global options:
  -q, --quiet                                  Do not print any output
  -v, --verbose...                             Use verbose output
      --color <COLOR_CHOICE>                   Control colors in output [default: auto] [possible values: auto, always, never]
      --native-tls                             Whether to load TLS certificates from the platform's native certificate store [env: UV_NATIVE_TLS=]
      --offline                                Disable network access, relying only on locally cached data and locally available files
      --python-preference <PYTHON_PREFERENCE>  Whether to prefer using Python installations that are already present on the system, or those that are downloaded and installed by uv [possible values:
                                               only-managed, managed, system, only-system]
      --python-fetch <PYTHON_FETCH>            Whether to automatically download Python when required [possible values: automatic, manual]
      --no-progress                            Hides all progress outputs when set
      --config-file <CONFIG_FILE>              The path to a `uv.toml` file to use for configuration [env: UV_CONFIG_FILE=]
      --no-config                              Avoid discovering configuration files (`pyproject.toml`, `uv.toml`) in the current directory, parent directories, or user configuration directories [env:
                                               UV_NO_CONFIG=]
  -h, --help                                   Print help
  -V, --version                                Print version

Cache options:
  -n, --no-cache               Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation [env: UV_NO_CACHE=]
      --cache-dir <CACHE_DIR>  Path to the cache directory [env: UV_CACHE_DIR=]
```

(Maybe placing `Global options` last?)

---

_Comment by @zanieb on 2024-07-31 20:12_

Yeah that's roughly what I'd expect. Annoying the indentation isn't the same, I wonder if that'll be a problem.

---

_Comment by @zanieb on 2024-07-31 20:15_

We probably want to have a `Python options` for `--python`, `--python-fetch`, etc. too

---

_Comment by @eth3lbert on 2024-07-31 20:19_

> Yeah that's roughly what I'd expect. Annoying the indentation isn't the same, I wonder if that'll be a problem.

Could you show me about this part you are refering to?

---

_Comment by @zanieb on 2024-07-31 20:30_

e.g. "Avoid reading from or writing to the cache" is not at the same level as "Print version"

---

_Comment by @eth3lbert on 2024-07-31 20:39_

> e.g. "Avoid reading from or writing to the cache" is not at the same level as "Print version"

Got it, but I currently don't know how to handle it.

---

_Comment by @zanieb on 2024-07-31 20:49_

Yeah let's deal with that separately haha seems hard

---

_Comment by @eth3lbert on 2024-08-01 03:36_

Rebased and updated the snapshots. You should be able to see the changes in the snapshots now.

---

_Marked ready for review by @eth3lbert on 2024-08-01 03:37_

---

_@zanieb approved on 2024-08-01 15:01_

Thanks!

---

_Label `cli` added by @zanieb on 2024-08-01 15:01_

---

_Merged by @zanieb on 2024-08-01 15:01_

---

_Closed by @zanieb on 2024-08-01 15:01_

---

_Branch deleted on 2024-08-01 15:03_

---

_Renamed from "Grouping `GlobalArgs` to dedicated heading" to "Add help sections for global options" by @zanieb on 2024-08-01 16:21_

---
