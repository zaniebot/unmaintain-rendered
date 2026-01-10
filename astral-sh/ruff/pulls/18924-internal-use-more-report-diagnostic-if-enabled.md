```yaml
number: 18924
title: "[Internal] Use more `report_diagnostic_if_enabled`"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - internal
assignees: []
merged: true
base: main
head: use-more-report_diagnostic_if_enabled
created_at: 2025-06-24T20:27:31Z
updated_at: 2025-06-25T01:43:55Z
url: https://github.com/astral-sh/ruff/pull/18924
synced_at: 2026-01-10T18:39:09Z
```

# [Internal] Use more `report_diagnostic_if_enabled`

---

_Pull request opened by @MeGaGiGaGon on 2025-06-24 20:27_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

From @ntBre https://github.com/astral-sh/ruff/pull/18906#discussion_r2162843366 :
> This could be a good target for a follow-up PR, but we could fold these `if checker.is_rule_enabled { checker.report_diagnostic` checks into calls to `checker.report_diagnostic_if_enabled`. I didn't notice these when adding that method.
> 
> Also, the docs on `Checker::report_diagnostic_if_enabled` and `LintContext::report_diagnostic_if_enabled` are outdated now that the `Rule` conversion is basically free 😅
> 
> No pressure to take on this refactor, just an idea if you're interested!

This PR folds those calls. I also updated the doc comments by copying from `report_diagnostic`.

Note: It seems odd to me that the doc comment for `Checker` says `Diagnostic` while `LintContext` says `OldDiagnostic`, not sure if that needs a bigger docs change to fix the inconsistency.

<details>
<summary>Python script to do the changes</summary>

This script assumes it is placed in the top level `ruff` directory (ie next to `.git`/`crates`/`README.md`)

```py
import re
from copy import copy
from pathlib import Path

ruff_crates = Path(__file__).parent / "crates"

for path in ruff_crates.rglob("**/*.rs"):
    with path.open(encoding="utf-8", newline="") as f:
        original_content = f.read()
    if "is_rule_enabled" not in original_content or "report_diagnostic" not in original_content:
        continue
    original_content_position = 0
    changed_content = ""
    for match in re.finditer(r"(?m)(?:^[ \n]*|(?<=(?P<else>else )))if[ \n]+checker[ \n]*\.is_rule_enabled\([ \n]*Rule::\w+[ \n]*\)[ \n]*{[ \n]*checker\.report_diagnostic\(", original_content):
        # Content between last match and start of this one is unchanged
        changed_content += original_content[original_content_position:match.start()]
        # If this was an else if, a { needs to be added at the start
        if match.group("else"):
            changed_content += "{"
        # This will result in bad formatting, but the precommit cargo format will handle it
        changed_content += "checker.report_diagnostic_if_enabled("
        # Depth tracking would fail if a string/comment included a { or }, but unlikely given the context
        depth = 1
        position = match.end()
        while depth > 0:
            if original_content[position] == "{":
                depth += 1
            if original_content[position] == "}":
                depth -= 1
            position += 1
        # pos - 1 is the closing }
        changed_content += original_content[match.end():position - 1]
        # If this was an else if, a } needs to be added at the end
        if match.group("else"):
            changed_content += "}"
        # Skip the closing }
        original_content_position = position
        if original_content[original_content_position] == "\n":
            # If the } is followed by a \n, also skip it for better formatting
            original_content_position += 1
    # Add remaining content between last match and file end
    changed_content += original_content[original_content_position:]
    with path.open("w", encoding="utf-8", newline="") as f:
        f.write(changed_content)
```

</details>

## Test Plan

<!-- How was it tested? -->

N/A, no tests/functionality affected.

---

_Review requested from @AlexWaygood by @MeGaGiGaGon on 2025-06-24 20:27_

---

_Review requested from @ntBre by @AlexWaygood on 2025-06-24 20:29_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-24 20:29_

---

_Comment by @github-actions[bot] on 2025-06-24 20:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @ntBre on 2025-06-24 20:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:404 on 2025-06-24 20:46_

You're right, these should all say `OldDiagnostic`, at least temporarily. We're in the middle of refactoring the diagnostic infrastructure, so before long it will be back to (a different) `Diagnostic` but there shouldn't be a `Diagnostic` type in scope right now, so I'm surprised our doctests don't complain about this.

```suggestion
    /// The guard derefs to an [`OldDiagnostic`], so it can be used to further modify the diagnostic
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:3165 on 2025-06-24 20:50_

```suggestion
    /// before it is added to the collection in the context on `Drop`.
```

---

_@ntBre approved on 2025-06-24 21:02_

This looks great, thank you! I just had a couple of small suggestions on the docs.

Also, I probably should have mentioned this earlier (sorry!), and it looks like your script did a good job, but this is a nice use case for [ast-grep](https://ast-grep.github.io/). I just started using it recently, and it's really handy. Something like this would probably work too:

```shell
ast-grep \
-p 'if checker.is_rule_enabled($RULE) { checker.report_diagnostic($KIND, $RANGE); }' \
-r 'checker.report_diagnostic_if_enabled($KIND, $RANGE);'
```

It only found two additional cases, and one of them I had to clean up manually since it was in an `else-if`.

<details><summary>Diff</summary>

```diff
diff --git a/crates/ruff_linter/src/checkers/ast/analyze/expression.rs b/crates/ruff_linter/src/checkers/ast/analyze/expression.rs
index 21e8aa0d11..8b094b346f 100644
--- a/crates/ruff_linter/src/checkers/ast/analyze/expression.rs
+++ b/crates/ruff_linter/src/checkers/ast/analyze/expression.rs
@@ -1311,16 +1311,12 @@ pub(crate) fn expression(expr: &Expr, checker: &Checker) {
                             typ: CFormatErrorType::UnsupportedFormatChar(c),
                             ..
                         }) => {
-                            if checker
-                                .is_rule_enabled(Rule::PercentFormatUnsupportedFormatCharacter)
-                            {
-                                checker.report_diagnostic(
-                                    pyflakes::rules::PercentFormatUnsupportedFormatCharacter {
-                                        char: c,
-                                    },
-                                    location,
-                                );
-                            }
+                            checker.report_diagnostic_if_enabled(
+                                pyflakes::rules::PercentFormatUnsupportedFormatCharacter {
+                                    char: c,
+                                },
+                                location,
+                            );
                         }
                         Err(e) => {
                             checker.report_diagnostic_if_enabled(
diff --git a/crates/ruff_linter/src/rules/flake8_2020/rules/compare.rs b/crates/ruff_linter/src/rules/flake8_2020/rules/compare.rs
index 01751361da..12c18a2dd7 100644
--- a/crates/ruff_linter/src/rules/flake8_2020/rules/compare.rs
+++ b/crates/ruff_linter/src/rules/flake8_2020/rules/compare.rs
@@ -307,8 +307,8 @@ pub(crate) fn compare(checker: &Checker, left: &Expr, ops: &[CmpOp], comparators
         {
             if value.len() == 1 {
                 checker.report_diagnostic_if_enabled(SysVersionCmpStr10, left.range());
-            } else if checker.is_rule_enabled(Rule::SysVersionCmpStr3) {
-                checker.report_diagnostic(SysVersionCmpStr3, left.range());
+            } else {
+                checker.report_diagnostic_if_enabled(SysVersionCmpStr3, left.range());
             }
         }
     }
```

</details>

---

_@MeGaGiGaGon reviewed on 2025-06-24 21:07_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/checkers/ast/mod.rs`:3165 on 2025-06-24 21:07_

Should it be `context` or `checker`? Since the `Checker` docs say `checker` there instead of `collector` or `context `

---

_@ntBre reviewed on 2025-06-24 21:13_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:3165 on 2025-06-24 21:13_

The diagnostics are actually in the `LintContext`, so in a literal sense they should both say `context`, but callers of the `Checker` version don't really need to know that. So I think it makes sense to say `checker` on line 405 and `context` here. What do you think?

---

_@MeGaGiGaGon reviewed on 2025-06-24 21:20_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/checkers/ast/mod.rs`:3165 on 2025-06-24 21:20_

Sounds reasonable, all this code is new to me so if it makes sense to you then it's good enough for me.

---

_Comment by @MeGaGiGaGon on 2025-06-24 21:40_

Docs updated to fix the inconsistencies, and I updated the script to handle those cases. `ast-grep` looks like a really cool alternative to my normal regex based shenanigans, I'll have to try it out if I do something like this again.

---

_@ntBre approved on 2025-06-25 01:21_

Nice, thank you!

I think I should have merged this before your rule code changes :grimacing: I hope the merge conflicts aren't too much of a pain. I'm happy to resolve them if you want.

---

_Comment by @MeGaGiGaGon on 2025-06-25 01:35_

I'm surprised there were any merge conflicts, I thought git would have been smarter. There weren't too many and they were all easy to resolve (assuming/hoping I did it right)

---

_Merged by @ntBre on 2025-06-25 01:43_

---

_Closed by @ntBre on 2025-06-25 01:43_

---

_Branch deleted on 2025-06-25 01:43_

---
