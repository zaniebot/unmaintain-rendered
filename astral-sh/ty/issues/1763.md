```yaml
number: 1763
title: "`__builtins__` as the `builtins` module, instead of `Any`"
type: issue
state: closed
author: jorenham
labels:
  - wish
assignees: []
created_at: 2025-12-04T21:44:30Z
updated_at: 2025-12-04T23:09:50Z
url: https://github.com/astral-sh/ty/issues/1763
synced_at: 2026-01-10T01:56:41Z
```

# `__builtins__` as the `builtins` module, instead of `Any`

---

_Issue opened by @jorenham on 2025-12-04 21:44_

this follows https://github.com/astral-sh/ty/issues/393, which added partial support for `__builtins__` by having it resolve to `Any`:

```py
reveal_type(__builtins__)
```

```
Revealed type: `Any` (revealed-type) [Ln 1, Col 13]
```

https://play.ty.dev/9baeb0d8-d23a-40b2-867c-c98a28e17463

It might be cool to have `__builtins__` resolve to the actual `builtins` module. Perhaps by adding a secret hidden phantom ghost import `import builtins as __builtins__` to each module, but without it being visible, i.e. in a way that it cannot be seen, so that it doesn't appear or show itself to the user. 


---

_Comment by @carljm on 2025-12-04 21:51_

Unfortunately the runtime behavior of `__builtins__` is really strange and hard to predict. Sometimes it is the builtins module, but sometimes it is just the dictionary of the builtins, and there's no clear way for a static analyzer to know which it will actually be (it depends whether the module is imported or executed -- and in fact the more common case, an imported module, is where `__builtins__` ends up being a dict, not a module). So trying to do anything more precise than `Any` will lead to false positives for some cases. That's why we ended up just going with `Any`: https://github.com/astral-sh/ruff/pull/18118#issuecomment-2884152617

Ultimately `__builtins__` is a CPython implementation detail that shouldn't be relied on, so we can't complain too much about it having weird and unpredictable behavior. But `Any` seems like the best option for dealing with that behavior.

---

_Label `wish` added by @carljm on 2025-12-04 21:52_

---

_Comment by @jorenham on 2025-12-04 22:29_

Hmm yea that sounds annoyingly complicated... It's not really a very interesting feature, or something I'm actively using, so it's also fine by me if you want to close this.

---

_Closed by @carljm on 2025-12-04 23:09_

---
