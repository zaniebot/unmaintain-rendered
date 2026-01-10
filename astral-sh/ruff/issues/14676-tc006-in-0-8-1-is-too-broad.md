---
number: 14676
title: "`TC006` in `0.8.1` is too broad"
type: issue
state: closed
author: dangotbanned
labels:
  - rule
assignees: []
created_at: 2024-11-29T11:45:21Z
updated_at: 2024-12-04T13:16:33Z
url: https://github.com/astral-sh/ruff/issues/14676
synced_at: 2026-01-10T01:22:55Z
---

# `TC006` in `0.8.1` is too broad

---

_Issue opened by @dangotbanned on 2024-11-29 11:45_

I was surprised by some of these *"fixes"* adding quotes

The reasoning in [why-is-this-bad](https://docs.astral.sh/ruff/rules/runtime-cast-value/#why-is-this-bad) makes me think the concern is regarding parameterizing a generic, or maybe time spent by `ruff`/ a type checker to see if the symbol is available at runtime?

<details><summary>diff (<a href="https://github.com/vega/altair"><code>altair</code></a>)</summary>
<p>

```diff
diff --git a/altair/utils/core.py b/altair/utils/core.py
index 1cac486b6..66a559fad 100644
--- a/altair/utils/core.py
+++ b/altair/utils/core.py
@@ -383,7 +383,7 @@ def sanitize_pandas_dataframe(df: pd.DataFrame) -> pd.DataFrame:  # noqa: C901
         # We know that the column names are strings from the isinstance check
         # further above but mypy thinks it is of type Hashable and therefore does not
         # let us assign it to the col_name variable which is already of type str.
-        col_name = cast(str, dtype_item[0])
+        col_name = cast("str", dtype_item[0])
         dtype = dtype_item[1]
         dtype_name = str(dtype)
         if dtype_name == "category":
diff --git a/altair/utils/mimebundle.py b/altair/utils/mimebundle.py
index 575b3f71c..56cd05a7e 100644
--- a/altair/utils/mimebundle.py
+++ b/altair/utils/mimebundle.py
@@ -140,7 +140,7 @@ def spec_to_mimebundle(
     if format in {"png", "svg", "pdf", "vega"}:
         return _spec_to_mimebundle_with_engine(
             spec,
-            cast(Literal["png", "svg", "pdf", "vega"], format),
+            cast("Literal['png', 'svg', 'pdf', 'vega']", format),
             internal_mode,
             engine=engine,
             format_locale=embed_options.get("formatLocale", None),
diff --git a/altair/utils/plugin_registry.py b/altair/utils/plugin_registry.py
index 81e34cb8d..91e9462eb 100644
--- a/altair/utils/plugin_registry.py
+++ b/altair/utils/plugin_registry.py
@@ -134,7 +134,7 @@ class PluginRegistry(Generic[PluginT, R]):
                 f"https://docs.astral.sh/ruff/rules/assert/"
             )
             deprecated_warn(msg, version="5.4.0")
-            self.plugin_type = cast(IsPlugin, _is_type(plugin_type))
+            self.plugin_type = cast("IsPlugin", _is_type(plugin_type))
         else:
             self.plugin_type = plugin_type
         self._active: Plugin[R] | None = None
@@ -214,7 +214,7 @@ class PluginRegistry(Generic[PluginT, R]):
                     raise ValueError(self.entrypoint_err_messages[name]) from err
                 else:
                     raise NoSuchEntryPoint(self.entry_point_group, name) from err
-            value = cast(PluginT, ep.load())
+            value = cast("PluginT", ep.load())
             self.register(name, value)
         self._active_name = name
         self._active = self._plugins[name]
diff --git a/altair/vegalite/v5/api.py b/altair/vegalite/v5/api.py
index 909ed621e..5b3d93f86 100644
--- a/altair/vegalite/v5/api.py
+++ b/altair/vegalite/v5/api.py
@@ -798,7 +798,7 @@ def _parse_when_compose(
     if constraints:
         iters.append(_parse_when_constraints(constraints))
     r = functools.reduce(operator.and_, itertools.chain.from_iterable(iters))
-    return t.cast(_expr_core.BinaryExpression, r)
+    return t.cast("_expr_core.BinaryExpression", r)
 
 
 def _parse_when(
@@ -1107,7 +1107,7 @@ class Then(ConditionLike, t.Generic[_C]):
         conditions = self.to_dict()
         current = conditions["condition"]
         if isinstance(current, list):
-            conditions = t.cast(_Conditional[_Conditions], conditions)
+            conditions = t.cast("_Conditional[_Conditions]", conditions)
             return ChainedWhen(condition, conditions)
         elif isinstance(current, dict):
             cond = _reveal_parsed_shorthand(current)
@@ -1384,7 +1384,7 @@ def param(
             parameter.empty = empty
         elif empty in empty_remap:
             utils.deprecated_warn(warn_msg, version="5.0.0")
-            parameter.empty = empty_remap[t.cast(str, empty)]
+            parameter.empty = empty_remap[t.cast("str", empty)]
         else:
             raise ValueError(warn_msg)
 
@@ -3086,7 +3086,7 @@ class TopLevelMixin(mixins.ConfigMethodMixin):
             verbose_composition = chart.transform_filter((datum.year == 2000) & (datum.sex == 1))
             chart.transform_filter(year=2000, sex=1)
         """
-        if depr_filter := t.cast(Any, constraints.pop("filter", None)):
+        if depr_filter := t.cast("Any", constraints.pop("filter", None)):
             utils.deprecated_warn(
                 "Passing `filter` as a keyword is ambiguous.\n\n"
                 "Use a positional argument for `<5.5.0` behavior.\n"
@@ -3986,7 +3986,7 @@ class Chart(
                 pass
 
         # As a last resort, try using the Root vegalite object
-        return t.cast(_TSchemaBase, core.Root.from_dict(dct, validate))
+        return t.cast("_TSchemaBase", core.Root.from_dict(dct, validate))
 
     def to_dict(
         self,

```


</p>
</details> 

I'm unsure the [new tests](
https://github.com/astral-sh/ruff/pull/14634/files#diff-3b0e267bd6580825e19a1c7ee865d577f77ca9acdaf94e7056a2540c4601e63d) sufficiently cover some of these cases:
- symbols declared within the same file, that are not forward references
- `builtins` with no free type variables like `str`
- There are less of these, but the same for `typing` special types like `Any`


### Alternative
Update the docs for [`TC006`](https://docs.astral.sh/ruff/rules/runtime-cast-value/) to give more detail on how cases like these would benefit from the fix.
I'm a big fan of `ruff` and feel like I've learned a lot from reading the [rules](https://docs.astral.sh/ruff/rules/); so I'm open to this simply being something that hasn't *clicked* for me yet

---

_Comment by @AlexWaygood on 2024-11-29 12:24_

@Daverball, I'm curious for your thoughts here. The rule's docs say:

> `typing.cast()` does not do anything at runtime, so the time spent on evaluating the type expression is wasted.

That implies that the concern is that a lot of time might be spent evaluating subscript expressions for generics in things like

```py
def f(y):
    x = cast(Foo[str], y)
```

It's true that evaluating the subscript expression will take the same amount of time for a first-party generic defined in the same module as it will for a third-party generic imported from another file. However, I feel like the specialising interpreter on Python 3.11+ is probably very good at making sure that repeated subscripts and attribute expressions inside `cast()` calls are optimised, and I expect this to only get better once the new JIT is stabilised. So I'm quite sympathetic to this issue. I feel like unless it actually results in you being able to remove imports from a module, this rule is currently quite noisy -- I feel like it's asking you to make your code somewhat uglier for not much benefit. What do you think?

> There are less of these, but the same for `typing` special types like `Any`

One common reason that people quote special types from the typing module is that it means that you can sometimes get rid of all imports from typing altogether from a module, which can speed up that module's import time. That doesn't apply here, though, since we know that the user is using `typing.cast()`, which means `typing` must already have been imported, and we won't be able to remove the import just by quoting some type expressions in a few places.

---

_Renamed from "`TC006` fix in `0.8.1` is too broad" to "`TC006` in `0.8.1` is too broad" by @AlexWaygood on 2024-11-29 12:25_

---

_Label `rule` added by @AlexWaygood on 2024-11-29 12:27_

---

_Comment by @dangotbanned on 2024-11-29 12:29_

Thanks for the quick response @AlexWaygood

I only had one question regarding:

> One common reason that people quote special types from the typing module is that it means that you can sometimes get rid of all imports from typing altogether from a module, which can speed up that module's import time.

@AlexWaygood would this still apply when you'd need to import `typing` for `typing.TYPE_CHECKING` anyway?

---

_Comment by @AlexWaygood on 2024-11-29 12:31_

> > One common reason that people quote special types from the typing module is that it means that you can sometimes get rid of all imports from typing altogether from a module, which can speed up that module's import time.
> 
> [@AlexWaygood](https://github.com/AlexWaygood) would this still apply when you'd need to import `typing` for `typing.TYPE_CHECKING` anyway?

There's a little hack you can do for mypy where you can replace `if typing.TYPE_CHECKING` with `if False`, if you really want to get rid of all typing imports entirely ;) I don't know if it works for other type checkers too; I don't think it's standardised.

---

_Comment by @AlexWaygood on 2024-11-29 12:34_

But yeah, if you have one import from `typing` then you may as well have them all. Importing one thing from `typing` shouldn't be noticeably faster than importing 20 things from `typing`. The slow thing about imports in Python is acquiring the lock on `sys.modules`, parsing the Python code in the other module, creating the module object to store in `sys.modules`, etc. Once the module has been fully loaded, it's just a matter of attribute access on that module and storing the results of those attribute accesses in the global dictionary of the module you're importing things into -- that should be fairly cheap.

---

_Comment by @Daverball on 2024-11-29 12:51_

No matter what you do, a quoted expression will always be fastest since, even if it's just one symbol you're getting rid of, the name lookup is more expensive than just putting a static value on the stack, while the optimizing compiler may alleviate this to a degree, where in the best case scenario this will eventually be just as fast once the code has been running for a while, you're still making the compiler work for that gain.

So my take is that there's no good reason to keep the argument unquoted. There are certainly cases where the positive impact of quoting is low enough, that you could say it doesn't really matter, but I'd rather be consistent and always quote the expression than end up with a wishy washy mix, where you have to explain to people why sometimes there are quotes and sometimes there are not.

So my take is, if you don't like the aesthetics and don't mind the overhead, then disable the rule. I would be open to adding additional settings to make this rule more flexible, so you can keep the expression unquoted in more cases, but I would personally turn all of those off in my code. In my mind this, next to TC005, is the least controversial rule in `flake8-type-checking`, because there is no potential for adverse side-effects, other than aesthetics.

---

_Comment by @Daverball on 2024-11-29 13:00_

But if someone wants to provide a better explanation of what this rule does and why, please feel free to open a pull request, or even just suggest better phrasing in here and I will do it for you. I admittedly did not spend a lot of time on writing the docs for this rule, because of how straightforward it is (i.e. there are no exceptions, all `cast` calls are treated the same).

I have been using it for quite a while through the flake8 plugin and it is very obvious to me, why it makes sense. But I can see how it could be less obvious to someone else. Especially if the initial gut reaction is, that it makes the code less pretty.

---

_Comment by @AlexWaygood on 2024-11-29 13:09_

> No matter what you do, a quoted expression will always be fastest since, even if it's just one symbol you're getting rid of, the name lookup is more expensive than just putting a static value on the stack, while the optimizing compiler may alleviate this to a degree, where in the best case scenario this will eventually be just as fast once the code has been running for a while, you're still making the compiler work for that gain.

Right, I'm not disputing that this rule gets rid of _some_ overhead at runtime. I'm just disputing whether the (probably quite marginal) performance gain is worth it in all these cases considering a somewhat significant cost to readability (in my opinion). As PEP 20 reminds us, _readability counts_ :-)

> would be open to adding additional settings to make this rule more flexible, so you can keep the expression unquoted in more cases

I could live with making the rule configurable, but adding too many configuration options to Ruff also comes with a cost. We already have so many configuration options that it's somewhat bewildering to users.

I'm curious for other people's opinions here -- maybe @carljm?

---

_Comment by @Daverball on 2024-11-29 13:57_


> Right, I'm not disputing that this rule gets rid of _some_ overhead at runtime. I'm just disputing whether the (probably quite marginal) performance gain is worth it in all these cases considering a somewhat significant cost to readability (in my opinion). As PEP 20 reminds us, _readability counts_ :-)

That's fair, although that only applies to syntax highlighted code. And syntax highlighters could easily detect with some simple heuristics most of the forward references and provide inline syntax highlighting for them, at which point the readability concerns would vanish.

FWIW I'm not really happy that Python chose to use strings for forward references (or reuse subscripts for generics) and I don't like how they look either. So that choice has come back to haunt us in myriad of different ways, where a dedicated syntax for type expressions and forward references could have allowed us a lot more flexibility, so we wouldn't have to choose between aesthetics and unwarranted runtime overhead. But I also recognize that it would have been difficult to get the optional static type system off the ground at all, if it involved introducing additional new syntax, so it is what it is.

Everyone will have to make their own trade-offs based on their use-cases, `flake8-type-checking` is definitely one of the more opinionated plugins, so it doesn't come as a surprise to me, that there are some rules or decisions a significant number of people disagree with, but it's also worth recognizing, that you may also find a significant number of people in the opposite camp.

But I can at least tell you that while, when this rule was first introduced in `flake8-type-checking`, there was one issue opened, that complained about it (mostly due to the churn it caused, since there are no automatic fixes in `flake8`), there has been no mention of it in the two years since then. So it appears that people are generally happy to either live with the rule in its current state or disable it, we haven't had any requests to change it, or make it more configurable.

---

_Comment by @LefterisJP on 2024-12-01 09:00_

I just hit this issue by upgrading 0.8.1. Took me a bit of time to manually fix the few occurrences where things could not be automatically fixed but nothing too long.

I don't think readability suffered, and even a tiny bit of performance increase by removing something from runtime sounds good to me.

But yes at first I thought something is off, until I read the rule's description and the discussion here. So my first reaction was "this may be a bug?".

---

_Comment by @eli-schwartz on 2024-12-01 16:43_

Count me as one of the very happy historic users of the flake8 plugin option for enforcing that all cast calls are quoted.

I'm not sure I understand the appeal of avoiding this from a syntax highlighting perspective. I don't consider type-based syntax highlighting as a function call parameter to be beneficial to readability. Strings are more visually distinctive, and in a way that lets me mentally skip over them when checking a line of code to see what the effective code is doing. I'd probably enable this rule even if there was no runtime overhead or imports to be concerned about, just for the readability improvements.

---

_Comment by @dangotbanned on 2024-12-01 17:25_

@Daverball appreciate your detailed responses. 
I get the impression that there's probably a performance improvement, but maybe only a marginal one.

I'm fine with the readability post-fix, I use `pylance` which preserves syntax highlighting in forward refs.
Mostly, I'm reaching for `typing.cast` in some kind of compatibility code and whatever that *looks* like is pretty low on my priorities anyway.

---

I'm happy with `TC006` as-is, but an [updated doc](https://github.com/astral-sh/ruff/issues/14676#issuecomment-2507775460) would also be welcome

Thanks again @Daverball , @AlexWaygood  

---

_Comment by @AlexWaygood on 2024-12-02 14:27_

It's clear to me from this discussion that there are lots of people who are used to the behaviour of the flake8 plugin and find it both useful and desirable. That's high enough signal for me that I think we should at least allow users to opt into having Ruff's current behaviour, where it will tell you to stringify the first argument of _all_ calls using `cast()`.

I'm still not convinced that this should be the default behaviour without any configuration, however. I feel like we already have too many rules that propose changes which, while they might be desirable in the abstract, offer only very minor improvements over the user's existing code. Having lots of rules like this means that running Ruff becomes more of a burden than a help for end users; you have to review a large quantity of changes with each Ruff minor upgrade, and for little gain. Autofixes help here, but they don't solve the problem, because the change an autofix makes still needs to be manually reviewed.

My feeling is that stringifications that wouldn't result in imports being put behind `if TYPE_CHECKING` blocks don't meet the bar to be proposed by default by this rule, so my proposed course of action here is to change the rule as follows:
1. Change the default behaviour so that it only stringifies the first argument to `cast()` if one of the names being referenced in that argument is a non-builtin originating from another module
2. Add a configuration option that allows users to opt into the more sweeping version of the rule (the one that currently exists), whereby the first argument to `cast()` is _always_ stringified

---

_Comment by @carljm on 2024-12-02 14:43_

My opinion here is that the pros and cons are so small that it's really not worth adding either configurability or any more complexity to the rule. I think that will just lead to more confusion, and I just don't think it's worth the implementation or maintenance cost. I would favor keeping the rule simple and straightforward, as it is. If you like it, use it, if you don't, don't. I don't see evidence in this thread of user demand for a more complex version of the rule. (I do see a case for adding a line to the docs to clarify that the rule prioritizes consistency, even though some type expressions have very little or no cost.)

I certainly wouldn't include this rule in the default rule-set, though. People should only see this rule if they opt into it, or if they choose to opt in to ALL.

---

_Comment by @AlexWaygood on 2024-12-02 14:54_

> I certainly wouldn't include this rule in the default rule-set, though. People should only see this rule if they opt into it, or if they choose to opt in to ALL.

I've been trying to have a bit more empathy for people opting into ALL after seeing https://github.com/astropy/astropy/issues/17430#issuecomment-2493924075 😄

_Is_ it sensible to opt into ALL if you mainly want rules that you can have high confidence in, and you're not a fan of having large amounts of churn in your codebase with each Ruff minor release? No, probably not, at least with our current categorisation. People with those concerns still opt into ALL, however, in part due to the other limitations of our current categorisation.

---

_Comment by @eli-schwartz on 2024-12-02 15:21_

In fact I don't consider it sensible to both:
- opt into "I trust the upstream tool maintainers to choose rules that they believe are broadly beneficial, so I don't have to agonize over that choice myself"
- have a feeling of discontent that the upstream tool maintainers develop new and interesting forms of "broadly beneficial" static analysis which fix your codebase in heretofore undiscovered ways

For example, let's say you have an existing rule that does one particular thing, and then a version update does the same thing but *detects it in more places*. Do you rejoice that you caught more code defects? Do you complain that the tool keeps changing? How can this problem be solved save by just sticking with a version and never upgrading?

If one does feel that the tool keeps changing in ways that decrease satisfaction, I can't help but feel the root cause of the dissatisfaction is a matter of disagreement about what constitutes "broadly beneficial", rather than a more generalized dislike of "change". The solution is probably to implement #12111 and let people freeze their config at a version of ruff which they like, and then manually curate rules after that.

On the other hand I think it would be reasonable to express discontent at the upstream tool changing how it works, such that one version of the tool makes one modification to your own codebase, and a new version of the tool changes course and modifies your code in a different way. That crosses the line from "the tool became better at doing what I asked it to do" to "the tool is inconsistent about what it wants".

(This is actually one of several reasons why I strongly oppose autoformatters.)

---

_Comment by @Daverball on 2024-12-02 15:33_

It's also worth mentioning that there's already some overlap with `TC004` when you change to `quote-annotations = true` and once `TC1XX` and `TC2XX` are in you can basically get @AlexWaygood's proposed behavior simply by opting out of `TC006`. All of the other cases would be handled by the other rules.  So changing `TC006`'s default behavior seems extremely redundant in that case.

So it seems more productive to give people that currently opt into `ALL` better options to get many of the same benefits of a blacklist approach, without causing too much disruption with each upgrade for a specific category/confidence level of rule you generally tend to opt out of.

I think `RUF` is currently causing a disproportionate amount of contention for users of `ALL` because in contrast to other plugins which usually broadly stick to one category of error, `RUF` contains a varied mix of rules, so you can't just add `RUF0` to your ignores. Most of the other plugins don't have that problem, if you inherently disagree with a plugin, you just opt out of that plugin entirely. Recategorization should help with that.

---

_Referenced in [vega/altair#3706](../../vega/altair/pulls/3706.md) on 2024-12-02 15:50_

---

_Referenced in [astral-sh/ruff#14749](../../astral-sh/ruff/pulls/14749.md) on 2024-12-03 07:42_

---

_Comment by @MichaReiser on 2024-12-03 08:21_

> If you like it, use it, if you don't, don't. I don't see evidence in this thread of user demand for a more complex version of the rule. (I do see a case for adding a line to the docs to clarify that the rule prioritizes consistency, even though some type expressions have very little or no cost.)

The challenge with this approach is that it becomes very unlikely that `TC006` can ever make it into a recommended rule set (even for typing only) because the way it works today is too opinionated. I'd even argue that it can't make it into a strict-recommended rule set. A less strict rule, however, could be made into a default rule set, making it more useful for many users. 

Leaving the rule as is seems fine to me if its main and only benefit is enforcing a specific coding style, but that's not what I understand is the main reason for the rule's existence. 

TLDR: The problem I see with how the rule is defined today is that it mixes stylistic and performance concerns, making it difficult to recategorize the rule in the future.

---

_Comment by @Daverball on 2024-12-03 08:57_

@MichaReiser As I've previously pointed out, other use-cases can partially already be served by TC004 and should eventually be serviceable through the rest of `flake8-type-checking`.

I don't think this entire plugin can ever make it into a recommended or strict ruleset (apart from a few uncontroversial exceptions like TC004, TC005 and TC010), since it requires configuration in order to play nicely with libraries that make use of a subset of the type annotations at runtime like pydantic or SQLAlchemy.

I'm fine with changing the docs to focus on style/consistency rather than performance. But I think you'll find categorizing `flake8-type-checking` rules to be difficult in general, since there are complex interactions between the various rules depending on the configuration that makes it difficult to say rule A's purpose is this and exactly this.

It's unfortunately the nature of the beast where varying concerns push the preferred style in vastly different directions and that style choice is usually not made purely for aesthetics or readability.

---

_Comment by @Daverball on 2024-12-03 09:51_

@MichaReiser What might make more sense to me is to add a new rule that focuses purely on the performance aspect of the creation of unnecessary `typing._GenericAlias` instances, i.e. it tries to quote all runtime type expressions that would create a `typing._GenericAlias` unless the runtime context is marked runtime required.

So this would apply to more than just `typing.cast` and provide the same benefits to all runtime annotations. Then we could mark TC006 as purely a style rule and reference this new rule as an alternative when only performance matters and consistency is not important.

---

_Comment by @AlexWaygood on 2024-12-03 10:30_

> But I think you'll find categorizing `flake8-type-checking` rules to be difficult in general, since there are complex interactions between the various rules depending on the configuration that makes it difficult to say rule A's purpose is this and exactly this.

That works well for a flake8 plugin where, as the plugin author, you intend all the rules to be either selected together or not selected. There's a similar interaction between the different rules in a plugin I maintain, flake8-pyi. It's a bad fit for Ruff, however, where we need to consider the merits of each rule in isolation, and where we cannot (and should not) force users to either select all rules from a certain category or none of them. As such, several PYI rules work somewhat differently in Ruff than they do in the original flake8 plugin, in order to make them less interdependent and work better in isolation. Sometimes this is frustrating from an implementation perspective (https://github.com/astral-sh/ruff/pull/14583 would have been unnecessary if we could force users to always enable PYI016 if they enabled PYI061), but I believe that overall it results in a better experience for Ruff users.

---

_Comment by @AlexWaygood on 2024-12-03 10:46_

> It's also worth mentioning that there's already some overlap with `TC004` when you change to `quote-annotations = true` and once `TC1XX` and `TC2XX` are in you can basically get [@AlexWaygood](https://github.com/AlexWaygood)'s proposed behavior simply by opting out of `TC006`. All of the other cases would be handled by the other rules. So changing `TC006`'s default behavior seems extremely redundant in that case.

@Daverball are you sure there's overlap? I tried running `uvx --with=flake8-type-checking flake8 foo.py --select=TC,TC1,TC2 --config=.flake8`, where `foo.py` contains this snippet:

```py
from typing import cast
from expensive import Foo

x = cast(Foo, "foo")
```

and `.flake8` looks like this:

```
[flake8]
type-checking-strict = true
```

The only flake8 error that was emitted was TC006. Same for Ruff with TC selected and `quote-annotations: true` in the Ruff config: https://play.ruff.rs/41018924-01ed-472f-a1b2-ad9317cbfea3

---

_Comment by @Daverball on 2024-12-03 10:56_

@AlexWaygood Absolutely, but that's not what I meant by that statement. `flake8-type-checking` already has slightly different semantics for some of the rules in Ruff to make the rules work better in isolation, and that's good, but even then, the rules need to be aware of which other rules are enabled and the settings in order to provide a good experience and avoid conflicting/redundant violations.

So the scope and the behavior of the rules can and will still drastically change according to the configuration, otherwise we will provide an objectively worse experience, where you can't realistically combine some of the rules without also adjusting the configuration. So it's still difficult to put individual rules in a single category, unless that category is `flake8-type-checking`, since their purpose and effect can change according to the rest of the configuration.

The only other way to get around that is to explode the number of rules and make some of them mutually exclusive, in order to provide the same number of possibilities, but each of them having a clearly distinct purpose, but that seems even more confusing to me. `flake8-type-checking` almost has to be a bit of an outlier in order to provide the flexibility its problem space requires, without degrading UX to the point that nobody will bother using it, because they can't figure out which rules to select or how to configure them to get the result they're after.

> @Daverball are you sure there's overlap?

If you move the import to a `TYPE_CHECKING` block I would expect TC004 to trigger and to suggest quoting `Foo`, but it looks like it's not yet doing that for `cast`, even though it would be safe to. So I guess there isn't any overlap yet. But there definitely will be if I add more of the missing rules.

Also I would consider that behavior a bug in the implementation of TC004. We also should probably not emit a `TC004` for references within a `typing.cast` when `TC006` is enabled, even though the fixes will eventually stabilize back to moving the import into the `TYPE_CHECKING` block, it could be irritating to see the conflicting errors before the fix.

---

_Comment by @AlexWaygood on 2024-12-03 11:00_

> So I guess there isn't any overlap yet. But there definitely will be if I add more of the missing rules.

but I also demonstrated that there doesn't seem to be any overlap when you use flake8 with the flake8-type-checking plugin. The flake8-type-checking plugin only seems to emit TC006 on that snippet. What am I missing?

---

_Comment by @Daverball on 2024-12-03 11:09_

Oh sorry, I missed that part of the question. Yes, that's one of the major design differences between the flake8 plugin and ruff. In the flake8 plugin there are separate rules for adding quotes to annotations, so TC004 would not really be relevant there, but rather TC200, but that rule explicitly only covers annotations and not `typing.cast` or type aliases, since those are already covered by `TC006-008`. So there's no overlap in the flake8 plugin. But in ruff there will be some overlap due to the different design philosophy, so those overlaps need to be detected and eliminated in order to make the user experience of those rules pleasant

---

_Comment by @AlexWaygood on 2024-12-03 11:32_

Okay. I'm still not sure I fully understand which yet-to-be-implemented rule you expect to cover the performance aspects of this rule. But if you're confident that it will be covered by another rule, then I agree that this rule should not be made configurable, and instead the docs should be reworked to make clear that it is an opinionated rule solely concerned with style.

---

_Comment by @Daverball on 2024-12-03 12:30_

The import overhead can be covered by TC001-003 by expanding `quote-annotations = true` to apply to `typing.cast` as well, instead of only runtime evaluated annotations. Although I could see that this decision may create demand for being able to configure the decision based on what kind of type expression it is. I.e. add a separate `quote-casts` setting.

Similar questions come up for TC007, which could technically also partially be rolled into TC001-003 by adding a `quote-annotated-type-alias-values` setting.

The subscript overhead could be covered by a more targeted rule that would also target `typing.cast` in addition to runtime evaluated annotations, as suggested above.

In either case TC006 should take precedence over TC001-003 and this potential new rule.

Then there's an entirely different topic covered by the other missing rules about removing redundant quotes, where ruff currently only provides partial support through UP037 for the `from __future__ import annotations` case, but not the runtime evaluated case (i.e. if you don't care about the overhead of evaluating the type expression, although in this case the rule is conservative and will not unquote expressions containing a subscript or attribute access, since either of those operations might only work in a typing context for libraries with third party stubs).

---

_Closed by @AlexWaygood on 2024-12-04 13:16_

---

_Closed by @AlexWaygood on 2024-12-04 13:16_

---

_Referenced in [astral-sh/ruff#14787](../../astral-sh/ruff/pulls/14787.md) on 2024-12-05 11:26_

---

_Referenced in [vega/vega-datasets#647](../../vega/vega-datasets/pulls/647.md) on 2024-12-14 19:07_

---

_Referenced in [zarr-developers/zarr-python#3032](../../zarr-developers/zarr-python/pulls/3032.md) on 2025-05-04 08:29_

---

_Referenced in [pypa/pipx#1626](../../pypa/pipx/pulls/1626.md) on 2025-05-04 09:25_

---
