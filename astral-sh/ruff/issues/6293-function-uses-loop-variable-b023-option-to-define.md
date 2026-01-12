```yaml
number: 6293
title: "`function-uses-loop-variable` (`B023`) - option to define functions where lambda is executed immediately"
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2023-08-03T01:14:07Z
updated_at: 2023-09-12T22:39:34Z
url: https://github.com/astral-sh/ruff/issues/6293
synced_at: 2026-01-12T15:54:46Z
```

# `function-uses-loop-variable` (`B023`) - option to define functions where lambda is executed immediately

---

_@DetachHead_

in my codebase there's a `find` function where the lambda is executed immediately which means it's safe to use the loop variable this way, so i have to suppress this rule on all of its usages:
```py
foo = [1, 2, 3]
for bar in foo:
    find([1, 2, 3], lambda value: value == bar)  # noqa: B023
```
it would be nice if there was an option to specify functions where the lambda is executed immediately, like with `extend-immutable-calls`:
```toml
[tool.ruff.flake8-bugbear]
lambdas-executed-immediately = ["find"] # perhaps also a way of specifying which argument takes the lambda
```

---

_Comment by @KotlinIsland on 2023-08-03 01:17_

- Related: https://github.com/python/typing/issues/1070

---

_Comment by @charliermarsh on 2023-08-04 02:16_

I understand the use-case, and it'd be nice to have a way to express this in the type system that we could leverage in Ruff, but I think this is too niche to justify a dedicated setting at present. Every setting adds some additional cognitive burden for users and has an implementation + ongoing maintenance cost, so it needs to meet some threshold for general usefulness and applicability. If we see this need arise again in the future, we can always reconsider.

---

_Closed by @charliermarsh on 2023-08-04 02:16_

---

_Comment by @max-sixty on 2023-09-12 22:39_

Forgive necro-ing this post, but this does seem overly strict. 

Rather than adding another option, could we just allow this usage when the lambda doesn't escape the loop?

It's particularly prevalent in pandas / xarray, when using a lambda as part of a pipe:

```python
for i in is:
    pd.DataFrame(foo).pipe(lambda x: x.where($someuseofi)
```

---
