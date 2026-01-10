```yaml
number: 19182
title: "[ty] Consider virtual files for workspace diagnostics"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - server
  - ty
assignees: []
draft: true
base: dhruv/move-check-mode
head: dhruv/virtual-files-for-workspace-diagnostics
created_at: 2025-07-07T14:33:11Z
updated_at: 2025-07-10T14:41:23Z
url: https://github.com/astral-sh/ruff/pull/19182
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Consider virtual files for workspace diagnostics

---

_Pull request opened by @dhruvmanila on 2025-07-07 14:33_

## Summary

Follow-up from #19181 and #19225, this PR adds support for virtual files to workspace diagnostics. This is part of astral-sh/ty#81.

The way this is implemented

## Test Plan

This video demonstrates that in VS Code when you open a new (unsaved) file whose language is Python, the diagnostics are present in the Problems tab and are updated as I change the content. The diagnostics are cleared when the file is closed without saving it.

https://github.com/user-attachments/assets/7329dfcb-9b37-4eab-80ef-b0fa90c23854




---

_Label `server` added by @dhruvmanila on 2025-07-07 14:33_

---

_Label `ty` added by @dhruvmanila on 2025-07-07 14:33_

---

_Renamed from "[ty] Track open files in the server" to "[ty] Consider virtual files for workspace diagnostics" by @dhruvmanila on 2025-07-07 14:33_

---

_Comment by @github-actions[bot] on 2025-07-07 14:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @Gankra on `crates/ty_project/src/lib.rs`:595 on 2025-07-08 14:49_

Personally I find `open_files.map(...).unwrap_or(0)` more clear.

---

_Review comment by @Gankra on `crates/ty_project/src/lib.rs`:620 on 2025-07-08 14:54_

```suggestion
                    open_files: open_files.into_iter().flatten(),
```

I *think* works here?

---

_Review comment by @Gankra on `crates/ty_project/src/lib.rs`:664 on 2025-07-08 15:46_

This would benefit from a brief comment to the effect of "iterate forward the open_files until we find the next virtual one, or the iterator returns None (and hash_set::Iter is a FusedIterator so it will keep yielding None)".

---

_@Gankra reviewed on 2025-07-08 16:01_

Having trouble finding where the notion of virtual files comes from -- is this the "open files are in-memory files" or "files in the playground" or both?

The idea here is that when checking a workspace, if we have an in-memory copy and an on-disk copy, the in-memory one should be preferred? or? 

---

_Comment by @MichaReiser on 2025-07-08 16:11_

> Having trouble finding where the notion of virtual files comes from -- is this the "open files are in-memory files" or "files in the playground" or both?

It's exclusively used for the LSP use case where a new file in an editor doesn't have a path yet. It only exists in the editor but without any associated path. Instead, it's identified by a unique string (it's an URL). The virtual file becomes a regular file once the user hits save in the editor and picks a location for it. 

Files that are open in the editor and have unsaved changes are handled by our [`System`](https://github.com/astral-sh/ruff/blob/ebc70a4002dbad17ffac27a35959d937793787ee/crates/ruff_db/src/system.rs#L49) abstraction. The `System` abstraction hides where ty is running and it provides methods for reading files etc. The LSP has a custom `System` implementation that returns the unsaved file changes over the file content on disk if it is open in the editor ([`LSPSystem](https://github.com/astral-sh/ruff/blob/ebc70a4002dbad17ffac27a35959d937793787ee/crates/ty_server/src/system.rs#L165))

---

_@dhruvmanila reviewed on 2025-07-09 09:39_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:620 on 2025-07-09 09:39_

That doesn't work but `open_files.as_deref().into_iter().flatten()` does which would then require updating the `open_files` type to be a bit verbose (`std::iter::Flatten<std::option::IntoIter<&std::collections::HashSet<File>>>`) and I'm not sure if that'll work. I'm leaning towards what I have currently, thanks for the suggestion!

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:545 on 2025-07-10 10:20_

We do want to drop AST nodes even in `CheckMode::AllFiles` if they're not open. Reparsing should only be necessary if a dependency of that file changed (and only if that files public interface changed). 

---

_@MichaReiser reviewed on 2025-07-10 10:20_

---

_@MichaReiser reviewed on 2025-07-10 10:22_

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:572 on 2025-07-10 10:22_

I think I would simply combine those and eagerly compute the Virtual files in `ProjectFiles::new`. 

---

_@MichaReiser reviewed on 2025-07-10 10:23_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/db.rs`:15 on 2025-07-10 10:23_

Do we still need `is_file_open`?

---

_@MichaReiser reviewed on 2025-07-10 10:26_

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:397 on 2025-07-10 10:26_

I'm not sure this is correct. Looking at this, I'm actually not sure if we even need to change this because `project.is_file_open` already correctly returns `true` for virtual paths

---

_@dhruvmanila reviewed on 2025-07-10 10:29_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:545 on 2025-07-10 10:29_

I noticed that for every `workspace/diagnostic` request that VS Code sent, all of the files were being re-parsed even though nothing changed. This is why I thought it might be useful to do this.

---

_@dhruvmanila reviewed on 2025-07-10 10:34_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:397 on 2025-07-10 10:34_

Hmm, yeah. I think I changed the ordering within the `project.is_file_open` after implementing this and I think you're right that this might not be required now.

---

_@MichaReiser reviewed on 2025-07-10 10:35_

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:545 on 2025-07-10 10:35_

Oh, I think I see the problem. It's because `check_file_impl` isn't a salsa query. I can fix this real quick.


---

_@MichaReiser reviewed on 2025-07-10 10:56_

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:545 on 2025-07-10 10:56_

https://github.com/astral-sh/ruff/pull/19255

---

_Comment by @dhruvmanila on 2025-07-10 14:37_

I've found a few other issues in the implementation, I'm looking at them. I'll create another PR and avoid the PR stack for now.

---

_Closed by @dhruvmanila on 2025-07-10 14:37_

---

_Branch deleted on 2025-07-10 14:41_

---
