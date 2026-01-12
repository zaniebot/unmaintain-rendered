```yaml
number: 11551
title: "Respect excludes in `ruff server` configuration discovery"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: charlie/extension-excludes
created_at: 2024-05-26T23:46:30Z
updated_at: 2024-05-27T17:15:05Z
url: https://github.com/astral-sh/ruff/pull/11551
synced_at: 2026-01-12T15:55:38Z
```

# Respect excludes in `ruff server` configuration discovery

---

_@charliermarsh_

## Summary

Right now, we're discovering configuration files even within (e.g.) virtual environments, because we're recursing without respecting the `exclude` field on parent configuration.

Closes https://github.com/astral-sh/ruff-vscode/issues/478.

## Test Plan

Installed Pandas; verified that I saw no warnings:

![Screenshot 2024-05-26 at 8 09 05 PM](https://github.com/astral-sh/ruff/assets/1309177/dcf4115c-d7b3-453b-b7c7-afdd4804d6f5)


---

_Review requested from @dhruvmanila by @charliermarsh on 2024-05-26 23:46_

---

_Label `bug` added by @charliermarsh on 2024-05-26 23:46_

---

_Label `server` added by @charliermarsh on 2024-05-26 23:46_

---

_@charliermarsh reviewed on 2024-05-26 23:46_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/index/ruff_settings.rs`:158 on 2024-05-26 23:46_

This is loosely drawn from `python_files_in_path`.

---

_Converted to draft by @charliermarsh on 2024-05-26 23:56_

---

_Comment by @github-actions[bot] on 2024-05-27 00:07_

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

_Marked ready for review by @charliermarsh on 2024-05-27 00:10_

---

_@charliermarsh reviewed on 2024-05-27 00:36_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/index/ruff_settings.rs`:88 on 2024-05-27 00:36_

We need some sort of mechanism here because we have to both immutably borrow `index` in `filter_entry` (which _has_ to be a closure) and mutably borrow it in the `for` loop body.

---

_@dhruvmanila approved on 2024-05-27 09:45_

So, if I understand this change correctly, does it mean that the root cause of the issue is that the server uses a low level API from the linter (`check_path`) which means the server doesn't benefit from any logic before that?

If that's the case, then I don't think the server benefits from the cache either and I wonder what other logic would be required here. Nevertheless, I don't think that needs to be checked or addressed in this PR.

---

_@dhruvmanila reviewed on 2024-05-27 09:51_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:88 on 2024-05-27 09:51_

Yeah, I don't see any obvious way to avoid using `RefCell` without collecting them all in a vector.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index/ruff_settings.rs`:128 on 2024-05-27 13:28_

Nit
```suggestion
                    .rfind(|(path, _)| directory.starts_with(path))
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index/ruff_settings.rs`:155 on 2024-05-27 13:31_

I first assumed that the `{` starts a new block, and only after removing it realised that it starts the `for` body. 

I think it would be good to extract the `WalkDir::new` into a variable

---

_@MichaReiser approved on 2024-05-27 13:32_

---

_Comment by @MichaReiser on 2024-05-27 13:34_

A possible alternative to avoid the `RefCell` is to use `WalkDir::skip_current_dir` instead of the `filter_entries` API. It avoids the need for a closure,

---

_Comment by @charliermarsh on 2024-05-27 15:03_

> So, if I understand this change correctly, does it mean that the root cause of the issue is that the server uses a low level API from the linter (check_path) which means the server doesn't benefit from any logic before that?

Yes, I believe so.

---

_Comment by @charliermarsh on 2024-05-27 15:08_

If either of you want to modify and merge + release this today, feel free -- otherwise I may be able to get to it tonight.

---

_Comment by @charliermarsh on 2024-05-27 16:40_

Gonna try and fix-up + merge this now.

---

_Comment by @charliermarsh on 2024-05-27 16:53_

I removed `RefCell` in favor of `skip_current_dir()`.

---

_Merged by @charliermarsh on 2024-05-27 16:59_

---

_Closed by @charliermarsh on 2024-05-27 16:59_

---

_Branch deleted on 2024-05-27 16:59_

---
