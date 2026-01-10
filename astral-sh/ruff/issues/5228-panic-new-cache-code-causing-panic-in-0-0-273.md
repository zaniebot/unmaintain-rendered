```yaml
number: 5228
title: "[Panic] new cache code causing panic in 0.0.273"
type: issue
state: closed
author: Edward-Knight
labels: []
assignees: []
created_at: 2023-06-20T22:44:54Z
updated_at: 2023-06-20T23:01:23Z
url: https://github.com/astral-sh/ruff/issues/5228
synced_at: 2026-01-10T11:09:47Z
```

# [Panic] new cache code causing panic in 0.0.273

---

_Issue opened by @Edward-Knight on 2023-06-20 22:44_

Running `RUST_BACKTRACE=full ruff <file>` on 0.0.273 resulted in this output:

```

error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', crates/ruff_cli/src/commands/run.rs:112:65
stack backtrace:
   0:        0x102bd4718 - _rust_eh_personality
   1:        0x10260fc84 - _main
   2:        0x102baf69c - _rust_eh_personality
   3:        0x102bd6108 - _rust_eh_personality
   4:        0x102bd5d48 - _rust_eh_personality
   5:        0x102bd5a8c - _rust_eh_personality
   6:        0x102a7d280 - _main
   7:        0x102bd6a30 - _rust_eh_personality
   8:        0x102bd67f0 - _rust_eh_personality
   9:        0x102bd6784 - _rust_eh_personality
  10:        0x102bd6778 - _rust_eh_personality
  11:        0x102c5093c - __rjem_je_witnesses_cleanup
  12:        0x102c509dc - __rjem_je_witnesses_cleanup
  13:        0x102a66e84 - _main
  14:        0x102a50cf0 - _main
  15:        0x102a2e424 - _main
  16:        0x102a26708 - _main
  17:        0x102591038 - __mh_execute_header
  18:        0x10258d788 - __mh_execute_header
  19:        0x102592328 - _main
```

My `.ruff_cache` looks like this:

```
$ tree .ruff_cache
.ruff_cache
├── CACHEDIR.TAG
└── content

2 directories, 1 file
$ cat .ruff_cache/CACHEDIR.TAG 
Signature: 8a477f597d28d172789f06886806bc55
```

Unfortunately I cannot provide the file that triggered this. This was run in a project without ruff configured, although it does have a `pyproject.toml`. This happens using the `--isolated` flag as well. Running on AArch64 macOS.

Here is the code snippet:
https://github.com/astral-sh/ruff/blob/fde5dbc9aa967b086347f87fc61511c8dfe7ad0e/crates/ruff_cli/src/commands/run.rs#L109-L112

Last modified in 17f1ecd56e376a75d66e01be47029f1b1b29c0aa.

---

_Comment by @Edward-Knight on 2023-06-20 22:47_

Looks like #5225 was created while I wrote this, seems to be the same issue

---

_Comment by @charliermarsh on 2023-06-20 22:48_

Sorry, yeah, I'll cut a fix for this tonight. I'm just trying to reproduce first.

---

_Closed by @charliermarsh on 2023-06-20 23:01_

---
