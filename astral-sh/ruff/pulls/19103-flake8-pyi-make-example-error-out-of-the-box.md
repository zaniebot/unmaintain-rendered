```yaml
number: 19103
title: "[`flake8-pyi`] Make example error out-of-the-box (`PYI007`, `PYI008`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-07-02T21:33:56Z
updated_at: 2025-07-07T20:12:32Z
url: https://github.com/astral-sh/ruff/pull/19103
synced_at: 2026-01-12T15:56:31Z
```

# [`flake8-pyi`] Make example error out-of-the-box (`PYI007`, `PYI008`)

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

Both in one PR since they are in the same file

No playground links since the playground does not support rules that only apply to PYI files

PYI007
---

This PR makes [unrecognized-platform-check (PYI007)](https://docs.astral.sh/ruff/rules/unrecognized-platform-check/#unrecognized-platform-check-pyi007)'s example error out-of-the-box

Old example:
```
PS ~\Desktop\New_folder\ruff>echo @"
```
```py
if sys.platform.startswith("linux"):
    # Linux specific definitions
    ...
else:
    # Posix specific definitions
    ...
```
```
"@ | uvx ruff check --isolated --preview --select PYI007 --stdin-filename "test.pyi" -
```
```
All checks passed!
```

New example:
```
PS ~\Desktop\New_folder\ruff>echo @"
```
```py
import sys

if sys.platform is "linux":
    # Linux specific definitions
    ...
else:
    # Posix specific definitions
    ...
```
```
"@ | uvx ruff check --isolated --preview --select PYI007 --stdin-filename "test.pyi" -
```
```snap
test.pyi:3:4: PYI007 Unrecognized `sys.platform` check
  |
1 | import sys
2 |
3 | if sys.platform is "linux":
  |    ^^^^^^^^^^^^^^^^^^^^^^^ PYI007
4 |     # Linux specific definitions
5 |     ...
  |

Found 1 error.
```

Imports were also added to the "use instead" section

> [!NOTE]
> `PYI007` is really hard to trigger, it's only specifically in the case of a comparison where the operator is not `!=` or `==`. The original example raises [complex-if-statement-in-stub (PYI002)](https://docs.astral.sh/ruff/rules/complex-if-statement-in-stub/#complex-if-statement-in-stub-pyi002) with or without the `import sys`

PYI008
---

This PR makes [unrecognized-platform-name (PYI008)](https://docs.astral.sh/ruff/rules/unrecognized-platform-name/#unrecognized-platform-name-pyi008)'s example error out-of-the-box

Old example:
```
PS ~\Desktop\New_folder\ruff>echo @"
```
```py
if sys.platform == "linus": ...
```
```
"@ | uvx ruff check --isolated --preview --select PYI008 --stdin-filename "test.pyi" -
```
```
All checks passed!
```

New example:
```
PS ~\Desktop\New_folder\ruff>echo @"
```
```py
import sys

if sys.platform == "linus": ...
```
```
"@ | uvx ruff check --isolated --preview --select PYI008 --stdin-filename "test.pyi" -
```
```snap
test.pyi:3:20: PYI008 Unrecognized platform `linus`
  |
1 | import sys
2 |
3 | if sys.platform == "linus": ...
  |                    ^^^^^^^ PYI008
  |

Found 1 error.
```

Imports were also added to the "use instead" section

> [!NOTE]
> The original example raises `PYI002` instead

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Review requested from @AlexWaygood by @MeGaGiGaGon on 2025-07-02 21:33_

---

_Comment by @github-actions[bot] on 2025-07-02 21:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/unrecognized_platform.rs`:26 on 2025-07-03 11:29_

this feels like it might be slightly more in keeping with the spirit of the rule, and also triggers PYI007:

```suggestion
/// if sys.platform == "xunil"[::-1]:
```

---

_@AlexWaygood reviewed on 2025-07-03 11:29_

thanks!

---

_Label `documentation` added by @MichaReiser on 2025-07-07 13:36_

---

_Merged by @AlexWaygood on 2025-07-07 20:11_

---

_Closed by @AlexWaygood on 2025-07-07 20:11_

---

_Branch deleted on 2025-07-07 20:12_

---
