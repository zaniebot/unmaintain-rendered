```yaml
number: 17659
title: "[`flake8-pyi`] Ensure `Literal[None,] | Literal[None,]` is not autofixed to `None | None` (`PYI061`)"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-PYI061
created_at: 2025-04-27T21:38:57Z
updated_at: 2025-04-30T20:56:40Z
url: https://github.com/astral-sh/ruff/pull/17659
synced_at: 2026-01-10T18:57:02Z
```

# [`flake8-pyi`] Ensure `Literal[None,] | Literal[None,]` is not autofixed to `None | None` (`PYI061`)

---

_Pull request opened by @LaBatata101 on 2025-04-27 21:38_

Handles the case where the `None` literal inside `Literal` is part of a tuple, e.g. `List[None,]`. We don't create the fix for this case.

Fixes #16177.
<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
Snapshot tests.


---

_Review requested from @AlexWaygood by @LaBatata101 on 2025-04-27 21:38_

---

_Label `bug` added by @AlexWaygood on 2025-04-27 21:42_

---

_Label `fixes` added by @AlexWaygood on 2025-04-27 21:42_

---

_Comment by @github-actions[bot] on 2025-04-27 21:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-04-27 22:03_

Looking at it again, I think this maybe fixes things the wrong way. If we have a `foo.pyi` file like this:

```py
from typing import Literal

f: Literal[None,] | Literal[None,]
g: Literal[None,] | None
h: None | Literal[None,]
```

and I run `cargo run -- check foo.pyi --select=PYI061 --preview --no-cache --diff` from the `main` branch, Ruff says it would apply these fixes:

```diff
--- foo.pyi
+++ foo.pyi
@@ -1,5 +1,5 @@
 from typing import Literal
 
-f: Literal[None,] | Literal[None,]
+f: None | None
 g: Literal[None,] | None
 h: None | Literal[None,]
```

I.e., the second and third of these are fine, because this rule already knows that converting `Literal[None,] | None` to `None | None` will produce a `TypeError`. The reason why the first expression gets converted into one that will produce a `TypeError` is because there are actually two fixes being applied to the same expression as part of a _single iteration_ of diagnostics-plus-fixes from Ruff. The second autofix isn't aware of the changes that the first autofix made (it isn't aware that the first element in the union has already been converted to `None`) so it thinks it's still safe to convert the second element in the union to `None`.

That indicates to me that the better fix here is to ensure that each element's autofix is forced to happen in a separate iteration of the diagnostics-plus-autofixes loop that Ruff does until there are no more autofixes to apply. I.e., this diff applied to `main` also fixes the issue for me, and feels to me like it more directly addresses the cause of the problem here:

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs b/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs
index 16a48bc5f..e3fafac62 100644
--- a/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs
+++ b/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs
@@ -130,6 +130,9 @@ pub(crate) fn redundant_none_literal<'a>(checker: &Checker, literal_expr: &'a Ex
                 literal_elements.clone(),
                 union_kind,
             )
+            .map(|fix| {
+                fix.map(|fix| fix.isolate(Checker::isolation(semantic.current_statement_id())))
+            })
         });
         checker.report_diagnostic(diagnostic);
     }
```

---

_Comment by @LaBatata101 on 2025-04-27 22:18_

> Looking at it again, I think this maybe fixes things the wrong way. If we have a `foo.pyi` file like this:
> 
> ```python
> from typing import Literal
> 
> f: Literal[None,] | Literal[None,]
> g: Literal[None,] | None
> h: None | Literal[None,]
> ```
> 
> and I run `cargo run -- check foo.pyi --select=PYI061 --preview --no-cache --diff` from the `main` branch, Ruff says it would apply these fixes:
> 
> ```diff
> --- foo.pyi
> +++ foo.pyi
> @@ -1,5 +1,5 @@
>  from typing import Literal
>  
> -f: Literal[None,] | Literal[None,]
> +f: None | None
>  g: Literal[None,] | None
>  h: None | Literal[None,]
> ```
> 
> I.e., the second and third of these are fine, because this rule already knows that converting `Literal[None,] | None` to `None | None` will produce a `TypeError`. The reason why the first expression gets converted into one that will produce a `TypeError` is because there are actually two fixes being applied to the same expression as part of a _single iteration_ of diagnostics-plus-fixes from Ruff. The second autofix isn't aware of the changes that the first autofix made (it isn't aware that the first element in the union has already been converted to `None`) so it thinks it's still safe to convert the second element in the union to `None`.
> 
> That indicates to me that the better fix here is to ensure that each element's autofix is forced to happen in a separate iteration of the diagnostics-plus-autofixes loop that Ruff does until there are no more autofixes to apply. I.e., this diff applied to `main` also fixes the issue for me, and feels to me like it more directly addresses the cause of the problem here:
> 
> ```diff
> diff --git a/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs b/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs
> index 16a48bc5f..e3fafac62 100644
> --- a/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs
> +++ b/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs
> @@ -130,6 +130,9 @@ pub(crate) fn redundant_none_literal<'a>(checker: &Checker, literal_expr: &'a Ex
>                  literal_elements.clone(),
>                  union_kind,
>              )
> +            .map(|fix| {
> +                fix.map(|fix| fix.isolate(Checker::isolation(semantic.current_statement_id())))
> +            })
>          });
>          checker.report_diagnostic(diagnostic);
>      }
> ```

Makes sense, thanks!

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-04-28 06:39_

---

_Comment by @AlexWaygood on 2025-04-28 11:13_

I realised that we could simplify some of the logic added in https://github.com/astral-sh/ruff/pull/14583, because ensuring that the individual autofixes are isolated provides a more general fix! I pushed the change to your branch here

---

_Renamed from "[`flake8-pyi`] Improve autofix safety for `PYI061`" to "[`flake8-pyi`] Ensure `Literal[None,] | Literal[None,]` is not autofixed to `None | None` (`PYI061`)" by @AlexWaygood on 2025-04-28 11:19_

---

_Comment by @AlexWaygood on 2025-04-28 11:23_

Thank you!

---

_Merged by @AlexWaygood on 2025-04-28 11:23_

---

_Closed by @AlexWaygood on 2025-04-28 11:23_

---

_Branch deleted on 2025-04-30 20:56_

---
