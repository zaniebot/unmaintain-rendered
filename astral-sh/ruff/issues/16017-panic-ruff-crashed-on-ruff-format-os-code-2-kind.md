```yaml
number: 16017
title: "[Panic] Ruff crashed on ruff format - Os { code: 2, kind: NotFound }"
type: issue
state: closed
author: prdai
labels:
  - question
assignees: []
created_at: 2025-02-07T11:16:27Z
updated_at: 2025-02-07T16:34:04Z
url: https://github.com/astral-sh/ruff/issues/16017
synced_at: 2026-01-12T15:54:55Z
```

# [Panic] Ruff crashed on ruff format - Os { code: 2, kind: NotFound }

---

_@prdai_


Ruff crashed while running `ruff format`. The error message indicates a missing file or directory:  

```
error: Ruff crashed. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

...quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/path-dedot-3.1.1/src/lib.rs:330:70:
called `Result::unwrap()` on an `Err` value: Os { code: 2, kind: NotFound, message: "No such file or directory" }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

### Steps to Reproduce:
1. Run `ruff format` on the project.
2. Observe the crash.

### Environment:
- **Python version:** 3.11.5
- **OS:** Ubuntu 24.04.1 LTS (WSL)

---

_Comment by @MichaReiser on 2025-02-07 11:35_

Thanks for the detailed write up. 

The error message would suggest that the current working directory was deleted. Can you `cd` out of the current directory and `cd` into it again to verify that it still exists?

Relevant lines in path-dedot: https://github.com/magiclen/path-dedot/blob/36c197f922d4c8779594ce25b8ffc6cb0315c6a8/src/lib.rs#L328C1-L330C80

---

_Label `question` added by @MichaReiser on 2025-02-07 11:35_

---

_Comment by @prdai on 2025-02-07 16:34_

Thank you for the prompt response! That was indeed the issue—switching directories and running the command again resolved it. Appreciate the help :)

---

_Closed by @prdai on 2025-02-07 16:34_

---
