```yaml
number: 4997
title: "`uv tool run` suggest valid commands when command is not found"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
  - cli
  - preview
assignees: []
merged: true
base: main
head: uvx-list-commands
created_at: 2024-07-11T19:20:19Z
updated_at: 2024-07-12T03:31:40Z
url: https://github.com/astral-sh/uv/pull/4997
synced_at: 2026-01-10T13:42:52Z
```

# `uv tool run` suggest valid commands when command is not found

---

_Pull request opened by @blueraft on 2024-07-11 19:20_

## Summary

Resolves #4979.


## Test Plan
`cargo test`


<img width="619" alt="Screenshot 2024-07-11 at 22 45 36" src="https://github.com/user-attachments/assets/62526010-9123-43f5-9f8d-1f9e89f6be59">

<img width="636" alt="Screenshot 2024-07-11 at 22 45 23" src="https://github.com/user-attachments/assets/a348cd73-f891-40b1-8934-afbd1aa19326">




---

_Comment by @T-256 on 2024-07-11 20:15_


> <img alt="Screenshot 2024-07-11 at 21 18 05" width="506" src="https://private-user-images.githubusercontent.com/11220895/347964190-bcc2484d-4813-4405-989a-e08316f053a8.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjA3Mjg3ODksIm5iZiI6MTcyMDcyODQ4OSwicGF0aCI6Ii8xMTIyMDg5NS8zNDc5NjQxOTAtYmNjMjQ4NGQtNDgxMy00NDA1LTk4OWEtZTA4MzE2ZjA1M2E4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA3MTElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwNzExVDIwMDgwOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTc0YzE2NDdhMTA1NTQwMjA4NDBkMWRkYzA4MzM0M2QzZWE2NWQ0NzAyOTgyMTdhZDM3ZjUxOTQ0OTdiNzJhMDgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.cAhRWa_8yEu3LG31k9h__-UmBeaVaEqpxgTK6O6cVWI">
As new user I expect to hint about `--from` when not used:
```
However, the following executables are available via `uvx --from fastapi-cli <EXECUTABLE>`:
    - fastapi.exe
```


---

_Comment by @blueraft on 2024-07-11 20:44_

> > <img alt="Screenshot 2024-07-11 at 21 18 05" width="506" src="https://private-user-images.githubusercontent.com/11220895/347964190-bcc2484d-4813-4405-989a-e08316f053a8.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjA3Mjg3ODksIm5iZiI6MTcyMDcyODQ4OSwicGF0aCI6Ii8xMTIyMDg5NS8zNDc5NjQxOTAtYmNjMjQ4NGQtNDgxMy00NDA1LTk4OWEtZTA4MzE2ZjA1M2E4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA3MTElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwNzExVDIwMDgwOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTc0YzE2NDdhMTA1NTQwMjA4NDBkMWRkYzA4MzM0M2QzZWE2NWQ0NzAyOTgyMTdhZDM3ZjUxOTQ0OTdiNzJhMDgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.cAhRWa_8yEu3LG31k9h__-UmBeaVaEqpxgTK6O6cVWI">
> 
> As new user I expect to hint about `--from` when not used:
> 
> ```
> However, the following executables are available via `uvx --from fastapi-cli <EXECUTABLE>`:
>     - fastapi.exe
> ```

Thanks! that makes sense, I went with `uv tool run --from cmd <EXECUTABLE>` instead because `uvx` is just an alias.

---

_@T-256 reviewed on 2024-07-11 21:01_

---

_Review comment by @T-256 on `crates/uv/src/commands/tool/run.rs`:157 on 2024-07-11 21:01_

Could it be ignored when user already used `--from`? I think he don't need to know about this hint. So IMO list of available executables is enough for him.
```rust
                if !entry_points.is_empty() {
                    let from_option_hint = if from == None { " via `uv tool run --from {from} <EXECUTABLE>`" } else { "" }
                    writeln!(
                        printer.stdout(),
                        "However, the following executables are available{from_option_hint}:",
                    )?;
                }
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-12 00:56_

---

_Label `enhancement` added by @charliermarsh on 2024-07-12 00:56_

---

_Label `cli` added by @charliermarsh on 2024-07-12 00:56_

---

_Label `preview` added by @charliermarsh on 2024-07-12 00:56_

---

_@charliermarsh approved on 2024-07-12 01:44_

---

_Comment by @charliermarsh on 2024-07-12 01:45_

Nice, thanks!

---

_Merged by @charliermarsh on 2024-07-12 02:26_

---

_Closed by @charliermarsh on 2024-07-12 02:26_

---

_@j178 reviewed on 2024-07-12 03:28_

---

_Review comment by @j178 on `crates/uv/src/commands/tool/run.rs`:144 on 2024-07-12 03:28_

Are executables from sub-depencencies considered as *available*? For instance, `uv tool run --from fastapi`, `uvicorn` is also an available executable, but it is not listed here.

---

_@zanieb reviewed on 2024-07-12 03:31_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:144 on 2024-07-12 03:31_

I think we'll want a separate flag to opt-in to that, per https://github.com/astral-sh/uv/issues/4994

Maybe we should even warn or error if someone uses an executable that's not provided by the `--from` package?

---
