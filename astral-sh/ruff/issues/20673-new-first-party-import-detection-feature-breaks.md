```yaml
number: 20673
title: "New first-party import detection feature breaks documented behavior of `detect-same-package` config"
type: issue
state: closed
author: bkurtz
labels:
  - documentation
  - isort
  - configuration
  - needs-info
assignees: []
created_at: 2025-10-01T19:30:10Z
updated_at: 2025-10-07T17:03:37Z
url: https://github.com/astral-sh/ruff/issues/20673
synced_at: 2026-01-10T11:09:59Z
```

# New first-party import detection feature breaks documented behavior of `detect-same-package` config

---

_Issue opened by @bkurtz on 2025-10-01 19:30_

### Summary

Ruff 0.13 [changed behavior](https://github.com/astral-sh/ruff/releases/tag/0.13.0) to do full path verification for first-party module detection.

This new behavior appears to conflict with the documented behavior of the [`detect-same-package`](https://docs.astral.sh/ruff/settings/#lint_isort_detect-same-package) option, which states that it "automatically mark[s] imports from within the same package as first-party", and that it is enabled by default.

My reading of the documentation is that, if `mypackage/x.py` exists and `mypackage/y.py` doe not, that:
* [Documentation for the new first-party module detection feature](https://docs.astral.sh/ruff/faq/#how-does-ruff-determine-which-of-my-imports-are-first-party-third-party-etc) says that `mypackage.x` should be treated as a first-party import and `mypackage.y` should be treated as third-party
* Documentation for [`detect-same-package`](https://docs.astral.sh/ruff/settings/#lint_isort_detect-same-package) implies that if `mypackage.x` is first party that `mypackage` and `mypackage.y` should also be treated as first party.

I'm unclear exactly whether this is a bug in the behavior of ruff (and, e.g. `detect-same-package` should override the new logic that requires full file paths to exist), or just in the documentation (e.g. `detect-same-package` should have a different documented behavior or be marked as deprecated).

(I found the `detect-same-package` flag when looking to see if there was a reasonable option to disable the new first-party detection logic; it feels to me like if `detect-same-package` were off-by-default and were able to override the new logic that that could be a clean way to give folks who don't like the new behavior an out.)

Reasons it might be interesting to continue to support `detect-same-package`:
1. We keep a folder in our repo for "analysis archives".  We want to enable strict code checks on new code checked in here (to ensure high quality with minimal toil), but also want not to invest effort in maintaining old code in this folder - it's primarily intended as documentation for past decisions - if imports etc go out of date.  The new behavior for verifying first-party imports is obviously not desirable in this case, and something like `detect-same-package` would solve most of our use cases without further interventions.
2. Namespace packages? Some potentially-related discussion in https://github.com/astral-sh/ruff/issues/20453 which I haven't read closely.

### Version

0.13.2

---

_Label `documentation` added by @amyreese on 2025-10-01 23:45_

---

_Label `configuration` added by @amyreese on 2025-10-01 23:45_

---

_Label `needs-decision` added by @amyreese on 2025-10-01 23:45_

---

_Label `isort` added by @amyreese on 2025-10-01 23:49_

---

_Label `documentation` removed by @amyreese on 2025-10-01 23:49_

---

_Comment by @MichaReiser on 2025-10-02 06:27_

CC: @dylwil3 

---

_Comment by @dylwil3 on 2025-10-03 19:03_

Thanks for submitting this! I agree that the documentation needs to be improved here. 

It's true that `detect-same-package` can override some of the new first-party module detection. Since the default is for this flag to be on, the main place where the new first-party module detection is visible is in the case of namespace packages (which was also the motivation for adding this detection in the first place). 

Would you be able to provide a minimal reproduction of your directory layout, your Ruff config, and an import that's behaving differently before and after Ruff 0.13? I can try to see if there is a config option that would restore behavior for your case.

---

_Label `documentation` added by @MichaReiser on 2025-10-04 11:41_

---

_Label `needs-info` added by @MichaReiser on 2025-10-04 11:41_

---

_Label `needs-decision` removed by @MichaReiser on 2025-10-04 11:41_

---

_Comment by @bkurtz on 2025-10-06 05:34_

Sure, here's a (pretty) minimal repro:
```
$ tree .
.
├── pyproject.toml
└── src
    ├── mymodule
    │   ├── __init__.py
    │   └── new_package.py
    └── scripts
        └── a_script.py
```
`pyproject.toml`:
```toml
[tool.ruff.lint]
select = [
	"I"
]
```
`src/scripts/a_script.py`:
```python
import os

import mymodule.new_package
import mymodule.old_package

print("this used to run")
```
the other two python files are empty.

With an older ruff:
```
$ ../ruff_venv/bin/ruff version
ruff 0.12.12 (c6516e9b6 2025-09-04)
$ ../ruff_venv/bin/ruff check .
All checks passed!
```
And with latest:
```
$ ../ruff_venv_new/bin/ruff version
ruff 0.13.3 (188c0dce2 2025-10-02)
$ ../ruff_venv_new/bin/ruff check .
I001 [*] Import block is un-sorted or un-formatted
 --> src/scripts/a_script.py:1:1
  |
1 | / import os
2 | |
3 | | import mymodule.new_package
4 | | import mymodule.old_package
  | |___________________________^
5 |
6 |   print("this used to run")
  |
help: Organize imports

Found 1 error.
[*] 1 fixable with the `--fix` option.
```
Letting it fix the error obviously gives
```python
import os

import mymodule.old_package

import mymodule.new_package

print("this used to run")
```

---

_Comment by @dylwil3 on 2025-10-06 14:59_

Got it, thank you! Yes, this is expected behavior. Moreover, `detect-same-package` isn't actually being triggered at all here inside `scripts`. That is possibly because your project layout is neither a flat layout nor a `src` layout but something nonstandard (i.e. one expects `src` to contain the top-level import packages, but I don't think `scripts` is a package here. Moreover, if it were, it would be a __different__ package than `mymodule` so `detect-same-package` would not apply).

In any event, I think the way to get the desired behavior in your case is to use [known-first-party](https://docs.astral.sh/ruff/settings/#lint_isort_known-first-party)

```toml
[tool.ruff.lint.isort]
known-first-party = ["mymodule"]
```



---

_Comment by @bkurtz on 2025-10-07 17:03_

Ah, I missed the first part of "**when analyzing files within the foo package**, any imports from within the foo package will be considered first-party" when reading the docs.  Okay, yeah, that seems like it works as documented then.

---

_Closed by @bkurtz on 2025-10-07 17:03_

---
