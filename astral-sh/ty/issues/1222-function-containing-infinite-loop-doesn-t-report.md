```yaml
number: 1222
title: "function containing infinite loop doesn't report any warning"
type: issue
state: open
author: KotlinIsland
labels:
  - control flow
  - lint
assignees: []
created_at: 2025-09-20T15:22:35Z
updated_at: 2025-09-22T23:42:09Z
url: https://github.com/astral-sh/ty/issues/1222
synced_at: 2026-01-12T15:54:24Z
```

# function containing infinite loop doesn't report any warning

---

_@KotlinIsland_

### Summary

```py
def f() -> int:
    while True:
        pass
```

while this is technically valid, it doesn't make any sense, imo the infinite loop control flow shouldn't allow this, or at least display a warning

### Version

_No response_

---

_Label `wish` added by @sharkdp on 2025-09-22 07:55_

---

_Label `control flow` added by @sharkdp on 2025-09-22 07:55_

---

_Label `wish` removed by @sharkdp on 2025-09-22 07:55_

---

_Label `lint` added by @sharkdp on 2025-09-22 07:59_

---

_Comment by @sharkdp on 2025-09-22 08:01_

Thank you for reporting this.

It looks like other type checkers also don't report an error here, and as you've stated, there is nothing technically wrong with this function. There are also perfectly valid use cases for functions that return `int`, but contain an infinite loop. I agree that this particular case (with no `return` statements inside the loop) looks like an oversight, though.

So having a lint rule that would catch functions that are annotated with a non-empty type, but whose actual return type is `Never`, sounds like a reasonable idea.

---

_Comment by @KotlinIsland on 2025-09-22 08:14_

> but contain an infinite loop

the specific loop i am describing here is a `Never` loop, not just any old infinite loop

---

_Comment by @carljm on 2025-09-22 20:23_

I agree that at best this should be a lint rule, not a type error.

> having a lint rule that would catch functions that are annotated with a non-empty type, but whose actual return type is `Never`, sounds like a reasonable idea.

I've seen this lint rule proposed before. One issue is that it doesn't play well with methods and inheritance. The method `def method(self) -> int: raise NotImplementedError()` is a common base-class pattern. One could argue that it should always be explicitly decorated with `@abstractmethod` or something? At that point it's firmly in the category of "opinionated opt-in lint rule."

---

_Comment by @KotlinIsland on 2025-09-22 23:42_

@carljm i am only concerned with `Never` loops, i don't think this check should apply to anything else

---
