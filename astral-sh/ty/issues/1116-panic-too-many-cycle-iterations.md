```yaml
number: 1116
title: "[panic] too many cycle iterations"
type: issue
state: closed
author: C0rn3j
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-09-02T12:40:46Z
updated_at: 2025-10-23T14:49:16Z
url: https://github.com/astral-sh/ty/issues/1116
synced_at: 2026-01-12T15:54:24Z
```

# [panic] too many cycle iterations

---

_@C0rn3j_

[AUR/ty-git](https://aur.archlinux.org/packages/ty-git) (latest master as of time of posting) on Arch Linux against [Tauon's codebase](https://github.com/Taiko2k/Tauon):

```
Checking ------------------------------------------------------------ 30/30 files                                                                                                                                                                                                                                           error[panic]: Panicked at /build/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/user/Projects/Tauon/src/tauon/t_modules/t_bootstrap.py`: `exported_names(Id(145d)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.19 (59bf06d26 2025-08-25)
info: Args: ["ty", "check", "src/tauon/"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: semantic_index(Id(1942f))
             at crates/ty_python_semantic/src/semantic_index.rs:54
   1: global_scope(Id(1942f))
             at crates/ty_python_semantic/src/semantic_index.rs:184
   2: Type < 'db >::member_lookup_with_policy_(Id(640b))
             at crates/ty_python_semantic/src/types.rs:715
   3: infer_definition_types(Id(3006))
             at crates/ty_python_semantic/src/types/infer.rs:174
   4: infer_scope_types(Id(2400))
             at crates/ty_python_semantic/src/types/infer.rs:145
   5: check_file_impl(Id(1002))
             at crates/ty_project/src/lib.rs:522
```

---

_Comment by @carljm on 2025-09-02 16:11_

This is most likely #256, which I'm currently working on.

---

_Label `fatal` added by @AlexWaygood on 2025-10-06 16:55_

---

_Label `bug` added by @AlexWaygood on 2025-10-06 16:55_

---

_Comment by @AlexWaygood on 2025-10-23 14:24_

@C0rn3j, does this still reproduce for you with the latest version of ty?

I'm able to run the latest version of ty succesfully on the Tauon codebase. But I did have to remove `jxlpy` from the `requirements.txt` file in Tauon before I was able to setup a development environment in which to run ty; it didn't install on my machine. And I also can't repro the originally reported panic with 0.0.1a19 -- if I run `ty check` using that version, ty simply never finishes on Tauon, and if I run `ty check src/tauon/t_modules/t_bootstrap.py`, I only see three non-panicking diagnostics.

---

_Comment by @C0rn3j on 2025-10-23 14:45_

Seems to work now, thanks!

---

_Closed by @C0rn3j on 2025-10-23 14:45_

---

_Comment by @AlexWaygood on 2025-10-23 14:49_

Great to hear -- and thanks for the report!

---
