```yaml
number: 6414
title: "Add autofix for import convention's `banned-from` rule (issue #6372)"
type: pull_request
state: closed
author: Sawbez
labels: []
assignees: []
draft: true
base: main
head: main
created_at: 2023-08-08T02:09:28Z
updated_at: 2023-09-13T04:05:05Z
url: https://github.com/astral-sh/ruff/pull/6414
synced_at: 2026-01-12T15:55:21Z
```

# Add autofix for import convention's `banned-from` rule (issue #6372)

---

_@Sawbez_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR is intended to add a fix to ICN003 `banned-from`. Closes #6372.

<!-- How was it tested? -->
This will be tested by adding specific tests for the autofix with different configurations.

---

_Renamed from "Improve autofix interop with TCH and ICN (issue #6372)" to "Add autofix for import convention's `banned-from` rule (issue #6372)" by @Sawbez on 2023-08-08 02:30_

---

_Comment by @Sawbez on 2023-08-08 03:04_

The plan I have right now (I need to implement):

1. Use the information about which imports are faulty to figure out which import members exist and track them (e.g. `Y` and `Z` in `from X import Y, Z`)
2. Change the original import to not be faulty
3. Add an AST checker that gets triggered and finds which members are now out of scope [sub-step: ensure that the members are NOT just variables with the same name)
4. Ensure that the out-of-scope members are now fully qualified

However, it's likely that this may "false-fix" certain things with complex variable assignments with the same name as the module (bad practice but may exist), any ideas on that?

---

_Closed by @Sawbez on 2023-09-13 04:04_

---

_Comment by @Sawbez on 2023-09-13 04:05_

I might come back to this later but I am currently extremely busy.

---
