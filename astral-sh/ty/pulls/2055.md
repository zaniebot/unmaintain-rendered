```yaml
number: 2055
title: "Typing FAQ: Attributes on Callable"
type: pull_request
state: merged
author: sharkdp
labels:
  - documentation
assignees: []
merged: true
base: main
head: david/callable-attribute-faq-entry
created_at: 2025-12-18T09:19:14Z
updated_at: 2025-12-18T13:04:53Z
url: https://github.com/astral-sh/ty/pull/2055
synced_at: 2026-01-10T02:34:11Z
```

# Typing FAQ: Attributes on Callable

---

_Pull request opened by @sharkdp on 2025-12-18 09:19_

Add a new FAQ entry based on issues like https://github.com/astral-sh/ty/issues/2038

---

_Label `documentation` added by @sharkdp on 2025-12-18 09:19_

---

_Review comment by @sharkdp on `docs/reference/typing-faq.md`:154 on 2025-12-18 09:20_

So cool that this works now! I'd love to add this type alias to `ty_extensions`, but we should probably add support for calling intersection types first (#1858).

---

_@sharkdp reviewed on 2025-12-18 09:20_

---

_@sharkdp reviewed on 2025-12-18 09:21_

---

_Review comment by @sharkdp on `docs/reference/typing-faq.md`:154 on 2025-12-18 09:21_

This is also a good example for advertising intersection types.

---

_@spaceone reviewed on 2025-12-18 12:41_

---

_Review comment by @spaceone on `docs/reference/typing-faq.md`:123 on 2025-12-18 12:41_

```suggestion
To fix this, you could use a `getattr(…, "__name__", default)` check to fall back to a default name when the
attribute is not present:

```py
name = getattr(operation, "__name__", "operation")
```

---

_Review comment by @AlexWaygood on `docs/reference/typing-faq.md`:94 on 2025-12-18 12:49_

```suggestion
Functions do (including lambdas), but other callable objects do not. The `FileUpload` class below, for example,
```

(`lambda`s are instances of `FunctionType` at runtime, just like functions created using `def` statements)

---

_Review comment by @AlexWaygood on `docs/reference/typing-faq.md`:95 on 2025-12-18 12:51_

```suggestion
is callable, but instances of `FileUpload` do not have a `__name__` attribute. Passing a `FileUpload` instance to `retry`
```

---

_Review comment by @AlexWaygood on `docs/reference/typing-faq.md`:114 on 2025-12-18 12:52_

I think it's easier for users to understand if we fill in the bodies here, or use `...` to visually indicate that "something's being left out"

```suggestion
        ...

    def __call__(self) -> bool:
        return True
```

---

_Review comment by @AlexWaygood on `docs/reference/typing-faq.md`:123 on 2025-12-18 12:52_

I agree that using `getattr()` with three arguments is probably more idiomatic here

---

_@AlexWaygood approved on 2025-12-18 12:53_

Fantastic, thank you

---

_Merged by @sharkdp on 2025-12-18 13:04_

---

_Closed by @sharkdp on 2025-12-18 13:04_

---

_Branch deleted on 2025-12-18 13:04_

---
