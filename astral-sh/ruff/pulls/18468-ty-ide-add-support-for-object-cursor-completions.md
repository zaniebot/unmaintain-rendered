```yaml
number: 18468
title: "[ty] IDE: add support for `object.<CURSOR>` completions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/completion-object
created_at: 2025-06-04T18:57:57Z
updated_at: 2025-06-06T05:52:20Z
url: https://github.com/astral-sh/ruff/pull/18468
synced_at: 2026-01-12T15:56:19Z
```

# [ty] IDE: add support for `object.<CURSOR>` completions

---

_@BurntSushi_

This PR adds logic for detecting `Name Dot [Name]` token patterns,
finding the corresponding `ExprAttribute`, getting the type of the
object and returning the members available on that object.

Here's a video demonstrating this working:

https://github.com/user-attachments/assets/42ce78e8-5930-4211-a18a-fa2a0434d0eb

Ref astral-sh/ty#86


---

_Review requested from @carljm by @BurntSushi on 2025-06-04 18:57_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-06-04 18:57_

---

_Review requested from @sharkdp by @BurntSushi on 2025-06-04 18:57_

---

_Review requested from @dcreager by @BurntSushi on 2025-06-04 18:57_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-06-04 18:57_

---

_Review request for @dcreager removed by @BurntSushi on 2025-06-04 18:58_

---

_Review request for @carljm removed by @BurntSushi on 2025-06-04 18:58_

---

_Comment by @github-actions[bot] on 2025-06-04 19:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-06-04 19:03_

> Ref #86

you mean astral-sh/ty#86 ;)

---

_Label `server` added by @AlexWaygood on 2025-06-04 19:03_

---

_Label `ty` added by @AlexWaygood on 2025-06-04 19:03_

---

_Comment by @BurntSushi on 2025-06-04 19:07_

Yup. That's a hard one to get right for me haha.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:30 on 2025-06-04 19:12_

to add an additional wrinkle here: names that start and end with a double-underscore ("dunders") indicate attributes/methods that are given special treatment by the interpreter. It's less common to access these directly -- you would usually do `1 + 2` rather than `(1).__add__(2)`, for example -- but they're also not really "private" in the same way as other names that start with an underscore. So IMO the sorting should be:
1. Any names that do not start with an underscore
2. Dunder names
3. Any other names that start with an underscore

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1038 on 2025-06-04 19:20_

the relevant issue here is https://github.com/astral-sh/ty/issues/159, if you want to link to it!

---

_@AlexWaygood approved on 2025-06-04 19:21_

Nice! I haven't reviewed the AST/tokenizing stuff in depth, but this looks reasonable to me!

---

_Comment by @sharkdp on 2025-06-04 19:25_

I am planning to review this tomorrow. One thing that surprised me when testing this locally in the playground: in a scenario like the following, I get no completions at all. I need to type one additional letter before I start to see the completions on `C`. The snapshot tests seem to indicate that this should work?
```py
class C:
    foo: int
    bar: str

C.<CURSOR>
```

---

_Comment by @dhruvmanila on 2025-06-05 05:32_

> One thing that surprised me when testing this locally in the playground: in a scenario like the following, I get no completions at all. I need to type one additional letter before I start to see the completions on `C`. The snapshot tests seem to indicate that this should work?

This is because the server capabilities needs to be updated to include the `.` (dot) trigger character:

```diff
diff --git a/crates/ty_server/src/server.rs b/crates/ty_server/src/server.rs
index 84093d6ffa..4846e0219c 100644
--- a/crates/ty_server/src/server.rs
+++ b/crates/ty_server/src/server.rs
@@ -180,6 +180,7 @@ impl Server {
             completion_provider: experimental
                 .is_some_and(Experimental::is_completions_enabled)
                 .then_some(lsp_types::CompletionOptions {
+                    trigger_characters: Some(vec!['.'.to_string()]),
                     ..Default::default()
                 }),
             ..Default::default()
diff --git a/playground/ty/src/Editor/Editor.tsx b/playground/ty/src/Editor/Editor.tsx
index fca03cd08b..a25e3b4ddb 100644
--- a/playground/ty/src/Editor/Editor.tsx
+++ b/playground/ty/src/Editor/Editor.tsx
@@ -179,7 +179,7 @@ class PlaygroundServer
       monaco.languages.registerDocumentFormattingEditProvider("python", this);
   }
 
-  triggerCharacters: undefined;
+  triggerCharacters: string[] = ["."];
 
   provideCompletionItems(
     model: editor.ITextModel,
```

This will then inform the client / playground to request completions on `.` (dot).

For reference, this is Pyright's trigger characters:
```
triggerCharacters = { ".", "[", '"', "'" }
```

For context, the client needs to know when it should request completions which by default is usually `[a-zA-Z]` but the server can expand that set to include additional characters relevant for the server.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/completion.rs`:30 on 2025-06-05 05:41_

I agree with this sorting behavior. I have a custom implementation in my editor that does the sorting of completion items in this way.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/completion.rs`:20 on 2025-06-05 05:44_

Should we use the completion context that the client could provide? The context would contain _how_ was the completion triggered and if it was invoked by a trigger character (e.g., `.`) then which character it was.

Regardless of whether we use this information or not, we'd still need to find the AST node so it might be that it isn't really useful today but it could be in the future.

Completion params: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionParams
Completion context: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionContext

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/completion.rs`:69 on 2025-06-05 05:55_

I think this is a great start but it seems to have a few limitations. I don't think these needs to be addressed here but I thought to write it down as a note. For example, doing `[].<CURSOR>` falls back to providing completions from the scope. Maybe we shouldn't use the fallback behavior when we know that the completion was triggered from a `.` character? I'm not sure, you might want to play around with this in an editor context.

A follow-up to this would be to expand the token kind that can occur before the `.` character so that completions on objects other than a variable work like list, dictionary, strings, etc. For example, in `[].<CURSOR>` we would provide completions for list attributes. We might have to be careful here because we can't just look at the previous token as it's only `[].<CURSOR>` (a syntactically valid list) should provide completion and not `].<CURSOR>`. Also, the list could be arbitrary long which means that there would could be `n` number of previous tokens to look before we see the `[` corresponding to the `]` in which case maybe we should rely on the AST instead.

Another example is when an attribute access happens directly on an instantiated class like `A().<CURSOR>`.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/completion.rs`:219 on 2025-06-05 06:07_

Another test case would be to have nested attributes which I don't think is being handled correctly:

```py
class A:
    x: str


class B:
    a: A


b = B()
b.a.<CURSOR>
```

<img width="486" alt="Screenshot 2025-06-05 at 11 36 22" src="https://github.com/user-attachments/assets/d97c10ef-9ee6-43d7-8294-c1271d650664" />


---

_@dhruvmanila approved on 2025-06-05 06:14_

Thank you! I think this is a good start, I've a few suggestions on what could be done next.

It's a bit unfortunate that we don't yet have E2E test infrastructure for the server, so when testing any server related features that require editor interaction, we usually add a short video demonstrating the functionality in the PR description. I'd recommend to include that whenever you think it'd be useful which I think would be useful in this PR.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:13 on 2025-06-05 07:10_

Maybe something for another PR but It would be great if we could extend `Completion` here with a `kind` that tells VS code what the completion is (is it a variable, a field, etc...). This is currently hard coded in the playground to always be `Variable` but that will be incorrect with this PR.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:69 on 2025-06-05 07:13_

Another one: Anything parenthesized: `(a).` or `{ a: True }.`...

---

_@MichaReiser approved on 2025-06-05 07:14_

Nice! 

I think we need to set the completion options in the playground and the LSP setup because `.` isn't a trigger character by default. From the LSP specification

```
	/**
	 * The additional characters, beyond the defaults provided by the client (typically
	 * [a-zA-Z]), that should automatically trigger a completion request. For example
	 * `.` in JavaScript represents the beginning of an object property or method and is
	 * thus a good candidate for triggering a completion request.
	 *
	 * Most tools trigger a completion request automatically without explicitly
	 * requesting it using a keyboard shortcut (e.g. Ctrl+Space). Typically they
	 * do so when the user starts to type an identifier. For example if the user
	 * types `c` in a JavaScript file code complete will automatically pop up
	 * present `console` besides others as a completion item. Characters that
	 * make up identifiers don't need to be listed here.
	 */
	triggerCharacters?: string[];
```

---

_@sharkdp reviewed on 2025-06-05 07:32_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:69 on 2025-06-05 07:32_

I had a similar thought while playing with this and reviewing the code. There are not that many places in Python's syntax where a `.` can appear(?). The ellipsis `...`, float literals `3.14`, and relative imports such as `from .mod import x` come to mind. This is very hand-wavy, but maybe this suggests that an "opt out" approach could work? That is, instead of explicitly matching on a set of patterns, maybe we should always try to find an enclosing attribute-expression if we see `DOT` or `DOT NAME`? This would mean we would also start the enclosing-node search for things like a float literal (`3.<CURSOR>`), but maybe that's not a problem?

---

_@BurntSushi reviewed on 2025-06-05 13:12_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:13 on 2025-06-05 13:12_

Yeah @dhruvmanila has mentioned this a few times, but I've been focusing on breadth (making completions work in more places) over depth (improving completion quality). Maybe I should pause on breadth for the moment and look at depth.

---

_@BurntSushi reviewed on 2025-06-05 13:15_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:219 on 2025-06-05 13:15_

I've captured this in a test. Thanks!

---

_Comment by @BurntSushi on 2025-06-05 13:24_

For the trigger characters, nice catch @sharkdp and @dhruvmanila. I didn't notice that because I've been testing this by specifically opting into auto-completions. My editor doesn't show them automatically. I'll look into changing that.

---

_@BurntSushi reviewed on 2025-06-05 13:30_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:69 on 2025-06-05 13:30_

Yeah I like that idea @sharkdp!

---

_@BurntSushi reviewed on 2025-06-05 14:50_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:30 on 2025-06-05 14:50_

Done!

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:20 on 2025-06-05 14:53_

I made a note about this. But in addition to the point you bring up, I don't think that completion context helps with completions that are triggered manually (which is primarily how I use compeltions). In that case, from what I can tell, there is no trigger character provided.

---

_@BurntSushi reviewed on 2025-06-05 14:53_

---

_Comment by @BurntSushi on 2025-06-05 14:57_

Thanks all for the feedback! I've addressed what I think is easy to address in this PR. Otherwise, in follow-up work, I would like to focus on improving the somewhat naive token-based detection here with something more flexible. i.e., To make things like `[].<CURSOR>` and `(a).<CURSOR>` work.

---

_@AlexWaygood reviewed on 2025-06-05 14:57_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:171 on 2025-06-05 14:57_

```suggestion
/// attributes, and all single-underscore attributes after dunder attributes.
```

---

_@BurntSushi reviewed on 2025-06-05 14:59_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:69 on 2025-06-05 14:59_

(I'll work on this in another PR.)

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:188 on 2025-06-05 15:00_

a name that starts with `__` but does not end with `__` ends up being a "mangled" name, which is even more private than names that only start with `_`. I don't think it's worth distinguishing here between names that start with a single `_` and "mangled" names, _but_ we definitely shouldn't categorise names as being dunder names unless they both start and end with `__`

```suggestion
            if name.starts_with("__") && name.ends_with("__") {
```

---

_@AlexWaygood reviewed on 2025-06-05 15:00_

---

_@AlexWaygood reviewed on 2025-06-05 15:01_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:188 on 2025-06-05 15:01_

(see https://docs.python.org/3/reference/lexical_analysis.html#reserved-classes-of-identifiers and https://docs.python.org/3/reference/expressions.html#index-5)

---

_@BurntSushi reviewed on 2025-06-05 15:07_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:188 on 2025-06-05 15:07_

I forgot about those. Thanks!

---

_Comment by @BurntSushi on 2025-06-05 15:09_

> we usually add a short video demonstrating the functionality in the PR description. I'd recommend to include that whenever you think it'd be useful which I think would be useful in this PR.

Yes, thank you! That's a good idea. I've added a video to the top-level comment here.

In speaking with @MichaReiser, it seems like it would be a good idea for me to be testing this in VS Code as well. So I'll look into setting that up.

---

_Merged by @BurntSushi on 2025-06-05 15:15_

---

_Closed by @BurntSushi on 2025-06-05 15:15_

---

_Branch deleted on 2025-06-05 15:15_

---

_Comment by @dhruvmanila on 2025-06-06 05:52_

> Yes, thank you! That's a good idea. I've added a video to the top-level comment here.

Thank you!

> In speaking with @MichaReiser, it seems like it would be a good idea for me to be testing this in VS Code as well. So I'll look into setting that up.

Sounds good! The README over at https://github.com/astral-sh/ty-vscode should be good on how to do it. Feel free to provide any feedback if anything was confusing :)

---
