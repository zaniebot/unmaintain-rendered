```yaml
number: 13857
title: "Make `requirements_txt_ssh_git_username` not write into homedir"
type: pull_request
state: merged
author: mgorny
labels:
  - internal
assignees: []
merged: true
base: main
head: ssh-knownhosts
created_at: 2025-06-05T10:06:28Z
updated_at: 2025-06-05T10:55:52Z
url: https://github.com/astral-sh/uv/pull/13857
synced_at: 2026-01-10T11:10:42Z
```

# Make `requirements_txt_ssh_git_username` not write into homedir

---

_Pull request opened by @mgorny on 2025-06-05 10:06_

## Summary

Modify `requirements_txt_ssh_git_username` test to pass `-o UserKnownHostsFile=/dev/null` to OpenSSH, in order to prevent it from trying to write into the known-hosts in user's home directory. To add insult to the injury, OpenSSH ignores `HOME` and writes into the actual home when it is explicitly overridden for the purpose of testing.

## Test Plan

`cargo test --no-default-features --features=git,pypi,python` in an environment where the user can't write to `pw_home` (but can to `${HOME}`). Previously the test would fail:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: requirements_txt_ssh_git_username
Source: crates/uv/tests/it/export.rs:1252
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    7     7 │   ├─▶ failed to clone into: [PATH]
    8     8 │   ├─▶ failed to fetch branch, tag, or commit `d780faf0ac91257d4d5a4f0c5a0e4509608c0071`
    9     9 │   ╰─▶ process didn't exit successfully: [GIT_COMMAND_ERROR]
   10    10 │       --- stderr
         11 │+      Could not create directory '/var/lib/portage/home/.ssh' (Permission denied).
         12 │+      Failed to add the host to the list of known hosts (/var/lib/portage/home/.ssh/known_hosts).
   11    13 │       Load key "[TEMP_DIR]/fake_deploy_key": [ERROR]
   12    14 │       git@github.com: Permission denied (publickey).
   13    15 │       fatal: Could not read from remote repository.
   14    16 │ 
────────────┴───────────────────────────────────────────────────────────────────
```


---

_Label `internal` added by @konstin on 2025-06-05 10:23_

---

_@konstin approved on 2025-06-05 10:24_

Thanks!

---

_Merged by @konstin on 2025-06-05 10:35_

---

_Closed by @konstin on 2025-06-05 10:35_

---

_Branch deleted on 2025-06-05 10:55_

---

_Comment by @mgorny on 2025-06-05 10:55_

Thanks!

---
