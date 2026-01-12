```yaml
number: 18830
title: "[ty] Add completions for `from module import <CURSOR>`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/import-completions
created_at: 2025-06-20T18:48:52Z
updated_at: 2025-06-23T14:43:26Z
url: https://github.com/astral-sh/ruff/pull/18830
synced_at: 2026-01-12T15:56:26Z
```

# [ty] Add completions for `from module import <CURSOR>`

---

_@BurntSushi_

There were two main challenges in this PR.

The first was mostly just figuring out how to get the symbols
corresponding to `module`. It turns out that we do this in a couple
of places in ty already, but through different means. In one approach,
we use [`exported_names`]. In another approach, we get a `Type`
corresponding to the module. We take the latter approach here, which is
consistent with how we do completions elsewhere. (I looked into
factoring this logic out into its own function, but it ended up being
pretty constrained. e.g., There's only one other place where we want to
go from `ast::StmtImportFrom` to a module `Type`, and that code also
wants the module name.)

The second challenge was recognizing the `from module import <CURSOR>`
pattern in the code. I initially started with some fixed token patterns
to get a proof of concept working. But I ended up switching to mini
state machine over tokens. I looked at the parser for `StmtImportFrom`
to determine what kinds of tokens we can expect.

[`exported_names`]: https://github.com/astral-sh/ruff/blob/23a3b6ef2337c8a2c1496ebf68d12582f0b0706d/crates/ty_python_semantic/src/semantic_index/re_exports.rs#L47


---

_Review requested from @carljm by @BurntSushi on 2025-06-20 18:48_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-06-20 18:48_

---

_Review requested from @sharkdp by @BurntSushi on 2025-06-20 18:48_

---

_Review requested from @dcreager by @BurntSushi on 2025-06-20 18:48_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-06-20 18:48_

---

_Review request for @dcreager removed by @BurntSushi on 2025-06-20 18:49_

---

_Review request for @carljm removed by @BurntSushi on 2025-06-20 18:49_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-06-20 18:49_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-06-20 18:49_

---

_Comment by @BurntSushi on 2025-06-20 18:49_

Demo video (featuring VS Code):

https://github.com/user-attachments/assets/6a377d49-ec5b-40e1-a3da-5fb7ecd4f8d5

(Doing this made me realize that VS Code seems to be sorting completions lexicographically, despite the fact that ty is returning completions in a different order. I still have to investigate that.)


---

_Comment by @github-actions[bot] on 2025-06-20 18:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `ty` added by @ntBre on 2025-06-20 19:20_

---

_Label `server` added by @AlexWaygood on 2025-06-20 21:07_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1 on 2025-06-20 21:11_

For completeness's sake, I feel like it would be ideal if we could also have a couple of tests for relative imports (where `level` is not 0). But I know that that's kind of a pain to setup given how the tests currently work.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:67 on 2025-06-20 21:28_

this looks great to me! I think the one thing it misses is that as well as _members_ of the module, we should _also_ suggest submodules. It wouldn't be correct to e.g. include `resources` in the result of `all_members(<module 'importlib'>)`, because it's _not_ a member of `importlib`:

```pycon
>>> import importlib
>>> importlib.resources
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    importlib.resources
AttributeError: module 'importlib' has no attribute 'resources'
```

But you _can_ do this!

```pycon
>>> from importlib import resources
>>> 
```

(Note that due to some ugly side effects of Python's import system, _some_ submodules _are_ available as attributes on their parent modules at import time, such as e.g. `collections.abc`. But that isn't generally true.)

I haven't thought too hard about how we could get the list of submodules of a parent module, but I could put some thought into that over the weekend or next week if it would be helpful!

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:293 on 2025-06-20 21:33_

I suppose an ellipsis token would be "part of a module" for something like `from ...foo import bar`?

---

_@AlexWaygood approved on 2025-06-20 21:35_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:93 on 2025-06-23 06:56_

Remove?

---

_@sharkdp reviewed on 2025-06-23 06:56_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:34 on 2025-06-23 06:57_

Remove? Or add some more information (what is printed)?

---

_@sharkdp reviewed on 2025-06-23 06:57_

---

_@sharkdp reviewed on 2025-06-23 07:06_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:255 on 2025-06-23 07:06_

Is it possible for this function to be called for more or less arbitrary positions in the source file? Could it be triggered from a completion request at the end of a very long file? Because it looks like it might walk back arbitrarily long distances (if no `from` token is ever encountered), potentially making completions slow?

---

_@MichaReiser reviewed on 2025-06-23 11:17_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:302 on 2025-06-23 11:17_

Allowing `Unknown` makes sense. E.g. the lexer emits `Unknown` for any token it doesn't recognize or if there's something "wrong" with the token (e.g. an. unterminated string): 

This creates two unknown tokens:

```py
from test import (
    ??
)
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:293 on 2025-06-23 11:18_

Do we also need to allow `Unknown` here?

```py
from ? import (
    
)
```

---

_@MichaReiser reviewed on 2025-06-23 11:18_

---

_@MichaReiser approved on 2025-06-23 11:20_

Nice!

---

_@BurntSushi reviewed on 2025-06-23 13:17_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/semantic_model.rs`:67 on 2025-06-23 13:17_

Yeah I do think that would be helpful! Thank you. I'll also add this to my notes.

I think `os.path` is another one.

Interestingly, if I do this in `bpython`, it lets me do `importlib.resources`:

```
bpython version 0.25 on top of Python 3.13.3 /usr/bin/python
>>> import importlib
>>> importlib.resources
<module 'importlib.resources' from '/usr/lib/python3.13/importlib/resour
ces/__init__.py'>
```

But in a standard Python interpreter, no dice:

```
Python 3.13.3 (main, Apr  9 2025, 07:44:25) [GCC 14.2.1 20250207] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import importlib
>>> importlib.resources
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    importlib.resources
AttributeError: module 'importlib' has no attribute 'resources'
>>>
```

I wonder if `bpython` is auto-importing sub-modules.

---

_@BurntSushi reviewed on 2025-06-23 13:20_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/semantic_model.rs`:67 on 2025-06-23 13:20_

The other side to this is that

```
import importlib.resources
importlib.<CURSOR>
```

should include `resources` in the completions offered.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:67 on 2025-06-23 13:23_

> Interestingly, if I do this in `bpython`, it lets me do `importlib.resources`:

This is almost certainly because `bpython` is written in Python, and somewhere deep in the guts of `bpython` (or some dependency of `bpython`) there's either an `import importlib.resources` statement, or a `from importlib.resources import <whatever>` statement. Either would result in the `importlib.resources` submodule being cached as a `.resources` attribute on the `importlib` module.

This is exactly what I meant above when I mentioned "ugly side effects" of Python's import system 😄

---

_@AlexWaygood reviewed on 2025-06-23 13:23_

---

_@AlexWaygood reviewed on 2025-06-23 13:25_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:67 on 2025-06-23 13:25_

> The other side to this is that
> 
> ```
> import importlib.resources
> importlib.<CURSOR>
> ```
> 
> should include `resources` in the completions offered.

In this case, I would _hope_ that we would recognize that `resources` is available as an attribute on the `importlib` module, and we would therefore include `"resources"` in the set returned by `all_members`...? (That's the behaviour I'd _expect_, but IDK if it's what we actually _do_, to be clear 😆)

---

_@BurntSushi reviewed on 2025-06-23 13:33_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:255 on 2025-06-23 13:33_

Yes it is possible. But the loop will bail in lots of cases, not just when no `from` token is encountered.

It could walk back arbitrarily long distances for what I suspect are pathological inputs though. But, for example, if any `Newline` token is seen, that alone will immediately stop the loop.

But maybe I'm wrong about this being limited to pathological inputs. For example, I if there's just a long list of name tokens before the cursor that isn't an import, then that would cause this state machine to loop over all of them I think.

On the other hand, by the time we get to this point, ty has to have already done at least _some_ work proportional to the length of `tokens`? Unless we know how to update a list of tokens based on a diff with a previous revision of the file I guess.

I suppose it wouldn't hurt to put an upper limit on this though. OK, I've done that, with 1,000 as the limit.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:293 on 2025-06-23 13:37_

@AlexWaygood Right. I learned that fact from reading the parser. :-)

@MichaReiser It's not clear to me that we do. I did add `Unknown` to the tokens making up the imported names because I actually ran into that case when testing. But, e.g., in your example, I don't think we'd be able to offer completions anyway since `?` doesn't resolve to a module. On the other hand, maybe that is indeed exactly what we want. If we don't match your example, then we'd fall through to other forms of completions, e.g., potentially scope based completions. Which seems wrong here.

---

_@BurntSushi reviewed on 2025-06-23 13:37_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:293 on 2025-06-23 13:58_

OK, this is done. Indeed, `from ? import <CURSOR>` was giving scope based completions, which is definitely not right. But recognizing the unknown token in module names makes this work as expected.

---

_@BurntSushi reviewed on 2025-06-23 13:58_

---

_@BurntSushi reviewed on 2025-06-23 14:01_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/semantic_model.rs`:67 on 2025-06-23 14:01_

Aye yeah, I've added some tests to cover this:

```rust
    #[test]
    fn import_submodule_not_attribute1() {
        let test = cursor_test(
            "\
import importlib
importlib.<CURSOR>
",
        );
        test.assert_completions_do_not_include("resources");
    }

    #[test]
    fn import_submodule_not_attribute2() {
        let test = cursor_test(
            "\
import importlib.resources
importlib.<CURSOR>
",
        );
        test.assert_completions_include("resources");
    }

    #[test]
    fn import_submodule_not_attribute3() {
        let test = cursor_test(
            "\
import importlib
import importlib.resources
importlib.<CURSOR>
",
        );
        test.assert_completions_include("resources");
    }
```

The first test passes, but the latter two fail.

---

_@BurntSushi reviewed on 2025-06-23 14:11_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1 on 2025-06-23 14:11_

Yeah I agree that would be nice. Is it okay with you if I do this in a follow-up PR? I think I'd like to do a little refactoring of the cursor test stuff to make this work nicely.

---

_@dhruvmanila approved on 2025-06-23 14:23_

Looks good, I haven't done a thorough review as there are already 3 reviewers :)

---

_@AlexWaygood reviewed on 2025-06-23 14:26_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:2168 on 2025-06-23 14:26_

Here's a patch that gets these tests passing:

```diff
diff --git a/crates/ty_ide/src/completion.rs b/crates/ty_ide/src/completion.rs
index eeef8324d0..30d5fd6616 100644
--- a/crates/ty_ide/src/completion.rs
+++ b/crates/ty_ide/src/completion.rs
@@ -2148,9 +2148,7 @@ import importlib.resources
 importlib.<CURSOR>
 ",
         );
-        // TODO: This is wrong. Completions should include
-        // `resources` here.
-        test.assert_completions_do_not_include("resources");
+        test.assert_completions_include("resources");
     }
 
     #[test]
@@ -2162,9 +2160,7 @@ import importlib.resources
 importlib.<CURSOR>
 ",
         );
-        // TODO: This is wrong. Completions should include
-        // `resources` here.
-        test.assert_completions_do_not_include("resources");
+        test.assert_completions_include("resources");
     }
 
     #[test]
diff --git a/crates/ty_python_semantic/src/module_name.rs b/crates/ty_python_semantic/src/module_name.rs
index 2111a64b83..39b14e9751 100644
--- a/crates/ty_python_semantic/src/module_name.rs
+++ b/crates/ty_python_semantic/src/module_name.rs
@@ -127,6 +127,16 @@ impl ModuleName {
         true
     }
 
+    pub fn relative_to(&self, other: &ModuleName) -> Option<ModuleName> {
+        if self.starts_with(other) {
+            let relative_name = self.0.strip_prefix(&*other.0)?.strip_prefix('.')?;
+            assert!(!relative_name.is_empty());
+            Some(ModuleName(CompactString::from(relative_name)))
+        } else {
+            None
+        }
+    }
+
     #[must_use]
     #[inline]
     pub fn as_str(&self) -> &str {
diff --git a/crates/ty_python_semantic/src/types/ide_support.rs b/crates/ty_python_semantic/src/types/ide_support.rs
index 0dac4db19b..135e22abf0 100644
--- a/crates/ty_python_semantic/src/types/ide_support.rs
+++ b/crates/ty_python_semantic/src/types/ide_support.rs
@@ -2,7 +2,7 @@ use crate::Db;
 use crate::place::{imported_symbol, place_from_bindings, place_from_declarations};
 use crate::semantic_index::place::ScopeId;
 use crate::semantic_index::{
-    attribute_scopes, global_scope, place_table, semantic_index, use_def_map,
+    attribute_scopes, global_scope, imported_modules, place_table, semantic_index, use_def_map,
 };
 use crate::types::{ClassBase, ClassLiteral, KnownClass, Type};
 use ruff_python_ast::name::Name;
@@ -130,8 +130,9 @@ impl AllMembers {
 
             Type::ModuleLiteral(literal) => {
                 self.extend_with_type(db, KnownClass::ModuleType.to_instance(db));
+                let module = literal.module(db);
 
-                let Some(file) = literal.module(db).file() else {
+                let Some(file) = module.file() else {
                     return;
                 };
 
@@ -151,6 +152,16 @@ impl AllMembers {
                             .insert(place_table.place_expr(symbol_id).expect_name().clone());
                     }
                 }
+
+                let module_name = module.name();
+                self.members.extend(
+                    imported_modules(db, literal.importing_file(db))
+                        .iter()
+                        .filter_map(|submodule_name| dbg!(dbg!(submodule_name).relative_to(module_name)))
+                        .filter_map(|relative_submodule_name| {
+                            Some(Name::from(relative_submodule_name.components().next()?))
+                        }),
+                );
             }
         }
     }
```

---

_@AlexWaygood reviewed on 2025-06-23 14:28_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:2168 on 2025-06-23 14:28_

probably needs e.g. a couple of doctests for the new `ModuleName::relative_to` function, though (`ModuleName("importlib.resources").relative_to(ModuleName("importlib"))` results in `ModuleName("resources")`)

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1 on 2025-06-23 14:28_

absolutely!

---

_@AlexWaygood reviewed on 2025-06-23 14:28_

---

_@AlexWaygood reviewed on 2025-06-23 14:29_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:2168 on 2025-06-23 14:29_

(feel free to tackle this in a followup too, if you prefer!)

---

_@BurntSushi reviewed on 2025-06-23 14:43_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:2168 on 2025-06-23 14:43_

Thanks! Yeah I'll tackle this in a follow-up too.

---

_Merged by @BurntSushi on 2025-06-23 14:43_

---

_Closed by @BurntSushi on 2025-06-23 14:43_

---

_Branch deleted on 2025-06-23 14:43_

---
