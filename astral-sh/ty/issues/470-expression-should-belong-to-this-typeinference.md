```yaml
number: 470
title: expression should belong to this TypeInference region and TypeInferenceBuilder should have inferred
type: issue
state: closed
author: PKizzle
labels:
  - bug
  - needs-mre
  - fatal
assignees: []
created_at: 2025-05-21T09:37:00Z
updated_at: 2025-05-21T15:26:25Z
url: https://github.com/astral-sh/ty/issues/470
synced_at: 2026-01-12T15:54:23Z
```

# expression should belong to this TypeInference region and TypeInferenceBuilder should have inferred

---

_@PKizzle_

I have encountered a language server crash on Linux with a previous version of  the ty VSCode extension. Therefore, I am not sure whether this has already been addressed in 2025.11.11402026.

```
0.003241442s  INFO ty:main ty_server::server: File watcher successfully registered
   0.027533849s  INFO ty:worker:1 ty_project: Indexed 159 file(s)
   6.645904567s  WARN     ty:main ty_server::server::api: Received notification $/cancelRequest which does not have a handler.
  32.932807635s ERROR ty:worker:1 ty_server::server: panicked at crates/ty_python_semantic/src/semantic_model.rs:58:44:
expression should belong to this TypeInference region and TypeInferenceBuilder should have inferred a type for it
   0: <unknown>
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
  16: start_thread
             at ./nptl/pthread_create.c:442:8
  17: __GI___clone3
             at ./misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:81:0

panicked at crates/ty_python_semantic/src/semantic_model.rs:58:44:
expression should belong to this TypeInference region and TypeInferenceBuilder should have inferred a type for it
   0: <unknown>
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
  16: start_thread
             at ./nptl/pthread_create.c:442:8
  17: __GI___clone3
             at ./misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:81:0

  33.110293788s  WARN     ty:main ty_server::server::api: Received notification $/cancelRequest which does not have a handler.
```

ty version: 2025.9.11391444
OS version: #61~22.04.1-Ubuntu (6.8.0-59-generic)

---

_Comment by @PKizzle on 2025-05-21 09:39_

It seems like the file mentioned above [semantic_model.rs](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/semantic_model.rs ) is actually part of ruff. Shall I open an issue over there?

---

_Comment by @MichaReiser on 2025-05-21 10:03_

Any chance you still have the file content from when ty panicked? Or can you try running the CLI to see if it panics too?

---

_Label `bug` added by @MichaReiser on 2025-05-21 10:03_

---

_Renamed from "Language server crash" to "expression should belong to this TypeInference region and TypeInferenceBuilder should have inferred" by @MichaReiser on 2025-05-21 10:04_

---

_Label `fatal` added by @AlexWaygood on 2025-05-21 11:27_

---

_Label `needs-mre` added by @AlexWaygood on 2025-05-21 11:28_

---

_Comment by @PKizzle on 2025-05-21 15:15_

@MichaReiser I tried re-opening all recent files but so far I am unable to find the one that caused the issue 😥

---

_Comment by @carljm on 2025-05-21 15:25_

We have fixed some occurrences of this error since the earliest ty releases, so it's possible the issue has been fixed. Given that, I don't think this is currently actionable unless we have a confirmed case on a current ty release.

---

_Comment by @carljm on 2025-05-21 15:26_

Going to close this for now, but please feel free to reopen if you see the issue occur on a current ty release!

---

_Closed by @carljm on 2025-05-21 15:26_

---
