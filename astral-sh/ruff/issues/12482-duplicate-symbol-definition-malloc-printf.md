```yaml
number: 12482
title: "duplicate symbol definition: _malloc_printf"
type: issue
state: open
author: serjflint
labels:
  - question
  - needs-mre
assignees: []
created_at: 2024-07-24T00:54:49Z
updated_at: 2025-01-15T07:43:54Z
url: https://github.com/astral-sh/ruff/issues/12482
synced_at: 2026-01-12T15:54:51Z
```

# duplicate symbol definition: _malloc_printf

---

_@serjflint_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hello! Thank you for the great tool.

I am cross-compiling ruff 0.5.3 on Ubuntu 20.04 against `x86_64-apple-darwin` target using cargo-zigbuild. Other targets are fine, but for this one I got an error like this:

```bash
  = note: error: duplicate symbol definition: _malloc_printf
              note: defined by /opt/zig/lib/libc/darwin/libSystem.tbd
              note: defined by /tmp/rustcmNzciC/libtikv_jemalloc_sys-d0b27344258bf71e.rlib(malloc_io.pic.o)
          

error: could not compile `ruff` (bin "ruff") due to 1 previous error
```

Full trace here https://gist.github.com/serjflint/2e67f06207383c3fbddf97798dd103f8

Last time I was building ruff 0.3.4 (with `tikv-jemallocator = { version = "0.5.0" }`) about 4 months ago and it was working.My Mac OS SDK is about 1 year old.
The issue is probably related to the recent bump of tikv-jemallocator to "0.6.0"

It is not issue with ruff per se, just heads up.
I've filed issues https://github.com/tikv/jemallocator/issues/97 and https://github.com/ziglang/zig/issues/20694

---

_Label `question` added by @MichaReiser on 2024-07-24 06:59_

---

_Comment by @MichaReiser on 2024-09-09 12:04_

Hi @serjflint. Have you been able to resolve this in the meantime?

---

_Comment by @serjflint on 2024-09-09 13:47_

> Hi @serjflint. Have you been able to resolve this in the meantime?

Hi! I didn't have time yet. I pinned the version for Intel + Mac OS

---

_Label `needs-mre` added by @MichaReiser on 2024-11-20 12:04_

---

_Comment by @MichaReiser on 2025-01-15 07:43_

I'll close this issue for now as it isn't actionable for us. Feel free to open a new issue or comment here if you're able to reproduce. 

---

_Closed by @MichaReiser on 2025-01-15 07:43_

---

_Reopened by @MichaReiser on 2025-01-15 07:43_

---
