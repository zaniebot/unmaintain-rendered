---
number: 1135
title: "error[unresolved-attribute] Type `Dropdown` has no attribute `id`"
type: issue
state: closed
author: janosh
labels: []
assignees: []
created_at: 2025-09-05T14:29:42Z
updated_at: 2026-01-09T02:39:02Z
url: https://github.com/astral-sh/ty/issues/1135
synced_at: 2026-01-10T01:48:23Z
---

# error[unresolved-attribute] Type `Dropdown` has no attribute `id`

---

_Issue opened by @janosh on 2025-09-05 14:29_

### Summary

```py
import dash

dropdown = dash.dcc.Dropdown(
    id="dropdown",
    options=list(range(5))
)

dropdown.id  # error[unresolved-attribute] Type `Dropdown` has no attribute `id`
```

### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @carljm on 2025-09-05 17:05_

Dash seems to auto-generate the Python classes for these components. The `__init__` method for `Dropdown` looks like this:

```py
    def __init__(
        self,
        options: typing.Optional[
            typing.Union[
                typing.Sequence[typing.Union[str, NumberType, bool]],
                dict,
                typing.Sequence["Options"],
            ]
        ] = None,
        value: typing.Optional[
            typing.Union[
                str,
                NumberType,
                bool,
                typing.Sequence[typing.Union[str, NumberType, bool]],
            ]
        ] = None,
        multi: typing.Optional[bool] = None,
        clearable: typing.Optional[bool] = None,
        searchable: typing.Optional[bool] = None,
        search_value: typing.Optional[str] = None,
        placeholder: typing.Optional[str] = None,
        disabled: typing.Optional[bool] = None,
        closeOnSelect: typing.Optional[bool] = None,
        optionHeight: typing.Optional[NumberType] = None,
        maxHeight: typing.Optional[NumberType] = None,
        style: typing.Optional[typing.Any] = None,
        className: typing.Optional[str] = None,
        id: typing.Optional[typing.Union[str, dict]] = None,
        persistence: typing.Optional[typing.Union[bool, str, NumberType]] = None,
        persisted_props: typing.Optional[typing.Sequence[Literal["value"]]] = None,
        persistence_type: typing.Optional[Literal["local", "session", "memory"]] = None,
        **kwargs
    ):
        self._prop_names = [
            "options",
            "value",
            "multi",
            "clearable",
            "searchable",
            "search_value",
            "placeholder",
            "disabled",
            "closeOnSelect",
            "optionHeight",
            "maxHeight",
            "style",
            "className",
            "id",
            "persistence",
            "persisted_props",
            "persistence_type",
        ]
        self._valid_wildcard_attributes = []
        self.available_properties = [
            "options",
            "value",
            "multi",
            "clearable",
            "searchable",
            "search_value",
            "placeholder",
            "disabled",
            "closeOnSelect",
            "optionHeight",
            "maxHeight",
            "style",
            "className",
            "id",
            "persistence",
            "persisted_props",
            "persistence_type",
        ]
        self.available_wildcard_properties = []
        _explicit_args = kwargs.pop("_explicit_args")
        _locals = locals()
        _locals.update(kwargs)  # For wildcard attrs and excess named props
        args = {k: _locals[k] for k in _explicit_args}

        super(Dropdown, self).__init__(**args)
```

There's no `id` attribute annotated on the class itself, either.

This is using highly-dynamic code to set attributes on the instance at runtime; I don't think there is any reasonable way for a type checker to understand this. Neither pyright nor mypy seem to understand it either.

It seems like since these are generated anyway, it wouldn't be hard for dash to also generate a `id: typing.Optional[typing.Union[str, dict]]` annotation on the class body (not just in the `__init__` signature) -- that would resolve the problem.

It does seem like we might need a better way for users to handle untyped libraries that will generate false positives.


---

_Comment by @carljm on 2026-01-09 02:39_

> It does seem like we might need a better way for users to handle untyped libraries that will generate false positives.

This is the actionable thing here, and I think it is covered by https://github.com/astral-sh/ty/issues/2082

---

_Closed by @carljm on 2026-01-09 02:39_

---
