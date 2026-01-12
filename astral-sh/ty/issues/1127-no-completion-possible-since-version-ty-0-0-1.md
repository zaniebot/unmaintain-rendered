```yaml
number: 1127
title: No completion possible since version ty 0.0.1-alpha.20
type: issue
state: closed
author: JaRoSchm
labels:
  - fatal
assignees: []
created_at: 2025-09-04T08:28:00Z
updated_at: 2025-09-05T15:36:10Z
url: https://github.com/astral-sh/ty/issues/1127
synced_at: 2026-01-12T15:54:24Z
```

# No completion possible since version ty 0.0.1-alpha.20

---

_@JaRoSchm_

### Summary

Hi, I updated ty to 0.0.1-alpha.20 this morning and since then completion in neovim doesn't work for me anymore. When starting neovim with `RUST_BACKTRACE=1` and start to type `ran<TAB>` I get the following logs:

```log
[START][2025-09-04 10:21:17] LSP logging initiated
[ERROR][2025-09-04 10:21:17] ...p/_transport.lua:36     "rpc"   "ty"    "stderr"        "2025-09-04 10:21:17.961338095  INFO Version: 0.0.1-alpha.20\n"
[ERROR][2025-09-04 10:21:17] ...p/_transport.lua:36     "rpc"   "ruff"  "stderr"        "2025-09-04 10:21:17.972934090  INFO No workspace options found for file:///home/jschmidt/Devel/Repositories, using default options\n"
[ERROR][2025-09-04 10:21:17] ...p/_transport.lua:36     "rpc"   "ty"    "stderr"        "2025-09-04 10:21:17.978580733  INFO Defaulting to python-platform `linux`\n"
[ERROR][2025-09-04 10:21:17] ...p/_transport.lua:36     "rpc"   "ty"    "stderr"        "2025-09-04 10:21:17.979515547  INFO Python version: Python 3.13, platform: linux\n"
[ERROR][2025-09-04 10:21:17] ...p/_transport.lua:36     "rpc"   "ty"    "stderr"        "2025-09-04 10:21:17.980217744  WARN Your LSP client doesn't support file watching: You may see stale results when files change outside the editor\n"
[ERROR][2025-09-04 10:21:17] ...p/_transport.lua:36     "rpc"   "ty"    "stderr"        "2025-09-04 10:21:17.980263299  WARN Received notification workspace/didChangeConfiguration which does not have a handler.\n"
[ERROR][2025-09-04 10:21:17] ...p/_transport.lua:36     "rpc"   "ruff"  "stderr"        "2025-09-04 10:21:18.000419202  INFO Registering workspace: /home/jschmidt/Devel/Repositories\n"
[ERROR][2025-09-04 10:21:17] ...p/_transport.lua:36     "rpc"   "ruff"  "stderr"        "2025-09-04 10:21:18.000532675  WARN LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.\n"
[ERROR][2025-09-04 10:21:18] ...p/_transport.lua:36     "rpc"   "ty"    "stderr"        "2025-09-04 10:21:18.049832392  INFO Indexed 8851 file(s) in 0.025s\n"
[ERROR][2025-09-04 10:21:25] ...p/_transport.lua:36     "rpc"   "ty"    "stderr"        "2025-09-04 10:21:25.007540482  INFO Error looking up `types.ModuleType` in typeshed: expected to find a class definition on Python 3.13, but found a symbol of type `Never` instead. Falling back to `Unknown` for the symbol instead.\n"
[ERROR][2025-09-04 10:21:25] ...p/_transport.lua:36     "rpc"   "ty"    "stderr"        "2025-09-04 10:21:25.295952661 ERROR An error occurred with request ID 12: request handler panicked at /home/conda/feedstock_root/build_artifacts/bld/rattler-build_ty_1756912065/build_env/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/maybe_changed_after.rs:268:21:\ndependency graph cycle when validating ClassLiteral < 'db >::decorators_(Id(9817)), set cycle_fn/cycle_initial to fixpoint iterate.\nQuery stack:\n[\n    Type < 'db >::member_lookup_with_policy_(Id(6012)),\n    Type < 'db >::class_member_with_policy_(Id(e002)),\n]query stacktrace:\n   0: DatabaseKeyIndex(IngredientIndex(99), Id(e002))\n   1: DatabaseKeyIndex(IngredientIndex(103), Id(6012))\n\n\nBacktrace:    0: <unknown>\n   1: <unknown>\n   2: <unknown>\n   3: <unknown>\n   4: <unknown>\n   5: <unknown>\n   6: <unknown>\n   7: <unknown>\n   8: <unknown>\n   9: <unknown>\n  10: <unknown>\n  11: <unknown>\n  12: <unknown>\n  13: <unknown>\n  14: <unknown>\n  15: <unknown>\n  16: <unknown>\n  17: <unknown>\n  18: <unknown>\n  19: <unknown>\n  20: <unknown>\n  21: <unknown>\n  22: <unknown>\n  23: <unknown>\n  24: <unknown>\n  25: <unknown>\n  26: <unknown>\n  27: <unknown>\n  28: <unknown>\n  29: <unknown>\n  30: <unknown>\n  31: <unknown>\n  32: <unknown>\n  33: <unknown>\n  34: <unknown>\n  35: <unknown>\n  36: <unknown>\n  37: <unknown>\n  38: <unknown>\n  39: <unknown>\n  40: <unknown>\n  41: <unknown>\n  42: <unknown>\n  43: <unknown>\n  44: <unknown>\n  45: <unknown>\n  46: <unknown>\n  47: <unknown>\n  48: <unknown>\n  49: <unknown>\n  50: <unknown>\n  51: <unknown>\n  52: <unknown>\n  53: <unknown>\n  54: <unknown>\n  55: <unknown>\n  56: <unknown>\n  57: <unknown>\n  58: <unknown>\n  59: <unknown>\n  60: <unknown>\n  61: <unknown>\n  62: <unknown>\n  63: <unknown>\n  64: <unknown>\n  65: <unknown>\n  66: <unknown>\n  67: <unknown>\n  68: <unknown>\n  69: <unknown>\n  70: start_thread\n             at ./nptl/pthread_create.c:448:8\n  71: __GI___clone3\n             at ./misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:78:0\n\n\n"
```

I am on Linux and ty is installed from conda-forge if this is relevant.

Thanks,
Jan


### Version

ty 0.0.1-alpha.20

---

_Comment by @JaRoSchm on 2025-09-04 08:35_

This seems to be related to the workspace I am in. If I do the same in `$HOME/temp`, everything works.

---

_Label `fatal` added by @AlexWaygood on 2025-09-04 08:41_

---

_Comment by @dhruvmanila on 2025-09-04 08:48_

Unrelated to this issue, you can direct the logs to a specific file to only get the messages (currently, they're being captured by Neovim which doesn't pretty print them):

```lua
vim.lsp.config["ty"] = {
  init_options = {
    logFile = '/path/to/ty.log'
  }
}
```

Pretty printing the logs:
```
2025-09-04 10:21:25.007540482  INFO Error looking up `types.ModuleType` in typeshed: expected to find a class definition on Python 3.13, but found a symbol of type `Never` instead. Falling back to `Unknown` for the symbol instead.
2025-09-04 10:21:25.295952661 ERROR An error occurred with request ID 12: request handler panicked at /home/conda/feedstock_root/build_artifacts/bld/rattler-build_ty_1756912065/build_env/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/maybe_changed_after.rs:268:21:
dependency graph cycle when validating ClassLiteral < 'db >::decorators_(Id(9817)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    Type < 'db >::member_lookup_with_policy_(Id(6012)),
    Type < 'db >::class_member_with_policy_(Id(e002)),
]
query stacktrace:
   0: DatabaseKeyIndex(IngredientIndex(99), Id(e002))
   1: DatabaseKeyIndex(IngredientIndex(103), Id(6012))


Backtrace:    0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: <unknown>
  18: <unknown>
  19: <unknown>
  20: <unknown>
  21: <unknown>
  22: <unknown>
  23: <unknown>
  24: <unknown>
  25: <unknown>
  26: <unknown>
  27: <unknown>
  28: <unknown>
  29: <unknown>
  30: <unknown>
  31: <unknown>
  32: <unknown>
  33: <unknown>
  34: <unknown>
  35: <unknown>
  36: <unknown>
  37: <unknown>
  38: <unknown>
  39: <unknown>
  40: <unknown>
  41: <unknown>
  42: <unknown>
  43: <unknown>
  44: <unknown>
  45: <unknown>
  46: <unknown>
  47: <unknown>
  48: <unknown>
  49: <unknown>
  50: <unknown>
  51: <unknown>
  52: <unknown>
  53: <unknown>
  54: <unknown>
  55: <unknown>
  56: <unknown>
  57: <unknown>
  58: <unknown>
  59: <unknown>
  60: <unknown>
  61: <unknown>
  62: <unknown>
  63: <unknown>
  64: <unknown>
  65: <unknown>
  66: <unknown>
  67: <unknown>
  68: <unknown>
  69: <unknown>
  70: start_thread
             at ./nptl/pthread_create.c:448:8
  71: __GI___clone3
             at ./misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:78:0
```

---

_Comment by @dhruvmanila on 2025-09-04 08:49_

Would you be able to share the code snippet on which this is being triggered?

---

_Comment by @JaRoSchm on 2025-09-04 08:56_

Thanks for the help!

The file contains only `ran` and then I tap `<TAB>` to trigger the completion. Sadly I am not able to share the whole workspace.

I will try to find a minimal environment which is able to reproduce this.

---

_Comment by @JaRoSchm on 2025-09-04 10:47_

Actually, I am not able to reliably reproduce this. I tried a new environment with a comparable setup, but I was not able to observe this there. Even in my usual setup, this problem doesn't always appear, even if I follow the exact same steps. If I type around and open/close the editor often enough, however, this will happen at some point. So somehow this feels like there is a race condition somewhere. I am grateful for further tips for debugging this.

Here is a video (with 3 minutes already cut off at the beginning), where I just type around a bit and try to trigger this error. It happens the second time I open `test.py` (see the red errors in the statusline).

https://github.com/user-attachments/assets/5880476f-ceb2-4802-807d-6a1b1e026f26

---

_Comment by @JaRoSchm on 2025-09-05 15:36_

After using ty for two days, I don't really observe this problem under real-work circumstances. I am not sure what went wrong there yesterday.

---

_Closed by @JaRoSchm on 2025-09-05 15:36_

---
