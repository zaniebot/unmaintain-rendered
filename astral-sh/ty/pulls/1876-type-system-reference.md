```yaml
number: 1876
title: Type system reference
type: pull_request
state: closed
author: sharkdp
labels:
  - documentation
assignees: []
draft: true
base: main
head: david/type-system-reference
created_at: 2025-12-13T10:40:32Z
updated_at: 2025-12-15T13:13:30Z
url: https://github.com/astral-sh/ty/pull/1876
synced_at: 2026-01-10T02:34:11Z
```

# Type system reference

---

_Pull request opened by @sharkdp on 2025-12-13 10:40_

A document in this style is probably waaay too detailed for users, but it's sort of helpful for us?

This document draws from various sources:
* https://typing.python.org/en/latest/
* https://peps.python.org/topic/typing/, and all included PEPs
* Our mdtests
* Our actual Rust source code
* ty issue tracker


---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:129 on 2025-12-13 12:31_

```suggestion
| Modules as protocol implementations           | ⚠️ [#931](https://github.com/astral-sh/ty/issues/931)           |
```

It's only if you use a method member that things don't work as expected. And arguably that's unsound anyway (i have some issues with the spec here 😆). But we may need to find a solution for compatibility sake anyway.

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:242 on 2025-12-13 12:32_

Worth mentioning https://github.com/astral-sh/ty/issues/1825?

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:184 on 2025-12-13 12:34_

I think from a user perspective this is basically the same as "not supported"?

```suggestion
| `TypeGuard[…]` user-defined type guards   | ❌ (no narrowing applied yet)                   |
```

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:203 on 2025-12-13 12:35_

This should be "partial", we still don't emit an error if you override a `Final` attribute on a subclass 

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:210 on 2025-12-13 12:36_

Looks like the formatting went awry here 

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:220 on 2025-12-13 12:36_

Should be partial, we still don't support fully stringified r.h.s. of a PEP-613 alias

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:241 on 2025-12-13 12:37_

Only partial, we still don't support the stricter version of the check that is also specified

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:246 on 2025-12-13 12:39_

Things we currently check:
- method overridden by method

Things we don't yet check:

- Method overridden by non-method
- Non-method overridden by non-method

---

_@AlexWaygood reviewed on 2025-12-13 12:42_

Awesome work!! I think this will be really helpful for users as well as us!

I assume this is structured according to the typing spec, but I wonder if a "stdlib" section could also be useful? Features I know we don't yet fully support include `functools.total_ordering`, `functools.partial`, `abc.abstractmethod` 

---

_@dhruvmanila reviewed on 2025-12-15 08:14_

---

_Review comment by @dhruvmanila on `docs/reference/type-system-features.md`:105 on 2025-12-15 08:14_

`Concatenate` is not supported yet https://github.com/astral-sh/ty/issues/1535

---

_Label `documentation` added by @dhruvmanila on 2025-12-15 08:16_

---

_@sharkdp reviewed on 2025-12-15 08:27_

---

_Review comment by @sharkdp on `docs/reference/type-system-features.md`:241 on 2025-12-15 08:27_

Do we have a ticket for this? What about https://github.com/astral-sh/ty/issues/1675?

---

_@AlexWaygood reviewed on 2025-12-15 08:43_

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:241 on 2025-12-15 08:43_

Just the original ticket #155, which was never closed.

> What about https://github.com/astral-sh/ty/issues/1675?

that one's not important enough to be worth mentioning here IMO, but no strong opinion!

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:56 on 2025-12-15 09:34_

I think we can only claim partial support while https://github.com/astral-sh/ty/issues/1124 is open 

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:265 on 2025-12-15 09:37_

This was added over the weekend in https://github.com/astral-sh/ruff/commit/a544c59186731d8cab4c5cce05a228af585fbafe!

I think we can check it off now 

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:354 on 2025-12-15 09:39_

I think we respect these as much as we ever intend to? I'd be inclined to check this one off 

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:406 on 2025-12-15 09:40_

How is this different to the previous one?

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:405 on 2025-12-15 09:41_

You can link https://github.com/astral-sh/ty/issues/1877

---

_@AlexWaygood reviewed on 2025-12-15 09:47_

You could also mention somewhere that we support PEP 800

---

_@AlexWaygood reviewed on 2025-12-15 09:49_

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:441 on 2025-12-15 09:49_

Could also mention that we currently only support slots if it's a single string or tuple of strings. We should also add support for list literals, dict literals and set literals

---

_Review comment by @AlexWaygood on `docs/reference/type-system-features.md`:108 on 2025-12-15 09:50_

Could mention https://github.com/astral-sh/ty/issues/1741 as a limitation we have currently that other type checkers don't have 

---

_@AlexWaygood reviewed on 2025-12-15 09:50_

---

_Comment by @sharkdp on 2025-12-15 13:12_

Alex had a good idea: this is now a pinned issue (https://github.com/astral-sh/ty/issues/1889), which should hopefully reduce the maintenance overhead.

I will consider adding a link to this pinned issue from the documentation.

---

_Closed by @sharkdp on 2025-12-15 13:13_

---
