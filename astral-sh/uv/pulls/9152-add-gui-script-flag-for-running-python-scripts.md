```yaml
number: 9152
title: Add --gui-script flag for running Python scripts with pythonw.exe on …
type: pull_request
state: merged
author: rajko-rad
labels:
  - windows
assignees: []
merged: true
base: main
head: add-gui-script-flag-6805
created_at: 2024-11-15T14:58:49Z
updated_at: 2024-12-10T20:35:17Z
url: https://github.com/astral-sh/uv/pull/9152
synced_at: 2026-01-12T16:08:40Z
```

# Add --gui-script flag for running Python scripts with pythonw.exe on …

---

_@rajko-rad_

Addresses #6805

## Summary

This PR adds a `--gui-script` flag to `uv run` that allows running Python scripts with `pythonw.exe` on Windows, regardless of file extension. This solves the issue where users need to maintain duplicate `.py` and `.pyw` files to run the same script with and without a console window.

The implementation follows the pattern established by the existing `--script` flag, but uses `pythonw.exe` instead of `python.exe` on Windows. On non-Windows platforms, the flag is present but returns an error indicating it's Windows-only functionality.

Changes:
- Added `--gui-script` flag (Windows-only)
- Added Windows test to verify GUI script behavior
- Added non-Windows test to verify proper error message
- Updated CLI documentation


## Test Plan

The changes are tested through:

1. New Windows-specific test that verifies:
   - Script runs successfully with `pythonw.exe` when using `--gui-script`
   - Console output is suppressed in GUI mode but visible in regular mode
   - Same script can be run both ways without modification

2. New non-Windows test that verifies:
   - Appropriate error message when `--gui-script` is used on non-Windows platforms

3. Documentation updates to clearly indicate Windows-only functionality


---

_Assigned to @zanieb by @zanieb on 2024-11-15 15:00_

---

_Review requested from @zanieb by @konstin on 2024-11-15 15:00_

---

_Comment by @zanieb on 2024-11-15 20:31_

You can add an ignore for that Clippy complaint, if you want — or we will. We generally take our time about splitting those bools out into a struct.

---

_@zanieb reviewed on 2024-11-18 15:42_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:2777 on 2024-11-18 15:42_

Should this be using `--gui-script`?

---

_Comment by @zanieb on 2024-11-18 15:49_

Nice! I made some minor tweaks and have one blocking question.

---

_@samypr100 reviewed on 2024-11-21 00:43_

---

_Review comment by @samypr100 on `crates/uv/tests/it/run.rs`:2777 on 2024-11-21 00:43_

Agreed, but would this pass with exit code 0 on a headless windows CI run?

---

_Label `windows` added by @samypr100 on 2024-11-21 00:50_

---

_Comment by @rajko-rad on 2024-11-21 15:56_

> You can add an ignore for that Clippy complaint, if you want — or we will. We generally take our time about splitting those bools out into a struct.

Perfect thank you!

---

_@rajko-rad reviewed on 2024-11-21 15:57_

---

_Review comment by @rajko-rad on `crates/uv/tests/it/run.rs`:2777 on 2024-11-21 15:57_

you are right, and I'm slightly confused why/how we passed tests with that typo... Changing now and will see how it behaves!

---

_Comment by @rajko-rad on 2024-11-22 20:02_

> Nice! I made some minor tweaks and have one blocking question.

Ok, blocking question addressed! For some reason I am getting a linting error that I think isn't real, see below?
Lint error:
![image](https://github.com/user-attachments/assets/63209259-b371-4e2e-90a5-5a80af541d6f)

Current codebase
![image](https://github.com/user-attachments/assets/75de4579-b38f-4f86-9ae7-d3031d469b34)

There isn't a double space, not sure what is causing the error...

---

_Comment by @zanieb on 2024-11-27 16:00_

The difference is some trailing whitespace

<img width="596" alt="Screenshot 2024-11-27 at 10 00 39 AM" src="https://github.com/user-attachments/assets/b52bc216-df26-406f-b3ab-b949553ef839">


---

_@zanieb approved on 2024-11-27 16:01_

Thank you!

---

_Comment by @zanieb on 2024-12-10 20:35_

Just got super confused that this wasn't merged — not sure what happened there.

---

_Merged by @zanieb on 2024-12-10 20:35_

---

_Closed by @zanieb on 2024-12-10 20:35_

---
