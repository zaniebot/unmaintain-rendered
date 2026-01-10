---
number: 5502
title: C417 auto-fixer breaks code with late-binding
type: issue
state: closed
author: mrcljx
labels:
  - bug
assignees: []
created_at: 2023-07-04T13:55:38Z
updated_at: 2023-07-05T02:11:30Z
url: https://github.com/astral-sh/ruff/issues/5502
synced_at: 2026-01-10T01:22:44Z
---

# C417 auto-fixer breaks code with late-binding

---

_Issue opened by @mrcljx on 2023-07-04 13:55_

Version: ruff 0.0.276 (no regression though)

```diff
  nums = range(4)

- callbacks = map(lambda x: lambda: x, nums)
+ callbacks = (lambda: x for x in nums)
```

Now `list(callbacks)[0]()` will change it's result:

```diff
- 0
+ 3
```

Additionally, a new error appears (pointing out the issue):

> B023 Function definition does not bind loop variable `x`

My expectation is that the either the rule doesn't trigger in this case, or at least the auto-fixer doesn't break code.

---

_Renamed from "C417 auto-fixer breaks with late-binding" to "C417 auto-fixer breaks code with late-binding" by @mrcljx on 2023-07-04 13:55_

---

_Label `bug` added by @charliermarsh on 2023-07-04 21:59_

---

_Comment by @charliermarsh on 2023-07-04 22:00_

Is this the "real" code that triggered the violation? It'd be helpful to have some more examples to inform the fix.

---

_Comment by @mrcljx on 2023-07-04 22:26_

A real (but harder to grok) code example is this:

```py
infos = map(
    lambda aspect: ColumnInfo(
        aspect.value,
        ma.fields.Enum(models.Resolution, required=True, by_value=True),
        AttributeImpl(
            lambda d, ctx: d.aspect_resolutions(aspect).effective(
                ctx.aspects.device_resolution_quota(aspect).fallback
            ),
            display_name="",
        ),
    ),
    models.Aspect,
)
```

which got transformed to

```py
infos = (
    ColumnInfo(
        aspect.value,
        ma.fields.Enum(models.Resolution, required=True, by_value=True),
        AttributeImpl(
            lambda d, ctx: d.aspect_resolutions(aspect).effective(
                ctx.aspects.device_resolution_quota(aspect).fallback
            ),
            display_name="",
        ),
    )
    for aspect in models.Aspect
)
```

for which then the following problem was caught:

> B023 Function definition does not bind loop variable `aspect`

---

The problem effectively happens if the iterator variable is used inside a `lambda`, so a some logic like the following would would be needed:

```
for lambda_node in find_lambda_nodes(map_arg_0_lambda_node):
  if loop_variable_name in find_all_names(lambda_node):
    # loop variable is late-bound to a lambda, using comprehension would introduce bugs, bail
    return
```

---

_Comment by @charliermarsh on 2023-07-04 22:27_

Thanks, yeah, this makes sense.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-05 01:17_

---

_Referenced in [astral-sh/ruff#5520](../../astral-sh/ruff/pulls/5520.md) on 2023-07-05 01:46_

---

_Closed by @charliermarsh on 2023-07-05 02:11_

---
