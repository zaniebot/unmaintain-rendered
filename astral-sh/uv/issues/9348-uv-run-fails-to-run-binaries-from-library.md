```yaml
number: 9348
title: "`uv run` fails to run binaries from `~/Library/Application Support` on macOS"
type: issue
state: closed
author: gusutabopb
labels:
  - bug
  - external
assignees: []
created_at: 2024-11-22T05:51:48Z
updated_at: 2024-12-03T21:11:23Z
url: https://github.com/astral-sh/uv/issues/9348
synced_at: 2026-01-12T15:59:48Z
```

# `uv run` fails to run binaries from `~/Library/Application Support` on macOS

---

_@gusutabopb_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When executing `uv run`, a list of available commands is shown. `python` and `python3` work fine, but other commands list do not. I don't think `uv` should be shipping with `pip`, etc so the provided command list seems wrong.

I tried using different Python version (e.g. `-p 3.12`) but the results were the same.

### Listing commands

```fish
⋊> ~> uv -v run
DEBUG uv 0.5.4 (c62c83c37 2024-11-20)
DEBUG No project found; searching for Python interpreter
DEBUG Searching for default Python interpreter in virtual environments or managed installations
DEBUG Searching for managed installations at `Library/Application Support/uv/python`
DEBUG Found managed installation `cpython-3.13.0-macos-aarch64-none`
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/Users/gustavo/Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13` (managed installations)
DEBUG Using Python 3.13.0 interpreter at: /Users/gustavo/Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
Provide a command or script to invoke with `uv run <command>` or `uv run <script>.py`.

The following commands are available in the environment:

- idle3
- idle3.13
- pip
- pip3
- pip3.13
- pydoc3
- pydoc3.13
- python
- python3
- python3-config
- python3.13
- python3.13-config

See `uv run --help` for more information.
```

###  Running `python` - OK
`python3` works as well.

```
⋊> ~> uv -v run python
DEBUG uv 0.5.4 (c62c83c37 2024-11-20)
DEBUG No project found; searching for Python interpreter
DEBUG Searching for default Python interpreter in virtual environments or managed installations
DEBUG Searching for managed installations at `Library/Application Support/uv/python`
DEBUG Found managed installation `cpython-3.13.0-macos-aarch64-none`
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/Users/gustavo/Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13` (managed installations)
DEBUG Using Python 3.13.0 interpreter at: /Users/gustavo/Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
DEBUG Running `python`
Python 3.13.0 (main, Oct  7 2024, 23:47:22) [Clang 18.1.8 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```


###  Running `pydoc3` - FAIL
`pip`, `pip3`, `idle3`, etc fail as well.

```
⋊> ~> uv -v run pydoc3
DEBUG uv 0.5.4 (c62c83c37 2024-11-20)
DEBUG No project found; searching for Python interpreter
DEBUG Searching for default Python interpreter in virtual environments or managed installations
DEBUG Searching for managed installations at `Library/Application Support/uv/python`
DEBUG Found managed installation `cpython-3.13.0-macos-aarch64-none`
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/Users/gustavo/Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13` (managed installations)
DEBUG Using Python 3.13.0 interpreter at: /Users/gustavo/Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
DEBUG Running `pydoc3`
/Users/gustavo/Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/pydoc3: line 2: /Users/gustavo/Library
Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13: No such file or directory
/Users/gustavo/Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/pydoc3: line 2: exec: /Users/gustavo/Library
Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13: cannot execute: No such file or directory
DEBUG Command exited with code: 126
```


---

_Renamed from "Wrong list of commands when running `uv run python`" to "`uv run` displays an incorrect list of commands" by @gusutabopb on 2024-11-22 05:52_

---

_Label `bug` added by @charliermarsh on 2024-11-22 13:58_

---

_Comment by @zanieb on 2024-11-22 15:20_

Hm I can't reproduce this

```
❯ uv run -v
DEBUG uv 0.5.3 (56d362208 2024-11-19)
DEBUG No project found; searching for Python interpreter
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `/Users/zb/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.0-macos-aarch64-none`
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13` (managed installations)
DEBUG Using Python 3.13.0 interpreter at: /Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
Provide a command or script to invoke with `uv run <command>` or `uv run <script>.py`.

The following commands are available in the environment:

- idle3
- idle3.13
- pip
- pip3
- pip3.13
- pydoc3
- pydoc3.13
- python
- python3
- python3-config
- python3.13
- python3.13-config

See `uv run --help` for more information.
❯ uv run python3.13
Python 3.13.0 (main, Oct 16 2024, 08:05:40) [Clang 18.1.8 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 
KeyboardInterrupt
>>> 
❯ uv run pydoc3
pydoc - the Python documentation tool

pydoc3 <name> ...
    Show text documentation on something.  <name> may be the name of a
    Python keyword, topic, function, module, or package, or a dotted
    reference to a class or function within a module or module in a
    package.  If <name> contains a '/', it is used as the path to a
    Python source file to document. If name is 'keywords', 'topics',
    or 'modules', a listing of these things is displayed.

pydoc3 -k <keyword>
    Search for a keyword in the synopsis lines of all available modules.

pydoc3 -n <hostname>
    Start an HTTP server with the given hostname (default: localhost).

pydoc3 -p <port>
    Start an HTTP server on the given port on the local machine.  Port
    number 0 can be used to get an arbitrary unused port.

pydoc3 -b
    Start an HTTP server on an arbitrary unused port and open a web browser
    to interactively browse documentation.  This option can be used in
    combination with -n and/or -p.

pydoc3 -w <name> ...
    Write out the HTML documentation for a module to a file in the current
    directory.  If <name> contains a '/', it is treated as a filename; if
    it names a directory, documentation is written for all the contents.


 /                                                                                                                     
❯ uv run pip3 --version
pip 24.1.2 from /Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/lib/python3.13/site-packages/pip (python 3.13)
```

---

_Label `needs-mre` added by @zanieb on 2024-11-22 20:11_

---

_Renamed from "`uv run` displays an incorrect list of commands" to "`uv run` fails to run binaries from `~/Library/Application Support` on macOS" by @gusutabopb on 2024-11-23 12:21_

---

_Comment by @gusutabopb on 2024-11-23 12:35_

The binaries (`pydoc3`, etc) were present in the installation directory (`~/Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/`) but uv failed to find them. I updated the issue description to clarify that.

I believe this is somewhat related to #5806 and perhaps with the whitespace in `Application Support` not being properly escaped somehow.

I can reproduce this consistently on macOS using this:

```
> rm -rf ~/.local/share/uv
> rm -rf ~/Library/Application\ Support/uv/
> mkdir -p ~/Library/Application\ Support/uv
> uv -v --python-preference only-managed run --python 3.13.0 pydoc3
```

Output:
```
DEBUG uv 0.5.4 (c62c83c37 2024-11-20)
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python 3.13.0 in virtual environments or managed installations
DEBUG Searching for managed installations at `Library/Application Support/uv/python`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `Library/Application Support/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.13.0%2B20241016-aarch64-apple-darwin-install_only_stripped.tar.gz to temporary location: /Users/gustavo/Library/Application Support/uv/python/.cache/.tmptaZksd
DEBUG Extracting cpython-3.13.0%2B20241016-aarch64-apple-darwin-install_only_stripped.tar.gz
DEBUG Moving /Users/gustavo/Library/Application Support/uv/python/.cache/.tmptaZksd/python to Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none
DEBUG Released lock at `/Users/gustavo/Library/Application Support/uv/python/.lock`
DEBUG Using Python 3.13.0 interpreter at: /Users/gustavo/Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
DEBUG Running `pydoc3`
/Users/gustavo/Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/pydoc3: line 2: /Users/gustavo/Library
Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13: No such file or directory
/Users/gustavo/Library/Application Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/pydoc3: line 2: exec: /Users/gustavo/Library
Support/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13: cannot execute: No such file or directory
DEBUG Command exited with code: 126
```

Note that if `~/Library/Application\ Support/uv/` doesn't exist, the issue goes away:

```
> rm -rf ~/Library/Application\ Support/uv/
> uv -v --python-preference only-managed run --python 3.13.0 pydoc3
```

```
DEBUG uv 0.5.4 (c62c83c37 2024-11-20)
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python 3.13.0 in virtual environments or managed installations
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.13.0%2B20241016-aarch64-apple-darwin-install_only_stripped.tar.gz to temporary location: /Users/gustavo/.local/share/uv/python/.cache/.tmp2Z3nJ4
DEBUG Extracting cpython-3.13.0%2B20241016-aarch64-apple-darwin-install_only_stripped.tar.gz
DEBUG Moving /Users/gustavo/.local/share/uv/python/.cache/.tmp2Z3nJ4/python to .local/share/uv/python/cpython-3.13.0-macos-aarch64-none
DEBUG Released lock at `/Users/gustavo/.local/share/uv/python/.lock`
DEBUG Using Python 3.13.0 interpreter at: /Users/gustavo/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
DEBUG Running `pydoc3`
pydoc - the Python documentation tool

pydoc3 <name> ...
    Show text documentation on something.  <name> may be the name of a
    Python keyword, topic, function, module, or package, or a dotted
    reference to a class or function within a module or module in a
    package.  If <name> contains a '/', it is used as the path to a
    Python source file to document. If name is 'keywords', 'topics',
    or 'modules', a listing of these things is displayed.

pydoc3 -k <keyword>
    Search for a keyword in the synopsis lines of all available modules.

pydoc3 -n <hostname>
    Start an HTTP server with the given hostname (default: localhost).

pydoc3 -p <port>
    Start an HTTP server on the given port on the local machine.  Port
    number 0 can be used to get an arbitrary unused port.

pydoc3 -b
    Start an HTTP server on an arbitrary unused port and open a web browser
    to interactively browse documentation.  This option can be used in
    combination with -n and/or -p.

pydoc3 -w <name> ...
    Write out the HTML documentation for a module to a file in the current
    directory.  If <name> contains a '/', it is treated as a filename; if
    it names a directory, documentation is written for all the contents.

DEBUG Command exited with code: 0
```

---

_Comment by @charliermarsh on 2024-11-23 13:01_

Interesting, thanks. (We no longer write there by default IIRC but it's preserved for backwards compatibility.)

---

_Comment by @charliermarsh on 2024-11-24 03:22_

I think it must be an issue with the shebang that's being used in python-build-standalone (upstream)?

---

_Comment by @charliermarsh on 2024-11-24 03:25_

Filed: https://github.com/indygreg/python-build-standalone/issues/394. Should be an easy fix.

---

_Label `needs-mre` removed by @charliermarsh on 2024-11-24 03:27_

---

_Label `upstream` added by @charliermarsh on 2024-11-24 03:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-24 03:30_

---

_Comment by @charliermarsh on 2024-11-24 03:30_

And now a PR here: https://github.com/indygreg/python-build-standalone/pull/395. I think that's right?

---

_Comment by @gusutabopb on 2024-11-24 08:19_

Thanks!

If anyone else hits this until it's fixed upstream, the quick fix is to simply delete your `~/Library/Application Support` and reinstall any necessary Python versions in the new default location (`~/.local/share/uv`).

---

_Closed by @charliermarsh on 2024-12-03 20:53_

---

_Closed by @charliermarsh on 2024-12-03 20:53_

---

_Comment by @zanieb on 2024-12-03 21:11_

This is fixed upstream, but we need to release it there and here still.

---
