```yaml
number: 11623
title: Return error when running uvx with a .py script
type: pull_request
state: merged
author: jtfmumm
labels: []
assignees: []
merged: true
base: main
head: jtfm/uvx-run-hint
created_at: 2025-02-19T15:32:02Z
updated_at: 2025-03-04T14:29:38Z
url: https://github.com/astral-sh/uv/pull/11623
synced_at: 2026-01-10T11:10:38Z
```

# Return error when running uvx with a .py script

---

_Pull request opened by @jtfmumm on 2025-02-19 15:32_

If we see `uvx script.py`, we exit early, giving a hint to use `uv run script.py` if the script exists. If it does not exist, we suggest running `uv run` with a normalized package name.

This PR includes a snapshot test for each of these scenarios.

An alternative approach would be to wait until we encounter an error, and then add the hint. But if there happens to be a malicious package called `script-py`, this would be run unintentionally (a point raised by @zanieb).
 
Closes #10784 


---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:111 on 2025-02-19 15:38_

We probably need to support `pyw` here too

---

_@zanieb reviewed on 2025-02-19 15:38_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:115 on 2025-02-19 15:39_

We usually do "Did you mean", I think.

---

_@zanieb reviewed on 2025-02-19 15:39_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:117 on 2025-02-19 15:40_

Often, we just do a sub-format, e.g.

```
format!("uv run {}", possible_path...).cyan()
```

---

_@zanieb reviewed on 2025-02-19 15:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:113 on 2025-02-19 15:42_

Should we error if we can't read the path? (earnestly not sure here)

---

_@zanieb reviewed on 2025-02-19 15:42_

---

_@zanieb reviewed on 2025-02-19 15:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:121 on 2025-02-19 15:43_

It seems nice to include the script name, maybe like

> It looks like you're trying to run a script (`{}`) but it does not exist.

Not in love with that, but don't have a better idea.

---

_@zanieb reviewed on 2025-02-19 15:45_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:121 on 2025-02-19 15:45_

In the "Did you mean" message, should we clarify what `uvx <name>` would do? Like "Did you mean to run a tool with `uvx {}`"?

I'm not sure if I prefer it, but wanted to put it out there.

---

_@zanieb reviewed on 2025-02-19 15:49_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:108 on 2025-02-19 15:49_

If `--from` is provided, e.g, `uvx --from <package> foo.py` we should probably not perform this check as they've opted-in to loading a specific source.

We probably want to have this logic for `uvx --from foo.py <command>` though? We can't give a good suggestion though since we don't support like `uv run --with-requirements foo.py <command>` yet — see https://github.com/astral-sh/uv/issues/6542

---

_Review comment by @jtfmumm on `crates/uv/src/commands/tool/run.rs`:121 on 2025-02-19 15:51_

Yeah, I think adding that context is helpful

---

_@jtfmumm reviewed on 2025-02-19 15:51_

---

_@jtfmumm reviewed on 2025-02-19 16:32_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/tool/run.rs`:108 on 2025-02-19 16:32_

Updated to reflect your suggestion

---

_@jtfmumm reviewed on 2025-02-19 16:51_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/tool/run.rs`:113 on 2025-02-19 16:51_

It's a good question. We could treat that as the "doesn't exist case" if we'd prefer.

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:114 on 2025-02-19 18:13_

Why not use the same suggestion as below? (but with `--from <package>`)

---

_@zanieb reviewed on 2025-02-19 18:13_

---

_@zanieb reviewed on 2025-02-19 18:15_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:114 on 2025-02-19 18:15_

We can't suggest `uv run ...` but we can suggest `uvx --from script-py ...` instead

---

_@zanieb reviewed on 2025-02-19 18:16_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:130 on 2025-02-19 18:16_

Sorry I missed this initially, you need to use `invocation_source` here instead of hard-coding `uvx` because that's an alias for `uv tool run` (and we want to match what the user used)

---

_Assigned to @jtfmumm by @jtfmumm on 2025-02-20 07:58_

---

_Unassigned @jtfmumm by @jtfmumm on 2025-02-20 07:59_

---

_@jtfmumm reviewed on 2025-02-20 08:55_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/tool/run.rs`:130 on 2025-02-20 08:55_

Updated

---

_@jtfmumm reviewed on 2025-02-20 08:56_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/tool/run.rs`:114 on 2025-02-20 08:56_

Updated

---

_Comment by @zanieb on 2025-02-20 20:26_

This looks good to me, thanks for addressing the feedback. I want to do one final pass on the messaging, since this is ostensibly breaking (though I really hope nobody is relying on this behavior). I'll do that on Monday.

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:113 on 2025-03-03 19:14_

We try to avoid single letter variables names

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:120 on 2025-03-03 19:14_

I think we can probably just the original `from` string instead of going from String -> Path -> String.

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:129 on 2025-03-03 19:16_

Did you mean to display the absolute path here? `user_display` is generally preferable, which handles display of paths in the working directory.

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:1907 on 2025-03-03 19:17_

This looks superfluous 

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:120 on 2025-03-03 19:22_

Minor note, rustfmt chokes on macros so you need to format those manually sometimes.

---

_@zanieb reviewed on 2025-03-03 19:45_

Sorry again about the delay here. I pushed a commit (https://github.com/astral-sh/uv/pull/11623/commits/41bbf8cc5e34e13e86a81287cc9b34eaed9fc69e) with some tweaks and left some notes via review for context.

My focus here was being a bit more verbose and consistent with the error messages, but I made some nit-changes at the same time.

---

_Comment by @zanieb on 2025-03-03 19:51_

If you think any of the error messages are worse for some reason please let me know so we can align on the best message

---

_Comment by @jtfmumm on 2025-03-04 13:14_

> If you think any of the error messages are worse for some reason please let me know so we can align on the best message

Your changes look good to me.

---

_Merged by @zanieb on 2025-03-04 14:29_

---

_Closed by @zanieb on 2025-03-04 14:29_

---

_Branch deleted on 2025-03-04 14:29_

---
