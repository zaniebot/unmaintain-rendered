```yaml
number: 2021
title: Semantic token highlighting causes wrong color for first line of Google-style docstrings
type: issue
state: closed
author: willjobs
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-17T16:15:46Z
updated_at: 2025-12-19T16:19:34Z
url: https://github.com/astral-sh/ty/issues/2021
synced_at: 2026-01-12T15:54:26Z
```

# Semantic token highlighting causes wrong color for first line of Google-style docstrings

---

_@willjobs_

Congratulations on your release! I'm liking the extension so far. This issue not a show-stopper by any means, but a little annoying thing that has to do with how the semantic tokenizing is interacting with coloring in VS Code.

If you write Python docstrings in this format (the ["Google format"](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)):

```py
def my_function(param1: int, param2: str) -> bool:
    """Example function with PEP 484 type annotations.

    Args:
        param1: The first parameter.
        param2: The second parameter.

    Returns:
        The return value. True for success, False otherwise.

    """
```

The first line of the docstring will be interpreted differently by VS Code when the `ty` extension is on, treating it as a string rather than part of a multi-line docstring, which means it will color the first line of the docstring differently than the rest.

With the `ty` extension disabled/uninstalled, if I open the "Developer: Inspect Editor Tokens and Scopes" tool in VS Code and click on the first line of the docstring I see:
- language: `python`
- standard token type: `String`
- values for foreground, background, contrast ratio
- textmate scopes: 
    - `string.quoted.docstring.multi.python`
    - `source.python`
- foreground:
    - `string.quoted.docstring.multi`
    - `{ "foreground": "#MYHEX1" }`

But when I turn on the `ty` extension this changes:
- Same language, standard token type, foreground, background, contrast ratio (but a different hex color for foreground than when `ty` was turned off)
- New section in the middle:
    - semantic token type: `string`
    - foreground:
        - `string`
        - `string`
        - `{ "foreground": "#MYHEX2" }`
- Same textmate section, EXCEPT **the foreground section is now crossed out**

---

# Things I tried to fix it

- I tried adding settings to my user settings to change the `editor.tokenColorCustomizations.textMateRules` for scope "string.quoted.docstring.multi.python", but that only affected the colors of the remaining lines of the docstring, not the first.
- I tried changing the coloring for `editor.semanticTokenColorCustomizations`, and that did change the color of that first line of the docstring, but it ALSO changed the color for all strings in the document (as you would expect).
- If I start the docstring text AFTER the line with the triple quotes, then the docstring text itself is correctly formatted with the rest of the text, but the triple quotes are still highlighted the same as other strings in my file.

---

_Renamed from "Semantic token highlighting of docstrings following numpy/Google style" to "Semantic token highlighting causes wrong color for first line of Google-style docstrings" by @willjobs on 2025-12-17 16:16_

---

_Comment by @MichaReiser on 2025-12-17 16:44_

Thanks for the detailed report. This should be easy to fix.

---

_Label `server` added by @AlexWaygood on 2025-12-17 16:47_

---

_Comment by @MichaReiser on 2025-12-17 16:48_

It looks like ty is the only one that classifies the docstring as a string, but the ranges are slightly different

---

_Comment by @Gankra on 2025-12-17 16:50_

Quite perplexing:

<img width="465" height="205" alt="Image" src="https://github.com/user-attachments/assets/23224070-f9ed-45df-aeb6-086bb6453f2b" />

If I check our semantic tokens for this it seems to consider the whole docstring to be a single entry.

<img width="856" height="195" alt="Image" src="https://github.com/user-attachments/assets/b4c20648-3da7-46d7-925a-39ff2778152b" />

So I'm a bit perplexed that ty is causing the thing to get split up. But anyway we can mark it as a docstring at least.

---

_Comment by @MichaReiser on 2025-12-17 17:00_

I think the issue here is that the logic for the semantic token encoding doesn't handle multiline tokens correctly (it pretends it does but it doesn't). I'll look into it

---

_Label `bug` added by @MichaReiser on 2025-12-17 17:00_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-12-17 17:01_

---

_Comment by @Gankra on 2025-12-17 17:38_

Ah yeah `supports_multiline_semantic_tokens()` is a client capability

---

_Comment by @Gankra on 2025-12-17 17:39_

...And we handle it in a very silly way

---

_Closed by @MichaReiser on 2025-12-18 11:38_

---

_Comment by @RyanSaxe on 2025-12-19 12:46_

@MichaReiser I see this closed as completed, but the solution implemented made those of us who have different highlighting between strings and docstrings break.

Docstrings are now identified as strings from ty instead of documentation. I have a customization to cover `string.documentation.python` differently than `string.python`. This worked with both basedpyright and pyrefly in neovim for context.

---

_Comment by @MichaReiser on 2025-12-19 13:08_

I think this requires [[ty] Classify docstrings in semantic tokens (syntax highlighting) ruff#22031](https://github.com/astral-sh/ruff/pull/22031)

---

_Comment by @willjobs on 2025-12-19 16:19_

Thank you for addressing this so quickly!

---
