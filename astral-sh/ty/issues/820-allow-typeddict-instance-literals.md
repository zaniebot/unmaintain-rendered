```yaml
number: 820
title: allow TypedDict instance literals
type: issue
state: closed
author: sslivkoff
labels:
  - typing semantics
assignees: []
created_at: 2025-07-13T03:05:17Z
updated_at: 2025-07-15T11:47:21Z
url: https://github.com/astral-sh/ty/issues/820
synced_at: 2026-01-12T15:54:24Z
```

# allow TypedDict instance literals

---

_@sslivkoff_

would like ty to allow literal instantiation of TypedDict's, as in:

```python
class Config(typing.TypedDict):
    version: str
    tracked_tables: list[TrackedTable]


def get_default_config() -> absorb.Config:
    # doesn't work
    return {
        'version': '0.1.0',
        'tracked_tables': [],
    }
```

it gives this error:

```
error[invalid-return-type]: Return type does not match returned value
  --> absorb/ops/config.py:12:29
   |
12 |   def get_default_config() -> Config:
   |                               ------------- Expected `Config` because of return type
13 |       return {
   |  ____________^
14 | |         'version': '0.1.0',
15 | |         'tracked_tables': [],
16 | |     }
   | |_____^ expected `Config`, found `dict[Unknown, Unknown]`
   |
info: rule `invalid-return-type` is enabled by default
```

instead I had to convert many parts of my codebase to using this style to pass ty's checks:
```python
def get_default_config() -> Config:
    return Config(
        version=absorb.__version__,
        tracked_tables=[],
    )
```

this is undesirable because:
1. one of the advantages of TypedDict's is being able to use them with normal simple dict syntax 
2. mypy (which I usually use) supports this style of TypedDict instance literals
3. the two syntaxes are nearly functionally equivalent

---

_Comment by @sslivkoff on 2025-07-13 03:08_

two areas where this syntax commonly shows up is 1) in the return values of functions as show above and 2) when trying to assign TypedDict's to a variable

this snippet:

```python
class TrackedTable(typing.TypedDict):
    source_name: str
    table_name: str
    table_class: str
    parameters: dict[str, typing.Any]


table_data: TrackedTable = {
    'source_name': source,
    'table_name': table,
    'table_class': 'absorb.datasets.' + source + '.' + camel_table,
    'parameters': {},
}
```

gives this ty error:

```
error[invalid-assignment]: Object of type `dict[Unknown, Unknown]` is not assignable to `TrackedTable`
  --> absorb/ops/coverage.py:41:13
   |
39 |             # TODO: read this information from a metadata.json
40 |             camel_table = absorb.ops.names._snake_to_camel(table)
41 |             table_data: absorb.TrackedTable = {
   |             ^^^^^^^^^^
42 |                 'source_name': source,
43 |                 'table_name': table,
   |
info: rule `invalid-assignment` is enabled by default
```

---

_Label `typing semantics` added by @sharkdp on 2025-07-14 06:44_

---

_Comment by @sharkdp on 2025-07-14 06:47_

Thank you for reporting this. At the moment, we do not support `TypedDict`s at all. This is being tracked in https://github.com/astral-sh/ty/issues/154. There's currently no detailed description in that ticket, but 
assignments of dict literals to `TypedDict`s is certainly a core feature that will be implemented as part of this ticket.

---

_Closed by @sharkdp on 2025-07-14 06:47_

---

_Closed by @AlexWaygood on 2025-07-15 11:47_

---
