```yaml
number: 9035
title: Fix detection of changed files in CI
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: debug/determine-changes
created_at: 2023-12-07T02:44:27Z
updated_at: 2023-12-07T14:52:35Z
url: https://github.com/astral-sh/ruff/pull/9035
synced_at: 2026-01-10T23:40:55Z
```

# Fix detection of changed files in CI

---

_Pull request opened by @zanieb on 2023-12-07 02:44_

These were literals instead of expressions... and were consequently not evaluated.

Fixes bug from #8225 

---

_Marked ready for review by @zanieb on 2023-12-07 02:53_

---

_Comment by @MichaReiser on 2023-12-07 03:04_

I'm not sure if that's the root cause because the checks did ran in the last few days and the [`if`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idif) documentation mentions that omitting the `${{ }}` is okay except when the expression starts with `!`.

---

_Comment by @github-actions[bot] on 2023-12-07 03:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `ci` added by @zanieb on 2023-12-07 03:12_

---

_Comment by @zanieb on 2023-12-07 03:13_

@MichaReiser they didn't run on this branch before the change, but did after 🤷‍♀️ perhaps a change on GitHub's end

---

_Comment by @zanieb on 2023-12-07 03:14_

Also, I ran the workflow with debug logs and the "Determine changes" workflow is not at fault

---

_Merged by @zanieb on 2023-12-07 03:14_

---

_Closed by @zanieb on 2023-12-07 03:14_

---

_Branch deleted on 2023-12-07 03:14_

---

_Comment by @zanieb on 2023-12-07 03:16_

Hm but it _does_ seem to be at fault over in the other branch https://github.com/astral-sh/ruff/actions/runs/7122120523/job/19395730196

---

_Comment by @zanieb on 2023-12-07 03:17_

```
 M	Cargo.toml
  ##[debug]All diff files: {"A":[],"C":[],"D":[],"M":["Cargo.toml"],"R":[],"T":[],"U":[],"X":[]}
  All Done!
  ::endgroup::
::group::changed-files-yaml-linter
changed-files-yaml-linter
::group::changed-files-yaml-formatter
changed-files-yaml-formatter
::group::changed-files-yaml-code
changed-files-yaml-code
  ##[debug]All filtered diff files for code: {"A":[],"C":[],"D":[],"M":[],"R":[],"T":[],"U":[],"X":[]}
  ##[debug]Dir names include file patterns: []
  ##[debug]Added files: {"paths":"","count":"0"}
  ##[debug]Dir names include file patterns: []
  ##[debug]Copied files: {"paths":"","count":"0"}
  ##[debug]Dir names include file patterns: []
  ##[debug]Modified files: {"paths":"","count":"0"}
  ##[debug]Dir names include file patterns: []
  ##[debug]Renamed files: {"paths":"","count":"0"}
  ##[debug]Dir names include file patterns: []
  ##[debug]Type changed files: {"paths":"","count":"0"}
  ##[debug]Dir names include file patterns: []
  ##[debug]Unmerged files: {"paths":"","count":"0"}
  ##[debug]Dir names include file patterns: []
  ##[debug]Unknown files: {"paths":"","count":"0"}
  ##[debug]Dir names include file patterns: []
  ##[debug]All changed and modified files: {"paths":"","count":"0"}
  ##[debug]Dir names include file patterns: []
  ##[debug]All changed files: {"paths":"","count":"0"}
  ##[debug]Dir names include file patterns: []
  ##[debug]All other changed files: {"paths":"Cargo.toml","count":"1"}
  ##[debug]other_changed_files: ["Cargo.toml"]
  ##[debug]Dir names include file patterns: []
  ##[debug]All modified files: {"paths":"","count":"0"}
  ##[debug]Dir names include file patterns: []
  ##[debug]other_modified_files: ["Cargo.toml"]
  ##[debug]Dir names include file patterns: []
  ##[debug]Deleted files: {"paths":"","count":"0"}
  ##[debug]Dir names include file patterns: []
  ##[debug]other_deleted_files: []
  ..
  ##[debug]Set output code_any_changed = false
```

---

_Comment by @zanieb on 2023-12-07 03:19_

~Ah it's because the `M Cargo.toml` gets included in the _linter_ changes~

---

_Comment by @ofek on 2023-12-07 14:18_

Now that the root cause has been fixed you may want to remove the `${{ }}` again to look idiomatic

---

_Comment by @zanieb on 2023-12-07 14:52_

Eh we use `${{ }}` for every other `if` statement in that file. I'd prefer to be consistent.

---
