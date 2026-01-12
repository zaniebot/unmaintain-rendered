```yaml
number: 469
title: "Server panic: \"ref count should be 1 because `zalsa_mut` drops all other DB references\""
type: issue
state: closed
author: nistath
labels:
  - bug
  - server
  - fatal
assignees: []
created_at: 2025-05-21T04:52:40Z
updated_at: 2025-05-22T14:10:08Z
url: https://github.com/astral-sh/ty/issues/469
synced_at: 2026-01-12T15:54:23Z
```

# Server panic: "ref count should be 1 because `zalsa_mut` drops all other DB references"

---

_@nistath_

### Summary

Using ty-vscode 2025.11.11402026.

`ty check` manages to index the files. However, the server panics.

```
 8.126400250s  WARN     ty:main ty_server::server::api: Received notification $/cancelRequest which does not have a handler.
  25.024617416s  INFO ty:worker:0 check_file{file=file(Id(1000))}:Project::index_files{project=openai}: ty_project: Indexed 100961 file(s)
  25.170448708s ERROR     ty:main ty_server::server: panicked at crates/ty_project/src/db.rs:100:14:
ref count should be 1 because `zalsa_mut` drops all other DB references.
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __pthread_cond_wait

panicked at crates/ty_project/src/db.rs:100:14:
ref count should be 1 because `zalsa_mut` drops all other DB references.
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __pthread_cond_wait

  25.212083333s ERROR        main ty_server::server: panicked at /Users/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/jod-thread-1.0.0/src/lib.rs:33:22:
called `Result::unwrap()` on an `Err` value: Any { .. }
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __mh_execute_header
  16: __mh_execute_header

panicked at /Users/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/jod-thread-1.0.0/src/lib.rs:33:22:
called `Result::unwrap()` on an `Err` value: Any { .. }
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __mh_execute_header
  16: __mh_execute_header
```

### Version

ty 0.0.1-alpha.6 (f11c6012d 2025-05-20)

---

_Label `bug` added by @MichaReiser on 2025-05-21 06:06_

---

_Label `server` added by @MichaReiser on 2025-05-21 06:06_

---

_Comment by @MichaReiser on 2025-05-21 06:06_

Thanks. I've seen this too this week and plan to have a closer look (if it isn't already fixed by some recent changes that I made)

---

_Label `fatal` added by @AlexWaygood on 2025-05-21 11:32_

---

_Renamed from "ty_server panics" to "Server panic: "ref count should be 1 because `zalsa_mut` drops all other DB references"" by @dhruvmanila on 2025-05-21 15:15_

---

_Closed by @MichaReiser on 2025-05-22 14:10_

---
