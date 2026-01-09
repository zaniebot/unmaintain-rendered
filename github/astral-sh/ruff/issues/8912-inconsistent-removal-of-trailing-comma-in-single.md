---
number: 8912
title: Inconsistent removal of trailing comma in single-argument functions
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-11-29T21:26:15Z
updated_at: 2023-12-01T02:49:30Z
url: https://github.com/astral-sh/ruff/issues/8912
synced_at: 2026-01-07T13:12:15-06:00
---

# Inconsistent removal of trailing comma in single-argument functions

---

_Issue opened by @charliermarsh on 2023-11-29 21:26_

See: https://play.ruff.rs/96810c27-3971-44af-8eb5-5b5ebae1668e. We remove the trailing comma in the first case, but not in the second, when the trailing comment is present.

Note that this only happens with `"skip-magic-trailing-comma": true`. If you set `"skip-magic-trailing-comma": false` in the example above, we consistently insert the trailing comma.

Originally reported here: https://github.com/astral-sh/ruff/issues/1200#issuecomment-1832718967.

---

_Label `bug` added by @charliermarsh on 2023-11-29 21:26_

---

_Label `formatter` added by @charliermarsh on 2023-11-29 21:26_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-29 23:41_

---

_Comment by @charliermarsh on 2023-11-30 02:21_

Another way to look at it: https://play.ruff.rs/5bf54dc0-235e-4cee-9902-90eff0a19150

Here, we don't insert a trailing comma, when we _should_.

---

_Comment by @charliermarsh on 2023-11-30 02:24_

This seems tricky to fix because we need to know if the _parent_ group breaks.

---

_Comment by @MichaReiser on 2023-11-30 02:34_

> This seems tricky to fix because we need to know if the _parent_ group breaks.

Can you explain in detail or share some IR. You might be able to assign a group id to the parent group and use `if_group_breaks(parent_group_id)` 

This is how it is implemented in biome

The array creates a group:

https://github.com/MichaReiser/biome/blob/59acbfd1e6458b13b5b44d4c5c33ead6a428435e/crates/biome_js_formatter/src/js/expressions/array_expression.rs#L45-L48

and we use it when formatting the trailing separator here

https://github.com/MichaReiser/biome/blob/59acbfd1e6458b13b5b44d4c5c33ead6a428435e/crates/biome_js_formatter/src/js/lists/array_element_list.rs#L42-L44

which ultimately resolves here

https://github.com/MichaReiser/biome/blob/63e4c40051ec15563cc586890c1c350b8c7b948e/crates/biome_formatter/src/separated.rs#L80-L82

But it depends on how the groups are structured in detail. But I thought it might help


---

_Comment by @MichaReiser on 2023-11-30 04:19_

This could also be solved by associating the comments with the arguments. Because it would allow you to move the comment out of the inner group. 

~~Using the parent group seems incorrect to me (which, breaks as well because the child group is breaking) because we want to have a trailing comma for~~

```python
func(
	a, b, c,
): pass
``

---

_Comment by @charliermarsh on 2023-11-30 04:20_

I don't think the comment is relevant. This, for example, needs a trailing comma:

```python
def _example_function_xxxxxxx(
    variable: Optional[List[str]]
) -> List[example.ExampleConfig]:
    pass
```

---

_Comment by @MichaReiser on 2023-11-30 04:22_

Hmm, but only if it is a single argument function. Could we avoid the inner group if the function only has a single argument? It seems unnecessary to emit two groups in that case.

---

_Comment by @charliermarsh on 2023-11-30 04:23_

Maybe? I can try.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-30 04:25_

---

_Referenced in [astral-sh/ruff#8921](../../astral-sh/ruff/pulls/8921.md) on 2023-11-30 04:29_

---

_Closed by @charliermarsh on 2023-12-01 02:49_

---
