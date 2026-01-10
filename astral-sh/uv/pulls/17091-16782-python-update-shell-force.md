```yaml
number: 17091
title: 16782 python update shell force
type: pull_request
state: open
author: F4RAN
labels: []
assignees: []
base: main
head: 16782-python-update-shell-force
created_at: 2025-12-11T19:32:06Z
updated_at: 2026-01-07T10:01:19Z
url: https://github.com/astral-sh/uv/pull/17091
synced_at: 2026-01-10T05:49:14Z
```

# 16782 python update shell force

---

_Pull request opened by @F4RAN on 2025-12-11 19:32_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds a `--force` flag to `uv python update-shell` and `uv tool update-shell` to allow prepending the executable directory to PATH even when it's already present. This fixes cases where binaries are shadowed by earlier PATH entries.

When `--force` is used, the command removes any existing entry and prepends it again, ensuring first priority. Works on both Unix/macOS (shell config files) and Windows (registry).

## Test Plan

- Added integration tests for normal, skip, and force cases
- Manually tested on macOS with both commands
- All existing tests pass
- Windows support will be tested automatically in CI

Fixes #16782


---

_@chatgpt-codex-connector[bot] reviewed on 2025-12-11 19:35_


### 💡 Codex Review

Here are some automated review suggestions for this pull request.
    

<details> <summary>ℹ️ About Codex in GitHub</summary>
<br/>

[Your team has set up Codex to review pull requests in this repo](http://chatgpt.com/codex/settings/general). Reviews are triggered when you
- Open a pull request for review
- Mark a draft as ready
- Comment "@codex review".

If Codex has suggestions, it will comment; otherwise it will react with 👍.




Codex can also answer questions or update the PR. Try commenting "@codex address that feedback".
            
</details>

---

_Review comment by @chatgpt-codex-connector[bot] on `crates/uv/src/commands/python/update_shell.rs`:47 on 2025-12-11 19:35_

**<sub><sub>![P0 Badge](https://img.shields.io/badge/P0-red?style=flat)</sub></sub>  Stop calling private Windows PATH helpers across crates**

The Windows force branch calls `uv_shell::windows::get_windows_path_var` (and then `prepend_to_path`/`apply_windows_path_var`), but those helpers are not `pub` in `crates/uv-shell/src/windows.rs`. Because this module lives in a different crate, these references fail to compile (E0603), so `uv python update-shell` and the analogous tool command cannot build until the helpers are exported or this code is reworked.

Useful? React with 👍 / 👎.

---

_Review comment by @chatgpt-codex-connector[bot] on `crates/uv/src/commands/python/update_shell.rs`:33 on 2025-12-11 19:35_

**<sub><sub>![P1 Badge](https://img.shields.io/badge/P1-orange?style=flat)</sub></sub>  Avoid mutating PATH while checking for existing entry**

On Windows the presence check `let is_in_path = !uv_shell::windows::prepend_path(&executable_directory)?;` invokes `prepend_path`, which actually writes to the registry when the entry is missing. As a result, when the executable directory was not yet on PATH, the first call already inserts it, and the later `!is_in_path` branch calls `prepend_path` again, which returns false and skips the “Updated PATH…” messaging. Users on Windows now get a silent PATH modification when the directory was absent (same pattern in the tool command). The check should avoid mutating state or only prepend once so the user-facing output remains accurate.

Useful? React with 👍 / 👎.

---

_Review comment by @EliteTK on `crates/uv-shell/src/windows.rs`:105 on 2025-12-19 14:28_

I'm personally not sure it's worth removing the old entry unless windows doesn't like that for some reason.

---

_Review comment by @EliteTK on `crates/uv/src/lib.rs`:1556 on 2025-12-19 16:48_

We don't normally pass the `*Args` to the function directly. Just pass `args.force` directly for consistency.

---

_Review comment by @EliteTK on `crates/uv/src/lib.rs`:1750 on 2025-12-19 16:49_

Ditto

---

_Review comment by @EliteTK on `crates/uv/tests/it/python_update_shell.rs`:12 on 2025-12-19 16:57_

I think probably it would be a little bit cleaner to just have a conditional `#[test]` for windows and non-windows.

Or maybe even a module for windows and one for not windows, since this pattern repeats in the other tests. Although we don't do that elsewhere.

I see `export::reduce_ssh_key_file_permissions` does it like _this_ though. I'll let someone else chip in an opinion here.

---

_Review comment by @EliteTK on `crates/uv/tests/it/python_update_shell.rs`:166 on 2025-12-19 17:17_

Since you need to restart cmd/powershell/windows terminal for this to take effect, wouldn't this still succeed without `--force`?

If we're just testing that the output on stderr indicates something happened, we probably don't need the initial call above in that case?

Maybe it is worth actually reading/writing the registry?

---

_Review comment by @EliteTK on `crates/uv/tests/it/python_update_shell.rs`:166 on 2025-12-19 17:19_

Also, if these tests write to the registry, and we were to check the result, I imagine we would need to run the tests serially...?

---

_Review comment by @EliteTK on `crates/uv/tests/it/main.rs`:105 on 2025-12-19 17:24_

This just tests `python_update_shell` which is only for managed pythons so should probably be "python-managed".

---

_Review comment by @EliteTK on `crates/uv/tests/it/main.rs`:137 on 2025-12-19 17:25_

Given we currently seem to have largely duplicate code for both `python update-shell` and `tool update-shell` we should probably still test it for whatever that's worth.

Although it seems a bit excessive to have that code duplicated at this point. But that can probably be address separately to this PR.

---

_Review comment by @EliteTK on `crates/uv/tests/it/python_update_shell.rs`:166 on 2026-01-06 15:44_

I think maybe an okay approach is to just use windows command line tools to set/check the registry in the tests...

---

_@EliteTK reviewed on 2026-01-06 15:45_

---

_Review requested from @EliteTK by @EliteTK on 2026-01-06 15:46_

---

_@F4RAN reviewed on 2026-01-07 04:45_

---

_Review comment by @F4RAN on `crates/uv-shell/src/windows.rs`:105 on 2026-01-07 04:45_

Hello @EliteTK,
Thanks for reviewing.

Removing the old entry is intentional. The purpose of `--force` is to move the path to the **front** of PATH (first priority), not to create duplicates. If we just prepend without removing, the path would appear twice (once in the middle, once at the front), which is redundant and doesn't solve the shadowing issue described in #16782.

This matches the Unix implementation, which also removes the old entry before re-adding it at the front.

I'm able to change it if you prefer.

---

_@F4RAN reviewed on 2026-01-07 04:55_

---

_Review comment by @F4RAN on `crates/uv/src/lib.rs`:1556 on 2026-01-07 04:55_

You're right 👍 
Fixed

---

_@F4RAN reviewed on 2026-01-07 04:56_

---

_Review comment by @F4RAN on `crates/uv/src/lib.rs`:1750 on 2026-01-07 04:56_

Done.

---

_@F4RAN reviewed on 2026-01-07 04:56_

---

_Review comment by @F4RAN on `crates/uv/tests/it/python_update_shell.rs`:12 on 2026-01-07 04:56_

Done.

---

_@F4RAN reviewed on 2026-01-07 04:56_

---

_Review comment by @F4RAN on `crates/uv/tests/it/python_update_shell.rs`:166 on 2026-01-07 04:56_

Done.

---

_@F4RAN reviewed on 2026-01-07 04:56_

---

_Review comment by @F4RAN on `crates/uv/tests/it/main.rs`:105 on 2026-01-07 04:56_

Done.

---

_@F4RAN reviewed on 2026-01-07 04:56_

---

_Review comment by @F4RAN on `crates/uv/tests/it/main.rs`:137 on 2026-01-07 04:56_

Done.

---

_@EliteTK reviewed on 2026-01-07 10:01_

---

_Review comment by @EliteTK on `crates/uv-shell/src/windows.rs`:105 on 2026-01-07 10:01_

> The purpose of `--force` is to move the path to the front of PATH (first priority), not to create duplicates.

I mean that duplicates won't matter. At least on *nix the paths are scanned sequentially and the first match wins. You won't get shadowing even if the same path appears a second time after another path which would shadow it.

With the approach we use on *nix we are already potentially creating duplicate entries by unconditionally pre-pending.

But, having given it a bit more time, I can sort of see it from the other perspective - someone using this tooling probably isn't interested in managing these variables themselves so minimising mess is probably a fair idea.

Leave it for now... Someone else will chip in if they have a better idea.

---
