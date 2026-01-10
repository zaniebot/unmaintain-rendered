---
number: 21081
title: Ruff format on multiple directories returns inconsistent results
type: issue
state: closed
author: OBarronCS
labels:
  - bug
  - cli
assignees: []
created_at: 2025-10-26T21:13:28Z
updated_at: 2025-10-28T17:49:27Z
url: https://github.com/astral-sh/ruff/issues/21081
synced_at: 2026-01-10T01:23:02Z
---

# Ruff format on multiple directories returns inconsistent results

---

_Issue opened by @OBarronCS on 2025-10-26 21:13_

### Summary

The pwndbg project - https://github.com/pwndbg/pwndbg/ -  is using `ruff format` to format the codebase. We have found that using  `ruff format` will inconsistently report formatting errors when running it with multiple directories.

# Reproduce
Pwndbg uses Ruff 0.4.10, however I have reproduced this with the latest version of Ruff as well. See the note at the end.

Clone the pwndbg repo on a version where this applies:
```sh
git clone --depth 1 https://github.com/pwndbg/pwndbg.git --branch 2025.10.20
cd pwndbg
```

Within the nested `pwndbg/` directory, there are 5 files with some formatting errors.

If you run this, you will see the errors (this uses our specific version of ruff through a virtual environment with `uv`):
```sh
uv run --group lint ruff format --check --diff pwndbg
```

However, if you add additional directories to be formatted, these errors will not always be reported.

```sh
uv run --group lint ruff format --check --diff pwndbg pwndbginit tests *.py scripts
```

You can run it in a loop of 100, and see it prints out the diff only on rare cases:
```
for i in `seq 1 100`; do uv run --group lint ruff format --check --diff pwndbg pwndbginit tests *.py scripts; done
```

As you remove directories from the list to be formatted, the likelihood of `ruff format` finding the formatting errors in the `pwndbg/` directory increases.

Note that if you don't use the `uv` virtual environment, and instead use the latest version of Ruff, 0.14.2, the same results apply. It will still find an inconsistent number of files with errors between different runs (the numbers vary from 0.4.10, as the new errors are resulting from newer versions of Ruff changing how things are formatted)

### Version

0.14.2 and 0.4.10

---

_Comment by @MichaReiser on 2025-10-27 07:28_

Curious. Looking at the logs, I suspect this is a `gitignore` related bug. I compared the logs from formatting just `pwndbg` and `pwndb scripts`. 

When only formatting `pwndbg`, I get

```
[2025-10-27][08:20:30][ignore::walk][DEBUG] whitelisting /Users/micha/astral/ecosystem/pwndbg/pwndbg/lib: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("/Users/micha/astral/ecosystem/pwndbg/.gitignore"), original: "!/pwndbg/lib/", actual: "pwndbg/lib", is_whitelist: true, is_only_dir: true })))
```

When formatting both `pwndbg` and `scripts`, I get 

```
[2025-10-27][08:20:22][ignore::walk][DEBUG] ignoring /Users/micha/astral/ecosystem/pwndbg/pwndbg/lib: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/micha/astral/ecosystem/pwndbg/.gitignore"), original: "lib/", actual: "**/lib", is_whitelist: false, is_only_dir: true })))
```

Note how the gitignore pattern is slightly different

It looks like https://github.com/astral-sh/ruff/pull/20979 fixes this (running with only `pwndbg` 


```
❯ ~/astral/ruff/target/debug/ruff format --check pwndbg
warning: Detected debug build without --no-cache.
Would reformat: pwndbg/aglib/disasm/instruction.py
Would reformat: pwndbg/aglib/kernel/buddydump.py
Would reformat: pwndbg/aglib/macho.py
Would reformat: pwndbg/aglib/onegadget.py
Would reformat: pwndbg/aglib/regs.py
Would reformat: pwndbg/commands/ai.py
Would reformat: pwndbg/commands/context.py
Would reformat: pwndbg/commands/hijack_fd.py
Would reformat: pwndbg/commands/kdmesg.py
Would reformat: pwndbg/commands/mallocng.py
Would reformat: pwndbg/commands/mmap.py
Would reformat: pwndbg/commands/vmmap.py
Would reformat: pwndbg/dbg/gdb/__init__.py
Would reformat: pwndbg/dbg/lldb/__init__.py
Would reformat: pwndbg/dbg/lldb/repl/__init__.py
Would reformat: pwndbg/dbg/lldb/repl/io.py
Would reformat: pwndbg/gdblib/events.py
Would reformat: pwndbg/gdblib/ptmalloc2_tracking.py
Would reformat: pwndbg/lib/cache.py
Would reformat: pwndbg/lib/kernel/kconfig.py
Would reformat: pwndbg/lib/memory.py
Would reformat: pwndbg/lib/pretty_print.py
Would reformat: pwndbg/lib/regs.py
Would reformat: pwndbg/lib/zig.py
```

```
❯ ~/astral/ruff/target/debug/ruff format --check pwndbg scripts
warning: Detected debug build without --no-cache.
Would reformat: pwndbg/aglib/disasm/instruction.py
Would reformat: pwndbg/aglib/kernel/buddydump.py
Would reformat: pwndbg/aglib/macho.py
Would reformat: pwndbg/aglib/onegadget.py
Would reformat: pwndbg/aglib/regs.py
Would reformat: pwndbg/commands/ai.py
Would reformat: pwndbg/commands/context.py
Would reformat: pwndbg/commands/hijack_fd.py
Would reformat: pwndbg/commands/kdmesg.py
Would reformat: pwndbg/commands/mallocng.py
Would reformat: pwndbg/commands/mmap.py
Would reformat: pwndbg/commands/vmmap.py
Would reformat: pwndbg/dbg/gdb/__init__.py
Would reformat: pwndbg/dbg/lldb/__init__.py
Would reformat: pwndbg/dbg/lldb/repl/__init__.py
Would reformat: pwndbg/dbg/lldb/repl/io.py
Would reformat: pwndbg/gdblib/events.py
Would reformat: pwndbg/gdblib/ptmalloc2_tracking.py
Would reformat: pwndbg/lib/cache.py
Would reformat: pwndbg/lib/kernel/kconfig.py
Would reformat: pwndbg/lib/memory.py
Would reformat: pwndbg/lib/pretty_print.py
Would reformat: pwndbg/lib/regs.py
Would reformat: pwndbg/lib/zig.py
Would reformat: scripts/_docs/extract_configuration_docs.py
25 files would be reformatted, 243 files already formatted
```

---

_Label `bug` added by @MichaReiser on 2025-10-27 07:28_

---

_Referenced in [astral-sh/ruff#20979](../../astral-sh/ruff/pulls/20979.md) on 2025-10-27 07:28_

---

_Label `cli` added by @MichaReiser on 2025-10-27 07:29_

---

_Closed by @MichaReiser on 2025-10-28 17:49_

---
