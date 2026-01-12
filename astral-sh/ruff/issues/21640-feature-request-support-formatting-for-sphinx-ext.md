```yaml
number: 21640
title: "Feature Request: Support formatting for `sphinx.ext.doctest` (`.. testcode::`) in docstrings"
type: issue
state: open
author: sawa3030
labels:
  - formatter
assignees: []
created_at: 2025-11-26T13:48:56Z
updated_at: 2025-11-27T07:25:16Z
url: https://github.com/astral-sh/ruff/issues/21640
synced_at: 2026-01-12T15:54:57Z
```

# Feature Request: Support formatting for `sphinx.ext.doctest` (`.. testcode::`) in docstrings

---

_@sawa3030_

## Summary
When formatting docstrings, Ruff currently does not recognize Python code inside `sphinx.ext.doctest` directive blocks (such as `.. testcode::`). It would be helpful if these doctest-style blocks were detected and formatted in the same way as other supported docstring code blocks.

## Description
`sphinx.ext.doctest` provides directives like `.. testcode::` for writing executable examples in documentation. For example:

```
class ExampleClass:
    """Example class.

    Example:
        .. testcode::
            first_example = ExampleClass()
            print(first_example)
    """
```
ref: [https://www.sphinx-doc.org/en/master/usage/extensions/doctest.html](https://www.sphinx-doc.org/en/master/usage/extensions/doctest.html)

At the moment, Ruff does not format the code inside the `.. testcode::` block.
Ref: [https://docs.astral.sh/ruff/formatter/#docstring-formatting](https://docs.astral.sh/ruff/formatter/#docstring-formatting)

It would be great if Ruff could also format the Python code within these blocks, similarly to other reStructuredText code directives.

I’m happy to open a PR to add support for this, if I could.

---

_Comment by @MichaReiser on 2025-11-27 07:25_

There are other directives that we should support in addition to just `testcode`, e.g. `testsetup` and `testcleanup`


I haven't taken a close look but I think this shouldn't be too hard to add (@BurntSushi feel free to correct me), given that we already support `code-block::`. All that might be needed is to add `testcode`, `testsetup`, ... here:

https://github.com/astral-sh/ruff/blob/7627d3879c26dd3e9510eea8a63239f08dae1cbe/crates/ruff_python_formatter/src/string/docstring.rs#L1025-L1041

---

_Label `formatter` added by @MichaReiser on 2025-11-27 07:25_

---
