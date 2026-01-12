```yaml
number: 19194
title: "[ty] Initial implementation of signature help provider"
type: pull_request
state: merged
author: UnboundVariable
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: signature_help
created_at: 2025-07-08T02:26:48Z
updated_at: 2025-07-11T02:32:04Z
url: https://github.com/astral-sh/ruff/pull/19194
synced_at: 2026-01-12T15:56:34Z
```

# [ty] Initial implementation of signature help provider

---

_@UnboundVariable_

This PR includes:
* Implemented core signature help logic
* Added new docstring method on Definition that returns a docstring for function and class definitions
* Modified the display code for Signature that allows a signature string to be broken into text ranges that correspond to each parameter in the signature
* Augmented Signature struct so it can track the Definition for a signature when available; this allows us to find the docstring associated with the signature
* Added utility functions for parsing parameter documentation from three popular docstring formats (Google, NumPy and reST)
* Implemented tests for all of the above

"Signature help" is displayed by an editor when you are typing a function call expression. It is typically triggered when you type an open parenthesis. The language server provides information about the target function's signature (or multiple signatures), documentation, and parameters.

Here is how this appears:

![image](https://github.com/user-attachments/assets/40dce616-ed74-4810-be62-42a5b5e4b334)


---

_Review requested from @carljm by @UnboundVariable on 2025-07-08 02:26_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-08 02:26_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-08 02:26_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-08 02:26_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-08 02:26_

---

_Comment by @github-actions[bot] on 2025-07-08 02:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo fields = ~152MB
+     memo fields = ~159MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-07-08 03:29_

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

_Review requested from @dhruvmanila by @dhruvmanila on 2025-07-08 04:07_

---

_Label `server` added by @MichaReiser on 2025-07-08 07:08_

---

_Label `ty` added by @MichaReiser on 2025-07-08 07:08_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/signature_help.rs`:18 on 2025-07-08 07:30_

Rust has explicit syntax for module documentation

```suggestion
//! This module handles the "signature help" request in the language server
//! protocol. This request is typically issued by a client when the user types
//! an open parenthesis and starts to enter arguments for a function call.
//! The signature help provides information that the editor displays to the
//! user about the target function signature including parameter names,
//! types, and documentation. It supports multiple signatures for union types
//! and overloads.

use crate::{Db, docstring::get_parameter_documentation, find_node::covering_node};
use ruff_db::files::File;
use ruff_db::parsed::parsed_module;
use ruff_python_ast::{self as ast, AnyNodeRef};
use ruff_text_size::{Ranged, TextRange, TextSize};
use ty_python_semantic::semantic_index::definition::Definition;
use ty_python_semantic::types::{
    CallSignatureDetails, ParameterLabelOffset, call_signature_details,
    get_docstring_for_definition,
};
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/signature_help.rs`:43 on 2025-07-08 07:31_

Can we document what the offsets are? Are those byte offsets? An index offset? 

If they're byte offsets, I recommend using `TextSize` which is what we use for all byte offsets.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/signature_help.rs`:72 on 2025-07-08 07:32_

What's the reason that this field is necessary when it's already present on `SignatureInfo`. Can we remove one of the two?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/signature_help.rs`:171 on 2025-07-08 07:39_

Nit, I think you can simplify this to 
```suggestion
    for (i, arg) in call_expr.arguments.arguments_source_order().enumerate() {
        if offset <= arg.end() {
            return i;
        }
        current_arg = i + 1;
    }
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/signature_help.rs`:186 on 2025-07-08 07:41_

Nit: Unlike Python, Rust doesn't require forward declarations. That's why we often put helper/utility functions later in the file (often called newspaper style). In this case, `get_callable_documentation` would come after `create_signature_info_from_details`.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/signature_help.rs`:286 on 2025-07-08 07:47_

Our intended split between the `ide` and `server` crates are:

* `ide` implements the semantic analysis and returns a general purpose data structure
* The `server` crate maps the result to whatever datastructure is required by the LSP specification. Respects client capabilities etc.
* The `wasm` crate maps the data structure to whatever data structure we want to expose (often a simplified representation of the LSP's representation)

That's why I think that the mapping from `SignatureDetails` to `SignatureInfo` should be moved to the `server` crate (we may want a `ParameterDetails` intermediate struct. I also think that we should move all the client capability handling out of the `ide` crate into the `server crate (e.g. WASM doesn't care about capabilities)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/signature_help.rs`:297 on 2025-07-08 07:47_

```suggestion
    let Some(first) = signature_details.first() else {
        return None;
    }

    // If there are no arguments in the mapping, just return the first signature.
    if first
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/signature_help.rs`:304 on 2025-07-08 07:49_

Nit: I would probalby use an inline snapshot for this as it seems cumbersome to write those expected docstrings

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/call/argument_matcher.rs`:1 on 2025-07-08 07:51_

Are there any changes to this module or is this a simple move? 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:747 on 2025-07-08 07:53_

Could we use `TextRange` instead?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:646 on 2025-07-08 07:57_

I would probably use an enum here over a trait with two structs because the number of implementations is closed (we don't want to allow arbitrary implementations of `SignatureWriterTrait`).


```
enum SignatureWriter<'a, 'b> {
	Formatter(&'a mut Formatter<'b>),
	Details(SiagnatureDetailsWriter)
}

impl SignatureWriter<'_, '_> {
	fn write_char(...) { ... }
	...
}
```

The advantage of an enum over a trait is that it doesn't lead to monomorphizing `write_signature`, resulting in a smaller binary

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:293 on 2025-07-08 07:58_

Can we use `TextRange` here instead?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:400 on 2025-07-08 08:00_

I wonder if this should be a method on `FunctionType` on `CallableType` instead

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:420 on 2025-07-08 08:39_

Is it necessary that `argument_forms` and `conflicting_forms` have exactly the length of `call_arguments.len`? If so, this API seems very fragile as this contract escapes `ArgumentMatcher::new`. It would be great if this could be isolated more

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:445 on 2025-07-08 08:41_

This seems to be identical to what we have in `TypeInferenceBuilder::parse_arguments`. Should we make this a method on `CallArguments` instead (e.g. `CallArguments::from_arguments`?)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:489 on 2025-07-08 08:42_

I'd be inclined to make this a method on `Definition` instead. It should be easier to discover than having this standalone function.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/signatures.rs`:933 on 2025-07-08 08:46_

Looking at the usage, the following seems to be a more convenient API
```suggestion
    /// Create a new signature with the given definition.
    pub(crate) fn with_definition(
    		self, 
        definition: Option<Definition<'db>>,
    ) -> Self {
        Self {
            definition: Some(definition),
            ..self
        }
```

The call in `ClassType` then becomes:

```rust
let new_signature = Signature::new(signature.parameters().clone(), Some(correct_return_type))
	.with_definition(signautre.definition())
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:668 on 2025-07-08 08:50_

Do we need to set the definition for regular function definitions too?

E.g. is the definition tracked for

```rust
def test(): pass


test(
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:363 on 2025-07-08 08:54_

Could this be moved to the `ty_ide` crate. It feels a bit misplaced or would that require making some APIs public that we rather don't make public? If so, could we expose higher level APIs for those operations instead of having the entire signature details in the semantic crate.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:1 on 2025-07-08 09:03_

I suggest to add docstring support in a separate PR and we should check if there's any infrastructure in Ruff that can be reused https://github.com/astral-sh/ruff/blob/815a074ccc9c0094fead9d56926973421e44632f/crates/ruff_linter/src/rules/pydocstyle/helpers.rs#L73-L148



---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:41 on 2025-07-08 09:04_

Building the regex-automate is somewhat expensive. That's why regex should be instantiated exactly once and then be reused.

https://docs.rs/regex/latest/regex/#avoid-re-compiling-regexes-especially-in-a-loop

We should also double check if any of the other performance recommendations apply

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:43 on 2025-07-08 09:04_

Rust's `lines` iterator doesn't handle `\r`. We should use the `UniversalNewlinesIterator` instead.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:43 on 2025-07-08 09:05_

Can we avoid collecting the lines here?

---

_@MichaReiser reviewed on 2025-07-08 09:06_

Nice. I think it makes sense to split the docstring support out of this PR to reduce (the already large scope) and have the main feature landed sooner.

---

_@UnboundVariable reviewed on 2025-07-08 16:49_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/signature_help.rs`:72 on 2025-07-08 16:49_

This is related to the way the language server protocol exposes this information. Some clients do not support per-signature "active parameters", which was added in later versions of the LSP. However, I agree that this could be removed from the IDE structure. The server logic doesn't need this field to build the final value returned to the client.

---

_@UnboundVariable reviewed on 2025-07-08 16:56_

---

_Review comment by @UnboundVariable on `crates/ty_python_semantic/src/types/call/argument_matcher.rs`:1 on 2025-07-08 16:56_

It's mostly a cut-and-paste move, but I needed to do some minor renames for the separation to make sense. The `MatchArgumentsError` was previously named `BindingError`, but its functionality didn't change.

---

_@UnboundVariable reviewed on 2025-07-08 16:58_

---

_Review comment by @UnboundVariable on `crates/ty_python_semantic/src/types/ide_support.rs`:420 on 2025-07-08 16:58_

I agree, but I'd prefer to leave that change to someone who is more familiar with the existing ArgumentMatcher logic. If it's OK with you, we could leave that as a follow-on to this PR.

---

_@UnboundVariable reviewed on 2025-07-08 17:02_

---

_Review comment by @UnboundVariable on `crates/ty_python_semantic/src/types/ide_support.rs`:363 on 2025-07-08 17:02_

This is a discussion I wanted to have with you. It depends on the design philosophy. I initially started down the path of writing this logic in `ty_ide`, but I found that I needed to expose many private APIs as public (I was up to six by the time I changed course and moved the logic into a single new public API). 

---

_@UnboundVariable reviewed on 2025-07-08 17:05_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/docstring.rs`:1 on 2025-07-08 17:05_

Before writing this, I looked in the ruff sources (or, rather had an AI agent scan through the sources), but I didn't uncover any logic that extracted parameter information from docstrings. Perhaps one of the ruff maintainers would know definitively.

---

_@MichaReiser reviewed on 2025-07-09 07:29_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:420 on 2025-07-09 07:29_

I think my concern here is that we expose an API that's considered to be internal. It also seems that we aren't prepending `self` which might be necessary for methods. And I think this also skips any overload selection. 

I feel like `Binding` should give you the information you need. You can create a binding from `Binding::single(callable_type.into(), signature.clone())` and you can then call `Binding::match_parameters`. The argument mapping is then stored on `argument_parameters`. 

This doesn't resolve the issue that we need to pass some unnecessary vecs. Maybe @dhruvmanila has an idea on how we can avoid this

---

_@MichaReiser reviewed on 2025-07-09 08:07_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:363 on 2025-07-09 08:07_

My initial aim was to have as much as possible in `ty_ide` and avoid LSP/IDE specific types in `ty_python_semantic`. However, I haven't implemented enough to have a good sense to what degree this is possible. think there's an established pattern yet and this is something you can help us shape. 

My general thinking is that the APIs in `ty_python_semantic` shouldn't be tight to the LSP and be useful in other contexts too. E.g. getting the docstring from a definition is not only useful in the LSP case but also something that's useful in a linter. 

I also expect this to be something that evolves over time as we learn more about how to slice and dice the API and keeping concerns separate. 

So I don't think we have to push it too hard here to move this out of `ty_python_semantic`. I mainly wanted to raise the question if parts of it could be moved to `ty_ide` or if we could return more generic data that then gets restructured into something that the LSP needs. 


Note: This is pre-existing and not related to your PR but I think that `ide_support` is probably not the best module name to encourage this distinction. 


---

_@MichaReiser reviewed on 2025-07-09 08:09_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:385 on 2025-07-09 08:09_

I find this field somewhat unexpected as it isn't specific about a signature, but a particular call to that signature. 

I wonder if we should make this a method instead

---

_@MichaReiser reviewed on 2025-07-09 08:12_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:129 on 2025-07-09 08:12_

Nit: You could use https://github.com/astral-sh/ruff/blob/9f3a38d408f473df7a6b3574c5d1389bef303dd5/crates/ruff_python_trivia/src/whitespace.rs#L49-L52

unless you want to account for unicode whitespace characters too

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:150 on 2025-07-09 08:13_

Nit:

```suggestion
            if lines.get(i + 1).is_some_and(|next| underline_regex.is_match(next) {
```

---

_@MichaReiser reviewed on 2025-07-09 08:13_

---

_@MichaReiser reviewed on 2025-07-09 08:14_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:356 on 2025-07-09 08:14_

Does rest require a 4 character indent or are 2-spaces or tab indents valid too?

---

_@MichaReiser reviewed on 2025-07-09 08:15_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:1 on 2025-07-09 08:15_

You know what. Let's leave this in but add a TODO that we should see if we can share some infrastructure with Ruff. The person working on docstrings can then pick th is up.

---

_Comment by @MichaReiser on 2025-07-09 17:43_

I had a second look. I think the only thing that would be great to address before merging is if we could avoid calling `ArgumentsMatcher` directly and instead use `Binding` (or see if @dhruvmanila has an idea what we could do instead) which is a slightly higher abstraction. 

The rest are mostly nits

---

_@UnboundVariable reviewed on 2025-07-10 03:11_

---

_Review comment by @UnboundVariable on `crates/ty_python_semantic/src/types/ide_support.rs`:400 on 2025-07-10 03:11_

Hmm, I'm not sure about that. It seems weird to me that a method on `FunctionType` or `CallableType` would accept a file and `ExprCall` ast node. This strikes me more as a higher-level utility function.

I'm going to leave it as is now, but if you feel strongly about it, we can move the logic separately from this PR.

---

_@UnboundVariable reviewed on 2025-07-10 03:37_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/docstring.rs`:43 on 2025-07-10 03:37_

I'm not sure what you mean by `UniversalNewlinesIterator`. I can't find any reference to this in the code base or in the Rust documentation. Does this come from some other crate?

---

_@UnboundVariable reviewed on 2025-07-10 03:44_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/docstring.rs`:43 on 2025-07-10 03:44_

For now, I'll include a custom implementation of `UniversalNewlinesIterator`. We can replace it in the future if there's a library that provides the same functionality.

---

_Comment by @dhruvmanila on 2025-07-10 04:11_

(Going to look at this today.)

---

_Comment by @UnboundVariable on 2025-07-10 04:26_

@MichaReiser, thanks for the feedback. I've incorporated the recommended changes.

I think the PR is looking pretty good at this point, so I think it's ready to merge. @dhruvmanila, if you want to do another round of reviews, I'll hold off until you're done.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/docstring.rs`:62 on 2025-07-10 05:04_

Is this the same as

https://github.com/astral-sh/ruff/blob/1ddda241f69d6e1aa651d279983a9738ddb2f892/crates/ruff_source_file/src/newlines.rs#L18-L40

?

If so, we should re-use that if possible.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/docstring.rs`:119 on 2025-07-10 05:04_

And the trait implementation:

https://github.com/astral-sh/ruff/blob/1ddda241f69d6e1aa651d279983a9738ddb2f892/crates/ruff_source_file/src/newlines.rs#L7-L16

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/docstring.rs`:1 on 2025-07-10 05:11_

We do have docstring related infrastructure in Ruff at https://github.com/astral-sh/ruff/tree/1ddda241f69d6e1aa651d279983a9738ddb2f892/crates/ruff_linter/src/docstrings but I think it's very tailored towards Ruff usage. I'm not sure if it could be used here (probably not).

I'll create a tracking issue for this.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/signature_help.rs`:30 on 2025-07-10 05:13_

In your example, is the label value "str" or "param1: str"? If it's the former, it might be useful to reword it as such: "(e.g., "str" in "param1: str")"

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2275 on 2025-07-10 05:22_

Can we just return the `Box` instead of allocating a new vector? That would be a bit cheaper instead: `self.argument_parameters.clone()`

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/signature_help.rs`:26 on 2025-07-10 05:42_

I'm not exactly sure why do we need this? Can we directly query the resolve client capabilities instead?

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/ide_support.rs`:396 on 2025-07-10 05:54_

Is a one-dimensional vector enough to represent everything that's required (unions, overloads, etc.)?

Related to my other comment on using `Bindings`, if one-dimensional vector is enough and we just need a list of all the signature details, we would just loop over each of the `CallableBinding` (in `Bindings`) and then the individual `Binding` (in `CallableBinding`) by flattening the iterator.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/ide_support.rs`:494 on 2025-07-10 05:57_

I don't think we should be doing this manually as `binding.match_parameters` is a private API which is meant to be used via `Bindings::match_parameters` (notice the `s` in `Bindings`) which is the top-level entry point to matching parameters against the arguments and it will take care of unions and overloads.

In the above match expression where you're matching against `func_type.into_callable(db)`, we should be doing:
```rs
if let Some(callable_type) = func_type.into_callable(db) {
	let call_arguments = CallArguments::from_arguments(arguments);
	let bindings = callable_type.bindings(db).match_parameters(&call_arguments);
}
```

And, access the match results from `bindings` instead. This provides access to the individual `CallableBinding` via the iterator protocol but we could possibly expose it as `&[CallableBinding]` as well. And, similarly the information from a single `Binding` can be accessed from the `CallableBinding`.

Does that make sense?

---

_@dhruvmanila approved on 2025-07-10 06:07_

This looks great, thanks!

My only suggestion, which could be done in a follow-up, is to use the `Bindings` API and not the low-level `Binding` API to perform parameter matching.

---

_@dhruvmanila reviewed on 2025-07-10 06:12_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/docstring.rs`:1 on 2025-07-10 06:12_

https://github.com/astral-sh/ruff/issues/19250

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:122 on 2025-07-10 06:56_

It seems this only returns `None` if the `HashMap` is empty. I suggest directly returning the `HashMap` here and let the caller check if it is empty. It avoids the ambiguity that `Some(HashMap)` could also contain an empty `HashMap` (not clear to the caller). Creating an empty `HashMap` doesn't allocate, so there's no cost to it.

```suggestion
fn extract_google_style_params(docstring: &str) -> HashMap<String, String> {
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:213 on 2025-07-10 06:56_

```suggestion
fn extract_numpy_style_params(docstring: &str) -> HashMap<String, String> {
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:381 on 2025-07-10 06:56_

```suggestion
fn extract_rest_style_params(docstring: &str) -> HashMap<String, String> {
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/call/bind.rs`:2273 on 2025-07-10 06:59_

```suggestion
    pub(crate) fn argument_to_parameter_mapping(&self) -> &[Option<usize>] {
```

This leaves the decision whether it's necessary to copy this over into a owned `Vec` to the caller but makes it "free" for cases where this isn't needed.

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/signature_help.rs`:95 on 2025-07-10 07:02_

The offset here are byte offsets. It isn't entirely clear to me after reading the specification what the supposed encoding of the range is:

> The offsets are based on a UTF-16 string representation as `Position` and `Range` does.

because the encoding of `Position` and `Range` depends on the negogiated position encoding. That's why I suspect that we need to honor the position encoding here too.



---

_@MichaReiser approved on 2025-07-10 07:06_

---

_@UnboundVariable reviewed on 2025-07-11 02:08_

---

_Review comment by @UnboundVariable on `crates/ty_server/src/server/api/requests/signature_help.rs`:95 on 2025-07-11 02:08_

Good catch. Yes, the spec isn't clear here, but looking at other language server implementations, it appears that your assumption is correct. The pyright language server doesn't handle this correctly; it assumes UTF-16 encoding offsets here even if that's different from what was negotiated.

---

_Merged by @UnboundVariable on 2025-07-11 02:32_

---

_Closed by @UnboundVariable on 2025-07-11 02:32_

---

_Branch deleted on 2025-07-11 02:32_

---
