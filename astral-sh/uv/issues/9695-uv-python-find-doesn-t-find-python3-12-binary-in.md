---
number: 9695
title: "`uv python find` doesn't find `python3.12` binary in $PATH when looking for `==3.12.8`"
type: issue
state: open
author: aDotInTheVoid
labels:
  - bug
  - uv python
assignees: []
created_at: 2024-12-06T22:28:00Z
updated_at: 2025-11-05T14:54:58Z
url: https://github.com/astral-sh/uv/issues/9695
synced_at: 2026-01-10T01:24:44Z
---

# `uv python find` doesn't find `python3.12` binary in $PATH when looking for `==3.12.8`

---

_Issue opened by @aDotInTheVoid on 2024-12-06 22:28_

On my system I have both python3.13.0 and python3.12.8 installed through homebrew:

```
% which python3
/opt/homebrew/bin/python3
% which python3.12
/opt/homebrew/bin/python3.12
% python3 --version
Python 3.13.0
% python3.12 --version
Python 3.12.8
```

`uv python list` is aware of both of them:

```

% cat ~/.config/uv/uv.toml
python-preference = "only-system"

% uv python list
cpython-3.13.0-macos-aarch64-none    /opt/homebrew/opt/python@3.13/bin/python3.13 -> ../Frameworks/Python.framework/Versions/3.13/bin/python3.13
cpython-3.12.8-macos-aarch64-none    /opt/homebrew/opt/python@3.12/bin/python3.12 -> ../Frameworks/Python.framework/Versions/3.12/bin/python3.12
cpython-3.9.6-macos-aarch64-none     /Applications/Xcode.app/Contents/Developer/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
```

But can only find one of them:

```
% uv python find '==3.13.0'
/opt/homebrew/opt/python@3.13/bin/python3.13
% uv python find '==3.12.8'
error: No interpreter found for Python ==3.12.8 in virtual environments or search path
```

Or with more logging:

```
% RUST_LOG=uv=trace uv python list
DEBUG uv 0.5.6 (Homebrew 2024-12-03)
DEBUG Searching for any Python interpreter in search path
TRACE Searching PATH for executables: python, python3, cpython, cpython3, pypy, pypy3, graalpy, graalpy3
TRACE Checking `PATH` directory for interpreters: /opt/homebrew/bin
TRACE Found possible Python executable: /opt/homebrew/bin/python3
TRACE Cached interpreter info for Python 3.13.0, skipping probing: /opt/homebrew/bin/python3
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/opt/homebrew/bin/python3` (search path)
TRACE Found possible Python executable: /opt/homebrew/bin/python3.12
TRACE Cached interpreter info for Python 3.12.8, skipping probing: /opt/homebrew/bin/python3.12
DEBUG Found `cpython-3.12.8-macos-aarch64-none` at `/opt/homebrew/bin/python3.12` (search path)
TRACE Found possible Python executable: /opt/homebrew/bin/python3.13
TRACE Cached interpreter info for Python 3.13.0, skipping probing: /opt/homebrew/bin/python3.13
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/opt/homebrew/bin/python3.13` (search path)
TRACE Checking `PATH` directory for interpreters: /opt/homebrew/sbin
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Checking `PATH` directory for interpreters: /System/Cryptexes/App/usr/bin
TRACE Checking `PATH` directory for interpreters: /usr/bin
TRACE Found possible Python executable: /usr/bin/python3
TRACE Querying interpreter executable at /usr/bin/python3
DEBUG Found `cpython-3.9.6-macos-aarch64-none` at `/usr/bin/python3` (search path)
TRACE Checking `PATH` directory for interpreters: /bin
TRACE Checking `PATH` directory for interpreters: /usr/sbin
TRACE Checking `PATH` directory for interpreters: /sbin
TRACE Checking `PATH` directory for interpreters: /Library/Apple/usr/bin
TRACE Checking `PATH` directory for interpreters: /Users/aloenr01/.cargo/bin
TRACE Checking `PATH` directory for interpreters: /Applications/iTerm.app/Contents/Resources/utilities
cpython-3.13.0-macos-aarch64-none    /opt/homebrew/opt/python@3.13/bin/python3.13 -> ../Frameworks/Python.framework/Versions/3.13/bin/python3.13
cpython-3.12.8-macos-aarch64-none    /opt/homebrew/opt/python@3.12/bin/python3.12 -> ../Frameworks/Python.framework/Versions/3.12/bin/python3.12
cpython-3.9.6-macos-aarch64-none     /Applications/Xcode.app/Contents/Developer/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3

% RUST_LOG=uv=trace uv python find '==3.13.0'
DEBUG uv 0.5.6 (Homebrew 2024-12-03)
DEBUG Using Python request `==3.13.0` from explicit request
DEBUG Searching for Python ==3.13.0 in virtual environments or search path
TRACE Searching PATH for executables: python3, python
TRACE Checking `PATH` directory for interpreters: /opt/homebrew/bin
TRACE Found possible Python executable: /opt/homebrew/bin/python3
TRACE Cached interpreter info for Python 3.13.0, skipping probing: /opt/homebrew/bin/python3
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/opt/homebrew/bin/python3` (search path)
/opt/homebrew/opt/python@3.13/bin/python3.13

% RUST_LOG=uv=trace uv python find '==3.12.8'
DEBUG uv 0.5.6 (Homebrew 2024-12-03)
DEBUG Using Python request `==3.12.8` from explicit request
DEBUG Searching for Python ==3.12.8 in virtual environments or search path
TRACE Searching PATH for executables: python3, python
TRACE Checking `PATH` directory for interpreters: /opt/homebrew/bin
TRACE Found possible Python executable: /opt/homebrew/bin/python3
TRACE Cached interpreter info for Python 3.13.0, skipping probing: /opt/homebrew/bin/python3
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/opt/homebrew/bin/python3` (search path)
DEBUG Skipping interpreter at `/opt/homebrew/opt/python@3.13/bin/python3.13` from search path: does not satisfy request `==3.12.8`
TRACE Checking `PATH` directory for interpreters: /opt/homebrew/sbin
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Checking `PATH` directory for interpreters: /System/Cryptexes/App/usr/bin
TRACE Checking `PATH` directory for interpreters: /usr/bin
TRACE Found possible Python executable: /usr/bin/python3
TRACE Querying interpreter executable at /usr/bin/python3
DEBUG Found `cpython-3.9.6-macos-aarch64-none` at `/usr/bin/python3` (search path)
DEBUG Skipping interpreter at `/Applications/Xcode.app/Contents/Developer/usr/bin/python3` from search path: does not satisfy request `==3.12.8`
TRACE Checking `PATH` directory for interpreters: /bin
TRACE Checking `PATH` directory for interpreters: /usr/sbin
TRACE Checking `PATH` directory for interpreters: /sbin
TRACE Checking `PATH` directory for interpreters: /Library/Apple/usr/bin
TRACE Checking `PATH` directory for interpreters: /Users/aloenr01/.cargo/bin
TRACE Checking `PATH` directory for interpreters: /Applications/iTerm.app/Contents/Resources/utilities
error: No interpreter found for Python ==3.12.8 in virtual environments or search path
```

What I think is happening is that `uv python list` code does this:

https://github.com/astral-sh/uv/blob/3aaa9594be4727fb4a6260b1cc5782eb66e47284/crates/uv-python/src/discovery.rs#L499-L501

where the `find_all_minor` means it can pick up on `/opt/homebrew/bin/python3.12`.

But when asking for `==3.12.8` specifically, it doesn't consider that `3.12` could be it:

https://github.com/astral-sh/uv/blob/3aaa9594be4727fb4a6260b1cc5782eb66e47284/crates/uv-python/src/discovery.rs#L579-L581

 _Originally posted by in [#9668](https://github.com/astral-sh/uv/issues/9668#issuecomment-2523678460), but that issue was unrelated. CC @zanieb _

#### System Information:

- uv 0.5.6 (Homebrew 2024-12-03)
- macOS Sonoma 14.7.1 (23H222)
- arm64


---

_Comment by @zanieb on 2024-12-06 22:31_

Thank you!

Does `uv python find 3.12.8` work? (instead of `uv python find ==3.12.8`)

---

_Label `bug` added by @zanieb on 2024-12-06 22:31_

---

_Comment by @aDotInTheVoid on 2024-12-06 22:52_

> Does `uv python find 3.12.8` work?

Yes it does:

```
% uv python find 3.12.8
/opt/homebrew/opt/python@3.12/bin/python3.12
```

Although this doesn't work in the `requires-python` field of `pyproject.toml` (sorry, I maybe should have put this in the issue, not sure if it's the same codepath or not):

```
% uv sync
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 4, column 19
  |
4 | requires-python = "3.12.8"
  |                   ^^^^^^^^
Failed to parse version: Unexpected end of version specifier, expected operator:
3.12.8
^^^^^^
```

And with `requires-python = "==3.12.8"` if fails in the same way as before:

```
% uv sync
error: No interpreter found for Python ==3.12.8 in search path
```

<details>

<summary> Full Log </summary>

```
% rm -rf ./.venv
% RUST_LOG=uv=trace  uv sync
DEBUG uv 0.5.6 (Homebrew 2024-12-03)
DEBUG Found project root: `/Users/alona/dev/feeds`
DEBUG No workspace root found, using project root
DEBUG Using Python request `==3.12.8` from `requires-python` metadata
DEBUG Searching for Python ==3.12.8 in search path
TRACE Searching PATH for executables: python3, python
TRACE Checking `PATH` directory for interpreters: /Users/alona/.vscode/extensions/ms-python.python-2024.20.0-darwin-arm64/python_files/deactivate/zsh
TRACE Checking `PATH` directory for interpreters: /Users/alona/.vscode/extensions/ms-python.python-2024.20.0-darwin-arm64/python_files/deactivate/zsh
TRACE Checking `PATH` directory for interpreters: /opt/homebrew/bin
TRACE Found possible Python executable: /opt/homebrew/bin/python3
TRACE Cached interpreter info for Python 3.13.0, skipping probing: /opt/homebrew/bin/python3
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/opt/homebrew/bin/python3` (search path)
DEBUG Skipping interpreter at `/opt/homebrew/opt/python@3.13/bin/python3.13` from search path: does not satisfy request `==3.12.8`
TRACE Checking `PATH` directory for interpreters: /opt/homebrew/sbin
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Checking `PATH` directory for interpreters: /System/Cryptexes/App/usr/bin
TRACE Checking `PATH` directory for interpreters: /usr/bin
TRACE Found possible Python executable: /usr/bin/python3
TRACE Querying interpreter executable at /usr/bin/python3
DEBUG Found `cpython-3.9.6-macos-aarch64-none` at `/usr/bin/python3` (search path)
DEBUG Skipping interpreter at `/Applications/Xcode.app/Contents/Developer/usr/bin/python3` from search path: does not satisfy request `==3.12.8`
TRACE Checking `PATH` directory for interpreters: /bin
TRACE Checking `PATH` directory for interpreters: /usr/sbin
TRACE Checking `PATH` directory for interpreters: /sbin
TRACE Checking `PATH` directory for interpreters: /Library/Apple/usr/bin
TRACE Checking `PATH` directory for interpreters: /Users/alona/.cargo/bin
TRACE Checking `PATH` directory for interpreters: /Applications/iTerm.app/Contents/Resources/utilities
error: No interpreter found for Python ==3.12.8 in search path
```

</details>

---

_Referenced in [astral-sh/uv#9697](../../astral-sh/uv/pulls/9697.md) on 2024-12-06 22:55_

---

_Comment by @zanieb on 2024-12-06 23:01_

Ah okay thanks that's helpful context.

w.r.t. https://github.com/astral-sh/uv/blob/3aaa9594be4727fb4a6260b1cc5782eb66e47284/crates/uv-python/src/discovery.rs#L579-L581 we should be in the `VersionRequest::Range` case there. And if not, we should be returning the possible names in `VersionRequest::executable_names` https://github.com/astral-sh/uv/blob/cfd00797b67247cee744308303c0a4e585a19fd4/crates/uv-python/src/discovery.rs#L1770

I'm not sure what's going on yet.

---

_Label `uv python` added by @samypr100 on 2024-12-09 00:35_

---

_Comment by @zamanh on 2025-11-05 12:25_

I would expect `uv python find` to return a list of all installed Python versions (by uv), in a similar manner to `uv python list` as that command knows which python(s) are installed compared to this one only knowing about "one".

---

_Comment by @zanieb on 2025-11-05 14:54_

@zamanh Your comment seems unrelated to this bug? Please don't bump issues with commentary that is not about the OP.

It sounds like you want `uv python list <request>` instead? Please open a new issue if you want to discuss further.

---
