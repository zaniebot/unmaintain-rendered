```yaml
number: 4368
title: Add config to disable S101 (assert detected) and a few others in test folders
type: issue
state: open
author: jankatins
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2023-05-11T09:51:01Z
updated_at: 2025-06-25T15:06:36Z
url: https://github.com/astral-sh/ruff/issues/4368
synced_at: 2026-01-10T11:09:47Z
```

# Add config to disable S101 (assert detected) and a few others in test folders

---

_Issue opened by @jankatins on 2023-05-11 09:51_

Per convention, testfolders usually reside in `<reporoot>/test/` or `<reporoot>/tests/`. Right now, every time I add certain rules, specifically S101, ARG, FBT, and a few others (e.g. via "ALL"), I have to add a folder exclude for these rules for the `tests/` folder:

```toml

[tool.ruff.per-file-ignores]
"tests/**/*.py" = [
    # at least this three should be fine in tests:
    "S101", # asserts allowed in tests...
    "ARG", # Unused function args -> fixtures nevertheless are functionally relevant...
    "FBT", # Don't care about booleans as positional arguments in tests, e.g. via @pytest.mark.parametrize()
    # The below are debateable
    "PLR2004", # Magic value used in comparison, ...
    "S311", # Standard pseudo-random generators are not suitable for cryptographic purposes
]
```

 It would be really nice if ruff would do that automatically, E.g. by disabling the above rules via a central `test-files` list which is on a commonly used default:

```toml
[tool.ruff]
# To disable tests which are ok in tests -> would not need to be set as it would be a nice default
test-files =  ["tests/**/*.py", "test/**/*.py"]
```

This was already discussed in https://github.com/charliermarsh/ruff/issues/714 where the original reporter (@harupy) closed the issue after confirming the per folder overwrites.

---

_Label `question` added by @charliermarsh on 2023-05-11 23:25_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:21_

---

_Label `configuration` added by @charliermarsh on 2023-07-10 01:21_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:21_

---

_Comment by @jpmckinney on 2023-07-12 23:10_

FWIW, I ignore the same rules (including the debatable ones), plus "D".

---

_Comment by @paduszyk on 2024-07-19 20:54_

@jankatins I totally agree with your idea 👍🏻 I would also propose to add S105, S106, and S107 to your list.

Hardcoded passwords usually appear in tests of authentication stuff. However, I am not entirely sure. For instance, this would require assuming that testers use naive "placeholder" passwords, not real passwords (maybe this could be set up via an extra `flake8-bandit` setting)...

@jankatins @jpmckinney What do you think?

---

_Comment by @jpmckinney on 2024-07-20 04:06_

Sounds good to me. I haven't had those rules trigger, even though I do have hardcoded passwords in tests, but I would exclude them if they were triggering.

---

_Comment by @charliermarsh on 2024-07-20 15:46_

Are there some opportunities to improve our rules here? E.g., avoid flagging unused arguments in pytest fixtures, if we can detect them?

---

_Comment by @tomsquest on 2024-07-23 15:30_

For the one finding that issue, as of Ruff 0.5.4, I adapted @jankatins snippet to:
```
[tool.ruff.lint.extend-per-file-ignores]
"tests/**/*.py" = [
    # at least this three should be fine in tests:
    "S101", # asserts allowed in tests...
    "ARG", # Unused function args -> fixtures nevertheless are functionally relevant...
    "FBT", # Don't care about booleans as positional arguments in tests, e.g. via @pytest.mark.parametrize()
    # The below are debateable
    "PLR2004", # Magic value used in comparison, ...
    "S311", # Standard pseudo-random generators are not suitable for cryptographic purposes
]
```

---

_Comment by @paduszyk on 2025-05-07 10:11_

What do you think about including `E501`? Test function names are usually quite long, particularly if they serve as documentation (e.g., when the names follow a pattern that clearly indicates the purpose, scenario, and expected outcome of the test).

---

_Comment by @starhel on 2025-06-25 15:06_

> "ARG", # Unused function args -> fixtures nevertheless are functionally relevant...

You should probably use `@pytest.mark.usefixtures`. There’s probably only one scenario where this won’t work – when one fixture depends on another, but only uses its state rather than the fixture directly.

---
