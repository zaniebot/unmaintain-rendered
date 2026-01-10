```yaml
number: 12927
title: "[`flake8-type-checking`] Adds implementation for TC007 and TC008"
type: pull_request
state: merged
author: Daverball
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat/tch007-tch008
created_at: 2024-08-16T11:43:49Z
updated_at: 2024-11-28T15:29:42Z
url: https://github.com/astral-sh/ruff/pull/12927
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-type-checking`] Adds implementation for TC007 and TC008

---

_Pull request opened by @Daverball on 2024-08-16 11:43_

This is part of a series of pull requests fulfilling my promise in #9573 to port some of the enhancements and new rules from flake8-type-checking to ruff.

## Summary

This adds the missing flake8-type-checking rules TC007 and TC008 including auto fixes.

There is some overlap between TC007 and TC004, currently the existing rule takes precedence, although ideally we would still emit both violations and share a fix for overlaps based on the selected strategy (if it is possible for violations of two different rules to share the same fix). We could probably move the analysis for TC007 into the same function as TC004 in order to accomplish that. But we could defer that to a future pull request.

There is also some potential overlap between TC008 and TC010. Currently TC010 is prioritized, but since it currently has no fix it might make more sense to either prioritize TC008 or add a fix for TC010, although that could be covered by a future pull request.

## Test Plan

`cargo nextest run`

---

_Comment by @github-actions[bot] on 2024-08-16 11:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch_cli/snakemake/config/utils.py#L13'>src/latch_cli/snakemake/config/utils.py:13:33:</a> TC008 [*] Remove quotes from type alias
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/analytics/views/stats.py#L342'>analytics/views/stats.py:342:14:</a> TC008 [*] Remove quotes from type alias
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/analytics/views/stats.py#L344'>analytics/views/stats.py:344:16:</a> TC008 [*] Remove quotes from type alias
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/confirmation/models.py#L76'>confirmation/models.py:76:5:</a> TC008 [*] Remove quotes from type alias
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/confirmation/models.py#L77'>confirmation/models.py:77:5:</a> TC008 [*] Remove quotes from type alias
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/zerver/lib/push_notifications.py#L75'>zerver/lib/push_notifications.py:75:49:</a> TC008 [*] Remove quotes from type alias
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC008 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @Daverball on 2024-08-16 12:25_

Looks like forward references to symbols that occur later in the code are not handled correctly yet. Is there already a standard way to perform a binding lookup in a runtime only context? Or will i have to manually compare the `TextRange` of the binding against the `TextRange` of the type alias' value expression?

Edit: I ended up doing the `TextRange` comparison for now.

---

_Comment by @codspeed-hq[bot] on 2024-08-16 13:25_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Daverball:feat/tch007-tch008)

### Merging #12927 will **not alter performance**

<sub>Comparing <code>Daverball:feat/tch007-tch008</code> (8cf8f27) with <code>main</code> (e0f3eaf)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Comment by @Daverball on 2024-08-16 15:48_

I'm struggling to implement an accurate runtime binding lookup since in addition to retrieving an accurate text range for the definition I need to special case class scopes:

```python
class A:
    x: TypeAlias = 'A'  # fine
    class B:
        x: TypeAlias = 'A' # fine
        y: TypeAlias = 'B' # fine
```

None of these should trigger TCH008 and the unwrapped case should trigger TCH007. I could port the logic from flake8-type-checking but that means recursively traversing the scopes upwards and checking each binding in every scope until we find one that is available at runtime, i.e. skipping any symbols that occur lexicographically after the reference and also skipping the class binding of the class scope(s) we started in. In addition for `AnnAssign` we need to consider the `TextRange` of the statement, rather than the name to get correct lexicographical lookup behavior. Similar to `parent_range` for import bindings.

It might be sufficient to add a hash table of scope ids to a vector of binding ids for bindings that violate the lexicographical order for name lookups within that scope.

So the scope of `A` would contain a reference to the binding of `A` and the scope of `B` would contain references to the bindings of `A` and `B`. So if our linked binding happens to be in that list, then we can assume the lookup will result in a `NameError` at runtime.

---

_Comment by @Daverball on 2024-08-16 20:12_

Looks like what I want is `lookup_symbol` but I will need to modify the state before the call so the function no longer thinks we're inside a forward reference. That will make things a lot more simple again.

Edit: Nevermind, it doesn't look like it takes lexicographical order into account and instead relies on being called at the right time during analysis, which puts me into a difficult spot, since if I do it at the right time, I will be forced to parse the type expression a second time before all the symbols can be resolved and if I do it at the wrong time I will have to jump through additional hoops to make the lookup behave correctly. Not sure what's better at this point. Instead of doing a full AST parse I could however do what I do in flake8-type-checking and use a simple Regex parser that just extracts all the names from the expression. That way the cost of parsing the expression twice should be lower.

---

_Comment by @Daverball on 2024-08-17 05:44_

Doing the check earlier doesn't work either, because I still need to know about bindings that occur lexicographically later, so it can be distinguished from actually undefined names. In flake8-type-checking I perform two lookups, once in a runtime context and once in a typing context, and only if the typing lookup succeeds while the runtime lookup fails I emit a TCH008.

The current way the AST/semantic checker is structured those two lookups would have to happen at two completely different times. But that makes the logic to determine which references should prevent a TCH008 pretty complicated. 

Maybe it makes more sense to do what I did in flake8-type-checking and add a new version of `lookup_symbol` that expects a `ast::NameExpr` and performs scope  + lexicographical analysis to determine whether or not a symbol actually could be available at runtime when this lookup happens. It will however, as previously mentioned, require some additional bookkeeping for the class bindings that should be unavailable if we start the lookup inside a class scope.

The lookup for `AnnAssign` bindings will be more expensive too, since we will need to expand the range from the `Name` to the whole `Stmt` so it would overlap the range of references inside the value expression for recursive type aliases.

---

_Comment by @Daverball on 2024-08-18 09:00_

@charliermarsh It might be worth to move the TCH010 logic to where the TCH008 logic currently sits, so we can actually autofix TCH010 in the same cases where we would otherwise emit a TCH008 by removing the quotes when we know that the quoted expression should be entirely available at runtime.

Concretely we would pass it at the stage where we parse deferred string annotations and check if the current parent expression is a `|` `BinOp`. Rather than walk the AST of type definitions to collect the string literals (which doesn't appear to currently properly handle deeply nested cases like eg. `list["Foo" | None]`).

The drawback would be that it may be more difficult to create an autofix in the opposite case that expands the quotes to the entire type expression, since we would need to walk the parent nodes in order to determine the root node of the type expression.

---

_Marked ready for review by @Daverball on 2024-08-20 16:01_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-09-02 06:08_

---

_Label `rule` added by @MichaReiser on 2024-09-02 06:08_

---

_Comment by @charliermarsh on 2024-09-03 00:49_

Thanks @Daverball, sorry that I haven't had a chance to review this yet.

---

_Comment by @Daverball on 2024-10-09 08:32_

@charliermarsh Just a friendly reminder in case this PR slipped through the cracks somehow

---

_Comment by @Daverball on 2024-11-05 14:00_

@charliermarsh Another reminder. Please let me know if these reminders are a bother. The contribution guidelines don't really seem to contain any information about whether it's acceptable/appreciated/frowned upon to bump review requests, it tends to vary from project to project and maintainer to maintainer.

---

_Comment by @charliermarsh on 2024-11-05 14:26_

No that's fine @Daverball -- sorry, it's just blocked on me finding time and clearly I haven't done a good job of doing so yet.

---

_@sbrugman reviewed on 2024-11-09 13:38_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:40 on 2024-11-09 13:38_

Nit:
```suggestion
/// - [PEP 613 – Explicit Type Aliases](https://peps.python.org/pep-0613/)
```



---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:98 on 2024-11-09 13:40_

Nit:
```suggestion
/// - [PEP 613 – Explicit Type Aliases](https://peps.python.org/pep-0613/)
/// - [PEP 695: Generic Type Alias](https://peps.python.org/pep-0695/#generic-type-alias)
/// - [PEP 604 – Allow writing union types as `X | Y`](https://peps.python.org/pep-0604/)
```

---

_@sbrugman reviewed on 2024-11-09 13:40_

---

_@sbrugman reviewed on 2024-11-09 13:41_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:136 on 2024-11-09 13:41_

Nit:

```rs
if names.is_empty(){
    return None;
}

...
```

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:38 on 2024-11-09 13:44_

Why is the fix marked as unsafe? We can document this in a `## Fix safety` section,

---

_@sbrugman reviewed on 2024-11-09 13:44_

---

_@sbrugman reviewed on 2024-11-09 13:50_

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TCH007.py`:21 on 2024-11-09 13:50_

What does the fix look like when comments are present?

example:
```python-console
>>> i: TypeAlias = (int |  # TCH007 x2 
float)
>>> i
int | float
```

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TCH008.py`:35 on 2024-11-09 13:50_

Is the fix still safe when comments are present?

example:

```python
type M = ( 'M' # hello
| None)
```

---

_@sbrugman reviewed on 2024-11-09 13:50_

---

_@Daverball reviewed on 2024-11-09 14:23_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:38 on 2024-11-09 14:23_

Good question, I went by the precedent set by `runtime_import_in_type_checking_block` which uses the same function to quote annotations in its fix and also doesn't explain its reasoning. So I don't know what the original reasoning was. It might just be the fact that the forward reference would not be able to be resolved at runtime. Which is a problem with flake8-type-checking in general.

---

_@Daverball reviewed on 2024-11-09 14:58_

---

_Review comment by @Daverball on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TCH008.py`:35 on 2024-11-09 14:58_

This one should be fine as long as you don't use implicit concatenation in the annotation.

So
```
type M = ( 'M' # hello
'| None')
```
may cause issues, but should not be commonly used, since it's technically out of spec, type checkers only have to support multi-line string annotations. mypy does support this style though.

So both in this case and the TCH007 case you mentioned above this would get rid of the comment. But I suppose this explains why the fix was marked unsafe and this means TCH007 should be unsafe as well.

---

_@Daverball reviewed on 2024-11-09 15:02_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:38 on 2024-11-09 15:02_

Looks like it is at least in part because it can remove comments on multi-line expressions. Maybe this documentation gap should first be addressed in a separate PR though, since it also affects other existing rules.

---

_@sbrugman reviewed on 2024-11-09 15:04_

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TCH008.py`:35 on 2024-11-09 15:04_

It's possible to check if the annotation contains comments and use that to mark the fix as safe or unsafe:

```rs
if !checker
            .comment_ranges()
            .has_comments(annotation, checker.source())
        {
...
}
```

---

_Review comment by @Daverball on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TCH007.py`:21 on 2024-11-09 15:44_

Until I know why this fix was marked unsafe in the other rules I will continue always to mark this fix as unsafe. If the only reason was the fix potentially eating comments I can apply the same comment detection logic to TCH007.

---

_@Daverball reviewed on 2024-11-09 15:44_

---

_Label `preview` added by @dhruvmanila on 2024-11-18 06:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/mod.rs`:39 on 2024-11-19 08:51_

What I understand is that TCH007 and TCH008 are kind of the opposite of each other. It could be nice to write a test that applies the  TCH007 fixes and then asserts that there are no TCH008 violations (and the other way round)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:38 on 2024-11-19 08:53_

It would be great if we can fill the gap for new rules. But I agree, filling the gap for existing rules can be done in a separate PR

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:19 on 2024-11-19 08:54_

How about: Referencing type-checking only imports results in a `NameError` at runtime.

To avoid using `we`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs`:381 on 2024-11-19 08:59_

What's the reason we can't fix this today?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:1862 on 2024-11-19 11:16_

I'm sorry if this is a dumb question, but I'm not very familiar with this part of the code base.

I suspect that mutating `self.semantic.flags` is necessary so that the flags are part of the snapshot created by `self.semantic.snapshot`? This wasn't immediately clear to me. It might be worth adding a comment explaining this.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:280 on 2024-11-19 11:21_

Is this what `quote_annotation` does? If so, it might be worth to link directly to the function

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/binding.rs`:138 on 2024-11-19 11:22_

I liked how you linked to the relevant pep in `checker/ast/mod.rs`. Could we do the same here?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/binding.rs`:268 on 2024-11-19 11:23_

What does `defn` stand for`. Is it `definition`? If so, I suggest we use the long form (it's not that much longer) because we generally try to avoid abbrevation, because it can be unclear what they mean.

What about `FunctionDef`?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:666 on 2024-11-19 11:27_

Can you expand on why you think this should happen above? 

My assumption is that it's here because it updates the state for the next iteration. The only possible issue I see is that it fails updating the `seen_function` if the `self.bindings[binding_id].kind` is an `Annotation`. I, unfortunately, don't have a good enough understanding of this code to assess whether this is a) possible and b) a problem

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:697 on 2024-11-19 11:40_

I don't think I'm the right person reviewing this. @AlexWaygood or @carljm could either of you take a look to see if that makes sense (overall)? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:160 on 2024-11-19 11:42_

I suspect this change behavior for existing rules. If so, then I think it would make sense to extract this change into its own PR so that it gets highlighted in the changelog.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:184 on 2024-11-19 11:48_

I think we have to make sure here that this is indeed the `typing.Literal` and not just any other literal

---

_@MichaReiser approved on 2024-11-19 11:51_

Nice. This overall looks good to me, but I know very little about typing or that part of the code base. That's why it would be great to get a second opinion from either @carljm or @AlexWaygood 

---

_@Daverball reviewed on 2024-11-19 12:41_

---

_Review comment by @Daverball on `crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs`:381 on 2024-11-19 12:41_

We can, I just didn't want to introduce unrelated changes to this PR, so I opted for a comment as reminder to myself to clean this up in a follow-up pull request.

---

_@Daverball reviewed on 2024-11-19 12:45_

---

_Review comment by @Daverball on `crates/ruff_linter/src/checkers/ast/mod.rs`:1862 on 2024-11-19 12:45_

Your suspicions are correct, we need to be able to distinguish type definitions that were created through a generic alias later on. I'm fine with adding a comment.

---

_@Daverball reviewed on 2024-11-19 12:52_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:160 on 2024-11-19 12:52_

Fair point. Although you'd get infinite fix/violation cycles without this extra check between TCH004 and TCH008. So maybe I should only perform this extra check if TCH008 are enabled. That way the behavior stays the same unless you enable the preview rules.

---

_@Daverball reviewed on 2024-11-19 12:57_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:184 on 2024-11-19 12:57_

Good catch, this should indeed use `match_typing_expr`.

---

_@Daverball reviewed on 2024-11-19 12:58_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:184 on 2024-11-19 12:58_

It also doesn't handle `typing.Annotated` yet. So I will need to fix that as well.

---

_@Daverball reviewed on 2024-11-19 13:10_

---

_Review comment by @Daverball on `crates/ruff_python_semantic/src/binding.rs`:268 on 2024-11-19 13:10_

This is a very specialized function to ensure that the following lexicographical lookups are correct:

```python
x: TypeAlias = x | None   # x is not available at runtime

class X:
    foo: TypeAlias = X   # X is not available at runtime
```

For that the entire statement needs to be used for looking up whether the binding occurred lexicographically before our lookup. Otherwise we get the wrong result. The same is not true for functions, since they can refer to themselves.

I couldn't really think of a good name for this function, maybe it would be better to inline this logic into `simulate_runtime_load`, since it's only used once.

---

_@MichaReiser reviewed on 2024-11-19 13:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/binding.rs`:268 on 2024-11-19 13:17_

moving it closer to `simulate_runtime_load` seems preferable to me (could be a standalone, but module private function where I care less about names)

---

_@Daverball reviewed on 2024-11-19 13:20_

---

_Review comment by @Daverball on `crates/ruff_python_semantic/src/model.rs`:666 on 2024-11-19 13:20_

I'm fairly confident it's incorrect to skip updating `seen_function` if you see a `BindingKind::Annotation`.

That being said, situations where this would actually lead to the wrong result should be incredibly rare, since the corresponding Python code itself would be highly suspect and probably incorrect.

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/mod.rs`:39 on 2024-11-19 15:11_

If I'm not mistaken, in order to accomplish this all I would have to do is to generate linter settings that enable both rules, since the test utility functions already make sure that fixes converge after a few iterations, correct?

---

_@Daverball reviewed on 2024-11-19 15:11_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/mod.rs`:39 on 2024-11-19 15:13_

Yes, I think you're right!

---

_@MichaReiser reviewed on 2024-11-19 15:13_

---

_@Daverball reviewed on 2024-11-19 15:30_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:160 on 2024-11-19 15:30_

Actually this doesn't seem to cause unstable fixes after all, I think I changed this so the behavior is more consistent with the flake8 plugin.

I'll remove this change for now and leave a comment. I think eventually we want a setting to independently configure the preferred action for runtime annotations and explicit type aliases, since depending on the use-case you want the type alias to be available at runtime, but you may not care about runtime annotations.

---

_Review comment by @carljm on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TCH008.py`:39 on 2024-11-20 00:04_

Not sure if/when we will forward-port https://github.com/astral-sh/ruff/pull/14438 (seems like we'd want to do it ASAP to avoid conflicts?), but until then I think this means to say `TCH010`?

```suggestion
type L = 'int' | None  # TCH008 (because TCH010 is not enabled)
```

Similarly, there are several references to `TC004` in comments below that should perhaps be `TCH004`.

---

_Review comment by @carljm on `crates/ruff_linter/src/checkers/ast/mod.rs`:1861 on 2024-11-20 00:31_

Naming nit: I would append `_value` to the names of both these methods to clarify that they handle the RHS or "value", not the entire type alias.

More substantive naming issue: I think that using "generic type alias" as an all-encompassing description of PEP 695 type aliases is inaccurate and confusing. (It is strange that the PEP text uses this as a header over that entire section of the PEP, but it's clear in code examples in that section that only a `type` statement with type parameters is actually generic; one without type parameters is clearly described there as "non-generic".)

Using "explicit type alias" to describe PEP 613 type aliases is not unreasonable, given that's the title of PEP 613, but I still think it's ambiguous (the `type` statement is equally explicit!) and based on an indirect reference that requires looking up the PEP to understand.

If we don't mind referencing PEPs, then I think we should just go all-in on that, and opt for concise and totally unambiguous, with names like `visit_pep613_type_alias_value`, `visit_pep695_type_alias_value`, `PEP613_TYPE_ALIAS`, `PEP695_TYPE_ALIAS`.

If we want to avoid using PEP numbers like this (which is totally unambiguous, but does require knowing or looking up the PEP numbers), then I think we have to be more verbose to be unambiguous: `visit_typealias_annotated_type_alias_value`, `visit_type_statement_type_alias_value`, `TYPEALIAS_ANNOTATED_TYPE_ALIAS`, `TYPE_STATEMENT_TYPE_ALIAS`.

One other note: either way, a code example in the docstring of each method could go a long way towards making it immediately obvious to the reader what form each one is handling.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/model.rs`:673 on 2024-11-20 01:00_

It was very much unclear to me, until I spent quite a while reading through the code, what it meant to "simulate a runtime load" and why or how this is different from what `lookup_symbol` and `resolve_load` do.

It seems the differences are a) this method ignores typing-only bindings (this explains the name choice), and b) this method is supposed to run after all bindings have been collected, but still do a flow-sensitive lookup (i.e. find the applicable binding for a name at a particular point in control flow, which is not necessarily the end of the scope.) Or semi-flow-sensitive, anyway; flow-sensitive as approximated by "source order within the scope, with no understanding of branches."

Difference (a) makes sense, though taken alone it seems like it could be a flag rather than an entirely separate method.

I have a few questions/thoughts about difference (b), some of which may be due to lack of familiarity with Ruff and its semantic model.

It seems like this can be an improvement over Ruff's current model in some cases. But why is this PR the one introducing this capability? What is it about `TCH007` and `TCH008` that require this capability, when Ruff's many other rules involving looking up name bindings have so far gotten away without this?

My understanding is that largely Ruff handles this by doing name lookups _while constructing_ the semantic model, so inapplicable bindings just don't exist yet. Is that approach not workable in this case? Why do we need a method that must instead be run after all bindings are collected, but then ignores the "late" ones, in inner scopes?

If this capability isn't critical to the functioning of the new rules specifically, but is really a more general improvement to Ruff's semantic model, perhaps it should be looked at more holistically in view of all Ruff rules, and not introduced as part of this PR?

If we do definitely need this new method for these new rules to work at all, then I think this also should be more clearly described in a longer doc comment on the method.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/model.rs`:697 on 2024-11-20 01:01_

I took a look and left some comments, though I think familiarity with Ruff's semantic model would make @AlexWaygood a better reviewer here.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/model.rs`:680 on 2024-11-20 01:13_

As I understand it, the idea here is that when we are doing a "lexicographical lookup", we only accept bindings that occur in source order prior to the name node whose load we are resolving, otherwise we take the last available binding in the scope, like `lookup_symbol` above always does.

I think I can see, with the benefit of having now figured out the actual meaning from reading the code, why the name "lexicographical lookup" may have been chosen here, since it is related to "lexical" (i.e. source) ordering of bindings. But this is not a use of the term "lexicographical" that I've ever previously encountered, or can find prior art for in searching (might just be my ignorance; feel free to point me to a reference), and the name did not illuminate the intent (for me, anyway.) I think a name like `source_order_sensitive_lookup` would be clearer.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/model.rs`:709 on 2024-11-20 01:16_

I _think_ that "valid" and "invalid" here mean "precedes the name node in source order" and "does not precede the name node in source order", but again I only figured that out with hindsight; this comment would have been more helpful if more explicit.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/model.rs`:725 on 2024-11-20 01:22_

It would have helped me understand this comment more quickly if it were clarified what "trimmed" means (I'm assuming this is accurate, I haven't looked):
```suggestion
                        // This ensures we perform the correct source-order lookup,
                        // since the ranges for these two types of bindings are trimmed to just the target,
                        // but the name is not available until the end of the entire statement
```

---

_@carljm reviewed on 2024-11-20 01:24_

---

_@Daverball reviewed on 2024-11-20 06:55_

---

_Review comment by @Daverball on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TCH008.py`:39 on 2024-11-20 06:55_

Indeed, I wasn't quite sure how the ordering of events would play out, so I decided against targeting the 0.8 branch with this change for now. I can clean this up either once the 0.8 branch has been merged or before that if we want to try to get these rules into 0.8.0, rather than 0.8.1+.

I don't think it's worth cleaning up before that decision has been made.

---

_@Daverball reviewed on 2024-11-20 07:17_

---

_Review comment by @Daverball on `crates/ruff_python_semantic/src/model.rs`:673 on 2024-11-20 07:17_

> My understanding is that largely Ruff handles this by doing name lookups while constructing the semantic model, so inapplicable bindings just don't exist yet. Is that approach not workable in this case? Why do we need a method that must instead be run after all bindings are collected, but then ignores the "late" ones, in inner scopes?

The main reason is that TCH007/TCH008 perform speculative lookups, rather than the ones that would actually happen at runtime, to ensure that we don't produce violations for the parts of the type alias value that need to available at runtime and vice-versa. (in fact they actually perform both the speculative and regular lookup to avoid violations/fixes going in circles)

The normal flow of the semantic model does not cover the speculative nature of questions like "what if this annotation wasn't a forward reference" or "what if this annotation was turned into a forward reference?".

There are certainly other ways to implement this check (i.e. performing the speculative lookup at the right time during semantic analysis), but this seemed like the least disruptive one (especially since at the time I wrote this there wasn't yet a cache for parsing string annotations) and matches my implementation in the flake8-plugin I wrote, so I can be more confident that it works and produces zero false positives in several large code-bases of mine.

I was also worried that speculatively walking the type definition earlier/later than it was supposed to could lead to other rules triggering or unwanted state leaking into the semantic model. It would be difficult to ensure that this excursion would have no unwanted side-effects.

> Difference (a) makes sense, though taken alone it seems like it could be a flag rather than an entirely separate method.

And in fact in my own implementation in the flake8 plugin they are the same method with a flag. However the potentially incorrect placement of the `seen_function` update prompted me to keep this method separate for now, in order to avoid potentially subtle regressions in `lookup_symbol`.

We could either take the risk and merge these methods now or put it off until later, when we're more confident that the method is stable.

---

_@Daverball reviewed on 2024-11-20 07:44_

---

_Review comment by @Daverball on `crates/ruff_linter/src/checkers/ast/mod.rs`:1861 on 2024-11-20 07:44_

I'll have a little think about what I prefer. Initially I did use the PEP numbers in their names, but it seemed too easy to accidentally read one as the other when you're just skimming the code, so I decided to switch to the names used in the PEP title/section header respectively, even if they're far from ideal.

The descriptive names are a little too tongue-twistery for my liking, but they may be the correct choice after all.

---

_Review requested from @AlexWaygood by @Daverball on 2024-11-20 15:58_

---

_Review requested from @sharkdp by @Daverball on 2024-11-20 15:58_

---

_@Daverball reviewed on 2024-11-20 16:03_

---

_Review comment by @Daverball on `crates/red_knot_workspace/tests/check.rs`:278 on 2024-11-20 16:03_

Not quite sure why that particular rename was missing from the main branch, I'm pretty sure I got them all originally, since grep didn't return anything. Maybe this was accidentally reverted?

---

_Renamed from "[flake8-type-checking] Adds implementation for TCH007 and TCH008" to "[flake8-type-checking] Adds implementation for TC007 and TC008" by @Daverball on 2024-11-20 16:23_

---

_Renamed from "[flake8-type-checking] Adds implementation for TC007 and TC008" to "[`flake8-type-checking`] Adds implementation for TC007 and TC008" by @Daverball on 2024-11-20 16:24_

---

_Review comment by @Daverball on `crates/ruff_linter/src/checkers/ast/mod.rs`:1861 on 2024-11-21 09:55_

I ended up sticking with the original names but added the `_value` portion where appropriate and improved the documentation with examples. If you and/or some of the other maintainers still feel strongly about the ambiguity of the names I'm happy to change them, but the current version reads the best to my eye.

---

_@Daverball reviewed on 2024-11-21 09:55_

---

_@carljm reviewed on 2024-11-21 22:48_

---

_Review comment by @carljm on `crates/ruff_linter/src/checkers/ast/mod.rs`:1861 on 2024-11-21 22:48_

I think calling a PEP 613 type alias an "explicit type alias" is ambiguous and unclear, and I would rather use clearer naming, but I could live with it, if there's sufficient additional documentation to explain our use of the term.

I feel quite strongly that we cannot refer to PEP 695 type aliases, as a whole, as "generic type aliases." It's simply wrong, and very confusing.

`type Foo[T] = list[T]` is a generic type alias. `type Foo = int` is a non-generic type alias. This distinction is made very clear right in the text of PEP 695, which has this code example:

```py
# A non-generic type alias
type IntOrStr = int | str

# A generic type alias
type ListOrSet[T] = list[T] | set[T]
```

Both of these are PEP 695 type aliases, and are currently (wrongly) lumped together in this PR as "generic type aliases."

---

_@Daverball reviewed on 2024-11-22 07:07_

---

_Review comment by @Daverball on `crates/ruff_linter/src/checkers/ast/mod.rs`:1861 on 2024-11-22 07:07_

Fair enough. For the record: I wasn't a fan of the names either. But I can put up with a bit of cognitive dissonance for the sake of readability.

That being said, I've had another think about it and came up with a pair of names that while not self-explanatory, unless you know enough, they should both be disambiguous and readable.

`annotated_type_alias`(`_value`) and `deferred_type_alias`(`_value`). Do you agree that those would be good enough? If not I'd probably go with the pep numbers and just hope people don't get tripped up by the two almost identical looking names.

---

_@carljm reviewed on 2024-11-22 07:25_

---

_Review comment by @carljm on `crates/ruff_linter/src/checkers/ast/mod.rs`:1861 on 2024-11-22 07:25_

I would be happy with those names! They both accurately describe a distinctive characteristic of that style of type alias. 

---

_@Daverball reviewed on 2024-11-23 10:48_

---

_Review comment by @Daverball on `crates/ruff_python_semantic/src/model.rs`:673 on 2024-11-23 10:48_

Actually I am wrong, I will almost always need to wait until all the bindings have been visited, since the outer scope lookups can be non source-order sensitive (e.g. looking up symbols in global scope from within a function). So there isn't really a way to perform the lookup I want to perform at the correct time, I will always need to wait until all the bindings have been visited. F405 also works this way, but can rely on the references caching which binding they point to, so it's easy to find the references that don't point anywhere.

We could do something similar here and keep a second state for every reference within a type definition (instead of just the one we use for `SemanticModel::resolve_name()`), one for the binding that will only be available at runtime, and one for the binding that will only be available at typing time. However, that seemed like overkill to me, especially since that information will not be useful for most rules. (It will however also be needed for the remaining TC1/TC2 rules that can remove quotes from annotations)

---

_@carljm reviewed on 2024-11-26 00:57_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/model.rs`:673 on 2024-11-26 00:57_

These comments make sense to me! I'm satisfied that it's reasonable to add this method. I think it would be ideal if more of this context around what the method does and how it differs from the other two methods available for looking up names, were captured in its doc comment.

Otherwise, this PR looks good to me.

---

_Review comment by @Daverball on `crates/ruff_python_semantic/src/model.rs`:673 on 2024-11-26 08:23_

I updated the docstring, does this explanation make sense?

---

_@Daverball reviewed on 2024-11-26 08:23_

---

_@carljm reviewed on 2024-11-26 15:56_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/model.rs`:673 on 2024-11-26 15:56_

Yes, it looks good to me, thank you.

---

_@charliermarsh reviewed on 2024-11-26 17:44_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:165 on 2024-11-26 17:44_

Can we move this TODO to the top of the closure? (I'm also wondering if this should just be comment on the review?)

---

_@charliermarsh reviewed on 2024-11-26 17:46_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:665 on 2024-11-26 17:46_

Why? Do you plan to follow-up on this? It seems unrelated to the PR.

---

_@charliermarsh reviewed on 2024-11-26 17:47_

The changes to the semantic model seem fine to me.

---

_@charliermarsh reviewed on 2024-11-26 17:52_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:719 on 2024-11-26 17:52_

Do you think you could include some examples in the comments here (like, Python cod examples) to explain what this source-sensitive lookup is doing, and why?

---

_@Daverball reviewed on 2024-11-26 19:00_

---

_Review comment by @Daverball on `crates/ruff_python_semantic/src/model.rs`:665 on 2024-11-26 19:00_

Because for `BindingKind::Annotation` this assignment is skipped. Maybe there's something subtle at play here, like `BindingKind::Annotation` never occurring inside a function scope, but otherwise this seems like a bug, albeit one very unlikely to affect anyone in any real way.

I noted it because `simulate_runtime_load` has a similar structure but moved the update of this variable above to where `class_variable_visible` is updated.

If you don't think there's a bug here I'm happy to remove the comment, otherwise I'll either leave it in and follow up or fix it as part of this PR, whichever you'd prefer.

---

_@Daverball reviewed on 2024-11-27 07:23_

---

_Review comment by @Daverball on `crates/ruff_python_semantic/src/model.rs`:719 on 2024-11-27 07:23_

Actually, this prompted me to improve the function, since it currently takes some shortcuts under the assumption, that it will only be called for type definitions, but if that ever becomes untrue, or type definitions can start showing up in some of the other problematic places, then there are other kinds of bindings than `AnnAssign` and `ClassDef` where the target can occur before a child expression that doesn't yet have access to the binding. The semantic model handles this by walking the nodes in the correct order if I'm not mistaken.

I.e. things like regular assignments and named assignments. Comprehensions have the opposite problem, where the bindings can occur after use, especially with named expressions inside one of the conditions. In the flake8 plugin I'm handling all of these corner-cases by carefully choosing the position of when the binding starts being available.

There is one case I can't handle with pure source position and accept being wrong on, and that would be a named expression referring to itself within a condition of a comprehension. But everything else works. Backtracking the AST to look for bindings seems more robust, but also a lot more expensive.

I think for now I will handle the few additional easy cases and add documentation about when this method will fail to produce the correct result.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:665 on 2024-11-27 08:06_

Let's address this in a follow up PR

---

_@MichaReiser approved on 2024-11-27 08:07_

---

_Comment by @Daverball on 2024-11-27 08:12_

@MichaReiser The tests should still be in their own test function, since both rules are enabled in that test function. That's how we made sure there were no circularity issues.

---

_Comment by @Daverball on 2024-11-27 08:17_

@MichaReiser Also the fix safety comment is only correct for one of the rules, the other rule is always marked as unsafe but I still don't know why, I'm just relying on the fix safety of the other rule that uses the same fix.

Either way I'm going to force push my local branch and include some of your changes, I already made some of the same changes locally, but I also elaborated on the documentation of `simulate_runtime_load`.

---

_Comment by @MichaReiser on 2024-11-27 08:20_

Oh sorry. I think it would be good to add a comment to that test explaining why it is a separate tests or it's likely that we'll merge the tests again when we promote the rule to preview. 

Could you also extend/correct the fix safety comments because that's the only remaining thing that needs resolving before merging this PR

---

_Comment by @Daverball on 2024-11-27 08:41_

@MichaReiser I think this should cover everything, but feel free to give it another cursory glance.

---

_Merged by @MichaReiser on 2024-11-27 08:51_

---

_Closed by @MichaReiser on 2024-11-27 08:51_

---

_Comment by @MichaReiser on 2024-11-27 08:52_

Congrats @Daverball on implementing this rather complex rules!

---

_Branch deleted on 2024-11-28 15:29_

---
