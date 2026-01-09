---
number: 1785
title: Implement flake8-type-checking
type: issue
state: closed
author: nstarman
labels:
  - plugin
assignees: []
created_at: 2023-01-11T18:02:50Z
updated_at: 2023-07-04T09:09:12Z
url: https://github.com/astral-sh/ruff/issues/1785
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement flake8-type-checking

---

_Issue opened by @nstarman on 2023-01-11 18:02_

It's a useful tool for determining which imports aren't run-time required, reducing import time. Thanks!

---

_Label `plugin` added by @charliermarsh on 2023-01-12 00:02_

---

_Comment by @mkniewallner on 2023-01-12 08:01_

Note that the project was renamed to [flake8-type-checking](https://pypi.org/project/flake8-type-checking/) (https://github.com/snok/flake8-type-checking).

---

_Comment by @schrockn on 2023-01-19 14:14_

Came here to file this exact same issue. I had no idea it was supported in flake8. This would be huge for us! cc: cc @smackesey

---

_Renamed from "Implement flake8-typing-only-imports" to "Implement flake8-type-checking" by @charliermarsh on 2023-01-19 16:44_

---

_Referenced in [astral-sh/ruff#283](../../astral-sh/ruff/issues/283.md) on 2023-01-19 16:52_

---

_Comment by @sondrelg on 2023-01-19 19:10_

I'd be very happy to take an initial stab at porting the library. I think most error codes can be auto-fixed, so the ported version might be significantly more useful in this form. 

Are there any docs or resources I should read up on before diving into the ruff source code? 

---

_Comment by @charliermarsh on 2023-01-19 22:27_

Awesome! We have some high-level docs on adding new rules in the [contributing guide](https://github.com/charliermarsh/ruff/blob/main/CONTRIBUTING.md#example-adding-a-new-lint-rule). Those are mostly helpful for guiding you on the mechanics of defining the rule, navigating the various files, etc. We'll also want to add a module in `src/rules/flake8_type_checking` to define the rules and settings (you could look at `src/rules/flake8_bugbear` as an example of the preferred structure).

Beyond the rule logic, I suspect we'll also need to adjust `src/checkers/ast.rs` to track some additional state -- that file is kind of a jumbo AST visitor that calls out to the various rule implementations at the appropriate time, and tracks a bunch of state (e.g., bindings, imports, scopes). For example, I'm guessing we need to track whether every import usage takes place within a type annotation or not? We already have state on the `Checker` for detecting whether we're _in_ an annotation, and we already have used and unused import tracking, so hopefully those pieces can be leveraged in some way.

My suggestion, just to get familiar with the abstractions and conventions, would be to start with maybe the empty type-checking block rule? It seems like that wouldn't require any significant changes to the `Checker` apart from calling out to `src/rules/flake8_type_checking/rules/non_empty_type_checking_block.rs` when we process a `StmtKind::If`. So it'll mostly give you a chance to focus on how it all fits together, and lay the groundwork for the plugin itself.

---

_Comment by @ofek on 2023-01-20 16:23_

Let me know if I can help, I'm competent in Rust but I'm not familiar with the code base. This would be an awesome feature and would reduce a lot of cognitive overhead for me personally because for Hatch I painstakingly use conditional and/or lazy imports to reduce CLI startup time.

---

_Comment by @charliermarsh on 2023-01-20 16:26_

I’m happy to do a first PR today to lay some of the groundwork here.

---

_Comment by @ofek on 2023-01-20 16:34_

That would be neat since I'm going to cut a release in a few days which will generate projects that use Ruff and it would be nice to enable this.

---

_Comment by @sondrelg on 2023-01-20 18:32_

It might be worth discussing whether the rules are worth porting 1:1 and whether any APIs should change.

For example, `TC006` has not been as helpful as I'd hoped. Pycharm's built-in type checker (not sure about other IDEs) struggles to understand that stringified imports still need to be there when type checking, so will remove the imports related to casts if you run the auto-clean imports command. I don't think I would have accepted that rule had I known about this ahead of time.

The `TC` error code also conflicts with another plugin, called [tryceratops](https://github.com/guilatrova/tryceratops/tree/main/docs/violations), so it might be nice to opt for another error code here; although it would mean it's not a complete drop-in replacement.

I was thinking I'd sit down an take a stab at a first rule tomorrow. I agree, the empty type checking block is probably a good candidate. If you've already started though @charliermarsh, feel free to go ahead and I'll try to get involved after the groundwork has been done :+1: 

---

_Comment by @charliermarsh on 2023-01-20 19:44_

A couple things:

- Would definitely like to learn from your experience regarding which rules are reliable or not. Seems like we should omit `TC006` based those comments.
- There's some precedent for changing the rule code prefixes. For example, we use `TID` for `flake8-tidy-imports`, so that it doesn't conflict with the `isort` rules (under `I`). We typically do it if a plugin uses a single-letter prefix which is very likely to cause conflicts, or if a plugin conflicts with something we've already implemented (e.g., we used `COM` for `flake8-commas` since `C` conflicts with `flake8-comprehensions`). In this case, I'm open to either choice. We could use `TC`, and then expect `tryceratops` to use a different code (like `TRY`) if it gets implemented. Or we could use something like `TYP`.

I _may_ get started on this tonight (at least to setup the plugin scaffolding), but either way, I'll post here at the end of the day so that you know whether there's any overlap!


---

_Referenced in [astral-sh/ruff#2048](../../astral-sh/ruff/pulls/2048.md) on 2023-01-21 03:31_

---

_Comment by @charliermarsh on 2023-01-21 03:41_

@sondrelg - I went ahead and added the scaffolding for the plugin in #2048, along with a basic implementation of `TC005`. Let me know if you have any questions on that PR.

I think the biggest thing to figure out is what additional state we need to track in `Checker` to support those other checks. We already track a `Binding` for every import, which tracks usages (and powers our unused-import check). We probably need to extend that in some way to detect whether the usages occurred in a type definition or not?


---

_Comment by @sondrelg on 2023-01-21 09:27_

The way the plugin works today is by exhaustively checking every element _except_ annotations, then matching imports to "[uses](https://github.com/snok/flake8-type-checking/blob/main/flake8_type_checking/checker.py#L396)". IIRC, most uses are either `ast.Value`, `ast.Name`, or `ast.Constant`. Since annotation slot attributes can be, e.g., `ast.Name`, we return early when we're in an annotation node, in the visitor.

It's also important to map the line start/end of all type checking blocks, since use of a guarded import within the scope of that guard, is fine: 

```python
from typing import TYPE_CHECKING as some_alias

if some_alias:
    import pandas as pd

    df = pd.DataFrame()  # this is fine

df = pd.DataFrame()  # this is not
``` 

Then we need a good way of normalizing aliases, if possible. Identifying whether `pd.DataFrame` means the `pandas` import was used or not really just boils down to identifying that the pandas import is aliased, then matching that string. Imports, the `typing` module, and the `TYPE_CHECKING` value can all be aliased.

Finally there's minor things like keeping track of futures imports, function scopes, and classifying imports as local or remote. The exhaustive list of plugin state can be found here: https://github.com/snok/flake8-type-checking/blob/main/flake8_type_checking/checker.py#L372:L434

I'll read over the PR now :slightly_smiling_face: 

---

_Comment by @charliermarsh on 2023-01-21 12:37_

> Then we need a good way of normalizing aliases, if possible. Identifying whether pd.DataFrame means the pandas import was used or not really just boils down to identifying that the pandas import is aliased, then matching that string. Imports, the typing module, and the TYPE_CHECKING value can all be aliased.

Cool -- we have support for and a pattern for this (`checker.resolve_call_path(...)` will detect aliases and normalize to the imported path, like `["pd", "DataFrame"]`).

> Finally there's minor things like keeping track of futures imports, function scopes, and classifying imports as local or remote.

👍 We do track future imports and function scopes as `Checker` state, and we do have logic for classifying imports via stuff in the `isort` directory. So hopefully we can leverage all that :)


---

_Comment by @sondrelg on 2023-01-21 19:51_

Yeah I noticed, that's great!

What do you think about import classification? The plugin uses [classify-imports](https://github.com/asottile/classify-imports) to distinguish between local imports, builtin imports, and library imports. The rationale behind that mainly being that only local application imports lead to import circularity issues, so for some users it might be nice to limit the linting to that class of imports.

I was thinking I might start taking a stab at porting the import classification logic we need as rust code?

---

_Comment by @charliermarsh on 2023-01-21 20:06_

Our import classification logic is actually a port of `classify-imports`, for the most part! (It might deviate in some small ways at this point.) Though we also have support for `isort`-style `known-first-party`, etc. That code is in `src/rules/isort/categorize.rs`, if helpful.

The other question is whether we want to have separate rules for different import classes, as in your existing plugin. I don't feel strongly! If we have the ability to categorize imports, then it makes sense to me to support different rules. But I'm curious if people turns those on and off in practice, what's your sense from issues, etc.?


---

_Comment by @charliermarsh on 2023-01-22 18:55_

@sondrelg - I realize it's probably unhelpful for me to just keep saying "We have something that does X" 😅 so let me take a second to outline how this plugin might be implemented in a bit of detail... If you're still interested, definitely let me know. If not, no worries at all.

---

_Comment by @charliermarsh on 2023-01-22 19:17_

So, for the rules around whether an import should / shouldn't be a in `TYPE_CHECKING` block, we need a few pieces of information:

1. Was the import itself in a type-checking block?
2. Was the import used in a type annotation?
3. Was the import used outside of a type annotation? (If an import isn't used at all, I think the plugin avoids flagging it?)

We probably want to have this information available to use when we run `check_dead_scopes`, which is also where we flag unused imports.

One way to implement this would be as follows:

1. Add a `type_checking_blocks` field to `Checker`. This could be `Vec<&'a Stmt>`, or `Vec<Range>`, and would contain references to the if-blocks or their ranges.
2. In `src/ast/types.rs`, we have a `Binding` struct that represents all bindings, and tracks usages. We could split that into two fields, `used_runtime` and `used_annotation`, or even some kind of `Usages` struct that has `runtime` and `annotation` fields, to track the most recent usage at runtime and as an annotation. Alternatively, we could make that a vector, and store all usages. Lots of options... (Note that we have `checker.in_annotation`, which we can leverage when setting those fields.)
3. In `check_dead_scopes`, iterate over all import bindings (see the `if self.settings.rules.enabled(&Rule::UnusedImport) { ... }` block for an example). For each binding, check its runtime and annotation usages, then check if the binding was created within a type-checking block. (Every `Binding` has a `range` field that tells you where it was defined, so we can check if that range is within any of the type-checking blocks, or whatever the exact logic should be).

(All of this ignores how we should treat redefinitions, i.e., modules that are imported multiple times.)


---

_Referenced in [astral-sh/ruff#2096](../../astral-sh/ruff/issues/2096.md) on 2023-01-23 00:17_

---

_Comment by @sondrelg on 2023-01-23 17:58_

That seems great. 

> If an import isn't used at all, I think the plugin avoids flagging it?

This is true, but has been a bit of a pain to get right. We essentially try to shadow pyflake's `F401` and avoid flagging an error if we suspect `F401` would have been reported already. This way a user, using default flake8 settings would get one error instead of two. The trade-off is that this requires us to track all annotation values, which we wouldn't otherwise have to do iirc.

> Add a `type_checking_blocks` field to `Checker`

I think there might also be a need to track function scopes, but perhaps the functionality that hinges on that information can be added later, or done in another way. Essentially I think we handle this case by tracking function scopes:

```python
if TYPE_CHECKING:
    from pandas import DataFrame

foo: DataFrame  # this is fine

def bar():
    from pandas import DataFrame

    baz = DataFrame()  # this is also fine, but only because of the function-scoped import
```

This is a bit of a contrived example, but there are cases where this type of behavior can happen; typically in large files with circular import problems.

-------------

Did you say there is tooling to classify imports as stdlib/application/third-party/future already as well? I know the futures import is mapped :+1: 

You are probably familiar, but `TC001`, `TC002`, and `TC003` are the same error, just for different classes of imports.


-----------

Lastly, I'd be very happy to write some or most of the code, but to be realistic I think I'm down to being able to contribute 3-4 hours a week. If that's fine, I'm happy to commit to giving that a go, but if you or anyone else wants to jump on this and get it done, then I'd of course be happy to help wherever I can, but let you do most of the work :slightly_smiling_face: 

---

_Comment by @charliermarsh on 2023-01-23 18:50_

> Did you say there is tooling to classify imports as stdlib/application/third-party/future already as well?

Yeah, it's all in `src/rules/isort/categorize.rs`. It's only used for import-sorting right now, but could be extracted out and generalized for this check!

> Lastly, I'd be very happy to write some or most of the code, but to be realistic I think I'm down to being able to contribute 3-4 hours a week.

No prob at all. I may get to it, I may not -- if I do, I'll comment here so that we don't duplicate work.


---

_Comment by @ngnpope on 2023-01-24 12:10_

> For example, TC006 has not been as helpful as I'd hoped. Pycharm's built-in type checker (not sure about other IDEs) struggles to understand that stringified imports still need to be there when type checking, so will remove the imports related to casts if you run the auto-clean imports command. I don't think I would have accepted that rule had I known about this ahead of time.

Yeah. Sorry about that rule @sondrelg. In principle it was a nice idea, but there are certainly problems as you've mentioned that we perhaps didn't foresee.

The main thing it trying to achieve was to reduce the runtime performance hit from building complex types when they're only relevant for static type checking. An alternative approach is to use type aliases in global scope to avoid the hit for building these complex types repetitively in every function call, loop, or whatever. As such `TC006` could be ignored for this port, but I wonder if there is some way we can still gain some benefit by highlighting that a cast type is "overly complex" and may benefit from being shoved in a type alias?

Ah, sweet hindsight...

---

_Comment by @sondrelg on 2023-01-24 12:58_

It's really only the lack of IDE support that makes it hard to work with in my experience. The rationale for the rule itself was great haha :clap: 

---

_Referenced in [astral-sh/ruff#2146](../../astral-sh/ruff/pulls/2146.md) on 2023-01-25 03:48_

---

_Referenced in [astral-sh/ruff#2147](../../astral-sh/ruff/pulls/2147.md) on 2023-01-25 04:34_

---

_Closed by @charliermarsh on 2023-01-25 04:48_

---

_Comment by @charliermarsh on 2023-01-25 04:50_

Okay, I just knocked this out in #2146 and #2147. It'll go out as soon as we can release again (we're blocked by PyPI limits right now and waiting for an exemption -- see #2136).

The current version doesn't include any autofix capabilities (it's challenging because we need to remove imports from one part of the AST and re-add them elsewhere), but I tested on Dagster and the inference seems reliable.

\cc @schrockn @smackesey @ofek 

---

_Comment by @charliermarsh on 2023-01-25 20:42_

My only hesitation here is that I used `TYP` as the prefix, but the codes conflict with [`flake8-typing-imports`](https://github.com/asottile/flake8-typing-imports). I don't intend to implement that plugin, but it is weird to have overlapping rule codes. Any other suggestions here? We _could_ use `TC`, since we used `TRY` for `tryceratops`.

---

_Comment by @charliermarsh on 2023-01-25 20:53_

On second thought: `TC` causes the same issue, since it conflicts with the _actual_ `tryceratops`. So `noqa: TC002` would be interpreted incorrectly in the wild.

---

_Referenced in [astral-sh/ruff#2175](../../astral-sh/ruff/pulls/2175.md) on 2023-01-25 21:22_

---

_Comment by @charliermarsh on 2023-01-25 21:23_

I'm going to rename to `TYC`, which is an uglier prefix, but it avoids all of the possible conflicts. I've added a redirect from `TYP` to `TYC` for now (with a warning), since a few users are already using `TYP`.

---

_Comment by @sondrelg on 2023-01-25 21:39_

I originally used TCH. Not sure if that's taken, but could be appropriate. Ugliest candidate yet though :smile: 

---

_Comment by @charliermarsh on 2023-01-25 21:46_

Oh, cool. If you used that originally, let's go with that over `TYC`. Neither seems to be used.

---

_Referenced in [astral-sh/ruff#2195](../../astral-sh/ruff/issues/2195.md) on 2023-01-26 14:49_

---

_Comment by @smackesey on 2023-03-10 20:40_

Nice to see all the progress on this. We haven't turned it on yet because I think autofix is essential here (to avoid annoying scenario where you are writing type annotations, import something via auto-complete, then have to go fix a lint error and manually move into a `TYPE_CHECKING` block). But if this gets autofix and works smoothly, it will provide huge value for us.

---

_Comment by @eli-schwartz on 2023-04-27 02:03_

> Yeah. Sorry about that rule @sondrelg. In principle it was a nice idea, but there are certainly problems as you've mentioned that we perhaps didn't foresee.
> 
> The main thing it trying to achieve was to reduce the runtime performance hit from building complex types when they're only relevant for static type checking. An alternative approach is to use type aliases in global scope to avoid the hit for building these complex types repetitively in every function call, loop, or whatever. [...]

Please don't apologize, I'm quite fond of that rule and am not worried about pycharm since I don't use it. :P

(I would be interested in using it from ruff, if it was implemented. It seems useful to avoid the runtime hit even once.)

---

_Comment by @charliermarsh on 2023-06-03 04:04_

FYI: autofix for these rules will be enabled in the next release.

---

_Comment by @messense on 2023-07-04 07:16_

It seems that this doesn't work well with [mashumaro](https://github.com/Fatal1ty/mashumaro), I got `mashumaro.exceptions.UnresolvedTypeReferenceError` when running auto-fixed code.

```
<string>:2: in to_dict
    ???
.venv/lib/python3.10/site-packages/mashumaro/core/meta/code/builder.py:884: in add_pack_method
    self._add_pack_method_lines(method_name)
.venv/lib/python3.10/site-packages/mashumaro/core/meta/code/builder.py:674: in _add_pack_method_lines
    field_types = self.field_types
.venv/lib/python3.10/site-packages/mashumaro/core/meta/code/builder.py:155: in field_types
    return self.__get_field_types()
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <mashumaro.core.meta.code.builder.CodeBuilder object at 0x7effba8e02e0>, recursive = True, include_extras = False

    def __get_field_types(
        self, recursive: bool = True, include_extras: bool = False
    ) -> typing.Dict[str, typing.Any]:
        fields = {}
        try:
            field_type_hints = typing_extensions.get_type_hints(
                self.cls, include_extras=include_extras
            )
        except NameError as e:
            name = get_name_error_name(e)
>           raise UnresolvedTypeReferenceError(self.cls, name) from None
E           mashumaro.exceptions.UnresolvedTypeReferenceError: Class CrossValidatedModel has unresolved type reference Self in some of its fields
```

---

_Comment by @Fatal1ty on 2023-07-04 08:54_

> It seems that this doesn't work well with [mashumaro](https://github.com/Fatal1ty/mashumaro), I got `mashumaro.exceptions.UnresolvedTypeReferenceError` when running auto-fixed code.

That’s interesting. Can you share the original and auto-fixed reproducible code? I think it’s worth to create a separate issue for that [here](https://github.com/Fatal1ty/mashumaro/issues).

P.S. From what I’ve read, putting imports to `if TYPE_CHECKING` block will be a problem not only for mashumaro but for other libraries that operate with types at runtime:

> The plugin assumes that the imports you only use for type hinting are not required at runtime.

This assumption can’t be applied to all the existing code, so this dangerous feature in auto-fixing must be disabled by default.

---
