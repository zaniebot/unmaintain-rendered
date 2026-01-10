```yaml
number: 16204
title: New rule in flake8-tidy-imports for banning specific function calls
type: pull_request
state: open
author: kujenga
labels:
  - rule
  - needs-decision
  - preview
assignees: []
base: main
head: at/banned-function-rule
created_at: 2025-02-17T03:49:57Z
updated_at: 2025-03-07T08:11:42Z
url: https://github.com/astral-sh/ruff/pull/16204
synced_at: 2026-01-10T19:49:01Z
```

# New rule in flake8-tidy-imports for banning specific function calls

---

_Pull request opened by @kujenga on 2025-02-17 03:49_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This commit introduces a new lint rule (TID254) that flags forbidden function calls, such as `os.system` and `os.popen`, in alignment with a project’s banned functions configuration. Highlights include:

- Creating the `BannedFunction` rule to detect and report banned function calls.
- Updating error code mappings to reference TID254.
- Adding new configuration options (`banned_functions`) within `flake8_tidy_imports`.
- Providing a new test fixture (`TID254.py`) and snapshot to verify detection.
- Ensuring seamless integration with expression analysis in the AST checker.

<!-- What's the purpose of the change? What does it do, and why? -->

This was requested here: https://github.com/astral-sh/ruff/discussions/9236 and I have encountered some use cases for this as well.

While this is not technically part of the `flake8-tidy-imports` module it seemed like the closest fit from the current set of them.

n.b. I don't have a huge amount of rust experience, and this was implemented with significant assistance from llms so please let me know if anything looks off!

## Test Plan

<!-- How was it tested? -->

Tests were added modeled off of TID251 for banned APIs to cover a variety of cases as outlined in the test file.

I also verified this against another codebase with a release build of ruff and it seems to be working as expected.


---

_Comment by @github-actions[bot] on 2025-02-17 06:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2025-02-17 10:20_

---

_Label `needs-decision` added by @MichaReiser on 2025-02-17 10:20_

---

_Label `preview` added by @MichaReiser on 2025-02-17 10:20_

---

_Comment by @MichaReiser on 2025-02-17 10:21_

We'll have to look into how this rule plays with the banned API rule and what happens if there are conflicting configurations or if it woudl be better to extend the existing banned api rule instead.

---

_Comment by @kujenga on 2025-02-17 16:05_

I can definitely see that being a cleaner design! Trying to do this sort of thing with that rule, and being unable to do so, is what lead me down this path. I wasn't sure how backward compatibility might factor in with changing that one, but happy to re-work this if that seems like a better end state here!

---

_Comment by @kujenga on 2025-03-06 22:25_

@MichaReiser any thoughts on the path forward for this? Is expanding the existing rule preferred?

---

_Comment by @MichaReiser on 2025-03-07 08:11_

Sorry, I haven't had time to look into this yet. It may be a while before someone from the team has the time to make a decision here because we want to focus our attention on solving fundamental problems over adding new rules.

---
