```yaml
number: 8494
title: "Avoid `FURB113` autofix if comments are present"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/furb113
created_at: 2023-11-05T11:00:21Z
updated_at: 2023-11-09T03:24:50Z
url: https://github.com/astral-sh/ruff/pull/8494
synced_at: 2026-01-12T15:55:26Z
```

# Avoid `FURB113` autofix if comments are present

---

_@dhruvmanila_

This PR avoids creating the fix for `FURB113` if there are comments in between the `append` calls.

fixes: #8105

---

_Label `bug` added by @dhruvmanila on 2023-11-05 11:00_

---

_Comment by @github-actions[bot] on 2023-11-05 11:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-11-05 14:46_

---

_Comment by @charliermarsh on 2023-11-05 14:46_

I'm a little torn on this because there's now no way to access this fix whereas with `unsafe` we at least allow the user to opt-in.

---

_Comment by @charliermarsh on 2023-11-05 19:07_

@zanieb - Do you have an opinion on this issue more broadly (of fixes that delete comments)? I could see us marking them as unsafe, or as display, or omitting them entirely as in this PR.

---

_Comment by @tdulcet on 2023-11-05 19:58_

(I am the author of the original issue.) If possible, I would prefer to just put the autofix below the comments, as it is otherwise a good fix as you said. 

Another option could be to write the autofix over multiple lines keeping the comments in place, but I suspect that would be much more difficult to implement:
```diff
-nums.append(4)
+nums.extend((4,
# Critically important comment 1
-nums.append(5)
+5,
# Critically important comment 2
-nums.append(6)
+6))
```

---

_Comment by @dhruvmanila on 2023-11-09 02:57_

> I'm a little torn on this because there's now no way to access this fix whereas with `unsafe` we at least allow the user to opt-in.

This seems to already be an unsafe fix. I'd actually then go with display only for this one.

> Another option could be to write the autofix over multiple lines keeping the comments in place, but I suspect that would be much more difficult to implement:

Yeah, we'd probably need to use LibCST to construct the fix instead.

---

_Comment by @dhruvmanila on 2023-11-09 03:07_

I'm going forward with the current solution as it's consistent with various other rules (`PT014`, `SIM117`, `SIM102`, etc.) where comments doesn't generate the fix but feel free to suggest otherwise.

---

_Merged by @dhruvmanila on 2023-11-09 03:10_

---

_Closed by @dhruvmanila on 2023-11-09 03:10_

---

_Branch deleted on 2023-11-09 03:10_

---

_Comment by @charliermarsh on 2023-11-09 03:16_

I feel like all of those should just be unsafe. I don't think we should prevent the user from accessing the fix due to comments -- they should just have to opt in. But I'd like @zanieb's opinion too. (Not asking for any changes right now.)

---
