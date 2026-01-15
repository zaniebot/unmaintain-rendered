```yaml
number: 2504
title: "ty seems to not understand that the decorator creates a dataclass and says can't find __init__"
type: issue
state: open
author: pumetu
labels: []
assignees: []
created_at: 2026-01-15T02:27:38Z
updated_at: 2026-01-15T02:27:38Z
url: https://github.com/astral-sh/ty/issues/2504
synced_at: 2026-01-15T02:41:01Z
```

# ty seems to not understand that the decorator creates a dataclass and says can't find __init__

---

_@pumetu_

### Summary

I have the following code snippet where it accepts a class and returns a dataclass, however, when I use the function as a decorator `ty` says there's an error as it tries to look for the `__init__` function. If i add the `@dataclasses.dataclass` decorator the error is gone. I'm not sure if this is a bug or is it the expected behavior.

```python
def pytree_dataclass(cls, meta_fields: tuple = ()):
    cls = dataclass(cls)
    all_fields = tuple(f.name for f in fields(cls) if f.init)
    data_fields = tuple(f for f in all_fields if f not in meta_fields)
    return jax.tree_util.register_dataclass(cls, data_fields=data_fields, meta_fields=meta_fields)

@partial(pytree_dataclass, meta_fields=("shape", "dtype", "initializer"))
@dataclass(frozen=True)
class Spec:
    shape: tuple[int, ...]
    dtype: jnp.dtype
    initializer: Callable | None = None

@pytree_dataclass
class Linear(Module):
    weight: jax.Array | Spec
    bias: jax.Array | Spec | None

    @classmethod
    def spec(cls, in_features: int, out_features: int, dtype: jnp.dtype, bias: bool = False):
        _init = jax.nn.initializers.he_normal()
        return cls(
            weight=Spec(shape=(in_features, out_features), dtype=dtype, initializer=_init),
            bias=Spec(shape(1, out_features), dtype=dtype, initializer=_init) if bias else None,
        )
```
The error message
```
Diagnostics:
1. ty: Argument `weight` does not match any known parameter of bound method `__init__` [unknown-argument]
```

### Version

ty 0.0.11

---
