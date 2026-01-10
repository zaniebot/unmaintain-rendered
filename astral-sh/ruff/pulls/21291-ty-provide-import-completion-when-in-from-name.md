```yaml
number: 21291
title: "[ty] provide `import` completion when in `from <name> <name>` statement"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: no-completions-in-from-import
created_at: 2025-11-06T12:09:39Z
updated_at: 2025-12-27T00:21:00Z
url: https://github.com/astral-sh/ruff/pull/21291
synced_at: 2026-01-10T16:36:18Z
```

# [ty] provide `import` completion when in `from <name> <name>` statement

---

_Pull request opened by @MatthewMckee4 on 2025-11-06 12:09_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/1494

## Test Plan

Add a test showing if we are in `from <name> <name> ` we provide the keyword completion "import"


---

_Review requested from @carljm by @MatthewMckee4 on 2025-11-06 12:09_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-11-06 12:09_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-11-06 12:09_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-11-06 12:09_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-11-06 12:09_

---

_Renamed from "[ty] Don't provide completions when in 'from <name> i' statement" to "[ty] Don't provide completions when in 'from <name> <name>' statement" by @MatthewMckee4 on 2025-11-06 12:10_

---

_Renamed from "[ty] Don't provide completions when in 'from <name> <name>' statement" to "[ty] Don't provide completions when in `from <name> <name>` statement" by @MatthewMckee4 on 2025-11-06 12:10_

---

_Comment by @github-actions[bot] on 2025-11-06 12:12_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-06 12:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Converted to draft by @MatthewMckee4 on 2025-11-06 12:19_

---

_Renamed from "[ty] Don't provide completions when in `from <name> <name>` statement" to "[ty] Don't `import` completion when in `from <name> <name>` statement" by @MatthewMckee4 on 2025-11-06 12:36_

---

_Marked ready for review by @MatthewMckee4 on 2025-11-06 12:37_

---

_Renamed from "[ty] Don't `import` completion when in `from <name> <name>` statement" to "[ty] provide `import` completion when in `from <name> <name>` statement" by @MatthewMckee4 on 2025-11-06 12:39_

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-11-06 12:57_

---

_Label `server` added by @AlexWaygood on 2025-11-06 12:58_

---

_Label `ty` added by @AlexWaygood on 2025-11-06 12:58_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-06 13:13_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-06 13:13_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-06 13:13_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-06 13:13_

---

_@MichaReiser approved on 2025-11-06 13:18_

Only showing `import` in a `from <NAME> <NAME2>` position does make sense to me. I was surprised to see that Pylance shows all completions, but gives `import` a higher ranking

---

_Comment by @MatthewMckee4 on 2025-11-06 13:38_

For some reason i dont get completions in playground locally for `import` when i have just `a`
<img width="498" height="201" alt="image" src="https://github.com/user-attachments/assets/03ea3633-ddbd-4312-9896-9973da7de274" />

This could be a playground thing though?


---

_Comment by @MichaReiser on 2025-11-06 13:54_

I just checked out this PR and built the playground and this works as expected when you have `from types i` but it doesn't work when you have `from types aa`. 

This could either be because the playground applies its own filtering (not sure if there's a way for us to disable it, do you want to take a look) or because our own "text prefix matching"

---

_Comment by @BurntSushi on 2025-11-06 14:21_

Yeah we may not be able to fully control whether `import` is shown for `from module import aa<CURSOR>`. LSP clients can and will do their own filtering AIUI. I'm not sure if there is a foolproof way to override that.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:908 on 2025-11-06 14:43_

this works well for the simple case of `from <NAME> <CURSOR>`, but doesn't work for:
- `from <NAME>.<NAME> <CURSOR>`
- `from .<NAME> <CURSOR>`
- `from ...<NAME>.<NAME> <CURSOR>`

etc.

<img width="1498" height="364" alt="image" src="https://github.com/user-attachments/assets/b2e52aef-104e-4b35-88b7-261ea7fb6170" />


---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:237 on 2025-11-06 14:45_

I don't think it's true in the long term that all locations where we might want to offer a keyword completion are places where we definitely won't want to offer any non-keyword completions. For example `pass` (a keyword) and `print` (a non-keyword) should both be included in our completion suggestions in this example:

```py
match x:
    case int():
        p<CURSOR>
```

so the semantics you're adding in this PR are totally correct, but that might imply a different code structure here that would be more extensible in the future

---

_@AlexWaygood reviewed on 2025-11-06 14:45_

Nice, thank you!

---

_@AlexWaygood reviewed on 2025-11-06 14:57_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:908 on 2025-11-06 14:57_

the full grammar for import statements given at https://docs.python.org/3/reference/grammar.html is:

```
import_stmt:
    | import_name
    | import_from

# Import statements
# -----------------

import_name: 'import' dotted_as_names 
# note below: the ('.' | '...') is necessary because '...' is tokenized as ELLIPSIS
import_from:
    | 'from' ('.' | '...')* dotted_name 'import' import_from_targets 
    | 'from' ('.' | '...')+ 'import' import_from_targets 
import_from_targets:
    | '(' import_from_as_names [','] ')' 
    | import_from_as_names !','
    | '*' 
import_from_as_names:
    | ','.import_from_as_name+ 
import_from_as_name:
    | NAME ['as' NAME ] 

dotted_as_names:
    | ','.dotted_as_name+ 
dotted_as_name:
    | dotted_name ['as' NAME ] 

dotted_name:
    | dotted_name '.' NAME 
    | NAME
```

---

_Comment by @MatthewMckee4 on 2025-11-06 14:59_

Thank you, I will revisit this in a few days, when I am back home.

---

_@MatthewMckee4 reviewed on 2025-11-06 15:10_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:237 on 2025-11-06 15:10_

Good point, it seems we need some indication of when keywords are the only completions we should provide.

I will think more about this

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:237 on 2025-11-06 15:15_

Right. What we'll want it some way to identify something about the context in which completions are being offered. There are other considerations in play here, like `raise <CURSOR>` should have the suggestions altered to things that are appropriate for `raise`. There are likely other examples.

---

_@BurntSushi reviewed on 2025-11-06 15:15_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:908 on 2025-11-06 15:17_

You might be able to copy this routine and tweak it a bit:

https://github.com/astral-sh/ruff/blob/0c7b93363e6b5b43b8f36adbd6e3adf1a13374c2/crates/ty_ide/src/completion.rs#L667-L670

---

_@BurntSushi reviewed on 2025-11-06 15:17_

---

_@BurntSushi reviewed on 2025-11-06 15:18_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:4360 on 2025-11-06 15:18_

Can you add a comment noting that this might not work the same way in real LSP clients (since they might do their own filtering based on what has been typed)?

---

_@MatthewMckee4 reviewed on 2025-11-10 00:03_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:237 on 2025-11-10 00:03_

I can implement something like this in another PR

---

_@MatthewMckee4 reviewed on 2025-11-10 00:03_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:908 on 2025-11-10 00:03_

I have done my best to mirror this.

---

_@AlexWaygood reviewed on 2025-11-10 00:26_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:4445 on 2025-11-10 00:26_

A version of this test with _three_ preceding dots doesn't pass. The reason is that `from ...foo` doesn't parse as `[<FROM>, <DOT>, <DOT>, <DOT>, <NAME("foo")>]`, it parses as `[<FROM>, <ELLIPSIS>, <NAME("foo")>]`.

You want to make this change:

```diff
diff --git a/crates/ty_ide/src/completion.rs b/crates/ty_ide/src/completion.rs
index d6896f3804..adf107d7c4 100644
--- a/crates/ty_ide/src/completion.rs
+++ b/crates/ty_ide/src/completion.rs
@@ -919,7 +919,8 @@ fn from_import_statement_keyword_completions<'db>(tokens: &[Token]) -> Option<Co
     // Skip over '('.' | '...')*'
     let dots_count = tokens_after_from
         .iter()
-        .take_while(|t| t.kind() == TokenKind::Dot)
+        .map(Token::kind)
+        .take_while(|t| matches!(t, TokenKind::Dot | TokenKind::Ellipsis))
         .count();
 
     let tokens_after_dots = &tokens_after_from[dots_count..];
@@ -4391,6 +4392,23 @@ type <CURSOR>
         assert!(completions.len() == 1);
     }
 
+    #[test]
+    fn from_import_nested_very_dotted_name_relative_import_suggests_import() {
+        let builder = CursorTest::builder()
+            // N.B. the `...` tokenizes as `TokenKind::Ellipsis`
+            .source("src/main.py", "from ...foo i<CURSOR>")
+            .source("foo.py", "")
+            .completion_test_builder();
+
+        let completions_test = builder.build();
+
+        completions_test.contains("import");
+
+        let completions = completions_test.completions();
+
+        assert!(completions.len() == 1);
+    }
+
     /// A way to create a simple single-file (named `main.py`) completion test
     /// builder.
     ///
```

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:4445 on 2025-11-10 00:30_

Ah, thank you.

---

_@MatthewMckee4 reviewed on 2025-11-10 00:30_

---

_@AlexWaygood reviewed on 2025-11-10 00:38_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:959 on 2025-11-10 00:38_

I find this a bit easier to understand:

```suggestion
fn is_dotted_name(tokens: &[Token]) -> bool {
    match tokens {
        [] | [_, _] => false,
        [tok] => tok.kind() == TokenKind::Name,
        [dotted_name @ .., penultimate, last] => {
            last.kind() == TokenKind::Name
                && penultimate.kind() == TokenKind::Dot
                && is_dotted_name(dotted_name)
        }
    }
}
```

---

_@AlexWaygood approved on 2025-11-10 00:40_

Thank you!

---

_@MatthewMckee4 reviewed on 2025-11-10 00:41_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:959 on 2025-11-10 00:41_

Very nice, my functional programming class clearly hasn't sunk in enough.

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-11-10 00:47_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:911 on 2025-11-10 09:11_

The search here seems to be too permissive to me. What's preventing it from going across logical lines? 

Can you add some tests with multiple statements? I suspect that we now incorrectly match on import completions if there's **any** import statement before `<CURSOR>`. 



---

_@MichaReiser reviewed on 2025-11-10 09:11_

---

_@MatthewMckee4 reviewed on 2025-11-10 10:16_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:911 on 2025-11-10 10:16_

Good point, will add that 

---

_@MichaReiser reviewed on 2025-11-10 10:29_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:911 on 2025-11-10 10:29_

I still think this is incorrect as it cross logical line boundaries. E.g. what if

```py
from module impo

a<CURSOR>
```

We would want to show completions for `a` and not suggest `import`

---

_@MatthewMckee4 reviewed on 2025-11-10 10:32_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:911 on 2025-11-10 10:32_

I am maybe misunderstanding you. we do still provide completions for `a` here.
<img width="590" height="214" alt="image" src="https://github.com/user-attachments/assets/1e4b7676-766a-4c5e-9016-5cc89d4ba19c" />


---

_@MichaReiser reviewed on 2025-11-10 12:07_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:911 on 2025-11-10 12:07_

I haven't checked out the PR. What I'm concerned with the current approach is that `tokens` is anything before `<CURSOR>` (unless this isn't the case?). That means, it's possible for `tokens.iter().rposition()` to find the `from` even though it belongs to an entirely different statement.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:934 on 2025-11-10 13:01_

Can you make sure comments are wrapped to 99 columns (inclusive) please? There are some other comments below that need to be wrapped too.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:943 on 2025-11-10 13:05_

I think this routine should be modeled on this existing function:

https://github.com/astral-sh/ruff/blob/f44598dc113a7380ca52d6c969431ca929a93473/crates/ty_ide/src/completion.rs#L667-L756

Maybe there is a way to factor out the common logic, but even if there isn't, just copying the _approach_ seems like the right move here. I agree with @MichaReiser above. And notice that the function above is careful to not do too much look-behind on the token sequence. It also handles things like ellipsis, non-logical newlines and so on.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:4318 on 2025-11-10 13:07_

For something really short, I'm fine with the `\n` here. But this starts to make very long lines. I'd rather see this written out using actual new lines:

```
        let builder = completion_test_builder(
            "from collections.abc import Sequence
             from typing i<CURSOR>"
        );
```

The test code should automatically handle dedenting.

---

_@BurntSushi requested changes on 2025-11-10 13:07_

---

_Comment by @BurntSushi on 2025-11-10 14:14_

@MatthewMckee4 I'm going to take over this PR since we'd like to get this fixed. And I'm planning to work on more stuff around keyword completions this week, which will really want to build on top of this PR. Thank you. :-)

---

_Comment by @MatthewMckee4 on 2025-11-10 14:23_

Cool, apologies for the delays, I've been a little busy. Thank you for the feedback too!

---

_Comment by @AlexWaygood on 2025-11-10 14:26_

> Cool, apologies for the delays, I've been a little busy.

never feel like you need to apologise for unpaid volunteer work :-) we really appreciate the contribution!!

---

_@BurntSushi approved on 2025-11-10 16:14_

---

_Merged by @BurntSushi on 2025-11-10 16:59_

---

_Closed by @BurntSushi on 2025-11-10 16:59_

---

_Branch deleted on 2025-12-27 00:21_

---
