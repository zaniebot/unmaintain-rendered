---
number: 18976
title: Possible to automatically ignore UP046 in cases of manual variance setting
type: issue
state: open
author: max-muoto
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2025-06-27T05:52:59Z
updated_at: 2025-06-27T13:49:02Z
url: https://github.com/astral-sh/ruff/issues/18976
synced_at: 2026-01-07T13:12:16-06:00
---

# Possible to automatically ignore UP046 in cases of manual variance setting

---

_Issue opened by @max-muoto on 2025-06-27 05:52_

### Question

While UP046 is an extremely helpful rule, in cases where I'm manually setting a `TypeVar` to covariant or contravariant (which is not something supported through the inline syntax) it would be convenient to avoid enforcement of the rule. Curious if this is something that's been considered, or that a setting could be added to allow for. The same thing goes for using type var defaults pre 3.13 (with `typing_extensions.TypeVar`).

Happy to see about contributing this if we're open to it.

### Version

0.12.1

---

_Label `question` added by @max-muoto on 2025-06-27 05:53_

---

_Comment by @Daverball on 2025-06-27 12:38_

Manual variance is a hard sell for me, because `infer_variance` is only available in `typing_extensions.TypeVar` in older versions, so it's unlikely many people will use it. So it would make this rule much less useful for its intended purpose (upgrading from older versions to Python 3.12+).

I think it can make sense to add a configuration option for that though.

I do however agree that the rule should not trigger if the `TypeVar` has a `default` argument and the target version is < 3.13

---

_Comment by @ntBre on 2025-06-27 13:44_

I think UP046 and UP047 should already ignore `TypeVar`s with `default` arguments: [playground](https://play.ruff.rs/0ac6271c-1042-4fac-be69-b4e188c6e73f). That's actually true even on 3.13, since we haven't added support for `default` at all yet.

I'd be open to adding a configuration option for variance. It's already mentioned in the rule docs as a reason for the fix being unsafe, so it seems like a reasonable concern. I think we should leave the default as-is (the fix is already unsafe) but allow opting out of the rules when you've manually specified the variance.

Does that make sense to you, @AlexWaygood?

---

_Label `configuration` added by @ntBre on 2025-06-27 13:44_

---

_Label `needs-decision` added by @ntBre on 2025-06-27 13:44_

---

_Label `question` removed by @ntBre on 2025-06-27 13:44_

---

_Comment by @AlexWaygood on 2025-06-27 13:49_

> in cases where I'm manually setting a `TypeVar` to covariant or contravariant (which is not something supported through the inline syntax) it would be convenient to avoid enforcement of the rule.

In most cases, type checkers _should_ be able to correctly infer the variance of a TypeVar (that's why it's not possible to manually specify them with the new syntax, after all!). So it _should_ be pretty rare that you _need_ to manually override the type checker's inference and specify a different variance to the one that the type checker infers. I'd argue it's rare enough that you should just be able to deal with this by adding `# noqa` comments to the specific problematic classes?

---

_Referenced in [astral-sh/ruff#18894](../../astral-sh/ruff/issues/18894.md) on 2025-10-29 13:35_

---
