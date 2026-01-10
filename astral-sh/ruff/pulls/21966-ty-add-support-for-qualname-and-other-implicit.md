```yaml
number: 21966
title: "[ty] Add support for `__qualname__` and other implicit class attributes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/implicit-class
created_at: 2025-12-13T19:32:44Z
updated_at: 2025-12-13T22:10:43Z
url: https://github.com/astral-sh/ruff/pull/21966
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Add support for `__qualname__` and other implicit class attributes

---

_Pull request opened by @charliermarsh on 2025-12-13 19:32_

## Summary

Closes https://github.com/astral-sh/ty/issues/1873


---

_Comment by @astral-sh-bot[bot] on 2025-12-13 19:34_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-13 19:36_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Renamed from "Add support for `__qualname__` and other implicit class attributes" to "[ty] Add support for `__qualname__` and other implicit class attributes" by @charliermarsh on 2025-12-13 19:42_

---

_Label `ty` added by @charliermarsh on 2025-12-13 19:42_

---

_Marked ready for review by @charliermarsh on 2025-12-13 20:05_

---

_Review requested from @carljm by @charliermarsh on 2025-12-13 20:05_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-13 20:05_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-13 20:05_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-13 20:05_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:1658 on 2025-12-13 20:56_

huh? I don't understand this comment :-)

```pycon
% uvx python3.10    
Python 3.10.19 (main, Oct 10 2025, 12:33:41) [Clang 20.1.4 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> class Foo:
...     print(__firstlineno__)
... 
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in Foo
NameError: name '__firstlineno__' is not defined
```

---

_@AlexWaygood reviewed on 2025-12-13 20:56_

---

_@charliermarsh reviewed on 2025-12-13 21:07_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/place.rs`:1658 on 2025-12-13 21:07_

Ugh sorry, that's clearly wrong.

---

_Converted to draft by @charliermarsh on 2025-12-13 21:09_

---

_@AlexWaygood reviewed on 2025-12-13 21:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:1658 on 2025-12-13 21:10_

this should do it:

```suggestion
        "__firstlineno__" if Program::get(db).python_version(db) >= PythonVersion::PY313 => Place::bound(KnownClass::Int.to_instance(db)).into(),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:9189 on 2025-12-13 21:15_

could use let-chains here to reduce the indentation ;)

```suggestion
                    if scope.node(db).scope_kind().is_class()
                        && let Some(symbol) = place_expr.as_symbol()
                    {
                        let implicit = class_body_implicit_symbol(db, symbol.name());
                        if implicit.place.is_definitely_bound() {
                            return implicit.map_type(|ty| {
                                self.narrow_place_with_applicable_constraints(
                                    place_expr,
                                    ty,
                                    &constraint_keys,
                                )
                            });
                        }
                    }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:1645 on 2025-12-13 21:24_

we also have `__doc__`, which is `str | None` (`None` if there's no class docstring):

```pycon
>>> class X:
...     """xy stuff"""
...     print(__doc__)
...     
xy stuff
```

---

_@AlexWaygood approved on 2025-12-13 21:25_

nice

---

_@charliermarsh reviewed on 2025-12-13 21:25_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/place.rs`:1645 on 2025-12-13 21:25_

Desperately trying to find a complete enumeration of these lol.

---

_@AlexWaygood reviewed on 2025-12-13 21:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:1645 on 2025-12-13 21:28_

hah, I just did trial and error on all of the ones in the table at https://docs.python.org/3/reference/datamodel.html#special-attributes to see which ones were available in the class body and which were only added after the class was defined 😆

---

_@AlexWaygood reviewed on 2025-12-13 21:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:1645 on 2025-12-13 21:30_

(`__annotations__` is also available in the class body, but only on Python <3.14, so let's not bother with it right now. It's bad form to access it directly on lower versions; you should use `inspect.get_annotations()`. We can add it in the future if someone actually asks for it...)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:1645 on 2025-12-13 21:30_

and even on Python <3.14, it's only there in the class body if there are also annotations in the class body.

---

_@AlexWaygood reviewed on 2025-12-13 21:30_

---

_Marked ready for review by @charliermarsh on 2025-12-13 22:04_

---

_@charliermarsh reviewed on 2025-12-13 22:07_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/place.rs`:1655 on 2025-12-13 22:07_

I think we could detect if a docstring exists and narrow this to `str` or `None` respectively. Is that worth doing?

---

_Merged by @charliermarsh on 2025-12-13 22:10_

---

_Closed by @charliermarsh on 2025-12-13 22:10_

---

_Branch deleted on 2025-12-13 22:10_

---

_@AlexWaygood reviewed on 2025-12-13 22:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:1655 on 2025-12-13 22:10_

we could do, but I'm not sure it's worth it. I'd vote for keeping it simple for now and adding it if someone asks for it

---
