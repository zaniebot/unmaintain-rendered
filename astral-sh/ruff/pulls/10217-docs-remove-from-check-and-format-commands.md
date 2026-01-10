```yaml
number: 10217
title: "`docs`: remove `.` from check and format commands"
type: pull_request
state: merged
author: hoel-bagard
labels:
  - documentation
assignees: []
merged: true
base: main
head: hoel/remove_dot_from_docs
created_at: 2024-03-03T23:56:33Z
updated_at: 2024-03-13T23:46:56Z
url: https://github.com/astral-sh/ruff/pull/10217
synced_at: 2026-01-10T22:47:01Z
```

# `docs`: remove `.` from check and format commands

---

_Pull request opened by @hoel-bagard on 2024-03-03 23:56_

## Summary

This PR modifies the documentation to use `ruff check` instead of `ruff check .`, and `ruff format` instead of `ruff format .`, as discussed [here](https://github.com/astral-sh/ruff/pull/10168#discussion_r1509976904)

---

_Renamed from "docs: remove `.` from check and format commands" to "`docs`: remove `.` from check and format commands" by @hoel-bagard on 2024-03-03 23:59_

---

_Comment by @github-actions[bot] on 2024-03-04 00:14_

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

_@MichaReiser reviewed on 2024-03-04 09:59_

---

_Review comment by @MichaReiser on `docs/formatter.md`:14 on 2024-03-04 09:59_

```suggestion
ruff format                  # Format all files in the current directory.
```

---

_@MichaReiser reviewed on 2024-03-04 10:01_

Thank you @hoel-bagard for PRing this change.

I'm conflicted on this. While `.` is not strictly necessary, it shows users how to pass a specific `path`. Removing the path everywhere might indicate that ruff doesn't accept custom paths. Maybe we leave it in one example and mention that the path can be omitted?

---

_Review requested from @zanieb by @MichaReiser on 2024-03-04 10:01_

---

_Comment by @hoel-bagard on 2024-03-04 10:35_

@MichaReiser 

> I'm conflicted on this. While . is not strictly necessary, it shows users how to pass a specific path. Removing the path everywhere might indicate that ruff doesn't accept custom paths.

That was also part of my reasoning when I left it in the preview mode documentation PR. I made this PR to have consistent documentation.

> Maybe we leave it in one example and mention that the path can be omitted?

Looking at the documentation, the `.` is used a lot, but doesn't seem to be explicitly explained that much. There is an explanation [here](https://docs.astral.sh/ruff/configuration/#python-file-discovery), and examples [here](https://docs.astral.sh/ruff/linter/#detecting-unused-suppression-comments), but one might expect a short explanation on  how to run ruff on a file/directory to be easier to find.
The note in the [ruff check section](https://docs.astral.sh/ruff/linter/#ruff-check) does link to an explanation with examples, but maybe the note could slightly expand on the fact that `ruff check` takes a path as input ?

The [ruff formatter documentation](https://docs.astral.sh/ruff/formatter/) has an explanation on how to format a single file/directory, using the same example for the linter should be enough, what do you think ?

---

_Comment by @charliermarsh on 2024-03-04 17:04_

I think I'd personally prefer to keep these, since in my view at least it's more explicit and clearer for readers that don't know that Ruff defaults to `.`.

---

_Closed by @hoel-bagard on 2024-03-13 14:07_

---

_Comment by @zanieb on 2024-03-13 14:27_

Humph... I want to get rid of these because people think they're needed and they're really not. It degrades the idea that we're a workspace oriented tool. Checking a specific path seems like a less common behavior that we could document separately.

---

_Comment by @charliermarsh on 2024-03-13 14:28_

I defer to you @zanieb.

---

_Reopened by @zanieb on 2024-03-13 14:39_

---

_@zanieb approved on 2024-03-13 14:47_

Sorry for the back-and-forth, I pushed a couple additional touch-ups and attempted to address Micha's concern.

---

_Label `documentation` added by @zanieb on 2024-03-13 15:10_

---

_Merged by @zanieb on 2024-03-13 15:10_

---

_Closed by @zanieb on 2024-03-13 15:10_

---

_Branch deleted on 2024-03-13 23:46_

---
