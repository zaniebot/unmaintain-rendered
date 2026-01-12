```yaml
number: 20483
title: I001 import sorting rule behaves differently in CI vs local with identical files and config
type: issue
state: closed
author: ReinforcedKnowledge
labels:
  - question
assignees: []
created_at: 2025-09-19T13:59:37Z
updated_at: 2025-09-19T14:03:43Z
url: https://github.com/astral-sh/ruff/issues/20483
synced_at: 2026-01-12T15:54:57Z
```

# I001 import sorting rule behaves differently in CI vs local with identical files and config

---

_@ReinforcedKnowledge_

### Question

## Bug Description
The I001 import sorting rule produces different results between local development and CI environments, despite identical file content, ruff version, and configuration.

## Environment
- **Ruff version**: 0.11.7
- **Python version**: 3.12.11
- **Local OS**: Linux
- **CI**: GitHub Actions (Linux)

## Expected Behavior
Ruff should produce consistent results across environments when file content, version, and configuration are identical.

## Actual Behavior
- **Local**: `ruff check testing_ci.py` → All checks passed!
- **CI**: `ruff check testing_ci.py` → I001 Import block is un-sorted or un-formatted

### Code Sample
```python
try:
    import pandas as pd

    import wandb
    from wandb.errors import CommError
except ImportError as e:
    raise 
```

### CI Configuration
```yaml
- name: Run code quality checks on changed files
  if: steps.changed-files.outputs.any_changed == 'true'
  run: |
    echo "Changed files: ${{ steps.changed-files.outputs.all_changed_files }}"

    # Format changed files
    uv run ruff format ${{ steps.changed-files.outputs.all_changed_files }}

    # Check changed files
    uv run ruff check ${{ steps.changed-files.outputs.all_changed_files }}
```

### CI error
That leads in CI to:
```
testing_ci.py:2:5: I001 [*] Import block is un-sorted or un-formatted
  |
1 |   try:
2 | /     import pandas as pd
3 | |
4 | |     import wandb
5 | |     from wandb.errors import CommError
  | |______________________________________^ I001
6 |   except ImportError as e:
7 |       raise
  |
  = help: Organize imports
```

## Notes:
- File hash identical between environments 
- Same `ruff` version: 0.11.7
- Same `pyproject.toml` config
- Same Python version: 3.12.11

The imports appear to follow standard sorting conventions (stdlib imports separated from third-party), but CI consistently flags this as unsorted while local passes.

**Questions**:
1. Are there any environment-specific factors that could cause I001 to behave differently?
2. Could this be related to how `ruff` processes files within `try/except` blocks?
3. Are there any debug flags to understand why `ruff` considers these imports unsorted?

### Version

ruff 0.11.7

---

_Label `question` added by @ReinforcedKnowledge on 2025-09-19 13:59_

---

_Comment by @dylwil3 on 2025-09-19 14:03_

Thanks for the report! What's happening here is that there is a `wandb` folder within your `src` folder present in one of these situations but not the other. This creates an implicit namespace package which is treated as first-party and so sorted differently. A solution is to add this to your `pyproject.toml`:

```toml
[tool.ruff.linter.isort]
known-third-party = ["wandb"]
```

We are working on including `wandb` as a default known third party since this happens quite a bit!

---

_Closed by @dylwil3 on 2025-09-19 14:03_

---
