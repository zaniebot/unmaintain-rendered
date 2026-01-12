```yaml
number: 2086
title: "Update RustPython to fix `Dict.keys` type"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: update-RustPython
created_at: 2023-01-22T14:31:19Z
updated_at: 2023-01-22T20:21:21Z
url: https://github.com/astral-sh/ruff/pull/2086
synced_at: 2026-01-12T15:55:07Z
```

# Update RustPython to fix `Dict.keys` type

---

_@harupy_

This PR upgrades RustPython to fix the type of `Dict.keys` to `Vec<Option<Expr>>` (see https://github.com/RustPython/RustPython/pull/4449 for why this change was needed) and unblock #1884.

---

_Review comment by @harupy on `src/rules/pyupgrade/rules/convert_typed_dict_functional_to_class.rs`:83 on 2023-01-22 14:32_

```suggestion
```

---

_@harupy reviewed on 2023-01-22 14:32_

---

_@harupy reviewed on 2023-01-22 14:40_

---

_Review comment by @harupy on `src/rules/pyflakes/rules/repeated_keys.rs`:35 on 2023-01-22 14:40_

Ah we can't use flatten here.

---

_Comment by @harupy on 2023-01-22 15:06_

Prior to https://github.com/RustPython/RustPython/pull/4449,

```python
{"a": 0, **b, "c": 1}
```

is parsed as:

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
                column: 21,
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
                        column: 21,
                    },
                ),
                custom: (),
                node: Dict {
                    👇 contains a single key
                    keys: [
                        Located {
                            location: Location {
                                row: 1,
                                column: 1,
                            },
                            end_location: Some(
                                Location {
                                    row: 1,
                                    column: 4,
                                },
                            ),
                            custom: (),
                            node: Constant {
                                value: Str(
                                    "a",
                                ),
                                kind: None,
                            },
                        },
                    ],
                    values: [
                        Located {
                            location: Location {
                                row: 1,
                                column: 6,
                            },
                            end_location: Some(
                                Location {
                                    row: 1,
                                    column: 7,
                                },
                            ),
                            custom: (),
                            node: Constant {
                                value: Int(
                                    0,
                                ),
                                kind: None,
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
                                    column: 12,
                                },
                            ),
                            custom: (),
                            node: Name {
                                id: "b",
                                ctx: Load,
                            },
                        },
                        Located {
                            location: Location {
                                row: 1,
                                column: 14,
                            },
                            end_location: Some(
                                Location {
                                    row: 1,
                                    column: 17,
                                },
                            ),
                            custom: (),
                            👇 `"c": 1` is parsed as dict!
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
                                                column: 17,
                                            },
                                        ),
                                        custom: (),
                                        node: Constant {
                                            value: Str(
                                                "c",
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
                                                column: 20,
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

_@charliermarsh reviewed on 2023-01-22 16:31_

---

_Review comment by @charliermarsh on `src/ast/comparable.rs`:430 on 2023-01-22 16:31_

I think this may need to preserve the `None`, to be truly comparable, right?

---

_@charliermarsh reviewed on 2023-01-22 16:33_

---

_Review comment by @charliermarsh on `src/rules/pyupgrade/rules/unpack_list_comprehension.rs`:25 on 2023-01-22 16:33_

Oh, we should rewrite this to use `any_over_expr`. Can be a separate PR, or included here...

---

_@charliermarsh reviewed on 2023-01-22 16:42_

---

_Review comment by @charliermarsh on `src/ast/comparable.rs`:430 on 2023-01-22 16:42_

(So we'd need to update the definition of `ComparableExpr`.)

---

_@charliermarsh reviewed on 2023-01-22 18:19_

---

_Review comment by @charliermarsh on `src/ast/comparable.rs`:430 on 2023-01-22 18:19_

(I'll go ahead and do this, then merge.)

---

_Merged by @charliermarsh on 2023-01-22 18:24_

---

_Closed by @charliermarsh on 2023-01-22 18:24_

---

_@harupy reviewed on 2023-01-22 20:21_

---

_Review comment by @harupy on `src/ast/comparable.rs`:430 on 2023-01-22 20:21_

@charliermarsh Thanks for the update!

---
