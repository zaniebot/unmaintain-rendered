```yaml
number: 1879
title: "Rust Python AST makes identifying `**` difficult"
type: issue
state: closed
author: sbdchd
labels: []
assignees: []
created_at: 2023-01-15T01:43:48Z
updated_at: 2023-01-23T02:32:59Z
url: https://github.com/astral-sh/ruff/issues/1879
synced_at: 2026-01-10T11:09:44Z
```

# Rust Python AST makes identifying `**` difficult

---

_Issue opened by @sbdchd on 2023-01-15 01:43_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->


I'm not sure there is much to do, but it seems tricky to identify spreads `**` in the Rust Python AST

With the Python AST `{"foo": 1, **{"bar": 1}}` would be represented with a `Dict` node with one key and two values, so detecting a spread is pretty straightforward.

With the Rust Python AST it seems like the only difference is the location information (which can also be affected by whitespace)

For example:

```python
{**foo,    "bar": True  }
```

<details>
<summary>ast</summary>

```
[
    Located {
        location: Location {
            row: 1,
            column: 0,
        },
        end_location: Some(
            Location {
                row: 1,
                column: 25,
            },
        ),
        custom: (),
        node: Expr {
            value: Located {
                location: Location {
                    row: 1,
                    column: 0,
                },
                end_location: Some(
                    Location {
                        row: 1,
                        column: 25,
                    },
                ),
                custom: (),
                node: Dict {
                    keys: [],
                    values: [
                        Located {
                            location: Location {
                                row: 1,
                                column: 3,
                            },
                            end_location: Some(
                                Location {
                                    row: 1,
                                    column: 6,
                                },
                            ),
                            custom: (),
                            node: Name {
                                id: "foo",
                                ctx: Load,
                            },
                        },
                        Located {
                            location: Location {
                                row: 1,
                                column: 11,
                            },
                            end_location: Some(
                                Location {
                                    row: 1,
                                    column: 16,
                                },
                            ),
                            custom: (),
                            node: Dict {
                                keys: [
                                    Located {
                                        location: Location {
                                            row: 1,
                                            column: 11,
                                        },
                                        end_location: Some(
                                            Location {
                                                row: 1,
                                                column: 16,
                                            },
                                        ),
                                        custom: (),
                                        node: Constant {
                                            value: Str(
                                                "bar",
                                            ),
                                            kind: None,
                                        },
                                    },
                                ],
                                values: [
                                    Located {
                                        location: Location {
                                            row: 1,
                                            column: 18,
                                        },
                                        end_location: Some(
                                            Location {
                                                row: 1,
                                                column: 22,
                                            },
                                        ),
                                        custom: (),
                                        node: Constant {
                                            value: Bool(
                                                true,
                                            ),
                                            kind: None,
                                        },
                                    },
                                ],
                            },
                        },
                    ],
                },
            },
        },
    },
]

```

</details>

and 

```python
{**foo, **{"bar": True }}
```
<details>
<summary>ast</summary>

```
[
    Located {
        location: Location {
            row: 1,
            column: 0,
        },
        end_location: Some(
            Location {
                row: 1,
                column: 26,
            },
        ),
        custom: (),
        node: Expr {
            value: Located {
                location: Location {
                    row: 1,
                    column: 0,
                },
                end_location: Some(
                    Location {
                        row: 1,
                        column: 26,
                    },
                ),
                custom: (),
                node: Dict {
                    keys: [],
                    values: [
                        Located {
                            location: Location {
                                row: 1,
                                column: 3,
                            },
                            end_location: Some(
                                Location {
                                    row: 1,
                                    column: 6,
                                },
                            ),
                            custom: (),
                            node: Name {
                                id: "foo",
                                ctx: Load,
                            },
                        },
                        Located {
                            location: Location {
                                row: 1,
                                column: 11,
                            },
                            end_location: Some(
                                Location {
                                    row: 1,
                                    column: 25,
                                },
                            ),
                            custom: (),
                            node: Dict {
                                keys: [
                                    Located {
                                        location: Location {
                                            row: 1,
                                            column: 12,
                                        },
                                        end_location: Some(
                                            Location {
                                                row: 1,
                                                column: 17,
                                            },
                                        ),
                                        custom: (),
                                        node: Constant {
                                            value: Str(
                                                "bar",
                                            ),
                                            kind: None,
                                        },
                                    },
                                ],
                                values: [
                                    Located {
                                        location: Location {
                                            row: 1,
                                            column: 19,
                                        },
                                        end_location: Some(
                                            Location {
                                                row: 1,
                                                column: 23,
                                            },
                                        ),
                                        custom: (),
                                        node: Constant {
                                            value: Bool(
                                                true,
                                            ),
                                            kind: None,
                                        },
                                    },
                                ],
                            },
                        },
                    ],
                },
            },
        },
    },
]
```

</details>

anyways, curious if you've run into this before, I'm still trying to work out a heuristic for `no-unnecessary-spread` which was pretty straightforward with the python ast:

```python
def pie800_no_unnecessary_spread(node: ast.Dict, errors: list[Error]) -> None:
    for key, val in zip(node.keys, node.values):
        if isinstance(val, ast.Dict) and key is None:
            errors.append(PIE800(lineno=val.lineno, col_offset=val.col_offset))
```

---

_Comment by @charliermarsh on 2023-01-15 02:17_

We could fix it upstream! We submit lots of patches to RustPython to improve compatibility. Maybe @harupy would have pointers in this case.

---

_Comment by @harupy on 2023-01-15 03:20_

> With the Python AST {"foo": 1, **{"bar": 1}} would be represented with a Dict node with one key and two values

`{"foo": 1, **{"bar": 1}}` is parsed as follows:

### CPython:

```
Module(
  body=[
    Expr(
      value=Dict(
        👇👇👇 Contains two keys, but the second key is None
        keys=[
          Constant(
            value='foo',
            lineno=1,
            col_offset=1,
            end_lineno=1,
            end_col_offset=6),
          None],
        values=[
          Constant(
            value=1,
            lineno=1,
            col_offset=8,
            end_lineno=1,
            end_col_offset=9),
          Dict(
            keys=[
              Constant(
                value='bar',
                lineno=1,
                col_offset=14,
                end_lineno=1,
                end_col_offset=19)],
            values=[
              Constant(
                value=1,
                lineno=1,
                col_offset=21,
                end_lineno=1,
                end_col_offset=22)],
            lineno=1,
            col_offset=13,
            end_lineno=1,
            end_col_offset=23)],
        lineno=1,
        col_offset=0,
        end_lineno=1,
        end_col_offset=24),
      lineno=1,
      col_offset=0,
      end_lineno=1,
      end_col_offset=24)],
  type_ignores=[])
```

### RustPython

```
[
    Located {
        location: Location {
            row: 1,
            column: 0,
        },
        end_location: Some(
            Location {
                row: 1,
                column: 24,
            },
        ),
        custom: (),
        node: Expr {
            value: Located {
                location: Location {
                    row: 1,
                    column: 0,
                },
                end_location: Some(
                    Location {
                        row: 1,
                        column: 24,
                    },
                ),
                custom: (),
                node: Dict {
                    👇👇👇 Contains one key
                    keys: [
                        Located {
                            location: Location {
                                row: 1,
                                column: 1,
                            },
                            end_location: Some(
                                Location {
                                    row: 1,
                                    column: 6,
                                },
                            ),
                            custom: (),
                            node: Constant {
                                value: Str(
                                    "foo",
                                ),
                                kind: None,
                            },
                        },
                    ],
                    👇👇👇 values look correct?
                    values: [
                        Located {
                            location: Location {
                                row: 1,
                                column: 8,
                            },
                            end_location: Some(
                                Location {
                                    row: 1,
                                    column: 9,
                                },
                            ),
                            custom: (),
                            node: Constant {
                                value: Int(
                                    1,
                                ),
                                kind: None,
                            },
                        },
                        Located {
                            location: Location {
                                row: 1,
                                column: 13,
                            },
                            end_location: Some(
                                Location {
                                    row: 1,
                                    column: 23,
                                },
                            ),
                            custom: (),
                            node: Dict {
                                keys: [
                                    Located {
                                        location: Location {
                                            row: 1,
                                            column: 14,
                                        },
                                        end_location: Some(
                                            Location {
                                                row: 1,
                                                column: 19,
                                            },
                                        ),
                                        custom: (),
                                        node: Constant {
                                            value: Str(
                                                "bar",
                                            ),
                                            kind: None,
                                        },
                                    },
                                ],
                                values: [
                                    Located {
                                        location: Location {
                                            row: 1,
                                            column: 21,
                                        },
                                        end_location: Some(
                                            Location {
                                                row: 1,
                                                column: 22,
                                            },
                                        ),
                                        custom: (),
                                        node: Constant {
                                            value: Int(
                                                1,
                                            ),
                                            kind: None,
                                        },
                                    },
                                ],
                            },
                        },
                    ],
                },
            },
        },
    },
]
```

---

_Comment by @charliermarsh on 2023-01-23 02:32_

Fixed upstream by @harupy!

---

_Closed by @charliermarsh on 2023-01-23 02:32_

---
