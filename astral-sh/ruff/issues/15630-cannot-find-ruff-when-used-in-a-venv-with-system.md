---
number: 15630
title: Cannot find ruff when used in a venv with --system-site-packages
type: issue
state: open
author: sillydan1
labels:
  - question
assignees: []
created_at: 2025-01-21T08:57:13Z
updated_at: 2025-12-04T19:36:53Z
url: https://github.com/astral-sh/ruff/issues/15630
synced_at: 2026-01-10T01:22:56Z
---

# Cannot find ruff when used in a venv with --system-site-packages

---

_Issue opened by @sillydan1 on 2025-01-21 08:57_

When running `ruff` as a module in a virtual environment with the `--system-site-packages`, I see a similar error as https://github.com/astral-sh/ruff/issues/2520 

## Reproduction steps:
Given this environment:
```dockerfile
FROM python:3-alpine
RUN python3 -m pip install ruff
```

```sh
# Build and run the container.
docker build -t ruff-test .
docker run --rm -it ruff-test sh

# In the container:
# Create and activate a venv with system-site-packages enabled
python3 -m venv venv --system-site-packages
. venv/bin/activate
```

After activating the venv, running `ruff` as a python module will result in an error:
```sh
(venv) / # python3 -m ruff
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/usr/local/lib/python3.13/site-packages/ruff/__main__.py", line 81, in <module>
    ruff = os.fsdecode(find_ruff_bin())
                       ~~~~~~~~~~~~~^^
  File "/usr/local/lib/python3.13/site-packages/ruff/__main__.py", line 77, in find_ruff_bin
    raise FileNotFoundError(scripts_path)
FileNotFoundError: /venv/bin/ruff
```

Luckilly, running `ruff` directly instead of as a python module seems to sidestep this problem:

```sh
(venv) / # ruff
Ruff: An extremely fast Python linter and code formatter.

Usage: ruff [OPTIONS] <COMMAND>

Commands:
  check    Run Ruff on the given files or directories
  rule     Explain a rule (or all rules)
  config   List or describe the available configuration options
  linter   List all supported upstream linters
  clean    Clear any caches in the current directory and any subdirectories
  format   Run the Ruff formatter on the given files or directories
  server   Run the language server
  analyze  Run analysis over Python source code
  version  Display Ruff's version
  help     Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version

Log levels:
  -v, --verbose  Enable verbose logging
  -q, --quiet    Print diagnostics, but nothing else
  -s, --silent   Disable all logging (but still exit with status code "1" upon detecting diagnostics)

Global options:
      --config <CONFIG_OPTION>  Either a path to a TOML configuration file (`pyproject.toml` or `ruff.toml`), or a TOML `<KEY> = <VALUE>` pair (such as you might find in a
                                `ruff.toml` configuration file) overriding a specific configuration option. Overrides of individual settings using this option always take precedence
                                over all configuration files, including configuration files that were also specified using `--config`
      --isolated                Ignore all configuration files

For help with a specific command, see: `ruff help <command>`.
```

---

_Comment by @dhruvmanila on 2025-01-29 09:11_

Thanks for the report, apologies for the late reply.

I tried to spend a couple of minutes to look at this issue but I'm not sure what could be wrong here. The scripts directory path for the preferred user scheme seems to be returning another path:

```
>>> sysconfig.get_path('scripts', scheme='posix_user')
'/root/.local/bin'
```

which I think is because the `HOME` directory is `/root`.

Related: https://github.com/astral-sh/ruff/issues/13110

---

_Comment by @sillydan1 on 2025-01-29 10:52_

Thanks for your reply.

Just to be clear, in the provided docker environment `ruff` is installed on the _system_ pip, so I would expect [`__main__.py`](https://github.com/astral-sh/ruff/blob/main/python/ruff/__main__.py) to resolve to `/usr/local/bin/ruff` (but only because I initialized the venv using `--system-site-packages`)

I think that it just need a last clause where it searches for `ruff` in the system site packages.

---

_Label `question` added by @dhruvmanila on 2025-03-18 11:27_

---

_Comment by @auxsvr on 2025-03-25 20:35_

This is still an issue and it breaks pylsp_ruff.plugin. Any idea or workaround? In my case, there is no venv, just the system installation.
The cause seems to be that `sysconfig.get_path('scripts') == '/usr/local/bin'`. Would `shutil.which('ruff')` improve the situation?

---

_Comment by @bnavigator on 2025-12-04 19:36_

Still an issue when ruff is installed by a system package manager into `/usr/bin/ruff` but the `__main__.py` looks only into `/usr/local/bin/` (through sysconfig.get_path("scripts"))

---
