```yaml
number: 17460
title: "[syntax-errors] Make `async-comprehension-in-sync-comprehension` more specific"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: brent/fix-async-comprehension-in-sync-comprehension
created_at: 2025-04-18T14:08:56Z
updated_at: 2025-04-24T19:45:56Z
url: https://github.com/astral-sh/ruff/pull/17460
synced_at: 2026-01-12T15:56:02Z
```

# [syntax-errors] Make `async-comprehension-in-sync-comprehension` more specific

---

_@ntBre_

## Summary

While adding semantic error support to red-knot, I noticed duplicate diagnostics for code like this:

```py
# error: [invalid-syntax] "cannot use an asynchronous comprehension outside of an asynchronous function on Python 3.9 (syntax was added in 3.11)"
# error: [invalid-syntax] "`asynchronous comprehension` outside of an asynchronous function"
 [reveal_type(x) async for x in AsyncIterable()]
```

Beyond the duplication, the first error message doesn't make much sense because this syntax is _not_ allowed on Python 3.11 either.

To fix this, this PR renames the `async-comprehension-outside-async-function` semantic syntax error to `async-comprehension-in-sync-comprehension` and fixes the rule to avoid applying outside of sync comprehensions at all. 

## Test Plan

New linter test demonstrating the false positive. The mdtests I'm updating in my red-knot PR will also reflect this change eventually.


---

_Label `bug` added by @ntBre on 2025-04-18 14:08_

---

_Comment by @github-actions[bot] on 2025-04-18 14:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @ntBre on 2025-04-18 14:23_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-18 14:23_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-18 14:23_

---

_Comment by @dhruvmanila on 2025-04-21 19:02_

I think the error message is a bit misleading. It suggests that an asynchronous comprehension can be used in a synchronous comprehension from Python 3.11 but that's not actually true.

```py
[[x async for x in foo(n)] for n in range(3)]
```

The following raises error for both <3.11 and 3.11+

The user actually need to include it in an async function for it to be considered valid:

```py
async def _():
	[[x async for x in foo(n)] for n in range(3)]
```

I'm running this via `python -m compileall`.

I might be misunderstanding what's being changed here so feel free to correct me.

---

_Comment by @ntBre on 2025-04-21 19:27_

I see what you mean, but that case will lead to a separate `AwaitOutsideAsyncFunction` error, as shown in the PR summary. Do you think the error message for this variant also needs to be extended to mention the function, or will the second error clarify things enough?

If it needs to be extended, do you have any suggestions? I was afraid the message was quite long already.

---

_Comment by @dhruvmanila on 2025-04-22 14:47_

> ```python
> # error: [invalid-syntax] "`asynchronous comprehension` outside of an asynchronous function"
> ```

Is this the error message that's removed in this PR?

---

_@dhruvmanila approved on 2025-04-22 14:50_

Testing this out locally for a few snippets, it seems good but I think we can still improve this. I'm not sure how but multi-span diagnostics is one way to go to provide additional context.

I don't have a good suggestion for improving the error message, maybe something like:
```
SyntaxError: cannot use an asynchronous comprehension inside of a synchronous scope created by the outer comprehension on Python 3.9 (syntax was added in 3.11)
```
Maybe @AlexWaygood has a better recommendation?

---

_Comment by @ntBre on 2025-04-22 14:59_

No, the top one (`cannot use an asynchronous comprehension outside of an asynchronous function on Python 3.9 (syntax was added in 3.11)`) is removed in this PR. The second message (`asynchronous comprehension outside of an asynchronous function`) is a separate `SemanticSyntaxError` entirely and will still be emitted in this example.

I don't think I explained this very well in the PR summary, looking back at it now. I'm doing two unrelated things in this PR:

- removing the false positive for `async` comprehensions _not_ in sync comprehensions (a bug in the implementation)
- updating the naming of the variant and error message to better reflect the error

I only added a new test for the first of these, which probably made it even less clear what was changing in the code. Sorry!

---

_Review requested from @carljm by @ntBre on 2025-04-24 16:23_

---

_Review requested from @AlexWaygood by @ntBre on 2025-04-24 16:23_

---

_Review requested from @sharkdp by @ntBre on 2025-04-24 16:23_

---

_Review requested from @dcreager by @ntBre on 2025-04-24 16:23_

---

_Comment by @ntBre on 2025-04-24 16:24_

I'm resolving the merge conflicts now. Just as one additional data point, the CPython error message for this is also quite poor:

```pycon
>>> async def f(): [[x async for x in []] for x in []]
...
  File "<stdin>", line 1
SyntaxError: asynchronous comprehension outside of an asynchronous function
```

That's not an excuse for ours to be bad too, but I do think 

```
SyntaxError: cannot use an asynchronous comprehension inside of a synchronous comprehension
```

is already a pretty good improvement over the current message in ruff and over Python's message. 

I personally prefer this to the suggestion above too, but I don't feel strongly about it. If nobody else feels strongly, I'll just merge this for now, and we can tweak the message later.

---

_Comment by @github-actions[bot] on 2025-04-24 16:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @ntBre on 2025-04-24 19:45_

---

_Closed by @ntBre on 2025-04-24 19:45_

---

_Branch deleted on 2025-04-24 19:45_

---
