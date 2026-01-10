```yaml
number: 9299
title: "[flake8-bandit/S506] Dont report violation when SafeLoader is imported from yaml.loader"
type: pull_request
state: merged
author: mikaelarguedas
labels:
  - bug
assignees: []
merged: true
base: main
head: fix_S506_safe_loader
created_at: 2023-12-28T14:01:57Z
updated_at: 2023-12-28T14:49:34Z
url: https://github.com/astral-sh/ruff/pull/9299
synced_at: 2026-01-10T23:07:18Z
```

# [flake8-bandit/S506] Dont report violation when SafeLoader is imported from yaml.loader

---

_Pull request opened by @mikaelarguedas on 2023-12-28 14:01_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Hey there :wave: thanks for this great project!

On python code looking like the following
```
import yaml
from yaml.loader import SafeLoader

with MY_FILE_PATH.open("r") as my_file:
    my_data = yaml.load(my_file, Loader=SafeLoader)
```

ruff reports this error:
```
S506 Probable use of unsafe loader `SafeLoader` with `yaml.load`. Allows instantiation of arbitrary objects. Consider `yaml.safe_load`.
```

This PR is an attempt to support SafeLoader being imported for either `yaml` or `yaml.loader`

Disclaimer:
I am not familiar with Rust so this is likely not the better way of doing it. Interested in hearing how to adapt this PR to provide similar behavior in a better way
 

## Test Plan

<!-- How was it tested? -->
The S506.py file was updated accordingly to cover the use cases and test were confirmed to pass with this change.

---

_Comment by @github-actions[bot] on 2023-12-28 14:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: 'quote-style = preserve' is a preview only feature. Run with '--preview' to enable it.
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-12-28 14:24_

Thanks, this is great! I just collapsed the `matches!` calls into a single pattern-match rather than two separate macros.

---

_Label `bug` added by @charliermarsh on 2023-12-28 14:25_

---

_Merged by @charliermarsh on 2023-12-28 14:30_

---

_Closed by @charliermarsh on 2023-12-28 14:30_

---

_Branch deleted on 2023-12-28 14:49_

---
