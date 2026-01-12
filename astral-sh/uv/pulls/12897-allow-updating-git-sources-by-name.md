```yaml
number: 12897
title: Allow updating Git sources by name
type: pull_request
state: merged
author: maxmynter
labels:
  - enhancement
assignees: []
merged: true
base: main
head: fix-git-ref-with-existing-source
created_at: 2025-04-15T12:57:35Z
updated_at: 2025-04-21T02:58:38Z
url: https://github.com/astral-sh/uv/pull/12897
synced_at: 2026-01-12T16:10:26Z
```

# Allow updating Git sources by name

---

_@maxmynter_


<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Closes: #4567

## Summary
When adding a package with Git reference options (`--rev`, `--tag`, `--branch`) that already has a Git source defined, use the existing Git URL with the new reference instead of reporting an error.

This allows commands like `uv add requests --branch main` to work when requests is already defined with a Git source in the project configuration. 

Previously, you would need to provide the whole Git url again for this to work:
```bash
uv add git+https://github.com/psf/requests --branch main
```
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
- [x] Add unit tests for project
- [x] Add unit tests for script
- [x] Tested locally for project and script environments like below

### Testing Project
In a directory using the `uv` executable from this PR (via replacing every `uv` with `cargo run --`) initialize a project and virtual environment
```bash
uv init
uv venv
```
move into the environment 
```bash
# on mac
source .venv/bin/activate
```
and add a dependency with a git url
```bash
uv add git+https://github.com/Textualize/rich --branch master
```
Then change the branch of the project to see that the branch can be changed without need of the whole git url:
```bash
uv add rich --branch py310
```

### Testing Script
Create the following file, e.g. `script.py`:
```python
import time
from rich.progress import track

print("Starting")
for i in track(range(20), description="For example:"):
    time.sleep(0.05)
print("Done")
```
Now using `uv` (referencing the executable of this PR) add the dependency
```bash
uv add --script script.py 'git+https://github.com/Textualize/rich' --branch master
```
and check we can execute the script:
```bash
uv run script.py
```
To test the change update the branch

```bash
uv add --script script.py rich --branch py310
```
and check that the dependency is updated and the script is executed:
```bash
uv run script.py
```

<!-- How was it tested? -->

----
This is my first time contributing to `uv` (yay, 🤗) so let me know if there is something obvious i am missing.
Unit tests will follow soon. 

---

_Comment by @konstin on 2025-04-15 13:14_

Looks good! To add an integration test for this behavior, you can add a test to `crates/uv/tests/it/edit.rs`, the file contains other `uv add` tests which should be similar.

---

_Label `enhancement` added by @konstin on 2025-04-15 13:14_

---

_Renamed from "Fallback to Git URL if it exists but Non-Git source with git reference option is provided" to "Allow updating Git sources by name" by @konstin on 2025-04-15 13:14_

---

_Comment by @maxmynter on 2025-04-16 18:47_

@konstin Added intergration tests for project and script using Git `--rev`, `--branch`, and `--tag`. Let me know if something is missing.

----
Edit: The CI failed with a timeout on a test that seems unrelated (i may be wrong); i seem to not have permissions to rerun CI and will refrain from pushing further empty retrigger commits. 

The tests that failed on [b8a15a1](https://github.com/astral-sh/uv/pull/12897/commits/b8a15a1d23fdb164e216363d1dcda1f0f1038d6e) and [62e1357](https://github.com/astral-sh/uv/pull/12897/commits/62e13576f6b345aa9a873cbde602747bf3247676) are different despite unchanged code.

---

_Assigned to @konstin by @zanieb on 2025-04-16 18:48_

---

_@charliermarsh approved on 2025-04-21 02:05_

This looks great -- thanks!

---

_Merged by @charliermarsh on 2025-04-21 02:58_

---

_Closed by @charliermarsh on 2025-04-21 02:58_

---
