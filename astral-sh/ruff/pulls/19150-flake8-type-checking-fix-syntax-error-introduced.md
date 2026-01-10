```yaml
number: 19150
title: "[`flake8-type-checking`] Fix syntax error introduced by fix (`TC008`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-TC008-fix-syntax-error
created_at: 2025-07-04T20:19:01Z
updated_at: 2025-07-07T20:37:58Z
url: https://github.com/astral-sh/ruff/pull/19150
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-type-checking`] Fix syntax error introduced by fix (`TC008`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-04 20:19_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

I noticed this while working on #18972. If the string targeted by [quoted-type-alias (TC008)](https://docs.astral.sh/ruff/rules/quoted-type-alias/#quoted-type-alias-tc008) is a multiline string, the fix would introduce a syntax error. This PR fixes that by adding parenthesis around the resulting replacement if the string contained any newline characters (`\n`, `\r`) if it doesn't already have parenthesis outside `("""...""")` or inside `"""(...)"""` the annotation.

Failing examples: https://play.ruff.rs/8793eb95-860a-4bb3-9cbc-6a042fee2946
```
PS D:\rust_projects\ruff> Get-Content issue.py
```
```py
from typing import TypeAlias

OptInt: TypeAlias = """int
| None"""

type OptInt = """int
| None"""
```
```
PS D:\rust_projects\ruff> uvx ruff check issue.py --isolated --select TC008 --fix --diff --preview
```
```

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `issue.py`, the rule codes TC008, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```

This PR also makes the example error out-of-the-box for #18972

Old example: https://play.ruff.rs/f6cd5adb-7f9b-444d-bb3e-8c045241d93e
```py
OptInt: TypeAlias = "int | None"
```

New example: https://play.ruff.rs/906c1056-72c0-4777-b70b-2114eb9e6eaf
```py
from typing import TypeAlias

OptInt: TypeAlias = "int | None"
```

The import was also added to the "Use instead" section.

## Test Plan

<!-- How was it tested? -->

Added multiple test cases

---

_Renamed from "[`flake8-typechecking" to "[`flake8-type-checking`] Fix syntax error introduced by fix (`TC008`)" by @MeGaGiGaGon on 2025-07-04 20:24_

---

_Comment by @github-actions[bot] on 2025-07-04 20:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @AlexWaygood on 2025-07-05 13:56_

---

_Label `fixes` added by @AlexWaygood on 2025-07-05 13:56_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TC008.py`:32 on 2025-07-07 14:11_

If you move these fixtures to the end of the file the diff will be smaller

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:296 on 2025-07-07 14:13_

1. let's use `checker.indexer().multiline_ranges()` here
2. we should avoid adding parentheses if we don't need to. So within this branch maybe check whether the expression is already parenthesized using `ruff_python_ast::parenthesize::parenthesized_range`

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TC008.py`:49 on 2025-07-07 14:16_

Can you add a test for a triple-quoted, multiline annotation that _already_ has parentheses? Also, perhaps more awkwardly, one where the parentheses are on the _inside_ of the quote?

---

_@dylwil3 requested changes on 2025-07-07 14:16_

---

_@MeGaGiGaGon reviewed on 2025-07-07 18:32_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:296 on 2025-07-07 18:32_

Is there another test I should use in addition to `checker.indexer().multiline_ranges()`? Since when I swap over to that it causes a single line multiline string to also get parenthesized:
```diff
-r: TypeAlias = """int | None"""
+r: TypeAlias = (int | None)
-type R = """int | None"""
+type R = (int | None)
```

---

_@MeGaGiGaGon reviewed on 2025-07-07 19:02_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TC008.py`:32 on 2025-07-07 19:02_

Moved

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TC008.py`:49 on 2025-07-07 19:03_

I added those as test cases, and used `ruff_python_ast::parenthesize::parenthesized_range` to make sure parenthesis aren't added in those cases.

---

_@MeGaGiGaGon reviewed on 2025-07-07 19:03_

---

_@dylwil3 reviewed on 2025-07-07 19:46_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:296 on 2025-07-07 19:46_

Oh no! My bad 😅 , you're right and what you had previously was correct. Apologies!

---

_@dylwil3 reviewed on 2025-07-07 19:49_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:296 on 2025-07-07 19:49_

But I could've sworn we had _some_ utility method that already knows about newlines even within strings... let me search a bit harder

---

_@dylwil3 reviewed on 2025-07-07 20:32_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:296 on 2025-07-07 20:32_

Couldn't find it stored someplace - so this looks good to me!

---

_@dylwil3 approved on 2025-07-07 20:33_

Thank you!

---

_Merged by @dylwil3 on 2025-07-07 20:34_

---

_Closed by @dylwil3 on 2025-07-07 20:34_

---

_Branch deleted on 2025-07-07 20:37_

---
