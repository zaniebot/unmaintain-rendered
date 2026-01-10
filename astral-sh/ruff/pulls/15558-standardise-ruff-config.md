```yaml
number: 15558
title: Standardise ruff config
type: pull_request
state: merged
author: calumy
labels:
  - internal
assignees: []
merged: true
base: main
head: standardise-ruff-config
created_at: 2025-01-17T21:00:01Z
updated_at: 2025-01-21T12:18:20Z
url: https://github.com/astral-sh/ruff/pull/15558
synced_at: 2026-01-10T20:05:43Z
```

# Standardise ruff config

---

_Pull request opened by @calumy on 2025-01-17 21:00_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Follow up to #15544 and https://github.com/astral-sh/ruff/pull/15544#pullrequestreview-2559748582 to move the Ruff config used in scripts into the `pyproject.toml` in the root directory now that Python scripts are being added outwith the scripts directory.

Only changes required relate to line length. I have updated to split at 88 characters; however, if a `noqa` would be better in this instance, please let me know, and I can update.

## Test Plan

- Pre-commit ci stage to ensure that all changes identified by the new config are caught. 
- The new ci step added in #15544 (https://github.com/astral-sh/ruff/blob/main/.github/workflows/ci.yaml#L389-L393) to ensure that the updated line lengths don't produce an incorrect result.

---

_Review requested from @MichaReiser by @calumy on 2025-01-17 21:00_

---

_Converted to draft by @calumy on 2025-01-17 21:09_

---

_Comment by @calumy on 2025-01-17 21:28_

The test failure appears to be from the `display_default_settings` test relying on the `pyproject.toml` config in the root directory for its default values. I think there are a couple of ways to fix this (potentially more):

- Update the `display_default_settings` snapshot to include the now updated settings
- Update the test to continue to use the default settings by updating the `pyproject.toml` file used in the test

I would appreciate some guidance on how best to approach a fix. 

---

_Comment by @AlexWaygood on 2025-01-17 21:52_

> * Update the `display_default_settings` snapshot to include the now updated settings

This is what we've done in the past whenever we've updated the pyproject.toml settings :-) it's a bit silly but it's easy 

---

_Review comment by @AlexWaygood on `pyproject.toml`:90 on 2025-01-17 21:53_

I think this no longer conflicts with the formatter now we have the 2025 style! We can probably remove this exclusion now 

---

_Review comment by @AlexWaygood on `pyproject.toml`:92 on 2025-01-17 21:54_

```suggestion
  # which seems unlikely for any of the scripts in this repo
```

---

_Review comment by @AlexWaygood on `pyproject.toml`:61 on 2025-01-17 21:54_

Some of our scripts require newer Python versions than this, but this seems fine 

---

_@AlexWaygood reviewed on 2025-01-17 21:55_

Thank you!

---

_Label `internal` added by @AlexWaygood on 2025-01-17 21:56_

---

_@calumy reviewed on 2025-01-17 22:07_

---

_Review comment by @calumy on `pyproject.toml`:61 on 2025-01-17 22:07_

Most scripts do; however, https://github.com/astral-sh/ruff/blob/main/python/ruff/__main__.py#L17  assumes that less than 3.10 may be used, so I went for the lowest currently supported Python version. 

Ref: https://devguide.python.org/versions/

---

_@calumy reviewed on 2025-01-17 22:09_

---

_Review comment by @calumy on `pyproject.toml`:90 on 2025-01-17 22:09_

Updated

---

_Marked ready for review by @calumy on 2025-01-17 22:23_

---

_Review comment by @MichaReiser on `scripts/pyproject.toml`:36 on 2025-01-17 22:26_

Do we still need this tool.ruff section? Can't we just remove it?

---

_@MichaReiser reviewed on 2025-01-17 22:26_

---

_Comment by @github-actions[bot] on 2025-01-17 22:29_

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

_@calumy reviewed on 2025-01-17 22:31_

---

_Review comment by @calumy on `scripts/pyproject.toml`:36 on 2025-01-17 22:31_

If it isn't added, then the following error is raised as the python version in the root directory is >=3.9 and in the scripts directory is >=3.11.

```vim
> uv tool run pre-commit run --all-files ruff
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1

scripts/check_ecosystem.py:286:20: F821 Undefined name `ExceptionGroup`. Consider specifying `requires-python = ">= 3.11"` or `tool.ruff.target-version = "py311"` in your `pyproject.toml` file.
    |
284 |                         ),
285 |                     )
286 |             except ExceptionGroup as e:
    |                    ^^^^^^^^^^^^^^ F821
287 |                 raise e.exceptions[0] from e
    |

Found 1 error.
```

---

_@calumy reviewed on 2025-01-17 22:33_

---

_Review comment by @calumy on `scripts/pyproject.toml`:36 on 2025-01-17 22:33_

3.9 is required in the root directory for this reason: https://github.com/astral-sh/ruff/pull/15558#discussion_r1920816322

---

_@MichaReiser reviewed on 2025-01-18 14:11_

---

_Review comment by @MichaReiser on `crates/ruff/tests/snapshots/show_settings__display_default_settings.snap`:74 on 2025-01-18 14:11_

I think we have to fix this test to be isolated from our own `pyproject.toml`. It otherwise becomes very annoying because it requires updating whenever we add or remove a rule from any enabled category.

---

_@MichaReiser reviewed on 2025-01-18 14:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/generate.py`:178 on 2025-01-18 14:12_

Splitting this over multiple line decreases readability. I suggest disabling E501 and keep it as regular string.

---

_@MichaReiser reviewed on 2025-01-18 14:13_

---

_Review comment by @MichaReiser on `pyproject.toml`:61 on 2025-01-18 14:13_

This should be Py38 then.
```suggestion
target-version = "py38"
```

---

_@MichaReiser reviewed on 2025-01-18 14:13_

---

_Review comment by @MichaReiser on `pyproject.toml`:69 on 2025-01-18 14:13_

```suggestion
```

This is the default

---

_Review comment by @MichaReiser on `crates/ruff/tests/snapshots/show_settings__display_default_settings.snap`:74 on 2025-01-20 09:18_

Done in https://github.com/astral-sh/ruff/pull/15610

---

_@MichaReiser reviewed on 2025-01-20 09:18_

---

_@calumy reviewed on 2025-01-20 14:01_

---

_Review comment by @calumy on `crates/ruff/tests/snapshots/show_settings__display_default_settings.snap`:74 on 2025-01-20 14:01_

Thanks. I'll rebase and update as required 

---

_Comment by @calumy on 2025-01-21 10:06_

Apologies for the delay; this should now be ready for review. 

Note: There may be some interference with #15632, cc @sharkdp, as some line lengths exceed the default 88 characters. Depending on merge order, I can rebase again as required.

---

_@MichaReiser reviewed on 2025-01-21 11:01_

---

_Review comment by @MichaReiser on `pyproject.toml`:14 on 2025-01-21 11:01_

I was about to revert this change, but we use 4 spaces according to our editorconfig. 

---

_@MichaReiser approved on 2025-01-21 11:02_

Nice, thank you. I disable E501 globally. We rely on the formatter and the good judgement of everyone writing code to not go overboard with very long lines. 



---

_Merged by @MichaReiser on 2025-01-21 11:09_

---

_Closed by @MichaReiser on 2025-01-21 11:09_

---

_@calumy reviewed on 2025-01-21 12:16_

---

_Review comment by @calumy on `crates/ruff_python_ast/generate.py`:177 on 2025-01-21 12:16_

@MichaReiser, I wonder if the removal of unnecessary `"""` from strings is something that could be handled by a future iteration of the formatter. 

---

_@MichaReiser reviewed on 2025-01-21 12:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/generate.py`:177 on 2025-01-21 12:18_

Changing from triple to single quoted style feels too opinionated for a formatter. 

---
