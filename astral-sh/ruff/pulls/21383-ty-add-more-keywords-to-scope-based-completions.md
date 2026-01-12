```yaml
number: 21383
title: "[ty] Add more keywords to scope-based completions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/more-keyword-completions
created_at: 2025-11-11T15:52:53Z
updated_at: 2025-11-11T22:20:57Z
url: https://github.com/astral-sh/ruff/pull/21383
synced_at: 2026-01-12T15:57:22Z
```

# [ty] Add more keywords to scope-based completions

---

_@BurntSushi_

This PR adds more keyword completions when asking for scoped-based
completions. This makes things like `pas<CURSOR>` return `pass`, among
other things.

We do this in a somewhat naive way. This means we might suggest
keywords even when they aren't really appropriate. I think I originally
wanted to use more context to be more precise here. And perhaps we
still should. But I think it's better to just offer these as choices
for now even if they aren't always correct.

More to the point, it seems that pylance does the same thing here.


---

_Review requested from @carljm by @BurntSushi on 2025-11-11 15:52_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-11-11 15:52_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-11-11 15:52_

---

_Review requested from @sharkdp by @BurntSushi on 2025-11-11 15:52_

---

_Review requested from @dcreager by @BurntSushi on 2025-11-11 15:52_

---

_Review request for @dcreager removed by @BurntSushi on 2025-11-11 15:53_

---

_Review request for @carljm removed by @BurntSushi on 2025-11-11 15:53_

---

_Review request for @MichaReiser removed by @BurntSushi on 2025-11-11 15:53_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 15:55_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `server` added by @AlexWaygood on 2025-11-11 15:55_

---

_Label `ty` added by @AlexWaygood on 2025-11-11 15:55_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 15:56_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:173 on 2025-11-11 15:57_

this means that they will be sorted above auto-import completions but below local-scope completions, correct?

EDIT: ah, just got to your big comment lower down in the `key()` method, which explains that this is not the case. Your reasoning there makes sense. But if these are special cased in the `key()` method, what difference does it make to mark them as "builtins"?

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:344 on 2025-11-11 16:00_

we'll have to remember to add `lazy` here when we add support for Python 3.15.

`type` is also a builtin symbol -- is it going to cause problems for it to be included twice?

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:4724 on 2025-11-11 16:13_

nit

```suggestion
    #[expect(clippy::struct_excessive_bools)] // free the bools!
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1 on 2025-11-11 16:16_

it doesn't look like you've added any unit tests that show that we _do_ offer keyword completions in some situations. (I do see that you added some ranking integration tests, and they seem probably sufficient, just wanted to double-check that not adding any unit tests was deliberate!)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:179 on 2025-11-11 16:17_

Took me a while to understand what a `keyword_value` is but I also don't have a great suggestion on how to name it instead. Maybe `value_keyword` is slightly better in the sense that it specifies what kind of keyword it is (and it's not a keyword value). I'm not sure if my explanation makes sense 😅 

---

_@MichaReiser approved on 2025-11-11 16:17_

---

_@AlexWaygood approved on 2025-11-11 16:19_

I had a quick muck around in a local playground build, and this is much improved, thank you ❤️

It's a _tiny_ bit annoying that `raise` is now suggested above `range` for `for x in ra<CURSOR>`, but `range` is the next suggestion, and it's fairly easy to select that one instead of `raise`

---

_@AlexWaygood reviewed on 2025-11-11 16:21_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:344 on 2025-11-11 16:21_

I suppose ideally here we'd also not include entries that aren't keywords on your selected Python version. E.g. `match` and `case` are not keywords on Python <=3.9, and `type` is not a keyword on Python <=3.11. (Doesn't need to be done in this PR obviously, just a possible future improvement.)

---

_@AlexWaygood reviewed on 2025-11-11 16:38_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:344 on 2025-11-11 16:38_

It would be _possible_ to generate the list of keywords here, ensuring that it's kept up to date with the parser:

<details>

```diff
diff --git a/Cargo.lock b/Cargo.lock
index d9f831125d..831abb6d78 100644
--- a/Cargo.lock
+++ b/Cargo.lock
@@ -3355,6 +3355,8 @@ dependencies = [
  "serde",
  "serde_json",
  "static_assertions",
+ "strum",
+ "strum_macros",
  "unicode-ident",
  "unicode-normalization",
  "unicode_names2",
@@ -4395,6 +4397,7 @@ dependencies = [
  "rustc-hash",
  "salsa",
  "smallvec",
+ "strum",
  "tracing",
  "ty_project",
  "ty_python_semantic",
diff --git a/crates/ruff_python_parser/Cargo.toml b/crates/ruff_python_parser/Cargo.toml
index ae45871866..199534a91c 100644
--- a/crates/ruff_python_parser/Cargo.toml
+++ b/crates/ruff_python_parser/Cargo.toml
@@ -24,6 +24,8 @@ get-size2 = { workspace = true }
 memchr = { workspace = true }
 rustc-hash = { workspace = true }
 static_assertions = { workspace = true }
+strum = { workspace = true }
+strum_macros = { workspace = true }
 unicode-ident = { workspace = true }
 unicode_names2 = { workspace = true }
 unicode-normalization = { workspace = true }
diff --git a/crates/ruff_python_parser/src/token.rs b/crates/ruff_python_parser/src/token.rs
index a5790a9597..06b592cf97 100644
--- a/crates/ruff_python_parser/src/token.rs
+++ b/crates/ruff_python_parser/src/token.rs
@@ -124,7 +124,18 @@ impl fmt::Debug for Token {
 }
 
 /// A kind of a token.
-#[derive(Copy, Clone, PartialEq, Eq, Hash, Debug, PartialOrd, Ord, get_size2::GetSize)]
+#[derive(
+    Copy,
+    Clone,
+    PartialEq,
+    Eq,
+    Hash,
+    Debug,
+    PartialOrd,
+    Ord,
+    get_size2::GetSize,
+    strum_macros::EnumIter,
+)]
 pub enum TokenKind {
     /// Token kind for a name, commonly known as an identifier.
     Name,
@@ -613,9 +624,9 @@ impl From<Operator> for TokenKind {
     }
 }
 
-impl fmt::Display for TokenKind {
-    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
-        let value = match self {
+impl TokenKind {
+    pub const fn as_str(self) -> &'static str {
+        match self {
             TokenKind::Unknown => "Unknown",
             TokenKind::Newline => "newline",
             TokenKind::NonLogicalNewline => "NonLogicalNewline",
@@ -722,8 +733,13 @@ impl fmt::Display for TokenKind {
             TokenKind::Case => "`case`",
             TokenKind::With => "`with`",
             TokenKind::Yield => "`yield`",
-        };
-        f.write_str(value)
+        }
+    }
+}
+
+impl fmt::Display for TokenKind {
+    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
+        f.write_str(self.as_str())
     }
 }
 
diff --git a/crates/ty_ide/Cargo.toml b/crates/ty_ide/Cargo.toml
index 4be5d49fe2..54453a9ea8 100644
--- a/crates/ty_ide/Cargo.toml
+++ b/crates/ty_ide/Cargo.toml
@@ -34,6 +34,7 @@ regex = { workspace = true }
 rustc-hash = { workspace = true }
 salsa = { workspace = true, features = ["compact_str"] }
 smallvec = { workspace = true }
+strum = { workspace = true }
 tracing = { workspace = true }
 
 [dev-dependencies]
diff --git a/crates/ty_ide/src/completion.rs b/crates/ty_ide/src/completion.rs
index c13ec95de9..c69cde7d0d 100644
--- a/crates/ty_ide/src/completion.rs
+++ b/crates/ty_ide/src/completion.rs
@@ -14,6 +14,8 @@ use ty_python_semantic::{
     types::{CycleDetector, Type},
 };
 
+use strum::IntoEnumIterator;
+
 use crate::docstring::Docstring;
 use crate::find_node::covering_node;
 use crate::goto::DefinitionsOrTargets;
@@ -330,12 +332,11 @@ fn add_keyword_completions<'db>(
         completions.push(Completion::keyword_value(name, ty));
     }
 
-    let keywords = [
-        "and", "as", "assert", "async", "await", "break", "class", "continue", "def", "del",
-        "elif", "else", "except", "false", "finally", "for", "from", "global", "if", "import",
-        "in", "is", "lambda", "nonlocal", "not", "or", "pass", "raise", "return", "try", "while",
-        "with", "yield", "case", "match", "type",
-    ];
+    let keywords: Vec<&'static str> = TokenKind::iter()
+        .filter(|token| token.is_keyword())
+        .map(TokenKind::as_str)
+        .collect();
+
     for name in keywords {
         if !query.is_match_symbol_name(name) {
             continue;
```

</details>

---

_@BurntSushi reviewed on 2025-11-11 16:50_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:179 on 2025-11-11 16:50_

Sure, I'm fine with that. Done.

---

_@BurntSushi reviewed on 2025-11-11 16:57_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:173 on 2025-11-11 16:57_

I guess `None`, `True` and `False` are all actually in `builtins`, so presumably those should be marked as `builtin`. But the language keywords aren't. So I updated the code to reflect that.

---

_@AlexWaygood reviewed on 2025-11-11 17:00_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:173 on 2025-11-11 17:00_

> I guess `None`, `True` and `False` are all actually in `builtins`, so presumably those should be marked as `builtin`.

huh, yeah, TIL:

```pycon
>>> import builtins
>>> builtins.__dict__["True"]
True
>>> builtins.__dict__["False"]
False
>>> builtins.__dict__["None"] is None
True
```

---

_@BurntSushi reviewed on 2025-11-11 17:05_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:344 on 2025-11-11 17:05_

Yeah, but: this will include value keywords twice... Which is easy enough to work-around. But the string representation for some of the keywords also includes backticks, which is kind of annoying.

Anyway, I took your patch, but changed it so that the code here trips on an assertion if the number of expected keywords doesn't match the number we suggest here. It doesn't avoid duplication per se, but should at least trip if someone forgets to add a new keyword here.

---

_@BurntSushi reviewed on 2025-11-11 17:06_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1 on 2025-11-11 17:06_

Oh, there is a `keywords` test. But it was wrong because of a late change where I flipped `builtin` to `true` for keyword completions. But I swapped `builtin` back to `false` for those as per discussion above.

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-11-11 17:06_

---

_@AlexWaygood reviewed on 2025-11-11 17:10_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:344 on 2025-11-11 17:10_

> Yeah, but: this will include value keywords twice...

`type` is also included defined as a class in typeshed's `builtins.pyi` file, so I think that means that `type` will be included twice, since you've also included it in the list of keywords?

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 17:21_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_@MichaReiser reviewed on 2025-11-11 17:38_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/Cargo.toml`:28 on 2025-11-11 17:38_

Lol no, I sort of hate strum 😆 

---

_@MichaReiser reviewed on 2025-11-11 17:40_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:357 on 2025-11-11 17:40_

I personally would probably just keep what you had. It seemed very reasonable to me. There's a risk that we forget about a token but this wouldn't lead to a catastrophic bug. We just wait for a user to report it and all is good :). But maybe that's just me disliking strum :)

If we go the `strum` approach, I'd probably go all in and iterate over all tokens and add a `is_value_keyword` to `TokenKind` instead (similar to `is_contextual_keyword`). We could then also add methods like `is_keyword_in(python_version)`.

If you want to keep your manual list, then I suggest moving this assertion into a test that asserts that every token in `TokenKind::iter` is contained within the completions. 

---

_@AlexWaygood reviewed on 2025-11-11 17:46_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:357 on 2025-11-11 17:46_

> Hmm, I'm not sure this warants adding `strum` to `TokenKind`.

yeah, I was in two minds about even making the suggestion, since it involves adding `strum` as a dependency to the parser crate 😄

If we moved this to become a test, then I suppose it could be a test-dependency only for the parser crate.

---

_@BurntSushi reviewed on 2025-11-11 19:49_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:344 on 2025-11-11 19:49_

Those are deduped. I thought I checked in VS Code and only one is shown.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:357 on 2025-11-11 21:18_

OK, I'll just drop this commit entirely and keep what I had.

---

_@BurntSushi reviewed on 2025-11-11 21:18_

---

_@BurntSushi reviewed on 2025-11-11 21:19_

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/Cargo.toml`:28 on 2025-11-11 21:19_

Macro magic adding methods? Yeah I hate that too, but I feel like we have a lot of it already so I kind of just shrug my shoulders lol.

---

_@BurntSushi reviewed on 2025-11-11 21:26_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:344 on 2025-11-11 21:26_

I doubled checked, and no, this is not deduped! I've fixed this by omitting `type` from keyword completions.

---

_Merged by @BurntSushi on 2025-11-11 22:20_

---

_Closed by @BurntSushi on 2025-11-11 22:20_

---

_Branch deleted on 2025-11-11 22:20_

---
