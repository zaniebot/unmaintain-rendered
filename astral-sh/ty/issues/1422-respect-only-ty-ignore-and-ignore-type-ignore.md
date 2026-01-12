```yaml
number: 1422
title: "Respect only `ty:ignore` and ignore `type:ignore`"
type: issue
state: closed
author: nth10sd
labels:
  - configuration
  - suppression
assignees: []
created_at: 2025-10-23T16:07:08Z
updated_at: 2025-12-23T07:36:52Z
url: https://github.com/astral-sh/ty/issues/1422
synced_at: 2026-01-12T15:54:25Z
```

# Respect only `ty:ignore` and ignore `type:ignore`

---

_@nth10sd_

### Question

To suppress single-line errors, `ty` seems to respect both `# ty: ignore` and `# type: ignore`. Can `ty` be set to only recognize `# ty: ignore`?

This is because `mypy` [cannot support](https://github.com/python/mypy/issues/12358) `# mypy: ignore`, and ideally would like to have both separated.

### Version

_No response_

---

_Label `question` added by @nth10sd on 2025-10-23 16:07_

---

_Label `suppression` added by @AlexWaygood on 2025-10-23 16:57_

---

_Label `configuration` added by @AlexWaygood on 2025-10-23 16:57_

---

_Comment by @MichaReiser on 2025-10-24 15:48_

I thought we discussed this before but I can't find the issue anymore. 

An option like this does make sense to me. 

---

_Label `question` removed by @MichaReiser on 2025-10-24 15:48_

---

_Comment by @MichaReiser on 2025-10-24 15:53_

Okay, it has come up in https://github.com/astral-sh/ty/issues/278 and this will also be relevant for https://github.com/astral-sh/ty/issues/606

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:26_

---

_Renamed from "Respect only ty:ignore and ignore type:ignore" to "Respect only `ty:ignore` and ignore `type:ignore`" by @MichaReiser on 2025-11-14 08:27_

---

_Comment by @cmp0xff on 2025-12-08 11:17_

Hi, this feature is very helpful for projects where both `mypy` and `ty` are used.

---

_Comment by @MichaReiser on 2025-12-19 17:58_

My current thinking on this is:

* Add a new `analysis.type-ignore` that controls whether ty respects `type: ignore` comments (true) or ignores them (false).
* Add a new `unused-type-ignore-comment` comment rule that's off by default

Here a few examples of how this would work in combination:

* `type-ignore=true`: errors can be suppressed with `type: ignore` and `ty: ignore`
  * `unused-type-ignore-comment="warn"`: ty removes both unused `ty: ignore` and `type: ignore` comments. 
  * `unused-type-ignore-comment="ignore"`: ty removes unused `ty: ignore` comments but leaves unused `type: ignore` comments untouched. 
* `type-ignore: false`: errors can only be suppressed with `ty: ignore`.
  * `unused-type-ignore-comment="warn"`: ty removes all `type: ignore` comments. This allows users to deny `type: ignore` comments in their code base
  * `unused-type-ignore-comment="ignore"`: ty leaves `type: ignore` comments unchanged. Useful when using ty alongside another type checker


The one case this combination doesn't support are projects that want to exclusively use `type: ignore` and forbid using `ty: ignore`. We could support that by replacing the `type-ignore` option with `ignore-style="ty" | "type" | "both"`, assuming we never want to support any other style (`noqa`?)


A third option is to make the `ignore-styles` option a list that can be set to `["type", "ty"]` to allow both, or just `"type"` or `"ty"`. 



---

_Comment by @carljm on 2025-12-19 18:37_

The `analysis.type-ignore` setting makes sense to me, although I think the name is too ambiguous; maybe `analysis.respect-type-ignore-comments`?

I'm confused about the `unused-type-ignore-comment` rule. Is this intended to be a new rule, separate from our existing `unused-ignore-comment`? If so, then a) I don't think we need this new rule at all, and b) if we did add it, I don't think this rule should ever have any impact on the handling of `# ty: ignore` comments.

I don't think that "deny all `type: ignore` comments in your code base" is a use case that has ever been requested or that we need to support in ty. It's not necessary for migrating to ty or using ty alongside another type checker. At best it's a trivial ruff lint rule.

I think that if `analysis.respect-type-ignore-comments: true`, then the behavior should be as it is today, where `unused-ignore-comment` fires on both unused `ty: ignore` and unused `type: ignore` comments. If `analysis.respect-type-ignore-comments: false`, then ty should never consider `type: ignore` comments at all -- it should be as if they were any other normal comment, with no special meaning. I think that option is sufficient to meet the actual user needs that have been expressed so far, and we should avoid adding more complexity to meet hypothetical use cases.

---

_Comment by @MichaReiser on 2025-12-19 19:13_

> I'm confused about the unused-type-ignore-comment rule. Is this intended to be a new rule, separate from our existing unused-ignore-comment? If so, then a) I don't think we need this new rule at all, and b) if we did add it, I don't think this rule should ever have any impact on the handling of # ty: ignore comments.

Yes it would be a new rule. No, it doesn't change how we handle `ty: ignore`. Not sure what I wrote that gave that impression. You'd have to enable `unused-ignore-comment` for the `ty: ignore` comments to be removed but i omitted this above because I assumed that was clear, sorry if that caused the confusion.


The issue I see with only having a single `respect-type-ignore-comment` is the case where you want to use `type: ignore` to suppress errors common to multiple type checkers but you want to remove unused `ty: ignore` comment (but leave `type: ignore` alone because you might use it to suppress both a mypy and pyright error)

Also, you suggested separate rules in https://github.com/astral-sh/ty/issues/278#issuecomment-2863650989 😅 

But I'm open to give it a try. Splitting out a new rule is easy

Note for myself: Pyright's setting name is `enableTypeIgnoreComments`



---

_Comment by @carljm on 2025-12-20 03:03_

> No, it doesn't change how we handle `ty: ignore`. Not sure what I wrote that gave that impression.

The part that gave me this impression was

> unused-type-ignore-comment="warn": ty removes both unused ty: ignore and type: ignore comments.

contrasted with

> unused-type-ignore-comment="warn": ty removes all type: ignore comments.

But I think I misunderstood this. I think there's an implicit assumption that `unused-ignore-comment` rule is also enabled, and I think unused `ty: ignore` would be removed in both cases, that's just not mentioned in the second case. With that understanding, the proposal definitely makes more sense to me.

> Also, you suggested separate rules in [#278 (comment)](https://github.com/astral-sh/ty/issues/278#issuecomment-2863650989) 😅

Haha, so I did! And I even said "and/or" there.

The problem with _just_ doing separate rules is that it doesn't give you any way to say "I don't even want `type: ignore` to suppress ty diagnostics, that's just for mypy". Given that we can't parse rule codes with `type: ignore` comments, I think it's important to be able to have ty totally ignore them.

So if we are just doing one, it should be the config option.

> The issue I see with only having a single `respect-type-ignore-comment` is the case where you want to use `type: ignore` to suppress errors common to multiple type checkers but you want to remove unused `ty: ignore` comment (but leave `type: ignore` alone because you might use it to suppress both a mypy and pyright error)

Yes, that's a more convincing use case. The alternative for this would be to just turn off `respect-type-ignore-comment` and then use `# type: ignore # ty: ignore` on lines where you want to ignore an issue both type checkers agree on, but that could be kind of irritating if there are a lot of shared issues. (Though I still think that personally I would prefer this double-ignore-comment strategy, since it allows ensuring that _all_ ignores use a rule code.)

Ok, I'm convinced! I think it makes sense to do both.

But I'm still not convinced by

> `type-ignore: false`: errors can only be suppressed with `ty: ignore`.
> 
>     * `unused-type-ignore-comment="warn"`: ty removes all `type: ignore` comments. This allows users to deny `type: ignore` comments in their code base

If I've said `respect-type-ignore-comments: false`, I would expect that to mean ty doesn't do anything at all with `# type: ignore` comments -- they might as well be "# foobar" comments. So I think `unused-type-ignore-comment` rule should just be a no-op in this case. If ty is not respecting `# type: ignore` comments, then it can't decide whether they are unused or not, and it should never remove them.

---

_Comment by @MichaReiser on 2025-12-20 10:02_

Let's start with the option, and split the rule based on user feedback. 

---

_Assigned to @MichaReiser by @MichaReiser on 2025-12-20 18:35_

---

_Comment by @MichaReiser on 2025-12-20 18:35_

I started to work on this. 

---

_Closed by @MichaReiser on 2025-12-23 07:36_

---
