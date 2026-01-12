```yaml
number: 7451
title: "uv-tool/install: ignore existing environments on interpreter mismatch"
type: pull_request
state: merged
author: lucab
labels:
  - enhancement
assignees: []
merged: true
base: main
head: ups/tool-install-interp-mismatch
created_at: 2024-09-17T08:20:55Z
updated_at: 2024-09-18T06:37:45Z
url: https://github.com/astral-sh/uv/pull/7451
synced_at: 2026-01-12T16:07:50Z
```

# uv-tool/install: ignore existing environments on interpreter mismatch

---

_@lucab_

This changes `uv tool install` behavior with regards to re-using existing environments.
In particular, this replaces the existing version-matching logic with a tighter one, enforcing
a same-interpreter match.
This allows to properly switch between system and managed interpreter, at the cost of
more eagerly invalidating existing environments every time there is an interpreter change.

Closes: https://github.com/astral-sh/uv/issues/7320

---

_Review comment by @lucab on `crates/uv-tool/src/lib.rs`:207 on 2024-09-17 08:23_

Note: drive-by change to make debug logs more uniform. After this, the output now looks like this:
```
DEBUG Found existing environment for tool `poetry`: .local/share/uv/tools/poetry
DEBUG Found existing interpreter for tool `poetry`: /home/lucab/.local/share/uv/tools/poetry/bin/python3
```

---

_@lucab reviewed on 2024-09-17 08:23_

---

_Label `enhancement` added by @konstin on 2024-09-17 09:51_

---

_Comment by @lucab on 2024-09-17 10:10_

It looks like I'm hitting some Windows-only different/buggy behavior, or some non-determinism in the CI there, which I'm trying to narrow down.

The symptom is that, on Windows runner only, running the same command twice results in the interpreter binary jumping around.
That is,  this simple `uv tool install black` on the second run will trigger environment invalidation due to a different interpreter:
https://github.com/astral-sh/uv/blob/f679987fe1908c8a815dbcbc6a2a112ae49858ba/crates/uv/tests/tool_install.rs#L843-L855
```
Snapshot: tool_install_already_installed-3
[...]
    4     4 │ ----- stderr -----
    5       │-`black` is already installed
          5 │+Ignored existing environment for `black` due to stale Python interpreter
          6 │+Interpreter mismatch: old='[TEMP_DIR]/tools/black/Scripts/python' vs new='[PYTHON-3.12]'
```

---

_Comment by @lucab on 2024-09-17 12:05_

I've abused the CI to run a few more tests, and found out that this failure on the Windows CI is specific to using a "system" interpreter.
The logic works and test is passing when forcing a managed interpreter via `--python-preference only-managed`, but fails in all other cases.

My hypothesis here is that there is indeed a difference in the OSes, when creating the venv and setting up the interpreter. On Unix this is done through symlinks (targeting the same binary file in the end) which Windows does not generally have, so there is some file duplication involved (thus failing the `is_same_file()` check).

---

_Marked ready for review by @lucab on 2024-09-17 15:54_

---

_Comment by @lucab on 2024-09-17 15:54_

For future reference, things we tried in this PR but didn't work due to false positive mismatches detected:
 - comparing interpreter binaries on-disk (via `same-file`): on Windows, interpreter files are copied around thus always mismatching
 - comparing interpreter paths: also on Windows, minor name differences and versioning (e.g. `python` vs `python3`) which result in mismatches

Eventually settled on comparing `base_prefix` as proxy, as suggested by @zanieb.

---

_Renamed from "uv-tool/install: ignore existing environment on interpreter mismatch" to "uv-tool/install: ignore existing environments on interpreter mismatch" by @lucab on 2024-09-17 16:05_

---

_@zanieb reviewed on 2024-09-17 16:16_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:279 on 2024-09-17 16:16_

As a style nit, we don't usually attach names to notes. Something like "N.B." is more common.

cc @charliermarsh not sure if you have a stance here.

---

_@zanieb reviewed on 2024-09-17 16:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:287 on 2024-09-17 16:19_

We'll always find an existing interpreter, maybe this should say something like... "Existing interpreter matches the requested interpreter"? I'd also be okay with lowering this to a trace log.

---

_@zanieb reviewed on 2024-09-17 16:23_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:295 on 2024-09-17 16:23_

My intuition is that this should be "Ignoring" but I'm not sure if that's clear in our style guide. If you search though, we use "Ignoring" consistently but not "Ignored".

Is the meaning of "stale" clear here? I worry it's not clear to the user if that means something is wrong with the existing interpreter or not. We might want to say something like "Ignoring existing environment for `{from}`: the requested Python interpreter does not match the environment interpreter". Wdyt?

---

_@zanieb reviewed on 2024-09-17 16:28_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:2255 on 2024-09-17 16:28_

This isn't super obvious, but I think this can fetch a Python managed version during the test which we currently avoid (downstream packagers do not like it). I think this test may also be brittle depending on the developer's machine — the test context is happy with whatever Python versions it can find whether they are system or managed. I am not sure how to properly write a test for this though :/ it seems like a pain.

---

_@zanieb reviewed on 2024-09-17 16:29_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:2255 on 2024-09-17 16:29_

For the sake of your time, I'd recommend omitting test coverage and opening an issue to add it. We'll need to consider a comprehensive solution here.

---

_@lucab reviewed on 2024-09-17 16:36_

---

_Review comment by @lucab on `crates/uv/src/commands/tool/install.rs`:279 on 2024-09-17 16:36_

I can drop that, but for reference this form is already in use in this codebase: https://github.com/search?q=repo%3Aastral-sh%2Fuv%20NOTE(&type=code

I don't remember exactly where I picked up this habit, I personally like it. It spares a bunch of recursive git-blaming to find out who wrote a specific note/todo and provides a breadcrumb in case of future doubts.

---

_@zanieb reviewed on 2024-09-17 16:38_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:279 on 2024-09-17 16:38_

Ah fair point — I don't mind keeping it. Thanks for correcting me.

---

_@lucab reviewed on 2024-09-17 16:47_

---

_Review comment by @lucab on `crates/uv/tests/tool_install.rs`:2255 on 2024-09-17 16:47_

I can split the new cases apart in a different test and mark it as ignored, if you like. That should still allow running the test manually if needed, but won't impact packagers.
I'll open a ticket about testing with system interpreters.

---

_Review comment by @lucab on `crates/uv/tests/tool_install.rs`:2156 on 2024-09-17 17:20_

```
$ cargo nextest run --run-ignored all tool_install_python_preference
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.29s
------------
 Nextest run ID 110712ea-a0a0-436a-ab95-4cdfbb384338 with nextest profile: default
    Starting 1 test across 86 binaries (1651 tests skipped)
        PASS [   6.088s] uv::tool_install tool_install_python_preference
------------
     Summary [   6.089s] 1 test run: 1 passed, 1651 skipped
```

---

_@lucab reviewed on 2024-09-17 17:34_

---

_@lucab reviewed on 2024-09-17 17:34_

---

_Review comment by @lucab on `crates/uv/tests/tool_install.rs`:2255 on 2024-09-17 17:34_

https://github.com/astral-sh/uv/issues/7473

---

_@zanieb approved on 2024-09-17 18:38_

Nice work!

---

_Merged by @lucab on 2024-09-18 06:37_

---

_Closed by @lucab on 2024-09-18 06:37_

---

_Branch deleted on 2024-09-18 06:37_

---
