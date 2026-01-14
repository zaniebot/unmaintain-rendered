```yaml
number: 2494
title: "`unused-ignore-comment` should not handle `type:` comments"
type: issue
state: open
author: lypwig
labels:
  - good first issue
  - configuration
assignees: []
created_at: 2026-01-14T14:57:10Z
updated_at: 2026-01-14T15:34:21Z
url: https://github.com/astral-sh/ty/issues/2494
synced_at: 2026-01-14T15:39:25Z
```

# `unused-ignore-comment` should not handle `type:` comments

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
