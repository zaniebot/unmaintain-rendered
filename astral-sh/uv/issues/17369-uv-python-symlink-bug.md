```yaml
number: 17369
title: uv Python symlink bug
type: issue
state: closed
author: devinbarry
labels:
  - needs-mre
assignees: []
created_at: 2026-01-08T22:52:00Z
updated_at: 2026-01-09T16:14:50Z
url: https://github.com/astral-sh/uv/issues/17369
synced_at: 2026-01-12T16:02:49Z
```

# uv Python symlink bug

---

_@devinbarry_

### Summary

On macOS (Apple Silicon) with Python 3.14 installed via Homebrew in `/opt/homebrew`, `uv run` creates a project `.venv` whose `pyvenv.cfg` records `home = /usr/local/bin` and whose `.venv/bin/python` symlinks to `/usr/local/bin/python3.14` (non-existent). As a result, `uv` considers the venv broken and repeatedly deletes/recreates it on subsequent `uv run` invocations.

This looks like incorrect venv metadata/symlink generation, since `uv python find --system` correctly discovers the interpreter under `/opt/homebrew`.

### Repro steps

1. On Apple Silicon macOS, install Python 3.14 with Homebrew (prefix `/opt/homebrew`).
2. Ensure there is no `/usr/local/bin/python3.14` on the system.
3. In a project directory, run `uv run which python` (or `uv run python --version`) to let uv create `.venv`.
4. Inspect `.venv/bin/python` and `.venv/pyvenv.cfg`.
5. Run `uv run which python` again; uv warns the existing `.venv` is linked to a non-existent interpreter and recreates it.

### Expected behavior

`.venv` should be created pointing at the discovered Homebrew Python interpreter:
- `.venv/bin/python` should resolve to `/opt/homebrew/opt/python@3.14/bin/python3.14` (or similar)
- `.venv/pyvenv.cfg` should not contain `home = /usr/local/bin` on an Apple Silicon Homebrew setup.

Subsequent `uv run ...` should reuse the existing `.venv` without warning or recreation.

### Actual behavior

`.venv/bin/python` and `.venv/pyvenv.cfg` point at `/usr/local/bin` even though the interpreter is in `/opt/homebrew`, and `/usr/local/bin/python3.14` does not exist. `uv` then warns and recreates `.venv` repeatedly.

### Environment

- macOS: 15.7.3
- CPU/arch:
  - `uname -m`: `arm64`
  - `arch`: `arm64`
- Homebrew:
  - `brew --prefix`: `/opt/homebrew`
  - `brew --prefix python@3.14`: `/opt/homebrew/opt/python@3.14`
- uv:
  - `uv --version`: `uv 0.9.22 (82a6a66b8 2026-01-06)`
  - binary: `Mach-O 64-bit executable arm64` (installed to `~/.local/bin/uv`)
- Python:
  - `command -v python3.14`: `/opt/homebrew/bin/python3.14`
  - `/opt/homebrew/opt/python@3.14/bin/python3.14 -c 'import sys; print(sys.executable); print(getattr(sys,"_base_executable",None)); print(sys.prefix); print(sys.base_prefix)'` prints:

    ```
    /opt/homebrew/opt/python@3.14/bin/python3.14
    /opt/homebrew/opt/python@3.14/bin/python3.14
    /opt/homebrew/opt/python@3.14/Frameworks/Python.framework/Versions/3.14
    /opt/homebrew/opt/python@3.14/Frameworks/Python.framework/Versions/3.14
    ```

### Python location

`uv python find --system`:

```
/opt/homebrew/opt/python@3.14/bin/python3.14
```

### Platform

Darwin 24.6.0 arm64

### Version

uv 0.9.22 (82a6a66b8 2026-01-06)

### Python version

3.14.2

---

_Label `bug` added by @devinbarry on 2026-01-08 22:52_

---

_Comment by @devinbarry on 2026-01-09 00:50_

A solution to this problem is to use `uv python pin /opt/homebrew/opt/python@3.14/bin/python3.14`

---

_Closed by @devinbarry on 2026-01-09 01:29_

---

_Comment by @konstin on 2026-01-09 11:01_

CC @woodruffw Is that a bug in the homebrew Python builds?

---

_Comment by @zanieb on 2026-01-09 13:40_

What does `<python> -c "import sys; print(sys._base_executable)"` show?

---

_Comment by @woodruffw on 2026-01-09 15:40_

> CC [@woodruffw](https://github.com/woodruffw) Is that a bug in the homebrew Python builds?

Potentially, I'll take a look at it. 



---

_Comment by @woodruffw on 2026-01-09 15:46_

Hmm, I don't see anything obvious in the `python@3.14.rb` formula that would cause the pyenv config to receive the wrong `home` prefix. Everything should be correctly located relative to the actual `brew --prefix`.

@devinbarry could you share the result of running `brew doctor`? Also, did you ever have an earlier Homebrew installation installed under `/usr/local`, e.g. an x64 installation via Rosetta?

Ref: https://github.com/Homebrew/homebrew-core/blob/fc1a970a29a1facbd92d2fb748cd97da9010716f/Formula/p/python%403.14.rb

---

_Label `bug` removed by @konstin on 2026-01-09 16:14_

---

_Label `needs-mre` added by @konstin on 2026-01-09 16:14_

---
