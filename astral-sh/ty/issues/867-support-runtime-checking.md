```yaml
number: 867
title: Support runtime checking
type: issue
state: closed
author: jdahlin
labels: []
assignees: []
created_at: 2025-07-22T08:32:47Z
updated_at: 2025-07-22T11:04:13Z
url: https://github.com/astral-sh/ty/issues/867
synced_at: 2026-01-12T15:54:24Z
```

# Support runtime checking

---

_@jdahlin_

Feature request: Support runtime type checking

Provide an easy way for end users to enable type checking in such a way that no separate build/lint step is needed.

One way to provide this is to have a wrapper binary (or -mty.runtime) around python which always type check code before running it, perhaps via a sys.meta_path hook.


---

_Comment by @AlexWaygood on 2025-07-22 11:04_

Thanks for the issue! This is a complicated topic, and supporting this use case is well out of scope for ty right now, unfortunately. For now, you might consider using one of the pre-existing frameworks for doing this kind of thing, which all have their pros and cons. Some of the more popular ones include:
- https://github.com/agronholm/typeguard
- https://github.com/beartype/beartype
- https://github.com/davidfstr/trycast

If we were to add something like this in the future, it would probably look more similar to [mypyc](https://github.com/mypyc/mypyc) rather than one of the above frameworks, though: this would also provide potentially big speedups, but at the cost of some dynamic features that Python typically provides and that type checkers can't support.

Since a mypyc-style compiler is already discussed in https://github.com/astral-sh/ty/issues/283, I'll close this in favour of that issue

---

_Closed by @AlexWaygood on 2025-07-22 11:04_

---
