```yaml
number: 10112
title: "[D417] Missing argument descriptions for variadics *args and **kwargs should be optional"
type: issue
state: closed
author: dfrtz
labels:
  - docstring
  - help wanted
assignees: []
created_at: 2024-02-24T15:50:01Z
updated_at: 2024-12-31T11:16:57Z
url: https://github.com/astral-sh/ruff/issues/10112
synced_at: 2026-01-12T15:54:49Z
```

# [D417] Missing argument descriptions for variadics *args and **kwargs should be optional

---

_@dfrtz_

Related to https://github.com/astral-sh/ruff/issues/8605, I believe that if "ruff" is to be the successor to "pydocstyle" (per their announcement back in Nov), then the current behavior of ruff to flag variadics is not keeping parity with existing functionality. I know 8605 is closed as more or less "expected", however I do not believe this is true, and I will provide details on why I believe this needs to be revisited. Specifically, if using pydocstyle 6.3.0, the last released build (or earlier), it does not flag on variadic args `*args` or `**kwargs`, whereas 0.2.2 does. Example:

**pyproject.toml**
```
[tool.ruff.lint]
select = [
  "D",
]

[tool.ruff.lint.pydocstyle]
convention = "google"
```

**Python File**:
```
"""Minimal repo."""


def dynamic_system1(arg1: int, arg2: int, *unused_args, **unused_kwargs) -> None:
    """Mock dynamic system 1 with intentional missing arg to flag.

    Args:
        arg1: First value.
    """
    print(f'Does something with matching values: {arg1} {arg2}')


def dynamic_system2(arg3: int, arg4: int, *unused_args, **unused_kwargs) -> None:
    """Mock dynamic system 2.

    Args:
        arg3: First value.
        arg4: Second value.
    """
    print(f'Does something with matching values: {arg3} {arg4}')


def main():
    """Primary function."""
    fake_pull_from_dynamic_resource = {
        "arg1": 1,
        "arg2": 2,
        "arg3": 3,
        "arg4": 4,
    }
    dynamic_system1(**fake_pull_from_dynamic_resource)
    dynamic_system2(**fake_pull_from_dynamic_resource)


main()
```

**pydocstyle 6.3.0**
```
$ pydocstyle --count min_repo.py 
min_repo.py:5 in public function `dynamic_system1`:
        D417: Missing argument descriptions in the docstring (argument(s) arg2 are missing descriptions in 'dynamic_system1' docstring)
1
```

**ruff 0.2.2**
```
# Isolated not included since config is needed.
$ ruff --version
ruff 0.2.2
$ ruff check min_repo.py 
min_repo.py:4:5: D417 Missing argument descriptions in the docstring for `dynamic_system1`: `**unused_kwargs`, `*unused_args`, `arg2`
min_repo.py:13:5: D417 Missing argument descriptions in the docstring for `dynamic_system2`: `**unused_kwargs`, `*unused_args`
Found 2 errors.
```

As we can see here, this is not expected behavior for those coming from pydocstyle due to this being the recommended replacement. Perhaps there is an argument to be had for whether or not one "should" document variadics, however I would say that debate is outside the scope of this recommendation: maintain parity with pydocstyle to allow a 100% drop in replacement. For example, this is the only blocker I have found for being able to replace pydocstyle with ruff in several projects, and I imagine as more try to transition they will experience the same issue. These projects would be unwilling to disable D417, as it is very beneficial to ensuring documentation quality for those arguments that are not used "dynamically". I imagine most projects that are using these for dynamic systems will not be interested in copy/pasting a generic args/kwargs message everywhere in order to start using ruff for pydocstyle lints. My recommendation to ensure ruff can fully replace pydocstyle would be:
- Disable checking for D417 on variadics to maintain parity with pydocstyle
- Or make it optional if wanting to change the behavior (default could go either way, as long as configurable)


---

_Label `docstring` added by @AlexWaygood on 2024-02-24 16:20_

---

_Comment by @charliermarsh on 2024-06-01 23:33_

We can change the default here in v0.5.0.

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-06-01 23:33_

---

_Removed from milestone `v0.5.0` by @MichaReiser on 2024-06-27 11:40_

---

_Added to milestone `v0.6` by @MichaReiser on 2024-06-27 11:40_

---

_Removed from milestone `v0.6` by @MichaReiser on 2024-08-14 08:54_

---

_Added to milestone `v0.7` by @MichaReiser on 2024-08-14 08:54_

---

_Label `help wanted` added by @MichaReiser on 2024-08-14 08:54_

---

_Removed from milestone `v0.7` by @AlexWaygood on 2024-10-17 15:18_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-10-17 15:18_

---

_Removed from milestone `v0.8` by @AlexWaygood on 2024-11-18 11:38_

---

_Added to milestone `v0.9` by @AlexWaygood on 2024-11-18 11:38_

---

_Closed by @MichaReiser on 2024-12-31 11:16_

---
