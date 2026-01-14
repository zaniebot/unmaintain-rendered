```yaml
number: 2494
title: "`unused-ignore-comment` should not handle unknown rules"
type: issue
state: open
author: lypwig
labels:
  - configuration
assignees: []
created_at: 2026-01-14T14:57:10Z
updated_at: 2026-01-14T16:02:32Z
url: https://github.com/astral-sh/ty/issues/2494
synced_at: 2026-01-14T16:38:48Z
```

# `unused-ignore-comment` should not handle unknown rules

---

_@lypwig_

### Summary

I work on a Django application and I use both ty and Pyright for type checking.

I need to ignore typing on `Meta` classes, otherwise Pyright complains about *reportIncompatibleVariableOverride*:

```py
    class Meta:  # type: ignore[reportIncompatibleVariableOverride]
```

But this comment triggers ty rule [`unused-ignore-comment`](https://docs.astral.sh/ty/reference/rules/#unused-ignore-comment) (*Unused blanket `type: ignore` directive*), incorrectly because the directive IS actually used, by Pyright.

### Version

ty 0.0.11

---

_Comment by @AlexWaygood on 2026-01-14 15:01_

Thanks for the report! You can configure this by tweaking [this setting](https://docs.astral.sh/ty/reference/configuration/#respect-type-ignore-comments). This is stated in the docs that you link to.

---

_Closed by @AlexWaygood on 2026-01-14 15:01_

---

_Comment by @MichaReiser on 2026-01-14 15:02_

I'll keep this open for now because I'm curious whether this setting works for them or if they want one that only disables `unused-ignore-comment` for `ty: ignore`.

---

_Reopened by @MichaReiser on 2026-01-14 15:02_

---

_Label `question` added by @MichaReiser on 2026-01-14 15:02_

---

_Comment by @lypwig on 2026-01-14 15:12_

Ok thank you the [respect-type-ignore-comments](https://docs.astral.sh/ty/reference/configuration/#respect-type-ignore-comments) settings works.

I was not aware of this option, I think it should be mentioned in [unused-ignore-comment](https://docs.astral.sh/ty/reference/rules/#unused-ignore-comment) rule documentation.

---

_Comment by @AlexWaygood on 2026-01-14 15:13_

> I was not aware of this option, I think it should be mentioned in [unused-ignore-comment](https://docs.astral.sh/ty/reference/rules/#unused-ignore-comment) rule documentation.

It's already mentioned as the last sentence in that documentation

---

_Comment by @lypwig on 2026-01-14 15:24_

> It's already mentioned as the last sentence in that documentation

Oh I'm so sorry I didn't pay attention.

---

That said, I would like to suggest an other behavior.

The `type: ignore` is actually handy because I don't have to ignore both ty and Pyright rules.

I could ignore the ty rule with:

```py
class Meta:  # type: ignore[reportIncompatibleVariableOverride] # ty: ignore[unused-ignore-comment]
```

But the fact is the ty rule `reportIncompatibleVariableOverride` does not exist...

So does it make sense for you to trigger `unused-ignore-comment` only if it an actual ty rule AND it is not used?

And eventually add an other rule to check if `type:` is used with an unknown rule?

---

_Comment by @MichaReiser on 2026-01-14 15:33_

> So does it make sense for you to trigger unused-ignore-comment only if it an actual ty rule AND it is not used?

That's exactly the feedback I was looking for :) Thank you. Yes, I think splitting the rules makes sense. We just didn't know if there's any demand for it, that's why we initially deferred it.

---

_Label `question` removed by @MichaReiser on 2026-01-14 15:34_

---

_Label `configuration` added by @MichaReiser on 2026-01-14 15:34_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-14 15:34_

---

_Label `good first issue` added by @MichaReiser on 2026-01-14 15:34_

---

_Renamed from "`unused-ignore-comment` should not handle `type:` comments" to "`unused-ignore-comment` should not handle unknown rules" by @lypwig on 2026-01-14 15:41_

---

_Comment by @carljm on 2026-01-14 15:46_

It's not clear to me exactly what behavior we intend to implement in this issue. What @lypwig proposes (basing the behavior of `unused-ignore-comment` on whether a code is used that we recognize or not, and then having a separate diagnostic for "type: ignore with unknown code") does not sound to me like any behavior we'd previously discussed when we discussed "splitting the rule."


---

_Comment by @MichaReiser on 2026-01-14 15:48_

Yeah, sorry. I didn't read the response carefully enough. 

> So does it make sense for you to trigger unused-ignore-comment only if it an actual ty rule AND it is not used? 

I don't feel comfortable with that because it assumes that no two type checkers have overlapping rule names. 

What I would suggest is that we make it a separate rule so that projects using multiple type checkers can disable the `type: ignore` one.

> And eventually add an other rule to check if type: is used with an unknown rule?

I'm not sure how this would work if you use multiple type checkers as ty would warn about rules that it doesn't know but those might be pyright rules.

---

_Label `good first issue` removed by @MichaReiser on 2026-01-14 15:52_

---

_Comment by @carljm on 2026-01-14 16:00_

Yes, I agree with all of that.

So then I just want to make sure that we properly understand @lypwig's use case, if we are going to take it as feedback in favor of two separate rules.

@lypwig I'm not sure I understand this example:

> class Meta:  # type: ignore[reportIncompatibleVariableOverride] # ty: ignore[unused-ignore-comment]

If you set `respect-type-ignore-comments: false`, then you wouldn't need any `# ty: ignore` there.

So can you clarify what is the problem you find with setting `respect-type-ignore-comments: false`? Is it that you have some `type: ignore` that should actually be respected by both ty and pyright? To me, in that case it actually seems better to use a separate `type: ignore` for pyright and `ty: ignore` for ty, because then you can use a rule code for both. If you use a single `type: ignore` to silence both, then (since pyright and ty don't share rule codes) it would have to be a blanket (code-less) ignore, which is more error-prone.

(Somewhat separately, I do wonder about our current behavior of interpreting any `type: ignore` with an unrecognized code as a blanket `type: ignore`. It seems unlikely to me that that will be the desired behavior for anyone -- but I'm also not sure what would be a better behavior.)

---
