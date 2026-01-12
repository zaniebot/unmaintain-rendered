```yaml
number: 129
title: "`F401` when type used in annotation"
type: issue
state: closed
author: samuelcolvin
labels: []
assignees: []
created_at: 2022-09-08T09:13:52Z
updated_at: 2023-09-08T11:46:15Z
url: https://github.com/astral-sh/ruff/issues/129
synced_at: 2026-01-12T15:54:40Z
```

# `F401` when type used in annotation

---

_@samuelcolvin_

Kind of the opposite to #128.

E.g. `NamedTuple` is imported [here](https://github.com/pydantic/pydantic/blob/e92f12efacb3fe1ef9577f712e3bdafc2427b5ec/pydantic/annotated_types.py#L2), and used [here](https://github.com/pydantic/pydantic/blob/e92f12efacb3fe1ef9577f712e3bdafc2427b5ec/pydantic/annotated_types.py#L58).

But shows up in errors:

```
pydantic/annotated_types.py:2:1: F401 `typing.NamedTuple` imported but unused
```

There are currently 30 of these errors from running ruff on pydantic.

As with #128, I've build ruff from `main` (a8f4faa6e41b227d36d5428246706fb68e8b0ec5).

---

_Closed by @charliermarsh on 2022-09-08 15:07_

---

_Comment by @gempir on 2023-09-08 09:43_

Is this actually fixed? 

I use a type annotation here:

```
for product_certificate in qa_product.product_certificates:  # type: ProductCertificate
```

And ruff just removes my import for ProductCertificate
I need to specific # noqa: F401 on the import line to avoid it.

---

_Comment by @charliermarsh on 2023-09-08 11:46_

@gempir - I believe the issue in that case is that Ruff does not parse type comments. See: https://github.com/astral-sh/ruff/issues/1619

---
