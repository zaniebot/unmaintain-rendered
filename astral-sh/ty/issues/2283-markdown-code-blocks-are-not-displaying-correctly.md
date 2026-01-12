```yaml
number: 2283
title: Markdown code blocks are not displaying correctly in hover docs
type: issue
state: closed
author: kfalcetano
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-31T00:01:46Z
updated_at: 2026-01-04T19:11:24Z
url: https://github.com/astral-sh/ty/issues/2283
synced_at: 2026-01-12T15:54:26Z
```

# Markdown code blocks are not displaying correctly in hover docs

---

_@kfalcetano_

### Summary

## Issue
There appear to be HTML rendering problems when viewing formatted hover docs containing markdown (triple backtick ` ``` `) code blocks. 

## Environment
VSCode  (Windows + WSL) with `astral-sh.ty` extension version 2025.80.0, packaged with `ty==0.0.8`

## Playground Link
This was reproducible in playground: https://play.ty.dev/ceef294d-c7fd-4c77-aac9-0ad6d48c091d

## Screenshots
### Markdown
<img width="508" height="218" alt="Image" src="https://github.com/user-attachments/assets/72c8c1c3-55af-4d88-a951-ba4b815e8c1e" />

### reStructuredText
<img width="368" height="222" alt="Image" src="https://github.com/user-attachments/assets/a619b4b1-75a4-42fd-9a40-16194cf579e5" />

### Google
<img width="493" height="255" alt="Image" src="https://github.com/user-attachments/assets/eab9ffa5-724a-415d-82de-29ec5758f15f" />

## Sample Code
```python3
def markdown_example():
    """Example docstring with markdown codeblock
    ## Example
    Usage
    ```
    from lib import markdown_example
    def some_function():
        markdown_example()
    ```
    """
    pass


def reST_example():
    """Example docstring with reST codeblock

    Example
    -------
    Usage ::
        from lib import reST_example
        def some_function():
            reST_example()
    """
    pass


def google_style_example():
    """Example docstring with google style codeblock

    Example:
        >>> from lib import google_style_example
        >>>
        >>> def main():
        >>>    google_style_example()
        >>>
        >>> main()
    """
    pass
```

### Version

ty 0.0.8

---

_Label `server` added by @AlexWaygood on 2025-12-31 00:13_

---

_Label `bug` added by @MichaReiser on 2025-12-31 08:14_

---

_Assigned to @Gankra by @MichaReiser on 2025-12-31 08:14_

---

_Comment by @MichaReiser on 2025-12-31 08:14_

@Gankra could you take a look at this. The `\_example` escaping looks suspicious. 

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:47_

---

_Comment by @kfalcetano on 2025-12-31 19:51_

Was curious about how this is handled, and it seems like `in_any_code` in [`render_markdown()`](https://github.com/astral-sh/ruff/blob/e71fd9c0408dbedff0c80b6855c5612c82091f59/crates/ty_ide/src/docstring.rs#L184) should be true when in a markdown code block, but the outputs indicate it's false at the offending lines (inserting escapes and nbsps). The only places it is set true are lines 239 and 251, so not sure if the `starting_literal` parsing is expected to handle the code block fence or the handling is just missing, but that might be the place to look.

---

_Comment by @Gankra on 2026-01-04 19:11_

This is the same underlying issue as #2291 -- we don't parse markdown codefences at all, and so we treat the contents as normal markdown that should be parsed and escaped.

---

_Closed by @Gankra on 2026-01-04 19:11_

---
