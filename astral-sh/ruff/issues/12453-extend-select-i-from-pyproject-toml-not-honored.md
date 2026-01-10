---
number: 12453
title: "`extend-select = [\"I\"]` from pyproject.toml not honored for tests in very specific circumstances"
type: issue
state: closed
author: jamesharr
labels: []
assignees: []
created_at: 2024-07-22T13:37:39Z
updated_at: 2024-07-22T15:03:22Z
url: https://github.com/astral-sh/ruff/issues/12453
synced_at: 2026-01-10T01:22:52Z
---

# `extend-select = ["I"]` from pyproject.toml not honored for tests in very specific circumstances

---

_Issue opened by @jamesharr on 2024-07-22 13:37_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

We've observed an odd behavior where the `extend-select = ["I"]` is not honored for code in `tests/` when a very specific set of circumstances occur. I'm not even sure I have the minimum reproduction steps completely down, but I can very consistently reproduce the situation.

1. The tests import items from specific module `mod1`
2. The python code structure starts at `src` instead of the repo root
3. A directory `mod1` exists at the repo root. This directory can be empty
4. This is the most bizarre part: This depends on the contents of the test files `tests/test_mod1.py`. Modifying the file contents can in some cases cause `extend-select` from `pyproject.toml` to be respected. Some of these are very insignificant changes, like insertion of a comment between imports, removal of a blank line between imports, etc.
5. Run `ruff check --no-cache` and observe that `tests/test_mod1.py` is not checked for `I001` as requested in `pyproject.toml`. Be sure to use `--no-cache` during tests because some actions will not invalidate the cache that otherwise would, like creating/removing the top-level `mod1` directory.

The minimum reproduction is below, but is also available as a git repository https://github.com/jamesharr/ruff-issue-12453

This bug was initially spotted due to a dirty working directory, and we eventually narrowed it down to an empty to-level folder that matches one of the modules a test imports. Of course git ignores empty directories, so we were chasing our tails on this one for a few days.

Ruff version: `ruff 0.5.4` on macOS 14.5, though we've also seen it with 0.5.2 and 0.5.3

Command: `ruff check --no-cache .`

Directory structure:
```sh
% tree .
.
├── README.md
├── mod1                   # Empty directory required for bug reproduction
├── pyproject.toml         # Sets "extend-select" to "I"
├── src
│   └── mod1
│       └── __init__.py
└── tests
    └── test_mod1.py       # File is not always check

5 directories, 4 files
```

Contents of `pyproject.toml`
```toml
[tool.poetry]
name = "ruff-bug"
version = "0.0.0"
description = "Ruff bug demo project"
authors = []
readme = "README.md"
packages = [
    { include = "mod1", from = "src" },
]

[tool.ruff.lint]
extend-select = ["I"]
```

Contents of `tests/test_mod1.py`
```py
def test_bug1():
    # For the bug to occur:
    # - [repo-root]/mod1 must exist as a directory; contents of directory do not seem to matter
    # - There must be an empty line after the first import import
    # - It cannot be two blank lines
    # - It cannot be line with a comment
    # - It can be a line consisting of only tabs/whitespaces
    # - All 3 imports must exist and in this order
    # - All 3 imports must exist within the function
    #
    # When the above conditions are met, then tests/* are NOT checked for I001 (unsorted imports)
    # as requested in pyproject.toml.
    #
    # Files in src/* are checked for I001 as requested in pyproject.toml, regardless of the contents
    # of this file.

    from external_lib import qux

    from mod1 import foo
    from mod1.bar import baz

    # I think the code below is only important because it uses the above imports
    print(qux)
    print(foo)
    print(baz)

% cat src/mod1/__init__.py
# The contents of this file seem to have no bearing bug reproduction
# - I001 seems to always be checked for this file, as specified in pyproject.toml
#
# You can illustrate this by re-ordering these imports
import os
import sys

# The following code only exists to use the imports and avoid F401 (imported but unused)
print(os)
print(sys)
```

---

_Comment by @charliermarsh on 2024-07-22 13:40_

A few quick comments:

- If you have `src` and `tests`, then imports from `src` won't be considered first-party unless you add the following to your `pyproject.toml`:

```toml
[tool.ruff]
src = ["src"]  # The default here is `src = ["."]`
```

- The presence of the `mod` at the top-level is likely causing you to treat imports of `mod` as first-party, because the default for `src` is `["."]` - -so it's finding `mod` at the root of your project.


---

_Comment by @charliermarsh on 2024-07-22 13:40_

Do you want imports of `src` modules in `tests` to be considered first-party or third-party?

---

_Comment by @jamesharr on 2024-07-22 13:58_

I believe I'm asking for `src` to be considered first-party (code that this repo owns). I'll try playing around with it and update the ticket appropriately. It makes sense to list that explicitly instead of relying on implicit detection.

There might still something bizarre going on here, because doing certain things to tests/ can cause `I001` to be checked, like messing with empty lines, moving imports up to the top-level.

---

_Comment by @charliermarsh on 2024-07-22 14:01_

Are you sure that `I001` isn't being _checked_? My guess is that it _is_ being checked, it's just not having any effect in your MRE because it considers the imports to be sorted already.

---

_Comment by @jamesharr on 2024-07-22 14:06_

You know what, I think you might be right. `I001` is being checked and this all is starting to make sense. The existence of that top-level directory is affecting the detection of the `src` setting, which is affecting what's being treated as a first/third-party import... Which is probably what you were getting at first.

What does MRE mean?

---

_Comment by @charliermarsh on 2024-07-22 14:08_

Apologies, MRE is "minimal reproducible example".

---

_Comment by @jamesharr on 2024-07-22 14:36_

So with my new found understanding, I'm trying to figure out if there's an issue worth keeping around or not, even if it's not this specific issue. I'm leaning towards "no", but I want to run my thoughts by you regardless:

1. Ultimately my tail-chasing revolves around me not understanding that I'm triggering an implicit behavior. I think there's value in that implicit behavior (so you don't need to specify the src directory every time), but I also wasn't aware that the first-party/third-party was changing.
    a. Is it worth detecting/notifying the user if there's ambiguity in first/third-party import detection?
    b. Is it worth adding `src` as part of the implicit first/third-party import detection?
2. More pragmatically, creation/deletion of `mod1` does not invalidate the cache. When I created `mod1`, if the previous `ruff check` passed, then it would report no issues even though it should if run without caching.

If any of these are worth keeping around, I can file a separate issue. If not, I'll close this ticket after I give my coworkers some time to skim over.

---

_Comment by @charliermarsh on 2024-07-22 14:40_

Well put!

I've been wanting to add `"src"` to the default `src` for a while now (which would've fixed this) -- but it's a breaking change, so it will need to be in the next minor. I'll create an issue for it now.

> More pragmatically, creation/deletion of mod1 does not invalidate the cache. When I created mod1, if the previous ruff check passed, then it would report no issues even though it should if run without caching.

Yeah this is a legitimate bug / footgun, I believe it's covered here: https://github.com/astral-sh/ruff/issues/10519.


---

_Referenced in [astral-sh/ruff#12454](../../astral-sh/ruff/issues/12454.md) on 2024-07-22 14:41_

---

_Comment by @charliermarsh on 2024-07-22 14:41_

I'm going to close out in favor of those other issues (e.g., https://github.com/astral-sh/ruff/issues/12454), thanks for taking the time to dig into this and for your thoughts.

---

_Closed by @charliermarsh on 2024-07-22 14:41_

---

_Comment by @jamesharr on 2024-07-22 15:03_

Thank you for taking the time to interact & help me understand, I appreciate the project's helpfulness

---

_Referenced in [astral-sh/ruff#12848](../../astral-sh/ruff/pulls/12848.md) on 2024-08-12 17:41_

---
