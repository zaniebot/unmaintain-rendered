---
number: 16315
title: PERF401 gives incorrect suggestions and requires suboptimal workarounds with async iterators
type: issue
state: open
author: MarkusSintonen
labels:
  - documentation
  - rule
assignees: []
created_at: 2025-02-22T12:46:42Z
updated_at: 2025-11-10T18:35:59Z
url: https://github.com/astral-sh/ruff/issues/16315
synced_at: 2026-01-07T13:12:16-06:00
---

# PERF401 gives incorrect suggestions and requires suboptimal workarounds with async iterators

---

_Issue opened by @MarkusSintonen on 2025-02-22 12:46_

### Description

PERF401 triggers in a following case incorrectly and gives an incorrect suggestion. Making Ruff happy requires creating unnecessary temporary list. Which is not a great workaround.

```python
from collections.abc import AsyncIterator


async def my_nested_aiter() -> AsyncIterator[AsyncIterator[int]]:
    async def inner() -> AsyncIterator[int]:
        yield 1

    yield inner()


async def test_ruff_issue():
    my_list: list[int] = []
    async for inner_aiter in my_nested_aiter():
        async for x in inner_aiter:
            # Fails in Ruff with. "PERF401 Use `list.extend` with an async comprehension to create a transformed list"
            my_list.append(x)


async def test_incorrect_ruff_suggestion():
    my_list: list[int] = []
    async for inner_aiter in my_nested_aiter():
        # TypeError: 'async_generator' object is not iterable
        my_list.extend(x async for x in inner_aiter)


async def test_bad_work_around():
    my_list: list[int] = []
    async for inner_aiter in my_nested_aiter():
        # Works but requires unnecessary temporary lists for each iteration, just to get Ruff happy
        my_list.extend([x async for x in inner_aiter])

```

version:
```
% uv run ruff --version
ruff 0.9.3
```


---

_Comment by @MarkusSintonen on 2025-02-22 12:57_

Related https://github.com/astral-sh/ruff/issues/11316#issuecomment-2114185025 but the original issue describes yet another issue with the rule as list extend does not work with async iterators  

---

_Comment by @InSyncWithFoo on 2025-02-22 17:43_

The intended fix is probably this:

```python
my_list = [
	x
	async for inner_aiter in my_nested_aiter()
	async for x in inner_aiter
]
```



---

_Comment by @ntBre on 2025-02-22 17:44_

Thanks for opening the issue! I think there's a lot to consider here.

Just to clarify, when I run 

```shell
cat <<'EOF' | ruff check --select PERF401 -
from collections.abc import AsyncIterator


async def my_nested_aiter() -> AsyncIterator[AsyncIterator[int]]:
    async def inner() -> AsyncIterator[int]:
        yield 1

    yield inner()


async def test_ruff_issue():
    my_list: list[int] = []
    async for inner_aiter in my_nested_aiter():
        async for x in inner_aiter:
            # Fails in Ruff with. "PERF401 Use `list.extend` with an async comprehension to create a transformed list"
            my_list.append(x)
EOF
```

I get the output

```
-:16:13: PERF401 Use `list.extend` with an async comprehension to create a transformed list
   |
14 |         async for x in inner_aiter:
15 |             # Fails in Ruff with. "PERF401 Use `list.extend` with an async comprehension to create a transformed list"
16 |             my_list.append(x)
   |             ^^^^^^^^^^^^^^^^^ PERF401
   |
   = help: Replace for loop with list.extend

Found 1 error.
```

which I would agree could be a bit misleading. 

If I add the `--preview` flag, this enables an unsafe fix for the rule, which I can then enable with`--unsafe-fixes` (I also throw in `--diff` to avoid changing any files):

```shell
ruff check --select PERF401 --preview --unsafe-fixes --diff
```

This produces the diff:

```diff
@@ -11,6 +11,5 @@
 async def test_ruff_issue():
     my_list: list[int] = []
     async for inner_aiter in my_nested_aiter():
-        async for x in inner_aiter:
-            # Fails in Ruff with. "PERF401 Use `list.extend` with an async comprehension to create a transformed list"
-            my_list.append(x)
+        # Fails in Ruff with. "PERF401 Use `list.extend` with an async comprehension to create a transformed list"
+        my_list.extend([x async for x in inner_aiter])

Would fix 1 error.
```

So I think ruff is actually giving a correct suggestion here, at least in the sense that the code still works. However, I agree that the help message, combined with the non-async `list.extend` example in the rule documentation, could easily lead you to the `list.extend(x async ...)` solution.

I also assume, like you and others in the linked issue, that creating a temporary list just to use `extend` is probably a net negative, so I'm a bit skeptical that this should apply to `async` iterators at all, at least in the `extend` case.

Finally, in this *specific* case, but possibly not in related real-world examples, a better fix would possibly be a nested list comprehension like this:

```python
async def test_ruff_issue():
    return [x async for inner_aiter in my_nested_aiter() async for x in inner_aiter]
```

I think that would be pretty hard to detect and suggest in the rule, though.

In summary, I think we should do some combination of (1) update the rule documentation with an `async` example including the caveats with `extend`, (2) update the help message, possibly with the suggested fix, (3) avoid applying the rule to `async` iterators when `extend` is needed.

---

_Label `documentation` added by @ntBre on 2025-02-22 17:44_

---

_Label `rule` added by @ntBre on 2025-02-22 17:44_

---

_Comment by @MarkusSintonen on 2025-02-22 18:18_

> The intended fix is probably this:
> 
> my_list = [
> 	x
> 	async for inner_aiter in my_nested_aiter()
> 	async for x in inner_aiter
> ]

I dont see how this would be any better. The nested comprehensions can get really spaghetti. The example I gave is also a very basic example. Also this suggestion has nothing to do with "PERF" rules performance-aspects.

> So I think ruff is actually giving a correct suggestion here

Yes it is correct but is it really performant as the "PERF" rules try to do, I would bet it is not. So the usage of temporary lists is really not a great workaround and is not inline with "PERF" rules. As it is not performance net positive.

> I also assume, like you and others in the linked issue, that creating a temporary list just to use extend is probably a net negative, so I'm a bit skeptical that this should apply to async iterators at all, at least in the extend case.

I would say the PERF401 should not be applied to async iterators at all whenever it tries to suggest usage of `list.extend`. Python just does not support `extend` with async iterators. Rule is not applicable in the first place here. None of the suggestions are performance related (ugly nested comprehensions), or performance net positive (temp lists).

Otherwise the rule is great so would not want to disable it. But too bad it can break so badly with async iters.

---

_Comment by @Tishka17 on 2025-11-10 18:35_

It is not also not optimal for normal generators:
```python
def foo():
    a = []
    for x in range(100):
        if x % 2 == 0:
            a.append(x)
def bar():
    a = []
    a.extend(
        x for x in range(100) if x % 2 == 0
    )

n= 1000000
print(timeit.timeit("foo()", globals=globals(), number=n))
print(timeit.timeit("bar()", globals=globals(), number=n))
```

output:
1.9236854760092683
2.2944130940013565

I see the same on python 3.12 and python 3.14
generators are slower than normal for loop

---

_Referenced in [astral-sh/ruff#21891](../../astral-sh/ruff/issues/21891.md) on 2025-12-10 16:21_

---
