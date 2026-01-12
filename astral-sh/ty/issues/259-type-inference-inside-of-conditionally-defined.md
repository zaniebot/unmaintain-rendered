```yaml
number: 259
title: type inference inside of conditionally defined functions appears incorrect
type: issue
state: closed
author: mikeshardmind
labels:
  - narrowing
assignees: []
created_at: 2025-05-08T02:10:12Z
updated_at: 2025-07-25T07:11:12Z
url: https://github.com/astral-sh/ty/issues/259
synced_at: 2026-01-12T15:54:22Z
```

# type inference inside of conditionally defined functions appears incorrect

---

_@mikeshardmind_

### Summary

`Callable[..., Any] & ~None` becomes `Callable[..., Any] | None` inside a function defined within the branch that narrowed to include `~None`

Minimal repro:

https://play.ty.dev/ba37c54e-a0ea-4382-87fb-e2f4945868bf

real world code:

https://github.com/mikeshardmind/async-utils/blob/7bf094f063a727901539598811d3fd907ccf5342/src/async_utils/task_cache.py#L153-L158

relevant errors on that real world code with reveal types added:

```
info: revealed-type: Revealed type
   --> src\async_utils\task_cache.py:243:9
    |
241 |     else:
242 |         from typing import reveal_type
243 |         reveal_type(cache_transform)
    |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ `((...) -> tuple[@Todo, dict[str, Any]]) & ~None`
244 |         def key_func(args: tuple[t.Any, ...], kwds: dict[t.Any, t.Any], /) -> Hashable:
245 |             reveal_type(cache_transform)
    |

info: revealed-type: Revealed type
   --> src\async_utils\task_cache.py:245:13
    |
243 |         reveal_type(cache_transform)
244 |         def key_func(args: tuple[t.Any, ...], kwds: dict[t.Any, t.Any], /) -> Hashable:
245 |             reveal_type(cache_transform)
    |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ `((...) -> tuple[@Todo, dict[str, Any]]) | None`
246 |             return make_key(*cache_transform(args, kwds))
    |

error: lint:call-non-callable: Object of type `None` is not callable
   --> src\async_utils\task_cache.py:246:30
    |
244 |         def key_func(args: tuple[t.Any, ...], kwds: dict[t.Any, t.Any], /) -> Hashable:
245 |             reveal_type(cache_transform)
246 |             return make_key(*cache_transform(args, kwds))
    |                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^
247 |
248 |     def wrapper[**P, R](coro: TaskCoroFunc[P, R], /) -> TaskFunc[P, R]:
    |
info: `lint:call-non-callable` is enabled by default
```

### Version

0.0.0-alpha.7

---

_Comment by @carljm on 2025-05-08 04:41_

Thanks for the report! Yeah, narrowing of nonlocal references inside nested functions is an area where we know we need to improve. It's tricky to find the right balance of useful vs sound, since functions are delayed-execution and the name could potentially be reassigned later in the scope (though of course in this case it isn't, and we should be able to see that)

---

_Label `narrowing` added by @carljm on 2025-05-08 04:42_

---

_Comment by @sharkdp on 2025-05-08 06:39_

Just adding a cross-reference to https://github.com/astral-sh/ty/issues/210, which is a related issue (in a non-narrowing setting)

---

_Closed by @MichaReiser on 2025-07-25 07:11_

---
