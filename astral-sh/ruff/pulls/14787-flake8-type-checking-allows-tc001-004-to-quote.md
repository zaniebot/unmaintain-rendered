```yaml
number: 14787
title: "[`flake8-type-checking`] Allows TC001-004 to quote more expressions"
type: pull_request
state: open
author: Daverball
labels:
  - rule
assignees: []
base: main
head: feat/allow-additional-quotables
created_at: 2024-12-05T11:26:58Z
updated_at: 2025-06-25T11:56:45Z
url: https://github.com/astral-sh/ruff/pull/14787
synced_at: 2026-01-10T18:39:08Z
```

# [`flake8-type-checking`] Allows TC001-004 to quote more expressions

---

_Pull request opened by @Daverball on 2024-12-05 11:26_

This idea came up during the discussion of #14676

## Summary

This adds a new more generic setting to replace `quote-annotation` to allow TC001-004 to be more liberal about moving imports into type checking blocks.

- Allowing cast type expressions to be quoted is a more conservative approach to TC006. (`quote-type-expressions = "safe"`)
- Allowing annotated type alias value expressions to be quoted is a more liberal approach to TC007. (`quote-type-expressions = "eager"`)
-  `quote-type-expressions = "balanced"` matches the old setting `quote-annotations = true`, except it also includes what `"safe"` does, i.e. casts.

## Test Plan

`cargo nextest run`


---

_@Daverball reviewed on 2024-12-05 12:54_

---

_Review comment by @Daverball on `crates/ruff_workspace/src/options.rs`:1922 on 2024-12-05 12:54_

I'm guessing this is not the right way to cross reference another rule, do I have to manually link to the rule's page? It seems like all the other references to other settings in here manually link via fragments, rather than relying on the docs to create the correct cross reference.

---

_Comment by @github-actions[bot] on 2024-12-05 13:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @AlexWaygood on 2024-12-05 22:50_

---

_Comment by @AlexWaygood on 2024-12-06 18:00_

Interesting, thanks for working on this! My overall reaction is that this is a good idea. Do we need another configuration knob, though? What's the profile of somebody who wants their annotations to be quoted if it removes the need for a runtime import, but _doesn't_ want the first argument to `cast()` quoted even if it would remove the need for a runtime import?

I get that the `quote-annotations` setting isn't particularly well named for the purpose, because the first argument to `cast()` is not an annotation. But I think it's much less confusing to users if we keep the number of configuration options to a minimum. So I would simply expand the `quote-annotations` option so that it also quotes the first argument to `cast()` (where it would remove the need for a runtime import), and expand the docs for `TC001-004` to state that this is the new behaviour, and expand the docs for the setting to state that this is the new behaviour. The only disadvantage I can think of, other than the name of the setting not being ideal, is that this would count as an expansion in scope of these rules -- so we might have to make the change as part of the next minor release rather than in a patch release.

It looks like your patch already does the right thing for all of these, but if you haven't already added some, it would be great to add tests for them, because it would obviously be incorrect to quote either of them:

```py
from typing import Annotated, TypeAlias
from expensive import Foo
from expensive2 import Foo2

Y: TypeAlias = Annotated[Foo, "metadata"]
some_object = Y()

class Z(Annotated[Foo2, "metadata"]): ...
```

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-12-06 18:00_

---

_Comment by @Daverball on 2024-12-06 21:16_

> Interesting, thanks for working on this! My overall reaction is that this is a good idea. Do we need another configuration knob, though? What's the profile of somebody who wants their annotations to be quoted if it removes the need for a runtime import, but _doesn't_ want the first argument to `cast()` quoted even if it would remove the need for a runtime import?
> 
> I get that the `quote-annotations` setting isn't particularly well named for the purpose, because the first argument to `cast()` is not an annotation. But I think it's much less confusing to users if we keep the number of configuration options to a minimum. So I would simply expand the `quote-annotations` option so that it also quotes the first argument to `cast()` (where it would remove the need for a runtime import), and expand the docs for `TC001-004` to state that this is the new behaviour, and expand the docs for the setting to state that this is the new behaviour. The only disadvantage I can think of, other than the name of the setting not being ideal, is that this would count as an expansion in scope of these rules -- so we might have to make the change as part of the next minor release rather than in a patch release.

I do think we want some additional knobs, but I do agree that having three separate toggleable switches is awkward, that's the main reason I called this half-baked. I think a sliding scale could make sense, at the low end you would only quote casts, then runtime evaluated annotations too and finally annotated type aliases at the high end. I do agree that there's likely nobody that would have e.g. annotations turned on but casts turned off, but I do think it's likely that there are people that want to have casts turned on, but annotations and type aliases turned off.

Especially type aliases is something some people might not want to ever turn on if they have a bunch of them in a single module that is used by other modules at runtime. They could just turn off TC001-004 in that module, but that seems awkward.

Maybe we add a new setting and deprecate `quote-annotations`. We can call it `quote-type-expressions` with settings of `none`, `safe`, `balanced` and `eager` for none, just casts, casts and annotations and everything. That way we leave ourselves the option to add additional things to the `safe` category in the future.

It might also make it easier to argue for making `safe` the default setting, rather than `never`.

Then we can emit a warning for people that have preview turned on and still use the old setting. People that have preview turned off will return the old default of `never`.

> It looks like your patch already does the right thing for all of these, but if you haven't already added some, it would be great to add tests for them, because it would obviously be incorrect to quote either of them:
> 
> ```python
> from typing import Annotated, TypeAlias
> from expensive import Foo
> from expensive2 import Foo2
> 
> Y: TypeAlias = Annotated[Foo, "metadata"]
> some_object = Y()
> 
> class Z(Annotated[Foo2, "metadata"]): ...
> ```

I do plan to add more tests. I added just enough to make sure that the type alias part works correctly, since that is the most difficult one to get right.

---

_Comment by @Daverball on 2024-12-10 13:15_

@AlexWaygood @MichaReiser Any inputs on the idea of using a sliding scale for configuring what kind of type expressions we're allowed to quote? This would be similar in spirit to `lint.flake8-tidy-imports.ban-relative-imports` where you can choose between `all` and `parents`, except you can choose between four options.

---

_Comment by @AlexWaygood on 2024-12-10 13:45_

Sorry for the delayed response. It makes me sad to have to deprecate a configuration setting, because it's quite disruptive for our users. However, I think your plan makes sense; I can't think of a better one. I agree that there is probably a group of people who want to quote their `cast()` type expressions when it can remove the need for imports, but never want to quote their parameter/return/variable annotations. The crucial difference is that type expressions passed to `cast()` are not introspectable at all, whereas annotations are introspected by many runtime tools.

We can bikeshed over the names of the sliding scale, but your overall plan sounds good to me.

We'll probably have to do the deprecation as part of the next minor release (I think probably in early January, but not sure), but there's no harm in working on a PR now. It'll make the minor release less frantic if it's already ready and has already been reviewed :-)

---

_Comment by @Daverball on 2024-12-10 15:26_

One loosely related thing I have been thinking about is the `from __future__ import annotations` use-case. Currently from a UX perspective it is not as nice as `quote-annotations = true` since you have to rely on other rules to enforce its use which will lead to redundant violations/fixes and potentially imports getting moved back and forth.

Before I had this idea I still had intended to deprecate this setting and add a new one in order to allow the strategy to be one of `move_import`, `quote_annotations` and `add_future_import`, but this interacts poorly with this change, since this selection of strategies only makes sense for annotations, not for casts or type aliases.

We could have a separate setting for `add-future-import`, but this somewhat steps on `flake8-future-annotations`' toes. The same goes for adding `from __future__ import annotations` to `lint.isort.required-imports`. But it still seems preferable over adding `TC100`, since that would once again be potentially bad for people that select `ALL`.  But we could also choose to not support the use-case and instead direct people towards the `isort` setting as a reasonable alternative.

---

_Comment by @AlexWaygood on 2024-12-10 15:42_

Hmm, I kinda think the status quo of telling people to use the isort setting if they want to have the `__future__` import everywhere is ~fine. The violations flagged by different rules should all get fixed in a single Ruff invocation. if you have autofixes enabled. There's value in having focussed rules that do one specific thing each. But if we don't document in the flake8-type-checking rules that you might want to also enable that isort option, then I'd absolutely be open to some docs improvements. (We might do already, I haven't checked!)

---

_Comment by @Daverball on 2024-12-10 16:10_

@AlexWaygood My concern are imports that will be flagged by TC004 for runtime use at the same time as the missing `__future__` import is flagged, fixing the future import gets rid of the TC004 violation, so if the fix moved the imports, you now have TC001-003 violations, so you need another run with fixes to move them back, which might mess with the ordering/formatting of the imports, when there was never a need to mess with the imports to begin with.

Which I assume is also the reason why TC001-004 have been repurposed by ruff to do the quoting of the annotation and moving of the import in a single step, rather than use a separate TC2xx rule to flag annotations like the flake8 plugin does.

That being said, the `__future__` import use-case is certainly not as bad as the quoting use-case, since it's less likely to actually end up being a problem, unless you intend to switch to adding the future import, in which case the imports should already be wrong unless  you also intend to adopt the TC rules at the same time. So I'm fine with leaving things as they are.

---

_Comment by @AlexWaygood on 2024-12-10 16:37_

> @AlexWaygood My concern are imports that will be flagged by TC004 for runtime use at the same time as the missing `__future__` import is flagged, fixing the future import gets rid of the TC004 violation, so if the fix moved the imports, you now have TC001-003 violations, so you need another run with fixes to move them back, which might mess with the ordering/formatting of the imports, when there was never a need to mess with the imports to begin with.

Running Ruff once with `--fix` should never lead to new Ruff errors being emitted if the new errors can be autofixed. Ruff runs all rules in a loop until there are no fixes left to apply. If you have a minimal repro of a snippet where running Ruff once with `--fix` leads to new Ruff violations that can be autofixed, meaning you have to run Ruff a second time with `--fix`, that's a bug -- please file an issue! :-)

---

_Comment by @Daverball on 2024-12-10 16:44_

@AlexWaygood Sorry, I was being a bit unclear. My concern is the redundant work that ruff does for the consecutive fixes and potentially moving the imports to a slightly different location in the code, since the fixes for TC001-004 always move imports to the top of the file/to a type checking block just below all the imports at the top, regardless of where they were originally defined.

So it's more that it might create a bit of mess, that you will then have to revert, not so much that it will result in broken code or end up in an unfixable state.

---

_Comment by @AlexWaygood on 2024-12-10 16:50_

I'm still not convinced that the current situation is that bad. (This conversation is getting a bit too hypothetical -- it's hard to say without concrete examples to look at where this _actually_ creates a mess. And the redundant work that Ruff does doesn't concern me much unless we can demonstrate that there's a measurable speedup from doing something differently -- which I'm sceptical of :)

But this all feels like a bit of a tangent anyway. Even if it is desirable to make the way these violations are fixed be configurable, this could easily be done with a separate configuration option.

---

_Comment by @Daverball on 2024-12-10 17:05_

It definitely is a tangent yes. The only reason I brought it up, is, that if there ended up being a need for an additional setting, that may inform the design of this one, in order to minimize confusion. But I'm happy to punt on this and only let it be an issue once it actually becomes one.

---

_Marked ready for review by @Daverball on 2024-12-12 15:17_

---

_Comment by @Daverball on 2024-12-17 10:47_

@AlexWaygood I implemented the change to a sliding scale using the names I suggested. If we wanted to bikeshed on the names, we should probably do it sooner rather than later.

I think there is a case to be made for more explicit names that directly hint at what will be quoted, although I think the more generic names give us more flexibility with where to draw the line, since there are still some type expressions we don't currently look at for any of the settings (like e.g. `X` in `x = list[X]("abc")`) but even beyond that, there may be additions to the language/stdlib in the future, that lead to additional type expressions in the semantic model, where it might make sense to do something. Like with `typing.cast`. So needing to change the name of a setting or sticking with inaccurate names every time that happens seems bad to me.

But even with the more generic names, there are probably better names, that better reflect the intent of each setting. So I am open to suggestions.

---

_Label `rule` added by @dhruvmanila on 2024-12-24 06:42_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-03 09:37_

---

_Comment by @Daverball on 2025-01-03 14:49_

@MichaReiser Can we try to get this into the 0.9 milestone?

---

_Comment by @MichaReiser on 2025-02-06 14:14_

I haven't forgotten about your PRs. I just need to find a quiet week to review them. 

---
