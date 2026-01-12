```yaml
number: 16424
title: "[pylint] Fix convert a code keyword argument to a positional argument (PLR1722)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - bug
assignees: []
merged: true
base: main
head: PLR1722
created_at: 2025-02-27T21:50:16Z
updated_at: 2025-03-03T14:31:20Z
url: https://github.com/astral-sh/ruff/pull/16424
synced_at: 2026-01-12T15:55:54Z
```

# [pylint] Fix convert a code keyword argument to a positional argument (PLR1722)

---

_@VascoSch92_

The PR addresses issue #16396 .

Specifically:

- If the exit statement contains a code keyword argument, it is converted into a positional argument.
- If retrieving the code from the exit statement is not possible, a violation is raised without suggesting a fix.

---

_Comment by @github-actions[bot] on 2025-02-27 22:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-02-27 23:20_

---

_Review requested from @ntBre by @MichaReiser on 2025-02-28 08:11_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs`:97 on 2025-02-28 19:55_

I think you can simplify this to:

```suggestion
    let arg = call.arguments.find_argument_value("code", 0);

    let code = if let Some(arg) = arg {
        &checker.source()[arg.range()]
    } else {
        // if it is not possible to retrieve the code, just a violation is raised
        checker.report_diagnostic(diagnostic);
        return;
    };
```

`Arguments::find_argument_value` is a really handy method for this kind of thing, and we can just slice the `checker.source` instead of doing the parsing stuff you have here. `find_argument_value` will already ensure the argument isn't starred (as in your nice `**dict` case). Additionally, this will allow us to accept variable arguments that are not starred. I'm surprised that none of the existing test cases had something like this:

```python
if success:
    code = 0
else:
    code = 1

exit(code)
```

We should add a test case where we fix that because I think it's a reasonable use case.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs`:105 on 2025-02-28 19:56_

If you accept my change above, you also need to remove the `:?` from the `code` format here to avoid  quotes around the result.

---

_@ntBre requested changes on 2025-02-28 20:00_

Thanks! I think we can simplify the implementation a bit here and also support more use cases at the same time.

---

_@ntBre reviewed on 2025-02-28 20:06_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs`:97 on 2025-02-28 20:06_

Even this is slightly overly-restrictive because it would exclude a valid starred expression like:

```python
import sys

code = [1]
sys.exit(*code)
```

but that's arguably an issue with `find_positional` itself, I think.

---

_Comment by @VascoSch92 on 2025-03-01 12:44_

Hey @ntBre 

thank you very much for the feedback. Yes, my solution was pretty convoluted. I suspected there it was a better way to do it but I could not find how ;-) 

Everything works fine.

I also added the test cases that you suggested and it seems that the output it is what expected. 

---

_Review requested from @ntBre by @VascoSch92 on 2025-03-01 12:44_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs`:72 on 2025-03-02 16:15_

Sorry for not catching this on the first round, but I just realized that with the new `format!` replace down below, we will end up replacing a call like `exit(2, 3, 4)` with just `exit(2)`. That's an invalid call to begin with, but I think we probably shouldn't drop the extra arguments, even in an unsafe fix.

The other unfortunate thing about `format!` is that we will drop the first two comments in a snippet like this:

```python
exit( # comment
    1, # 2
    ) # 3
```

This is a pretty contrived example, and I think simple calls like `exit(1)` are definitely the most common, but I think we can still handle it more carefully without too much more work.

As a result, I would propose this diff:

```diff
diff --git a/crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs b/crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs
index 41a32eafb..f9950567c 100644
--- a/crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs
+++ b/crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs
@@ -70,14 +70,17 @@ pub(crate) fn sys_exit_alias(checker: &Checker, call: &ExprCall) {
         call.func.range(),
     );

-    let arg = call.arguments.find_argument_value("code", 0);
-    let code = if let Some(arg) = arg {
-        &checker.source()[arg.range()]
-    } else {
-        // if it is not possible to retrieve the code, just a violation is raised
+    let has_star_kwargs = call
+        .arguments
+        .keywords
+        .iter()
+        .any(|kwarg| kwarg.arg.is_none());
+
+    // only one optional argument allowed, and we can't convert **kwargs
+    if call.arguments.len() > 1 || has_star_kwargs {
         checker.report_diagnostic(diagnostic);
         return;
-    };
+    }

     diagnostic.try_set_fix(|| {
         let (import_edit, binding) = checker.importer().get_or_import_symbol(
@@ -85,8 +88,15 @@ pub(crate) fn sys_exit_alias(checker: &Checker, call: &ExprCall) {
             call.func.start(),
             checker.semantic(),
         )?;
-        let reference_edit = Edit::range_replacement(format!("{binding}({code})"), call.range);
-        Ok(Fix::unsafe_edits(import_edit, [reference_edit]))
+        let reference_edit = Edit::range_replacement(binding, call.func.range());
+        let mut edits = vec![reference_edit];
+        if let Some(kwarg) = call.arguments.find_keyword("code") {
+            edits.push(Edit::range_replacement(
+                checker.source()[kwarg.value.range()].to_string(),
+                kwarg.range,
+            ));
+        };
+        Ok(Fix::unsafe_edits(import_edit, edits))
     });
     checker.report_diagnostic(diagnostic);
 }
```

Along with a few new test cases:

```python
# comments preserved with a positional argument
exit( # comment
    1, # 2
    ) # 3

# comments preserved with a single keyword argument
exit( # comment
    code=1, # 2
    ) # 3

# no diagnostic for multiple arguments
exit(2, 3, 4)

# this should now be fixable
codes = [1]
exit(*codes)
```

This will convert a `code` keyword without dropping any comments, avoid a diagnostic on weird calls like `exit(2, 3, 4)`, allow a fix for `*args`, and still avoid a fix for `**kwargs`. What do you think?

---

_@ntBre reviewed on 2025-03-02 16:15_

---

_@VascoSch92 reviewed on 2025-03-02 18:57_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs`:72 on 2025-03-02 18:57_

It makes sense. I start always from the point that the python source code is correct, but I understand your point. 

I added the new test cases and updated the code ;-) 

---

_Review requested from @ntBre by @VascoSch92 on 2025-03-02 18:58_

---

_@ntBre reviewed on 2025-03-03 14:19_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/sys_exit_alias.rs`:72 on 2025-03-03 14:19_

I think that's the right approach :) 

The main thing here was avoiding the `format!` call to save comments. We probably could have omitted the arguments length check but it seemed easy enough to throw in.

---

_@ntBre approved on 2025-03-03 14:20_

This looks great, thank you!

---

_Merged by @ntBre on 2025-03-03 14:20_

---

_Closed by @ntBre on 2025-03-03 14:20_

---

_Branch deleted on 2025-03-03 14:31_

---
