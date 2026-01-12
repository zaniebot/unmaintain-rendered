```yaml
number: 4742
title: Add autofix for flake8-type-checking
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/add-typing-import
created_at: 2023-05-31T03:17:32Z
updated_at: 2023-05-31T17:53:37Z
url: https://github.com/astral-sh/ruff/pull/4742
synced_at: 2026-01-12T03:50:03Z
```

# Add autofix for flake8-type-checking

---

_Pull request opened by @charliermarsh on 2023-05-31 03:17_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR enables autofix for the most useful flake8-type-checking rules -- those that detect imports that are _only_ used in type-checking contexts, and suggests that users move them into `if TYPE_CHECKING:` blocks. In the end, it's not a _ton_ of code (most of this diff is tests + snapshots), but there's a large chain of changes that were required to support this.

The general flow is as follows. When we detect a typing-only import, we generate a new fixes, which consists of up to three edits: (1) an edit to remove the imported member from the import statement; (2) an edit to import `TYPE_CHECKING` from `typing`; and (3) an edit to add the imported member to the `TYPE_CHECKING` block, which could also require creating an entirely new `TYPE_CHECKING` block.

For example, given:

```py
from __future__ import annotations

import pandas as pd

def f(x: pd.DataFrame):
    pass
```

We need to make all three of those changes, in order to get to:

```py
from __future__ import annotations

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    import pandas as pd

def f(x: pd.DataFrame):
    pass
```

The exact edit depends on the ordering and existence of the `typing` import, and/or the `if TYPE_CHECKING` blocks, and their positions relative to the first runtime usage -- but in typical usages, the edit looks something like the above.

Closes #2329.

## Test Plan

- Ran `cargo run -p ruff_cli -- check ../airflow --select TCH -n --fix`; verified by eye that the edits looked reasonable and correct, and that there were no panics.
- Ran `cargo run -p ruff_cli -- check ../rotki --select TCH -n --fix`; verified by eye that the edits looked reasonable and correct, and that there were no panics.
- Ran `cargo run -p ruff_cli -- check ../<project> --select TCH -n --fix` over a few of my personal projects for which I have `mypy` setup, and verified that `mypy` passed without error before and after.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-31 03:17_

---

_Review requested from @konstin by @charliermarsh on 2023-05-31 03:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/codemods.rs`:130 on 2023-05-31 03:18_

(This is effectively the inverse of `remove_imports`, above.)

---

_@charliermarsh reviewed on 2023-05-31 03:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/codemods.rs`:201 on 2023-05-31 03:18_

Using LibCST here means we should generally retain comments.

---

_@charliermarsh reviewed on 2023-05-31 03:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:372 on 2023-05-31 03:21_

Note that we're generating an edit for every diagnostic, so we're generating an edit for every member in an `StmtKind::ImportFrom` statement. We may want to generate one edit for the entire statement, like we do for unused imports... But this does work correctly in practice. The main issue is that if you have:

```py
from foo import bar, bar
```

Then after the fixes, we'll probably give you:

```py
if TYPE_CHECKING:
    from foo import bar
    from foo import baz
```

These will get merged when you run the `isort` rules, so most users will never see this state (or will have it immediately corrected), but I guess it's not ideal (and it does mean that if you're using the LSP, you need to take one Code Action per member).

---

_@charliermarsh reviewed on 2023-05-31 03:21_

---

_@charliermarsh reviewed on 2023-05-31 03:21_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:132 on 2023-05-31 03:21_

Open to suggestions, but given that we're generating three edits for the fix, and they can come in almost any order, this feels like the easiest solution.

---

_@charliermarsh reviewed on 2023-05-31 03:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:372 on 2023-05-31 03:22_

The alternative, as hinted above, is that we generate one edit for the entire import statement. We may also want to then generate one _diagnostic_ for the entire import statement? So this would take some refactoring.

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/mod.rs`:83 on 2023-05-31 07:43_

Nit: I would expect `to_typing_import` to return a `TypingImport`. Maybe `to_typing_import_edits`? 

Can we document the return type? Why does it return a tuple? What's the first/second edit? Or maybe return a struct here with named fields (and call it `TypingImport` and you're good with `to_typing_import` ;))

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_type_checking/mod.rs`:275 on 2023-05-31 07:44_

Can you add a test that contains some comments in the type checking block?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:375 on 2023-05-31 07:45_

Please document why calling `unwrap` here is safe

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:376 on 2023-05-31 07:45_

This code looks (identical?) to your other PR. Is there an opportunity to share some of the logic?

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:132 on 2023-05-31 07:46_

I think I would prefer storing the `Edit`s in sorted order internally if this is a constraint that must be uphold. Pushing the responsibility onto rule authors leaks abstraction in my view

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:372 on 2023-05-31 07:48_

Merging the diagnostics on a per-statement level seems desirable to me but we can do this as a separate PR. 

---

_@MichaReiser approved on 2023-05-31 07:48_

---

_Review comment by @konstin on `crates/ruff/src/autofix/codemods.rs`:163 on 2023-05-31 08:35_

nit: can you move this match one scope higher so it's outside the iterator?

---

_Review comment by @konstin on `crates/ruff/src/autofix/codemods.rs`:206 on 2023-05-31 09:02_

https://github.com/charliermarsh/ruff/pull/4749

---

_Review comment by @konstin on `crates/ruff/src/importer/mod.rs`:333 on 2023-05-31 09:29_

```suggestion
    /// Return the type checking block that precedes the given position, if any.
```

---

_Review comment by @konstin on `crates/ruff/src/importer/mod.rs`:338 on 2023-05-31 09:31_

Are there cases where we want to use anything but the first type checking block? Type checking blocks don't suffer from circular imports. Shadowing could be a thing but i'm not aware of any cases

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_type_checking/snapshots/ruff__rules__flake8_type_checking__tests__exempt_modules.snap`:18 on 2023-05-31 09:47_

nit: is there a reason why we have blank line after the top level import but not after the type checking block? (Seems to work everywhere but here anyway)

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_type_checking/snapshots/ruff__rules__flake8_type_checking__tests__import_from.snap`:25 on 2023-05-31 09:48_

really cool that you manage to keep those comments around

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_type_checking/snapshots/ruff__rules__flake8_type_checking__tests__type_checking_block_after_usage.snap`:4 on 2023-05-31 09:52_

nit: we could name those `type_checking_block_after_usage.py`

---

_@konstin approved on 2023-05-31 09:54_

---

_Comment by @konstin on 2023-05-31 10:07_

ecosystem ci looks good (https://gist.github.com/konstin/cf6f633a73a701bc94b089d2066e4675), most interesting case was https://github.com/apache/airflow/blob/27001a23718d6b8b5118eb130be84713af9a4477/airflow/models/__init__.py (module level `__getattr__` with mypy)

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:132 on 2023-05-31 17:18_

https://github.com/charliermarsh/ruff/pull/4762

---

_@charliermarsh reviewed on 2023-05-31 17:19_

---

_@charliermarsh reviewed on 2023-05-31 17:38_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/codemods.rs`:163 on 2023-05-31 17:38_

I thought we could, but it turns out we can't because we need to resolve relative imports within the block.

---

_@charliermarsh reviewed on 2023-05-31 17:40_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer/mod.rs`:338 on 2023-05-31 17:40_

Hmm, I guess not. Changed it!

---

_@charliermarsh reviewed on 2023-05-31 17:45_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:376 on 2023-05-31 17:45_

We have these everywhere that we do statement deletion :(

---

_Merged by @charliermarsh on 2023-05-31 17:53_

---

_Closed by @charliermarsh on 2023-05-31 17:53_

---

_Branch deleted on 2023-05-31 17:53_

---
