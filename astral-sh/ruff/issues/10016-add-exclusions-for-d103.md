```yaml
number: 10016
title: Add exclusions for D103
type: issue
state: open
author: clairem-sl
labels:
  - configuration
  - docstring
assignees: []
created_at: 2024-02-17T06:51:13Z
updated_at: 2025-09-05T02:37:30Z
url: https://github.com/astral-sh/ruff/issues/10016
synced_at: 2026-01-12T15:54:49Z
```

# Add exclusions for D103

---

_@clairem-sl_

With my current `ruff` (version **0.2.1**), it keeps warning me that `main()` has no docstring (rule `D103`).

For me, I see no need for `main()` to ever have a docstring. After all, it's the `main()` function anyways, and I always use the `if __name__ == "__main__"` invocation to start it, and functionalities that may find use in other modules are already extracted to their own functions.

That results in me having to add `# noqa: D103` every single time I create a new `__main__.py`, and it gets old quickly.

It would be nice if we have this configuration knob:

```toml
[tool.ruff.lint.pydocstyle]
allow-undocumented = ["main"]
```

This knob will control the behavior of rules `D100` to `D104`.

A more granular approach can be implemented as well, for instance:

```toml
[tool.ruff.lint.pydocstyle]
allow-undocumented-public-function = ["main", "get_options"]
allow-undocumented-public-class = ["Options"]
allow-undocumented-public-method = ["SomeClass.public_method"]
# ... and so on, following pydocstyle rule names in https://docs.astral.sh/ruff/rules/#pydocstyle-d
```

(The `allow-undocumented-public-(function|class)` examples above are particularly useful for me because I define a `Protocol` named `Options`, and I `cast()` the result of `get_options()` [parses CLI for options using `argparse`] into `Options` for easy reference in `main()` and other functions that uses the options from CLI. For example, [here](https://github.com/clairem-sl/sl-cartography/blob/8a758119657603f78612051bff2b88939b9804c4/src/worldmap_v4/nightlights2/__main__.py#L42-L71), [here](https://github.com/clairem-sl/sl-cartography/blob/8a758119657603f78612051bff2b88939b9804c4/src/worldmap_v4/mosaic/__main__.py#L43-L72), [here](https://github.com/clairem-sl/sl-cartography/blob/8a758119657603f78612051bff2b88939b9804c4/src/retriever_v4/maps/__main__.py#L44-L69), and [here](https://github.com/clairem-sl/sl-cartography/blob/8a758119657603f78612051bff2b88939b9804c4/src/analysis/clumps/__main__.py#L21-L37))


---

_Label `configuration` added by @AlexWaygood on 2024-02-18 11:32_

---

_Label `docstring` added by @AlexWaygood on 2024-02-18 11:32_

---

_Comment by @jn64 on 2025-09-05 02:37_

Duplicate / superset of #7401 (that one only mentions D103, not D100 to D104)

For reference, previous (unresolved) issues in pydocstyle:
- https://github.com/PyCQA/pydocstyle/issues/537
- https://github.com/PyCQA/pydocstyle/issues/532

---
