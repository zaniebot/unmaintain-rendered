```yaml
number: 16285
title: Auto generate ast expression nodes
type: pull_request
state: merged
author: Glyphack
labels:
  - internal
assignees: []
merged: true
base: main
head: autogen-ast
created_at: 2025-02-20T19:13:11Z
updated_at: 2025-05-21T20:26:28Z
url: https://github.com/astral-sh/ruff/pull/16285
synced_at: 2026-01-10T18:51:01Z
```

# Auto generate ast expression nodes

---

_Pull request opened by @Glyphack on 2025-02-20 19:13_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of https://github.com/astral-sh/ruff/issues/15655

- Auto generate AST nodes using definitions in `ast.toml`. I added attributes similar to [`Field`](https://github.com/python/cpython/blob/main/Parser/asdl.py#L67) in ASDL to hold field information

## Test Plan

<!-- How was it tested? -->

Nothing outside the `ruff_python_ast` package should change.

---

_Review requested from @dcreager by @MichaReiser on 2025-02-20 19:25_

---

_Comment by @MichaReiser on 2025-02-20 19:26_

Uhh, exciting. 

I do feel a bit conflicted about using `toml` to define our grammar over e.g. something like ungrammar but maybe it's the right choice and doesn't really matter?



---

_Comment by @github-actions[bot] on 2025-02-20 19:34_

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

_Label `internal` added by @AlexWaygood on 2025-02-20 19:34_

---

_Comment by @Glyphack on 2025-02-20 19:35_

@MichaReiser I will also use ungrammar on a separate branch to see the difference I just know the name nothing more. I was also unsure if I should extend the `ast.toml` file or not.

I first started by re-using the ASDL parser by python and then realized the ASDL is not offering more functionality here for generating Nodes and Enums. I decided to continue with `toml` file since we can add custom fields there as well. For example the fields order or other payload can be added to the `toml`.

---

_Comment by @MichaReiser on 2025-02-20 20:45_

RustPython used to use ASDL and, uff, that was painful. It's probably different because we actually parsed the official python grammar and derived the AST from it. The problem with that is that our AST diverged in many places. 

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:112 on 2025-02-20 21:16_

> Is the AST definition easy to understand and maintain?

Given this concern, this syntax does start to look a bit unwieldy.

> Is it easy to parse in our code generator?

But on the other hand, continuing to use TOML still makes it easier to iterate on the code generator itself.

So given all of this, I'd still lean towards using this TOML syntax.  We can always revisit the syntax later if we feel there's something that e.g. ungrammar would give us that the TOML doesn't.

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:647 on 2025-02-20 21:17_

I would suggest checking whether the AST node type has a non-`None` `fields` instead of hard-coding a list of nodes here

---

_Review comment by @dcreager on `crates/ruff_python_ast/src/nodes.rs`:505 on 2025-02-20 21:18_

In other places, we are using

```rust
use crate::{self as ast};
```

and then `ast::ExprDict` instead of `crate::ExprDict`.  That makes it mimic how this crate is used in the rest of the ruff codebase.

---

_@dcreager reviewed on 2025-02-20 21:19_

> I do feel a bit conflicted about using `toml` to define our grammar over e.g. something like ungrammar but maybe it's the right choice and doesn't really matter?

To be honest I don't have a strong opinion between our hand-rolled TOML, ASDL, and ungrammar.  To me, the main concerns are:

- Is the AST definition easy to understand and maintain?
- Is it easy to parse in our code generator?

Our hand-rolled TOML was great for the first proof-of-concept, since we could rely on Python's builtin `tomllib` to do the bulk of the parsing.

I thought ASDL could be nice primarily because we could reuse a lot of the parsing logic from CPython, since (at least at the moment) we are using a Python script to parse the AST definition and generate our (Rust) code.  (At first I hoped it would also have the benefit of letting us reuse the _grammar itself_ from CPython, but as @MichaReiser points out, our AST has diverged from CPython's representation.)

> RustPython used to use ASDL and, uff, that was painful.

What part was painful about it?  Was the syntax itself too limited?  Or was it cumbersome to parse and generate code from?

---

_Comment by @MichaReiser on 2025-02-20 22:02_

> What part was painful about it? Was the syntax itself too limited? Or was it cumbersome to parse and generate code from?

The main pain point was that it parsed python's official AST grammar and then did some hacky overrides in code for where we wanted to diverge. 

My other concern with using ASDL is that it isn't easy for us to extend if we need to and I always found it hard to read (e.g. could we extract field documentation?)

---

_Comment by @Glyphack on 2025-02-21 22:25_

good news 🎉 I was able to complete the generation for all expression nodes. There are a few hacky things I need to fix.

We need a way to provide the derive attribute to structs. Right now I just added a `derives` field that adds additional derives for nodes that have the `Default` derive.
```
ExprNoneLiteral = { fields = [], derives = ["Default"]}
```

I initially thought only knowing a field is a sequence or not would be enough to insert it in a `Vec` but I found in `ExprCompare` one of the types is `Box<[crate::CmpOp]>`.
```
ExprCompare = { fields = [
    { name = "ops", type = "Box<[crate::CmpOp]>"},
    { name = "comparators", type = "Box<[Expr]>"}
]}
```

I added a rule to automatically box if a field type is the group of the node it belongs to so we box every `Expr` but this might be confusing because for `Box<str>` we need to box explicitly. Or for `[Expr]` type the auto boxing won't work. I'm also not interested in making it more complicated but I'm not sure what way would be better.
Also it won't work in this example:
```
{ name = "parameters", type = "Box<crate::Parameters>", optional = true },
```

I'm not sure if we refer to `Name` just by it's name and import it in the file or like this:
```
{ name = "id", type = "name::Name" },
```

The current code also uses `crate::` before every type. This is wrong because if a type is `bool` it should not become `crate::bool` but removing that requires every type to contain the `crate` word which is redundant mostly. We can solve this by having a map of types that should not have `crate::` I don't know what would be the easiest solution.

I'm reading about ungrammar but I feel because of the extensibility we need in the generator using a custom file would be easier because we can add stuff without hacking it on top of something else. But I have to read it first.

---

_Review requested from @dcreager by @MichaReiser on 2025-02-25 08:13_

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:100 on 2025-02-25 14:43_

as a style nit, the following are equivalent TOML (i.e., it shouldn't require changes to the generator) that I think might be a bit easier to read:

```toml
[Expr.nodes.ExprUnaryOp]
fields = [
  { name = "op", type = "UnaryOp" },
  { name = "operand", type = "Expr" }
]
```

or

```toml
[[Expr.nodes.ExprUnaryOp.fields]]
name = "op"
type = "UnaryOp"

[[Expr.nodes.ExprUnaryOp.fields]]
name = "operand"
type = "Expr"
```

I have a slight preference for the last one, since it eliminates the nested maps and lists from the TOML.  What do you think?

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:167 on 2025-02-25 14:45_

and combining the above suggestion with the extra `derives` field, you'd get:

```toml
[Expr.nodes.ExprBooleanLiteral]
derives = ["Default"]

[[Expr.nodes.ExprBooleanLiteral.fields]]
name = "value"
type = "bool"
```

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:581 on 2025-02-25 14:49_

two nits:

Since you use `[]` as the default for `derives` in the `Field` constructor I don't think you need it here.

And you can eliminate the trailing comma for an empty `derives` list as follows:

```suggestion
                "#[derive(Clone, Debug, PartialEq"
                + "".join(f", {derive}" for derive in node.derives)
```

(The second part is less important since we're going to `rustfmt` the code anyway)

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:591 on 2025-02-25 14:52_

I like your suggestion of adding a hard-coded list of types (defined near the top of this file) that don't require a `crate::` qualifier

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:593 on 2025-02-25 14:53_

We will want to do this if the field type is _any_ group, not just the current group.  Most of the `Stmt` nodes, for instance, contain expressions, which are all `Box<Expr>` even though that's a different node group:

https://github.com/astral-sh/ruff/blob/5c007db7e23b44a8824e2848bb15d8448b46ceee/crates/ruff_python_ast/src/nodes.rs#L178-L183

> I added a rule to automatically box if a field type is the group of the node it belongs to so we box every `Expr` but this might be confusing because for `Box<str>` we need to box explicitly. Or for `[Expr]` type the auto boxing won't work. I'm also not interested in making it more complicated but I'm not sure what way would be better.

I think this suggestion also makes this concern less concerning: auto-boxing for singleton node groups is great, and that doesn't mean we have to find a way to auto-box every place that `Box` appears.

---

_Review comment by @dcreager on `crates/ruff_python_ast/src/nodes.rs`:392 on 2025-02-25 14:58_

These comments have value, so they should either move over to the TOML file as TOML comments near the corresponding node definitions, or should get added as another TOML field so that we can still add them to the generated output.

We are not currently publishing this to `crates.io`/`docs.rs`, so I would be fine with just moving them over as TOML comments for now, since the TOML file will become our source-of-truth documentation for these types and their fields.  We can always find a way to add them as rustdocs in the future should we need to.

---

_Review comment by @dcreager on `crates/ruff_python_ast/src/nodes.rs`:650 on 2025-02-25 14:59_

As we get closer to a mergeable version of this PR, don't forget to remove all of these commented-out blocks.  They will exist in the git history if we ever need to resurrect them.

---

_Review comment by @dcreager on `crates/ruff_python_ast/src/relocate.rs`:21 on 2025-02-25 14:59_

:+1: 

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:185 on 2025-02-25 15:07_

> I'm not sure if we refer to `Name` just by it's name and import it in the file or like this:
> 
> ```
> { name = "id", type = "name::Name" },
> ```

I think it's worth ensuring that we can use just `Name` here in the TOML, and importing it at the top of the generated Rust file is a perfectly fine way to do that

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:147 on 2025-02-25 15:09_

> I initially thought only knowing a field is a sequence or not would be enough to insert it in a `Vec` but I found in `ExprCompare` one of the types is `Box<[crate::CmpOp]>`

Doing this as a hard-coded type without a `seq` is just fine as a special case.  We might also check whether they _need_ to be a box-of-slice instead of a vec.  The only benefit I can see is that we save one word of storage since we only need pointer+length instead of pointer+length+capacity.  And if that's worth doing, why aren't we doing that for all of the other vecs?  But we don't have to tackle that in this PR, this is fine as you have it.

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:169 on 2025-02-25 15:13_

> We need a way to provide the derive attribute to structs. Right now I just added a `derives` field that adds additional derives for nodes that have the `Default` derive.

:+1:

---

_@dcreager reviewed on 2025-02-25 15:13_

> good news 🎉 I was able to complete the generation for all expression nodes. There are a few hacky things I need to fix.

This is starting to come together very nicely! Thank you for tackling this.

> I'm reading about ungrammar but I feel because of the extensibility we need in the generator using a custom file would be easier because we can add stuff without hacking it on top of something else. But I have to read it first.

Given some of my style nits about cleaning up the TOML, I think it's fine to proceed with this PR without also looking for a way to migrate to ungrammar.  I think that can be a separate experiment/PR — and in all honesty, a lower priority one, since this seems to be shaping up nicely as it currently is.



---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:185 on 2025-02-25 18:54_

I defined the imports at the top of the `generate.py`.

One additional improvement is to define the imports in a way we can check if a name is imported and not use `crate` prefix for it.

For now I just used the global list of types we don't want to prefix with `crate`(the other comment) instead. Because we only have one import and it might be too soon to generalize.


---

_Review comment by @Glyphack on `crates/ruff_python_ast/src/nodes.rs`:392 on 2025-02-25 19:19_

I moved this comment and the python ast links to the TOML file. I skipped the rust doc. But it might be useful to add them even before publishing the crates because the docs will show up in the editor.

---

_@Glyphack reviewed on 2025-02-25 19:20_

---

_@Glyphack reviewed on 2025-02-25 19:32_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:100 on 2025-02-25 19:32_

Sounds good.

---

_@Glyphack reviewed on 2025-02-25 19:46_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/src/nodes.rs`:392 on 2025-02-25 19:46_

I decided to do this now since we were adding the comments for `Mod` I followed the same approach and this way the comments also show up in the editor.

---

_Review requested from @dcreager by @Glyphack on 2025-02-26 07:15_

---

_Marked ready for review by @Glyphack on 2025-02-26 07:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:88 on 2025-02-26 07:40_

Should we use inline arrays? I find the syntax very verbose to read and find it hard to get a sense of a nodes fields
```suggestion
fields = [
	{ name = "op", type = "BoolOp" },
	{ name = "values", type = "Expr", seq = true },
	...
}
```

The downside of this is that inline tables can't be multiline (this can be a problem with comments)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:96 on 2025-02-26 07:40_

Nit: Just doc?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:266 on 2025-02-26 07:48_

Having to explicitly use `Box<[Expr]>` here has the downside that it still requires manually updating all types when changing from one representation to another. 

I also find that `type = Expr` + `seq = true` reads somewhat strangely. It also allows for combination that aren't allowed. E.g. is `seq = true` + `optional = true` supported? I wonder if we should do something like:

```
type = "Expr" // single item or escape hatch 
type = { kind: "sequence", element-type: "Expr", slice: true } 
type = { kind: "required", value: "Expr" }
type = { kind: "optiona", value: "Expr" }
```

I'm not very convinced it is better. Maybe you have ideas to improve it further?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:287 on 2025-02-26 07:50_

Nit: We could also use a list here and join the elements?
```suggestion
rustdoc = [
	"An AST node that represents either a single-part f-string literal",
	"or an implicitly concatenated f-string literal.",
	"",
	"This type differs from the original Python AST ([JoinedStr]) in that it",
	"doesn't join the implicitly concatenated parts into a single string. Instead,",
	"it keeps them separate and provide various methods to access the parts.",
	"",
	"[JoinedStr]: https://docs.python.org/3/library/ast.html#ast.JoinedStr"
]
```

But I must admit it looks worse with the empty lines... hmm

I would definetely remove the need for `///` They're unnecessary noise when authoring the TOML file and they can easily be inserted by the script.

---

_@MichaReiser reviewed on 2025-02-26 07:52_

I like where this is going. 

I do find the TOML somewhat hard to parse because of how verbose fields is. I'd suggest exploring using regular lists with inline tables to make the format more compact. We can still use the "full-table" layout in cases where it is necessary (because inline-tables need to be single line)

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:96 on 2025-02-26 12:19_

`rustdoc` is consistent with the existing per-group comments.  If we use `doc` here we should change the other one to be consistent.

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:88 on 2025-02-26 12:22_

In fairness to @Glyphack I had suggested the syntax he's using here https://github.com/astral-sh/ruff/pull/16285#discussion_r1969927355!

Another possibility, since `name` is required and unique, would be

```toml
[Expr.nodes.ExprBoolOp]
fields = {
  op = { type = "BoolOp" },
  values = { type = "Expr", seq = true }
}
```

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:287 on 2025-02-26 12:24_

That's a good point that the script can insert the `///`s.  I think removing those, but keeping the multi-line string, is likely to be a good compromise:

```suggestion
rustdoc = """
An AST node that represents either a single-part f-string literal
or an implicitly concatenated f-string literal.

This type differs from the original Python AST ([JoinedStr]) in that it
doesn't join the implicitly concatenated parts into a single string. Instead,
it keeps them separate and provide various methods to access the parts.

[JoinedStr]: https://docs.python.org/3/library/ast.html#ast.JoinedStr"""
```

---

_@dcreager reviewed on 2025-02-26 12:25_

---

_@MichaReiser reviewed on 2025-02-26 12:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:88 on 2025-02-26 12:34_

Oh, I didn't see that one.

The second doesn't work, I think, because toml inline tables don't allow line breaks

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:287 on 2025-02-26 20:15_

Good point I'm going with the second version with multi line strings.

---

_@Glyphack reviewed on 2025-02-26 20:15_

---

_@Glyphack reviewed on 2025-02-26 20:21_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:96 on 2025-02-26 20:21_

I also prefer `doc` it's shorter I'll change all to `doc`.

---

_@Glyphack reviewed on 2025-02-26 21:05_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:88 on 2025-02-26 21:05_

yes unfortunately it's not possible to have line break inside an inline table.
https://toml.io/en/v1.0.0#inline-table

To make that work we need to have a table `[Expr.nodes.ExprBoolOp.fields.op]` which takes it more verbose I think?

But I think instead of the inline table using an inline array would solve the problem of having all the fields in one section and we only need to write an extra "name = ..." does sound good to me.

But one thing to keep in mind is that if we use inline table for the field attributes and have tables inside attributes(for example if we apply this comment https://github.com/astral-sh/ruff/pull/16285#discussion_r1971093809) it might get very messy example:

```
[Expr.nodes.ExprBoolOp]
fields = [
  { name = "values", type = { kind = "sequence", element-type = "Expr" } }
]
```

I feel we end up making type a more complex field in the end.

---

_@Glyphack reviewed on 2025-02-26 21:28_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:266 on 2025-02-26 21:28_

Yeah the current format has no restrictions and definitely has some problems like You can use type `Vec<Expr>` and also set the sequence. Honestly I was not sure how specific should I make the types in this stage.
I think your suggestion is needed in the end. For example when we want to provide functions to access data without accessing the fields the exact types are useful.

My feeling is that if you are also unsure we can leave this undecided to when it's needed?

Update: I will implement this along with the comment https://github.com/astral-sh/ruff/pull/16285#discussion_r1973039467

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:17 on 2025-02-27 02:47_

> One additional improvement is to define the imports in a way we can check if a name is imported and not use `crate` prefix for it.
> 
> For now I just used the global list of types we don't want to prefix with `crate`(the other comment) instead. Because we only have one import and it might be too soon to generalize.

I agree that it's too soon to generalize.  Given that, I don't think it's worth defining this as a Python array — just put the necessary `use` statements directly into the Rust preamble below.

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:25 on 2025-02-27 02:52_

Come to think of it, once we have migrated _all_ of the node types over to this mechanism, none of them will need `crate::` qualifiers, since everything will be either (a) defined in the generated file, (b) in an `use` statement at the top of the generated file, or (c) a primitive of some kind.  So it might make sense to invert this into a list of types that _do_ need a crate prefix.  That would also eliminate the `Box` special case.

---

_@dcreager reviewed on 2025-02-27 02:54_

---

_@MichaReiser reviewed on 2025-02-27 07:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:88 on 2025-02-27 07:48_

That's a fair concern. Maybe we need ungrammar after all (just joking). 

The alternative is to change `type` to accept some form of DSL (which is what ungrammar would do as well)

* `Expr` (single expression)
* `Expr?` (optional expression)
* `Expr*` or `[Expr]` (Vec)
* `&Expr*` or `&[Expr]` (Slice) maybe there's a better way of spelling this


---

_@Glyphack reviewed on 2025-02-27 21:35_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/generate.py`:25 on 2025-02-27 21:35_

Good idea, also while doing that I realized a lot of the `create::` that was added before was unnecessary. Also the list has a few elements we can get rid of soon after moving other things to generated.

---

_@Glyphack reviewed on 2025-02-27 21:43_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:88 on 2025-02-27 21:43_

Let me try this out 👍. I'll use `Expr*` and `&Expr*` syntax for now. 

Also if we decide to make this a DSL we can just drop the name since it can be easily added by having something like: `value:Expr` and eliminate the table in toml:

```
[Expr.nodes.ExprBoolOp]
fields = [
  "op:BoolOp",
  "values:Expr*"
]
```

---

_@MichaReiser reviewed on 2025-02-28 07:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:88 on 2025-02-28 07:57_

I don't mind the inline table. I probably prefer it because it's less *magic*. So I think I prefer 

```toml
fields = [
  { name = "op", type="BoolOp" },
  { name = "values", type = "Expr*" }
]

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:88 on 2025-03-02 11:30_

A question, do you think we would want to nest these rules? for example creating a vector with optional values. The rule for it would be `Expr?*`. It doesn't look good so I intentionally left this out of the current implementation. We can introduce parenthesis if we want to do it.

I think the correct way to do this also would be to not only have `FieldType` class but have a class for each data type it represents for example `FieldTypeSeq` for fields that are sequence:

```python
class FieldTypeSeq:
  element_ty: FieldType
  is_slice: bool


class FieldTypeOptional:
  some_ty: FieldType
```

I did not want to make code more complex without a reason right now, it's probably needed when porting more code to generate script.

Let me know what do you think about the implementation.

https://github.com/astral-sh/ruff/pull/16285/files#diff-c52a3d2dd4a45e516b700b71c4d38350f80c2796c8eb236f05e7a2dddf3689f1R145

---

_@Glyphack reviewed on 2025-03-02 11:30_

---

_@MichaReiser reviewed on 2025-03-03 09:54_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:88 on 2025-03-03 09:54_

Do we have use cases today where we have optional sequence elements (I hope not). I think it's very unlikely that we'll need this in the future because we want a node for each element in a sequence to at least preserve the node's range (each element in a sequence should implement the `AstNode` trait and `None` can't do that)

---

_@MichaReiser approved on 2025-03-03 09:54_

---

_Review requested from @dcreager by @MichaReiser on 2025-03-03 09:55_

---

_@Glyphack reviewed on 2025-03-03 17:56_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:88 on 2025-03-03 17:56_

Just checked and there's no use of this (I searched for Vec<Option in the codebase)

---

_Comment by @MichaReiser on 2025-03-04 09:38_

@dcreager this looks good to me. I'll let you have the final review as the issue author

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:1 on 2025-03-04 20:49_

Update the comment at the top of this file:

- `rustdoc` → `doc` in the "group options" section
- Describe `doc`, `derives`, and `fields` in the "syntax node options" section
- In particular, make sure to describe the mini-language for `fields.type`

---

_@dcreager reviewed on 2025-03-04 20:49_

This is great! My only comment is that we need to update the documentation to describe the changes.

---

_@Glyphack reviewed on 2025-03-04 21:14_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:1 on 2025-03-04 21:14_

Thanks for the reminder. I delayed this to document when changes are final but completely forgot.

---

_Review requested from @dcreager by @Glyphack on 2025-03-04 21:15_

---

_@dcreager approved on 2025-03-05 13:24_

Love it.  Thanks for tackling this!

---

_Merged by @dcreager on 2025-03-05 13:25_

---

_Closed by @dcreager on 2025-03-05 13:25_

---

_Branch deleted on 2025-05-21 20:26_

---
