```yaml
number: 791
title: Solid semantic tokens
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-07-09T11:48:22Z
updated_at: 2025-12-18T17:42:27Z
url: https://github.com/astral-sh/ty/issues/791
synced_at: 2026-01-12T15:54:24Z
```

# Solid semantic tokens

---

_@MichaReiser_

Follow up on the TODOs in the semantic tokens provider

See https://github.com/astral-sh/ruff/blob/278f93022acf4ddbb693510cadf99d0eaa53153a/crates/ty_ide/src/semantic_tokens.rs#L26-L51

---

_Added to milestone `GA` by @MichaReiser on 2025-07-09 11:48_

---

_Label `server` added by @MichaReiser on 2025-07-09 11:48_

---

_Comment by @UnboundVariable on 2025-07-24 17:32_

There are many aspects of the semantic token feature where there is no objectively "correct" answer. For example, if a symbol is a variable whose type is declared as `Callable`, should it be a "variable" or "function" token? The "correct" answer to these questions typically comes down to "whatever is preferred by the most users". That means this feature will require adjustments over time based on user feedback. In that sense, this feature will never be "complete".

The functionality in the current implementation relies on some semantic analysis features that are not yet complete. The most notable one is support for `self` and `cls` parameters and attributes accessed from `self` or `cls`. Once the semantic analyzer is able to infer types of expressions like `self.foo`, the semantic token implementation will start classifying these attributes correctly. 

Here's a checklist of additional work that will likely need to be done.

* [x] Improve classification for name tokens that are imported from other modules. Currently, these are classified based on their types, which often means they're classified as variables when they should be classes, etc. 

* [ ] Properly handle `Annotated` type annotations. All type arguments to an `Annotated` type form other than the first should be treated as value expressions, not as type expressions. Currently, the code blindly treats all type arguments within all annotations as type expressions.

* [x] Properly categorize names that are parameters as either `parameter` ✅ , `selfParameter` ✅  or `clsParameter` tokens. 

* [ ] Properties (or perhaps more generally, descriptor objects?) defined within a class should be classified as a `property` token rather than just a `variable`.

* [ ] Special forms like `Protocol` and `TypedDict` should probably be classified as `class` tokens (rather than `variable` tokens), but this is debatable. Could wait for user feedback.

* [ ] Type aliases (including those defined within the Python 3.12 `type` statement) do not currently have a dedicated semantic token type, but maybe they should? Could wait for user feedback.

* [ ] Additional token modifiers could be added (e.g. for static methods, abstract methods, abstract classes, etc.)? Could wait for user feedback.

---

_Comment by @lucach on 2025-11-08 08:22_

There's also no dedicated support for unions, which seems a secondary aspect but plays a role even in simple common scenarios. Here's one that I bumped into. 

Given that [the semantics treats `float` as a union of `int` and `float`](https://github.com/astral-sh/ruff/blob/16de4aa3ccc3bb8acfb6e750fad639a767b68a0e/crates/ty_python_semantic/src/types.rs#L6373-L6379), out of the three type annotations in this simple example:

```py
def f(a: int, b: float, c: str): pass
```

`int` and `str` are correctly identified as `SemanticTokenType::Class`, while `float` falls back to `SemanticTokenType::Variable`. When classes/types have a special color (common in editor themes, but currently not on ty's playground), the difference in coloring becomes a bit annoying.

---

Edit: Issue #1470 already covers this specific case (I found this issue first because I searched "semantic tokens"  -- this comment will link the two issues).

---

_Comment by @MichaReiser on 2025-11-11 19:17_

For anyone working on this, VS Code has a debugging tool to inspect the semantic token at any given position, see https://code.visualstudio.com/api/language-extensions/syntax-highlight-guide#scope-inspector

Relevant for `float`, a type var `U` gets classified as `Class` (Related to *Type aliases (including those defined within the Python 3.12 type statement) do not currently have a dedicated semantic token type, but maybe they should? Could wait for user feedback.*) and classifying `float` the same way would fix the "mismatch"



---

_Comment by @MichaReiser on 2025-11-12 16:21_

> int and str are correctly identified as SemanticTokenType::Class, while float falls back to SemanticTokenType::Variable. When classes/types have a special color (common in editor themes, but currently not on ty's playground), the difference in coloring becomes a bit annoying.

This should be fixed in the next release ([#21399](https://github.com/astral-sh/ruff/pull/21399))

<img width="404" height="197" alt="Image" src="https://github.com/user-attachments/assets/334e79fa-4936-401c-b68f-8eedd6facc28" />

---

_Renamed from "Follow up on TODOs in semantic token provider" to "Solid semantic tokens" by @MichaReiser on 2025-11-14 09:28_

---

_Comment by @hamdanal on 2025-12-14 13:52_

Let me know if this needs its own issue but it would be cool if both `pandas` and its `pd` alias below are colored the same.
<img width="839" height="281" alt="Image" src="https://github.com/user-attachments/assets/d9330e7f-abff-40f9-aa6b-f4b40ddbce55" />

---

_Comment by @Gankra on 2025-12-15 15:48_

Is there any intentionality to us having two different colorings for all these different kinds of variable name?

<img width="328" height="257" alt="Image" src="https://github.com/user-attachments/assets/e094d6e0-b97e-4f95-9531-9081e721b10e" />

Specifically SCREAMING_CASE variable names that are longer than 2 characters and don't have numbers in their name get the darker treatment which feels extremely arbitrary.

---

_Comment by @MichaReiser on 2025-12-15 16:57_

@Gankra probably not. I suggest taking a look at the classification to understand why we classify the names the way we do, see this comment for how to do that

https://github.com/astral-sh/ruff/blob/6599092e8d4cf9ea196e4c1777acc0ed80d4bb4f/crates/ty_ide/src/semantic_tokens.rs#L6-L11

It's also worth comparing with what pylance does in that instance.

---

_Comment by @Gankra on 2025-12-15 18:02_

Something else we can do in some cases without totally bespoke extensions:

If I put this in a docstring it will render as a link in vscode and clicking it does indeed jump to that file+line:

```markdown
[Test](file:///Users/gankra/dev/tmp/tyerr/main.py#L10)
```

This would not work in code-blocks (and thus would not work in type signatures) but we could potentially do it for single-tick inline `MyClass`, which aiui is conventionally supposed to refer to an actual item.

---

_Comment by @Gankra on 2025-12-15 18:04_

(Alternatively we could value clickability over syntax highlighting of signatures)

---

_Comment by @AlexanderHott on 2025-12-17 16:29_

It would be nice if more types of ranges were supplied by the semantic token highlighter. In rust-analyzer, things like keywords and punctuation get their own ranges.

<img width="615" height="479" alt="Image" src="https://github.com/user-attachments/assets/ad732b34-2470-4fe2-b531-0f92e005d7f4" />

<img width="605" height="609" alt="Image" src="https://github.com/user-attachments/assets/c88a7cde-55b3-4e14-bcab-e7248f0f9183" />



---

_Comment by @MichaReiser on 2025-12-17 16:42_

Can you say more about what the benefit is of doing this as part of semantic tokens instead of leaving it to syntatic tokens (which most editors provide anyway)

---

_Comment by @AlexanderHott on 2025-12-18 17:13_

From my limited understanding of how LSP servers work, I'm under the assumption that the LSP knows the range for each token, but chooses not to output it in the semantic token range request. 

(My guess: IDEs are the primary use case for LSPs. IDEs need syntax highlighting without LSPs, because they can't assume the user has the LSP installed, so LSPs only output the ranges that changed.)

Developing tools around tokens, highlighting, and semantic analysis outside of IDEs is quite complex because of the multi-layer approach that most IDEs use. VSCode uses TextMate, which is not very standardized. From what I've seen, each language has its own set of scopes. VSCode themes that take advantage of semantic highlighting are about 50-100 lines of styling based on semantic tokens, and 4000+ lines of targeting very language-specific TextMate scopes. From a surface level look, tree-sitter seems much better. The types of CST nodes are standardized between parsers.

I'm building a tool that interfaces with an LSP outside the context of an IDE (I chose rust-analyzer as my first target), and was surprised when I saw that most other LSPs don't output all the highlighting ranges. I *could* just use tree-sitter on top of the semantic highlighting ranges, but I'm a bit confused why I need another parser on top of the LSP, which already parses and understands the structure of the source code you give it.

I haven't looked into it at all for the ty codebase, but it seems like it would be easier for me (or someone else) to add the tokens to the LSP response than to integrate a second parser into my project.

I'm willing to take a shot at implementing the feature, with a bit of guidance, of course.

---

_Comment by @MichaReiser on 2025-12-18 17:24_

The main reason not to do it in the LSP is that it makes the response larger, and most editors already run syntax highlighting, so it's just unnecessary duplication. I'm not saying that it's reason enough not to do it, but there are certainly downsides to it for the common use case of usingt ty in an editor.

---

_Comment by @AlexanderHott on 2025-12-18 17:38_

Do you have an estimate for how much larger the response would be? I think only the first response would be large, otherwise it sends incremental updates. Also, the response is already encoded as a list of integers (5 per token I think), which is pretty compact for json.

---

_Comment by @AlexanderHott on 2025-12-18 17:42_

rust-analyzer does it, and I haven't noticed a difference in the highlighting speed. However, I'm not aware of any other LSP that do it. The only other LSP I looked into was typescript-language-server, and they also don't output all the ranges.

---
