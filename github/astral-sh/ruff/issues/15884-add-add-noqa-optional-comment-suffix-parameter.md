---
number: 15884
title: "Add `--add-noqa` optional comment suffix parameter"
type: issue
state: closed
author: Skylion007
labels: []
assignees: []
created_at: 2025-02-02T19:56:02Z
updated_at: 2025-02-02T19:59:19Z
url: https://github.com/astral-sh/ruff/issues/15884
synced_at: 2026-01-07T13:12:16-06:00
---

# Add `--add-noqa` optional comment suffix parameter

---

_Issue opened by @Skylion007 on 2025-02-02 19:56_

### Description

Motivation: often when I add a new rule to a large code base I enable `--add-noqa` to a large portions of those files. I may want to revisit those changes and differentiate them from those which are manually added by authors. I often do this by adding a suffix after the `noqa` with a suffix, ie: revisit, TODO, etc. It would be great if the `--add-noqa` clI option also took an optional user specified string to add to the end of the end of comment to make enabling new rules on large codebases easier, and allow future authors to revisit the noqa comments easier.

---

_Renamed from "Allow `--add-noqa` optional comment suffix parameter" to "Add `--add-noqa` optional comment suffix parameter" by @Skylion007 on 2025-02-02 19:56_

---

_Comment by @MichaReiser on 2025-02-02 19:59_

I think this is the same as https://github.com/astral-sh/ruff/issues/10070

---

_Closed by @MichaReiser on 2025-02-02 19:59_

---
