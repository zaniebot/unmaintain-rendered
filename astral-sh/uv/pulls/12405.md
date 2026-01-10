```yaml
number: 12405
title: Add JSON Output for uv sync --dry-run
type: pull_request
state: closed
author: x0rw
labels: []
assignees: []
base: main
head: main
created_at: 2025-03-23T23:20:07Z
updated_at: 2025-11-03T11:25:18Z
url: https://github.com/astral-sh/uv/pull/12405
synced_at: 2026-01-10T06:28:12Z
```

# Add JSON Output for uv sync --dry-run

---

_Pull request opened by @x0rw on 2025-03-23 23:20_

UPDATE:
## Summary
This PR adds support for the --format=json flag to the uv sync --dry-run command. When enabled, the command outputs details in JSON format, making it easier to parse and integrate with other tools.

the dry run output json includes:
- [x] Project directory.
- [x] Environment path & action taken(Create/Update/None).
- [x] Python executable path.
- [x] Python version.
- [x] Lockfile path & action. 
- [ ] Packages (would install/download/remove).

The Json output is currently pretty-printed for readability.

## Test Plan
Manual testing has been performed to verify JSON output.
### --script
`cargo run sync --dry-run --script foo.py --format=json `
output:
```
{
  "project_dir": "/home/root/rust/uv/uv",
  "environment": {
    "path": "/home/root/.cache/uv/environments-v2/foo-bc19cb0acffe1e16",
    "action": "already_exist",
    "python_executable": "/home/root/.cache/uv/environments-v2/foo-bc19cb0acffe1e16/bin/python3",
    "python_version": "3.13.0"
  },
  "lockfile": null
}
```
### --project
`cargo run sync --dry-run --project test_dir --format=json`
output:
```
{
  "project_dir": "/home/root/rust/uv/uv/test_dir",
  "environment": {
    "path": "/home/root/rust/uv/uv/test_dir/.venv",
    "action": "already_exist",
    "python_executable": "/home/root/rust/uv/uv/test_dir/.venv/bin/python3",
    "python_version": "3.13.0"
  },
  "lockfile": {
    "lockfile_path": "/home/root/rust/uv/uv/test_dir/uv.lock",
    "action": "create"
  }
}

```

**Note:** `2>/dev/null ` can be appended to the end of the command to redirect stderr away and show only the stdout(which contains the json only).
Related Issue
Resolves #12387.

---

_Converted to draft by @x0rw on 2025-03-23 23:21_

---

_@InSyncWithFoo reviewed on 2025-03-24 05:00_

---

_Review comment by @InSyncWithFoo on `crates/uv-cli/src/lib.rs`:55 on 2025-03-24 05:00_

I don't think any other Ruff or uv commands have `PrettyJson` as a format. It's just unnecessary.

(Small note: JSON is by definition a data-interchange format intended to be machine-readable, so "a machine-readable JSON format" can be replaced with just "JSON".)

---

_@x0rw reviewed on 2025-03-24 05:07_

---

_Review comment by @x0rw on `crates/uv-cli/src/lib.rs`:55 on 2025-03-24 05:07_

Reasonable, the idea behind this is that while testing i tried to serialize a whole struct(i guess `Environment`, i'm on my phone can't check it now), and it all crammed into one line so i thought about this to make it more readable for testing, i will change it and add tests as well
Do you think the default json format should be the pretty one? 

---

_@InSyncWithFoo reviewed on 2025-03-24 05:19_

---

_Review comment by @InSyncWithFoo on `crates/uv-cli/src/lib.rs`:55 on 2025-03-24 05:19_

I don't; considering its purpose, there's not much of a point in making it slightly more human-readable. Other commands like `uv pip list` output minified JSON as well.

But then again, `ruff check` does prettify its JSON output. No strong feelings from me on this one.

---

_@x0rw reviewed on 2025-03-24 13:27_

---

_Review comment by @x0rw on `crates/uv-cli/src/lib.rs`:55 on 2025-03-24 13:27_

Addressed in the latest commit, let me know if any further changes are needed

---

_Assigned to @zanieb by @zanieb on 2025-03-25 02:22_

---

_Assigned to @Gankra by @zanieb on 2025-03-25 02:22_

---

_Comment by @x0rw on 2025-03-25 02:35_

I can't fix the failling test for windows atm, the issue arises from filtering directories on windows in the test(`sync_dry_run_format`), a custom filter or writing another test for windows will solve it

---

_Marked ready for review by @x0rw on 2025-03-26 01:30_

---

_Review requested from @InSyncWithFoo by @x0rw on 2025-03-28 02:08_

---

_Comment by @x0rw on 2025-04-09 22:35_

Hey @zanieb , this PR is almost complete, with just a small part left to finish. Could you please review it? If a rebase is needed, let me know.


---

_Review request for @InSyncWithFoo removed by @Gankra on 2025-04-14 19:40_

---

_Review requested from @Gankra by @Gankra on 2025-04-14 19:40_

---

_Comment by @Gankra on 2025-05-27 20:10_

I've rebased and continued this impl over here:

* https://github.com/astral-sh/uv/pull/13689

---

_Comment by @zanieb on 2025-07-14 16:43_

Added in https://github.com/astral-sh/uv/pull/13689

---

_Closed by @zanieb on 2025-07-14 16:43_

---
