```yaml
number: 12755
title: Skip checking a file if it failed to read
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: handle-file-read-errors
created_at: 2024-08-08T14:57:19Z
updated_at: 2024-08-12T07:40:35Z
url: https://github.com/astral-sh/ruff/pull/12755
synced_at: 2026-01-12T15:55:42Z
```

# Skip checking a file if it failed to read

---

_@MichaReiser_

## Summary

Up to know, there was the risk of a race condition between when we discovered the files of a package and when we called `check_file` 
because a file could have been deleted in the meantime before the file watch notification came in. This PR changes `check_file` to raise an error (diagnostic) if that happens. 

I explored a few different designs and ultimately ended with using an accumulator. It's not the nicest API. 

**`try_source_text` function with specify**

My first approach was to introduce a `try_source_text` that returns a `Result<SourceText, SourceTextError>` and uses `specify` (Salsa) to cache the read source text
when the file could be read successfully. Unfortunately, this doesn't work because `specify` requires that the query arguments are tracked Salsa structs, but `File` is an input. 

**`try_source_text` query**
My second approach was to change `source_text` to `try_source_text` that returns a `Result<SourceText, SourceTextError` (now a salsa query) and
to introduce a new `source_text` function (not a salsa query) that invokes the fallback logic. 

This could technically work but there are a few caveats:

* Every (uncached) `source_text` call would create an empty fallback notebook. This should not be too bad because we only do this when we have a very specific race.
* `std::io::Error` doesn't implement `Eq`. That means `source_text` can no longer use Salsa's backdating optimization (because the `Result` no longer implements `Eq`)

**Accumulator**

I do this in this PR: I use a salsa accumulator to emit a diagnostic in source text and explicitly consume the diagnostic in `check_file`. 

I would have preferred if the test for reading the file was more explicit (e.g., by using a `Result`), but I also think the way it is now is fine. 
However, we'll have to add these checks everywhere where we perform file-centric operations (`check`, `fix`, `format`).

We don't need the check for other operations like providing auto-completion because the fallback of an empty document is sufficient in that case. 
The fallback primarily exists to avoid creating additional diagnostics because the text is empty. 


## Should we just change `source_text` to always return a `Result`. 

Besides the points mentioned above: Changing `source_text` to return a `Result` would require making all queries that transitively read a file's source text (which is almost all queries)
to return a `Result`. That would be very painful without having a clear upside. In most cases, assuming the file is empty is actually a pretty good default (e.g., it results in an *import not found** error when trying to import a symbol from that file. This is not 100% accurate but good enough considering that this is a race condition. 


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

See the added integration test


---

_Review requested from @carljm by @MichaReiser on 2024-08-08 14:57_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-08 14:57_

---

_Comment by @github-actions[bot] on 2024-08-08 15:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_box.ipynb:13:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb:15:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sql_database.ipynb:2:2:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_box.ipynb:13:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb:15:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sql_database.ipynb:2:2:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Comment by @MichaReiser on 2024-08-08 17:14_

I think what @carljm would prefer is:

* Add a `has_read_errors()` method to `SourceText` and set it to true if reading the file failed. 
* Define `Diagnostics` in `ruff_db` and pull the diagnostics in `RootDatabase::check` and `RootDatabase::check_file`. 

Finding a generic abstraction for `Diagnostics` will complicate migrating to a type checker CLI

---

_Comment by @MichaReiser on 2024-08-08 19:51_

I thought about this more. I don't mind adding a flag to source text and testing that flag instead of branching based on whether there are Diagnostics.

But I do prefer to defer the decision on whether we want a single Diagnostic type or not because that's less clear to me, but I plan to look into it next week.

That's why I would prefer keeping the accumulator as is (querying an accumulator in a query is supported by salsa). 

---

_@carljm reviewed on 2024-08-08 20:36_

I think you've represented my preferences accurately :)

I don't like making logical decisions in dependent queries based on the diagnostic output of called queries; it feels too fragile and too tightly coupled to implementation details. So I would definitely prefer if this PR were changed to put an explicit flag on `Parsed` instead if there were any I/O errors.

Best IMO would be if we rearranged `Parsed` such that you can only get access to the source text by calling a method which returns a `Result`, so it's not even possible to silently get the empty text when there were errors; you have to choose some way to handle the error.

IMO the question of whether we use a single Diagnostic type and a single global accumulator or we use leveled aggregation of Diagnostics is a very different question, which I don't have a firm opinion on yet and I have no problem with deferring it from this PR.

---

_Comment by @carljm on 2024-08-08 20:41_

Tbh it seems to me that just having `source_text` query return a `Result` is a pretty good option here: it's the most idiomatic/intuitive way to represent this. The `Result` doesn't have to carry the original `std::io::Error`, it can carry our own error type that does implement `Eq`, so back-dating can still work. Caller queries shouldn't care about the details of the I/O error anyway, those details just belong in the user diagnostic.

And the concern about all dependent queries then also having to return a `Result` doesn't make sense to me. The whole point of this PR is that caller queries should be aware if there was a file read error and handle it in some way. It's better if they have to be aware of it. Returning a `Result` achieves that. It doesn't force them to propagate the `Result`; they can instead match on it and handle the `Err` branch however they want (e.g. abort checking, or whatever makes sense.) They have all the same options for handling that situation that they will have no matter how we signal the error to them. There is no law requiring use of the `?` operator to unwrap a `Result` ;)

---

_Label `red-knot` added by @AlexWaygood on 2024-08-08 23:09_

---

_Comment by @MichaReiser on 2024-08-09 05:49_

> Tbh it seems to me that just having source_text query return a Result is a pretty good option here: it's the most idiomatic/intuitive way to represent this. The Result doesn't have to carry the original std::io::Error, it can carry our own error type that does implement Eq, so back-dating can still work. Caller queries shouldn't care about the details of the I/O error anyway, those details just belong in the user diagnostic.

We could do that, although I would then just return an `Option`. But I don't think we should do it...

> And the concern about all dependent queries then also having to return a Result doesn't make sense to me. The whole point of this PR is that caller queries should be aware if there was a file read error and handle it in some way.

To me, the only two options are:

1. `source_text` emits the diagnostic and performs the recovery itself as it does today
2. All queries that use `source_text` or transitively depend on it return `Result` 

The middle ground doesn't make sense to me. It only introduces unclear responsibilities:B

If `source_text` returns an `Result`, is it then the responsibility of `parsed_module` to do the error recovery? I don't think it should because we could then as well have done the error recovery in `source_text`. That leaves the question about which query should then do the error recovery? It would have to be the top queries: `format`, `lint_syntax`, `lint_semantic` because they can then decide not to emit diagnostics. That's option 2 and it introduces the ambiguity of who's now responsible for emitting a diagnostic. Only one method should push a diagnostic of we end up with duplicates but we can't know which queries are called in a single execution. It can be `format` only or `format`, `lint` and `type_check`. 

Returning a `Result` would work better in a non-Salsa world where `check_file` would read the result, deal with any IO errors and call into `lint_syntax` when there are none, passing the source code as an argument. But this doesn't work in a salsa design where queries **pull** the information they need. There's not even a need for `check_file` to call `source_text` at all because it doesn't **need** to pass the result as an argument to any functions it calls. That means, requiring explicit error handling doesn't really work (unless we propagate the error everywhere through) because:

* The methods that **should** handle the error have no need to call `source_text`, and therefore, don't profit from any explicit error handling
* The methods that don't care about error handling (because they should be resilient to errors, similar to the parser) don't really know what to do, other than implementing their own error recovery mechanism


That's why I still think that doing the error recovery in `source_text` is the right approach. I think adding a boolean flag to `SourceText` indicating whether the read has failed and can be used to make such decisions does make sense.




---

_Comment by @carljm on 2024-08-09 05:57_

I agree that there should be clear responsibilities. To me, clear responsibilities means that whatever layer wants to be able to make a different decision depending on whether there was an error, is the (only) layer that should handle the error, and report an appropriate diagnostic to the user depending on its specific needs.

To me, that means that `source_text` should return a `Result` (or `Option`, if callers never need to distinguish different failure modes, which seems plausible) and not report any diagnostic or provide any fallback, `parsed_module` should propagate the error/option and likewise not report any diagnostic or provide any fallback, and any queries that call `parsed_module` should "catch" the error/option and take action as appropriate for their particular case, including possibly reporting a diagnostic. (And not propagate the error/option, of course.)

---

_Comment by @MichaReiser on 2024-08-09 06:01_

> To me, that means that source_text should return a Result (or Option, if callers never need to distinguish different failure modes, which seems plausible) and not report any diagnostic or provide any fallback, parsed_module should propagate the error/option and likewise not report any diagnostic or provide any fallback, and any queries that call parsed_module should "catch" the error/option and take action as appropriate for their particular case, including possibly reporting a diagnostic. (And not propagate the error/option, of course.)

Yeah, but I don't see how this can work (as explained above)

* Multiple queries call `parsed_module` at different stages. There's no guarantee in which order or which queries are executed. We would report the same diagnostic multiple times or never
* Let's say `parsed_module` propagates the error. But which query catches it? `semantic_index`? It can't, because `lint_syntax` would then still not have to deal with handling the error. So it has to propagate and so do all type inference methods all the way up to `lint_syntax`

---

_Comment by @carljm on 2024-08-09 06:09_

I would need to look at the details of who actually calls `parsed_module`, but I need to go to sleep now instead :) I think the more principled approach is to propagate as much as needed to avoid that problem, but I'm also fine with falling back to empty plus setting a flag if you prefer to go that route!

---

_Comment by @MichaReiser on 2024-08-09 06:14_

Everything calls `parsed_module` transitively. Every `SemanticIndex` method ;) My worry is that it would make our code extremely cumbersome. All code in type inference would have to deal with the possibility that the semantic index is `None` because we can't statically assert that it is guaranteed to be non-none when doing type inference. 

I'm happy to discuss this more in person but I do think a flag is the best way to go. It's also closest to what we already to with our AST where incorrect identifiers are "marked" as invalid and require explicit checking, because for most code, it doesn't matter.

---

_Comment by @MichaReiser on 2024-08-09 08:28_

Overall, this seems a fundamental design question and I think our two options really are:

1. Handle the error in `source_text` and recover
2. Propagate the query through the entire semantic analysis and bubble it up all the way to `check_file`. But that would mean that all our query methods (and all methods that use anything derived from a query) become `Result`. I don't think it's worth the complexity that it introduces.

---

_@MichaReiser reviewed on 2024-08-09 08:41_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace.rs`:368 on 2024-08-09 08:41_

Note: Querying both `source_text(...).has_read_error()` and the diagnostics is slightly less efficient than only testing for diagnostics because `source_text` now gets called twice: Once by the query itself to read the `has_read_error` flag and once when resolving the accumulated diagnostics.

---

_@carljm approved on 2024-08-09 19:02_

Looks good, thanks!

---

_Merged by @MichaReiser on 2024-08-12 07:26_

---

_Closed by @MichaReiser on 2024-08-12 07:26_

---

_Branch deleted on 2024-08-12 07:26_

---
