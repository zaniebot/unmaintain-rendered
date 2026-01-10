```yaml
number: 11386
title: Detect infinite recursion in uv run.
type: pull_request
state: merged
author: ssanderson
labels: []
assignees: []
merged: true
base: main
head: check-recursion-depth
created_at: 2025-02-10T14:47:21Z
updated_at: 2025-02-12T18:58:43Z
url: https://github.com/astral-sh/uv/pull/11386
synced_at: 2026-01-10T11:10:37Z
```

# Detect infinite recursion in uv run.

---

_Pull request opened by @ssanderson on 2025-02-10 14:47_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Handle potential infinite recursion if `uv run` recursively invokes `uv run`. This can happen if the shebang line of a script includes `uv run`, but does not pass `--script`.

Handled by adding a new environment variable `UV_RUN_RECURSION_DEPTH`, which contains a counter of the number of times that uv run has been recursively invoked. If unset, it defaults to zero, and each time uv run starts a subprocess we increment the counter, erroring if the value is greater than a configurable (but not currently exposed or documented) threshold.

Closes https://github.com/astral-sh/uv/issues/11220.

## Test Plan

I've added a snapshot test to `uv/crates/uv/tests/it/run` that tests the end-to-end recursion detection flow. I've currently made it a unix-only test because I'm not sure offhand how uv run will interact with shebang lines on windows.


---

_@ssanderson reviewed on 2025-02-10 14:49_

---

_Review comment by @ssanderson on `crates/uv-cli/src/lib.rs`:2938 on 2025-02-10 14:49_

I marked both of these as hidden b/c they didn't seem useful enough to users to warrant polluting the `uv run` output. I think the `RECURSION_DEPTH` argument at least should probably stay hidden. I could see an argument for `max_recursion_depth` being made visible in case someone is implementing some weird recursive algorithm with uv subprocesses and legitimately needs to raise the limit, but that seems pretty unlikely to me as well.

---

_@ssanderson reviewed on 2025-02-10 14:50_

---

_Review comment by @ssanderson on `crates/uv-static/src/env_vars.rs`:618 on 2025-02-10 14:50_

This is implemented mostly be squinting at what already exists and trying to follow the existing pattern. Happy to adjust as needed.

---

_@ssanderson reviewed on 2025-02-10 14:50_

---

_Review comment by @ssanderson on `crates/uv/src/commands/project/run.rs`:97 on 2025-02-10 14:50_

This check could be pushed up higher in the call stack, but it seemed nice to have the logic for the check and the logic for setting/incrementing the counter in the same place.

---

_@ssanderson reviewed on 2025-02-10 15:08_

---

_Review comment by @ssanderson on `crates/uv/src/settings.rs`:310 on 2025-02-10 15:08_

At a depth of 100 this takes about 2.5 seconds to error on a relatively beefy laptop. The time to detection scales more or less linearly with this value, so I think the way to set this is basically to pick something large enough to be unlikely to cause problems, but low enough that we actually detect recursion in enough time to be useful.

The 2.5s number above is from running in debug, so if a target error time of ~2s sounds ok, then this can probably be bumped up to 500-1000 in release.

---

_Assigned to @zanieb by @zanieb on 2025-02-10 17:55_

---

_@zanieb reviewed on 2025-02-10 17:56_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2938 on 2025-02-10 17:56_

I think it'd make sense to read this directly without using Clap / adding a flag to the CLI.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2943 on 2025-02-10 17:57_

I can see a case for adding this to the CLI but... we could also just read it directly. Since you already did this, it seems okay to leave it (as a hidden option).

---

_@zanieb reviewed on 2025-02-10 17:57_

---

_Comment by @zanieb on 2025-02-10 17:58_

Thanks for contributing! This looks pretty good to me. 

It's fine to gate the test case to Unix.

---

_Review comment by @ssanderson on `crates/uv-cli/src/lib.rs`:2938 on 2025-02-10 19:44_

Pushed up an update to move this to no longer use clap. The main downside of this is we have to do a bit more manual parsing/error handling, but I think this makes sense if you want to think of this as an implementation detail rather than part of the uv run interface.

---

_@ssanderson reviewed on 2025-02-10 19:44_

---

_@ssanderson reviewed on 2025-02-10 19:46_

---

_Review comment by @ssanderson on `crates/uv-cli/src/lib.rs`:2943 on 2025-02-10 19:46_

Yeah, I made this configurable primarily to support testing (without this being configurable the test for this takes 2-3 seconds in debug), but this seemed like something that a user could theoretically want to control, and it isn't too hard to plumb this to where it needs to go.

---

_@zanieb approved on 2025-02-12 13:43_

I made some small changes. Thank you!

---

_Merged by @zanieb on 2025-02-12 18:58_

---

_Closed by @zanieb on 2025-02-12 18:58_

---
