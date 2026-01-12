```yaml
number: 1506
title: "Implement `flake8-pytest-style`"
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: pytest-style
created_at: 2022-12-31T09:02:53Z
updated_at: 2023-01-20T02:01:33Z
url: https://github.com/astral-sh/ruff/pull/1506
synced_at: 2026-01-12T05:25:13Z
```

# Implement `flake8-pytest-style`

---

_Pull request opened by @edgarrmondragon on 2022-12-31 09:02_

## Notes for reviewers

- Won't implement `PT014` for now. It requires checking for _equivalent_ nodes, which is a bit more complicated. See https://github.com/afonasev/flake8-plugin-utils/blob/e333e10a72f1c8225357e0d475db20ccbb12a09a/flake8_plugin_utils/utils/equiv_nodes.py for the implementation used in the flake8 plugin.
- Tests are copied and adapted from https://github.com/m-burst/flake8-pytest-style/tree/v1.6.0/tests
- Setting names were shortened by removing the `pytest-` prefix, since they'll be nested under the corresponding plugin in Ruff's configuration.
- `pytest-fixture-no-parentheses` and `pytest-mark-no-parentheses` were changed to `fixture-parentheses` and `mark-parentheses`, respectively. This is consistent with how those settings are managed internally in the flake8 plugin, are a bit easier to reason about and they're not meant to be used as CLI flags, so it's OK to default to true values.

## Rules

- [x] PT001
- [x] PT002
- [x] PT003
- [x] PT004
- [x] PT005
- [x] PT006
- [x] PT007
- [x] PT008
- [x] PT009
- [x] PT010
- [x] PT011
- [x] PT012
- [x] PT013
- [ ] PT014
- [x] PT015
- [x] PT016
- [x] PT017
- [x] PT018
- [x] PT019
- [x] PT020
- [x] PT021
- [x] PT022
- [x] PT023
- [x] PT024
- [x] PT025
- [x] PT026

Closes #921


---

_Comment by @charliermarsh on 2023-01-01 02:21_

@edgarrmondragon - Probably won't get to it tonight but mark as ready for review whenever and I'll take a look :)


---

_Marked ready for review by @edgarrmondragon on 2023-01-02 02:27_

---

_Comment by @edgarrmondragon on 2023-01-02 02:35_

@charliermarsh this is ready for review :)

---

_Comment by @charliermarsh on 2023-01-02 19:12_

Dang, this is seriously impressive and thorough work. I need to skim the actual check implementations but the general structure etc. looks great. I'll merge this today.

---

_Merged by @charliermarsh on 2023-01-02 21:34_

---

_Closed by @charliermarsh on 2023-01-02 21:34_

---

_Branch deleted on 2023-01-03 01:56_

---

_Comment by @henryiii on 2023-01-20 01:22_

Not sure if this is inherited from the flake8 plugin, but this is a false positive for PT007:

```python
@pytest.mark.parametrize("archs", [["x86_64"], ["arm64", "universal2"]])
```

If the item on the left is a string with no commas, then the thing on the right doesn't have to be a list of tuples, it can be a list of anything, including lists.

---

_Comment by @charliermarsh on 2023-01-20 01:52_

Lemme take a look...

---

_Comment by @charliermarsh on 2023-01-20 02:01_

Admittedly I've almost never used Pytest but based on comparing the Flake8 plugin to our results, I think you're right :) Will fix tonight.

---
