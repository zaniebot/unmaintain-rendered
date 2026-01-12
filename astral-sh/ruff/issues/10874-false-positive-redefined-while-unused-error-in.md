```yaml
number: 10874
title: "False-positive \"redefined while unused\" error in stub files for constants in class scopes with the same name as module constants"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - linter
assignees: []
created_at: 2024-04-11T09:44:15Z
updated_at: 2024-12-03T12:51:51Z
url: https://github.com/astral-sh/ruff/issues/10874
synced_at: 2026-01-12T15:54:50Z
```

# False-positive "redefined while unused" error in stub files for constants in class scopes with the same name as module constants

---

_@AlexWaygood_

Ruff currently emits a false positive F811 on the following stub file:

```py
from x import y as y

class Foo:
    y = 42  # F811 Redefinition of unused `y` from line 1
```

There are really two `y` variables here -- one scoped within the `Foo` class, and one scoped within the module -- so I don't think this should trigger F811. Assignments without annotations in stub files are rare (which is why this hasn't come up before), but can be useful in enum classes. This bug came up in https://github.com/python/typeshed/pull/11299.

---

_Label `bug` added by @AlexWaygood on 2024-04-11 09:44_

---

_Label `linter` added by @AlexWaygood on 2024-04-11 09:44_

---

_Comment by @Avasam on 2024-04-16 17:20_

The same happens for methods as well.
Two examples from typeshed:
- https://github.com/python/typeshed/blob/7d56cd9a6cf6e0a4ea89c68d0397e197aff32cbe/stdlib/socket.pyi#L700
- https://github.com/python/typeshed/blob/7d56cd9a6cf6e0a4ea89c68d0397e197aff32cbe/stubs/pysftp/pysftp/__init__.pyi#L79

---

_Comment by @AlexWaygood on 2024-04-16 17:22_

> The same happens for methods as well.

Ah, thanks, that makes this slightly higher priority!

---

_Comment by @dylwil3 on 2024-12-03 06:32_

A couple questions:

1. Why does the complaint only apply to stub files? That doesn't look like a redefinition to me in an ordinary python file either.
2. In general, it's confusing to me that this rule applies across scopes when the purported "redefinition" of an import has binding kind different than an import. Why is that?

If the answer to (2) ends up being "it shouldn't", then that's an easy fix:


```diff
diff --git a/crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs b/crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs
index ddc7fa54b..7611c3adb 100644
--- a/crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs
+++ b/crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs
@@ -260,6 +260,16 @@ pub(crate) fn deferred_scopes(checker: &mut Checker) {
                         ) {
                             continue;
                         }
+                        // And then only if the new binding is an import
+                        if !matches!(
+                            binding.kind,
+                            BindingKind::Import(..)
+                                | BindingKind::FromImport(..)
+                                | BindingKind::SubmoduleImport(..)
+                                | BindingKind::FutureImport
+                        ) {
+                            continue;
+                        }
                     }
 
                     // If the bindings are in different forks, abort.

```

---

_Comment by @AlexWaygood on 2024-12-03 12:51_

F811 is a pretty interesting rule. It's sort-of delightfully simple conceptually, but it can catch a lot of bugs.

Consider a module that consists just of the following:

```py
from foo import x

x = 42
print(x)
```

That code won't flag an unused-import rule in any linter, because an `x` variable is imported, and then an `x` variable _is_ used in the `print()` statement. But F811 spots that there were 0 uses of the variable in between the two definitions, so flags this code as likely being buggy. The fix here is obviously to remove the import statement on the first line, but this is hard for a static analyzer to figure out with any degree of confidence. The best it can say is that there's likely a bug here, but it's up to the user to fix it.

There's also a common case where you accidentally have two methods with the same name in a large class. The second method entirely overrides the first one, so if it's a `unittest.TestCase` class, for example, the first test method will be "silently skipped":

```py
from unittest import TestCase

class FooTests(TestCase):
    def test_fooey_stuff(self):
        ...  # important assertions

    # many hundreds of lines of tests

    # oops! this completely overrides the earlier definition
    def test_fooey_stuff(self):
        ...  # different assertions
```

So this rule is pretty useful for non-import bindings as well as import bindings. I think the reason we exclude cross-scope redefinitions for all non-import bindings is that those are entirely covered by other rules.

However, let's go back to the original example here:

> ```py
> from x import y as y
> 
> class Foo:
>     y = 42  # F811 Redefinition of unused `y` from line 1
> ```

Here, the "redundant alias" marks the import of `y` as an [explicit re-export](https://typing.readthedocs.io/en/latest/spec/distributing.html#import-conventions), a part of this module's public API. So in this case (and also if `y` were included in `__all__`), I agree that this should _not_ be flagged by F811 either in a stub file _or_ at runtime. Great point @dylwil3!

---
