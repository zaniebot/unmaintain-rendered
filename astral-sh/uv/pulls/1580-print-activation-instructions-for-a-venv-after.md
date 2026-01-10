```yaml
number: 1580
title: Print activation instructions for a venv after one has been created
type: pull_request
state: merged
author: 0v00
labels: []
assignees: []
merged: true
base: main
head: activation-prompt
created_at: 2024-02-17T10:03:57Z
updated_at: 2024-02-20T12:39:42Z
url: https://github.com/astral-sh/uv/pull/1580
synced_at: 2026-01-10T15:33:24Z
```

# Print activation instructions for a venv after one has been created

---

_Pull request opened by @0v00 on 2024-02-17 10:03_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

- Add prompts for users to activate the `.venv` after it has been created.
- Closes [https://github.com/astral-sh/uv/issues/1326](https://github.com/astral-sh/uv/issues/1326)

If the platform os matches `Os::Windows` then we provide the following prompt:
```rust
writeln!(
   printer,
   "Activate with .\\{}\\Scripts\\activate.ps1",
   path.normalized_display().cyan()
)
```

Otherwise we provide the following prompt:
```rust
writeln!(
  printer,
  "Activate with source {}/bin/activate",
  path.normalized_display().cyan()
)
```

I've added additional filters in `crates/uv/tests/venv.rs` so there shouldn't be issues on any of the CI runs (not sure what some of the Windows paths could look like, so these filters might need to be changed):
```rust
        (
            r".*?\\\.venv\\Scripts\\activate\.ps1", 
            "Activate with source /home/ferris/project/.venv/bin/activate"
        )
```

## Test Plan

<!-- How was it tested? -->

- Ran tests in `crates/uv/tests/venv.rs` and updated snapshots.
- Ran `uv venv` in a Docker container to test that the output is correct (running Ubuntu)

---

_Review comment by @T-256 on `crates/uv/src/commands/venv.rs`:193 on 2024-02-17 10:29_

what about cmd users? 
- https://github.com/astral-sh/uv/pull/1523

---

_Review comment by @T-256 on `crates/uv/src/commands/venv.rs`:203 on 2024-02-17 10:29_

```suggestion
                "Activate with: source {}/bin/activate",
```
or put command in quotations for improve readability.

---

_@T-256 reviewed on 2024-02-17 10:29_

---

_@zanieb reviewed on 2024-02-17 16:54_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:193 on 2024-02-17 16:54_

Note we usually use a `cfg` for this, e.g. (from elsewhere)

```rust
    let bin_name = if cfg!(unix) {
        "bin"
    } else if cfg!(windows) {
        "Scripts"
    } else {
        unimplemented!("Only Windows and Unix are supported")
    };
```

---

_@zanieb reviewed on 2024-02-17 16:54_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:193 on 2024-02-17 16:54_

I wonder if we can detect the current terminal type in Windows...

---

_@zanieb reviewed on 2024-02-17 16:54_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:193 on 2024-02-17 16:54_

(or if we should just show both?)

---

_@0v00 reviewed on 2024-02-18 02:43_

---

_Review comment by @0v00 on `crates/uv/src/commands/venv.rs`:193 on 2024-02-18 02:43_

I'm also not sure if we can correctly/reliably detect the current terminal type in Windows, I've added in the below prompt:

```bash
Activate with:
- Powershell: .\.venv\Scripts\activate.ps1
- CMD: .\.venv\Scripts\activate.bat
```

and updated filters and snapshots

---

_@0v00 reviewed on 2024-02-18 18:48_

---

_Review comment by @0v00 on `crates/uv/src/commands/venv.rs`:203 on 2024-02-18 18:48_

Good suggestion, have added this in.

---

_@T-256 reviewed on 2024-02-18 18:55_

---

_Review comment by @T-256 on `crates/uv/src/commands/venv.rs`:196 on 2024-02-18 18:55_

I usually don't write `.\` when I'm in CMD environment.

---

_@0v00 reviewed on 2024-02-18 19:27_

---

_Review comment by @0v00 on `crates/uv/src/commands/venv.rs`:196 on 2024-02-18 19:27_

removed `.\` from the prompt

---

_@T-256 approved on 2024-02-18 19:32_

---

_Review requested from @zanieb by @0v00 on 2024-02-19 01:42_

---

_@zanieb reviewed on 2024-02-19 17:24_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:193 on 2024-02-19 17:24_

@AlexWaygood / @MichaReiser can you take over review of this?

---

_@AlexWaygood reviewed on 2024-02-19 17:29_

---

_Review comment by @AlexWaygood on `crates/uv/src/commands/venv.rs`:193 on 2024-02-19 17:29_

I believe typing `.venv\Scripts\activate` (omitting the file extension) _should_ actually work on both PowerShell and CMD. (It does on my Windows machine!) If we just suggested doing that, it woudl avoid us having to figure out how to detect which Windows shell the user is using (and it's also fewer characters to have to type out ;).

If you're on CMD, it'll naturally pick up the `.bat` file; if you're on PowerShell, it'll naturally pick up the `.ps1` file

---

_@0v00 reviewed on 2024-02-20 03:38_

---

_Review comment by @0v00 on `crates/uv/src/commands/venv.rs`:193 on 2024-02-20 03:38_

Thank you, that definitely helps make the prompt a lot cleaner/simpler. Have updated it.

---

_Review comment by @AlexWaygood on `crates/uv/src/commands/venv.rs`:200 on 2024-02-20 08:18_

Possibly a comment here wouldn't go amiss:

```suggestion
            printer,
            // This should work whether the user is on CMD or PowerShell:
```

---

_@AlexWaygood approved on 2024-02-20 08:19_

LGTM, thanks!

---

_@0v00 reviewed on 2024-02-20 08:44_

---

_Review comment by @0v00 on `crates/uv/src/commands/venv.rs`:200 on 2024-02-20 08:44_

Good idea, added this in

---

_Comment by @AlexWaygood on 2024-02-20 12:39_

I tested locally with zsh on my mac, CMD on my Windows PC, and PowerShell on my Windows PC. All looked good!

---

_Renamed from "Prompt activation after uv venv has successfully run" to "Print activation instructions for a venv after one has been created" by @AlexWaygood on 2024-02-20 12:39_

---

_Merged by @AlexWaygood on 2024-02-20 12:39_

---

_Closed by @AlexWaygood on 2024-02-20 12:39_

---
