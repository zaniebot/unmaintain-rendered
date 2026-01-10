```yaml
number: 1242
title: "`--output-format gitlab` fails in Gitlab CI environment"
type: issue
state: closed
author: burrscurr
labels:
  - bug
assignees: []
created_at: 2025-09-23T18:16:32Z
updated_at: 2025-09-24T17:16:52Z
url: https://github.com/astral-sh/ty/issues/1242
synced_at: 2026-01-10T02:06:25Z
```

# `--output-format gitlab` fails in Gitlab CI environment

---

_Issue opened by @burrscurr on 2025-09-23 18:16_

### Summary

The recently added `--output-format gitlab` option works locally, but causes a panic in Gitlab CI environment:

```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
thread 'main' panicked at crates/ruff_db/src/diagnostic/render/gitlab.rs:173:14:
Could not diff paths
stack backtrace:
   0:     0x5a07a266f993 - <unknown>
   1:     0x5a07a232c153 - <unknown>
   2:     0x5a07a266bc83 - <unknown>
   3:     0x5a07a266f7f2 - <unknown>
   4:     0x5a07a267136f - <unknown>
   5:     0x5a07a2670eee - <unknown>
   6:     0x5a07a2671bce - <unknown>
   7:     0x5a07a26718aa - <unknown>
   8:     0x5a07a266fe69 - <unknown>
   9:     0x5a07a267155d - <unknown>
  10:     0x5a07a2329880 - <unknown>
  11:     0x5a07a232985b - <unknown>
  12:     0x5a07a2511623 - <unknown>
  13:     0x5a07a25766d2 - <unknown>
  14:     0x5a07a232c153 - <unknown>
  15:     0x5a07a2741bb4 - <unknown>
  16:     0x5a07a273fad6 - <unknown>
  17:     0x5a07a274b5ac - <unknown>
  18:     0x5a07a274bea3 - <unknown>
  19:     0x5a07a274be89 - <unknown>
  20:     0x5a07a2660531 - <unknown>
  21:     0x5a07a274bb35 - <unknown>
  22:     0x7dc9a881924a - <unknown>
  23:     0x7dc9a8819305 - __libc_start_main
  24:     0x5a07a224b2e9 - <unknown>
  25:                0x0 - <unknown>

Cleaning up project directory and file based variables
00:01
ERROR: Job failed: exit code 1
```

Full output: https://gitlab.com/burrscurr/ty-output-format-gitlab/-/jobs/11462253464

Gitlab CI Job config:

```yml
ty:
  stage: build
  script:
    - RUST_BACKTRACE=full uv run ty check --output-format=gitlab --exit-zero
```

For the full setup, see this repo where I reproduced the panic: https://gitlab.com/burrscurr/ty-output-format-gitlab

From preliminary research, this crash

 - seems to occur only in CI environment, `uv run ty check --output-format gitlab` works as expected in locally
 - does not occur with ruff (`ruff check --output-format gitlab` works fine in CI)
 - seems to occur with common Gitlab Runner configurations (I also reproduced this with a private Gitlab instance)
 - occurs independently from the specific `ty` diagnostic

Pinging @ntBre since I think you worked on this recently.

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Comment by @burrscurr on 2025-09-23 18:18_

(By the way, thank you for the `--output-format gitlab` option! I'm looking forward to actually integrating this into our Gitlab CI setup!)

---

_Assigned to @ntBre by @ntBre on 2025-09-23 19:42_

---

_Comment by @ntBre on 2025-09-23 19:43_

Interesting, thanks for the report and the ping! I can reproduce this with my test gitlab repo too.

It looks like the filenames in ty are already relative to the current directory, which explains why this isn't an issue for Ruff. I added a second call with `--output-format github`, which shows the path passed to [`relativize_path_to`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_db/src/diagnostic/render/gitlab.rs#L169-L172):

```
::error title=ty (invalid-assignment),file=main.py,line=3,col=1,endLine=3,endColumn=2::main.py:3:1: invalid-assignment: Object of type `Literal["1"]` is not
```

In Ruff, the first `file=...` is the full path, while the `main.py:3:1` filename should be relative to the current directory. In ty, we're passing an already-relative path to a path relativizing function, which is causing the error.

A quick fix would be to fall back on the original path if this call fails instead of panicking, but removing `relativize_path_to` has other [benefits](https://github.com/astral-sh/ruff/issues/20119), so I'd kind of lean towards that if possible. 

@MichaReiser what do you think? I meant to follow up on this when you got back from PTO :)

---

_Comment by @ntBre on 2025-09-23 19:51_

This is probably a bad idea in general, but we only do this when the `CI_PROJECT_DIR` variable is set, so if you unset it, you can get CI to run, as a temporary workaround. Alternatively, you can also reproduce this locally by setting the variable too:

```shell
> CI_PROJECT_DIR=/tmp uvx ty check try.py --output-format=gitlab
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files
thread 'main' panicked at crates/ruff_db/src/diagnostic/render/gitlab.rs:173:14:
Could not diff paths
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

That's a default CI variable on GitLab runners, which is why this only happens there.

---

_Label `bug` added by @ntBre on 2025-09-23 19:52_

---

_Comment by @MichaReiser on 2025-09-24 08:03_

It's not clear to me why `FileResolver.path` returns a relative path (it's only called by `UnifiedFile.path`) when we also have `UnifiedFile.relative_path`. 

I think the fix here is to make `path` return the absolute path and review all call sites whether they need to be changed to use `relative_path`.

---

_Closed by @ntBre on 2025-09-24 17:16_

---
