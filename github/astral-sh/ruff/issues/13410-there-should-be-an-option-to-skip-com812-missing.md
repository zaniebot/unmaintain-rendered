---
number: 13410
title: "There should be an option to skip COM812 (missing-trailing-comma) on single-item calls/lists. [Feature request]"
type: issue
state: closed
author: echo-lalia
labels:
  - question
assignees: []
created_at: 2024-09-19T18:59:16Z
updated_at: 2024-09-24T19:49:11Z
url: https://github.com/astral-sh/ruff/issues/13410
synced_at: 2026-01-07T13:12:15-06:00
---

# There should be an option to skip COM812 (missing-trailing-comma) on single-item calls/lists. [Feature request]

---

_Issue opened by @echo-lalia on 2024-09-19 18:59_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  COM812, missing trailing comma, single parameter argument, not a constant
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Currently, the rule `COM812` will flag any calls/lists where the final item has no comma.  
For the most part, this is useful and I like the consistency it provides. However, there are two issues with this rule that keep popping up in my code:  

Calls with only one item, that can *never* have another item added, are still flagged. This is a matter of preference, but I find this to actually harm readability, because it suggests that this is a list of items, where other items may be provided afterwards, and that is often not the case. Example:
``` Py
float(
    width
    * height
    / step,  # <- I find this comma more confusing/distracting than helpful
)
```

My second issue with this is MicroPython-specific *(which I realize may be out-of-scope for this project, but it causes problems for me so I think it's worth mentioning)*. This rule also recommends adding commas to constants, which breaks the code until the offending comma is removed. Example:
``` Py
_DISPLAY_PADDING = const(
    (_DISPLAY_HEIGHT - (6 * _SMALL_FONT_HEIGHT + _NUM_LINES * _FONT_HEIGHT))
    // (_NUM_LINES),
)
# SyntaxError: not a constant
```

<br/>

If there was an option to disable the comma rule when only a single value is present, then both of these issues would be solved.  

*Alternatively: if there was an optional list of names to ignore this rule inside of, then both `float` and `const` could be ignored, and both of these issues would be solved.*



---

_Comment by @MichaReiser on 2024-09-20 08:19_

Hi @echo-lalia 

Hmm, I can see how this is problematic but I think these two cases are exactly what the rule aims to catch

> Calls with only one item, that can never have another item added, are still flagged.

I can see how this might be desired, but it requires very involved analysis for a rule to determine if a callee has only a single parameter. Resolving the callee's signature would make the rule significantly slower. Resolving the callee will only work reliably for typed projects. 

But this also ignores the fact that code evolves. A function that only accepts a single parameter may accept multiple parameters tomorrow. Having the trailing comma in place reduces the diff where the new arguments are added because the formatting of the first argument remains unchanged. 

> My second issue with this is MicroPython-specific (which I realize may be out-of-scope for this project, but it causes problems for me so I think it's worth mentioning). This rule also recommends adding commas to constants, which breaks the code until the offending comma is removed. Example:

This certainly is frustrating. I don't know MicroPython myself and what there goal is but have you considered to open an issue and asking them to make `const` function calls python spec compliant so that they accept optional trailing commas? 



---

_Label `question` added by @MichaReiser on 2024-09-20 08:19_

---

_Comment by @echo-lalia on 2024-09-20 17:40_

Thanks for taking the time to respond and to explain!

I have no understanding of how Ruff works internally, and so when I submitted this issue, I had assumed this might be a relatively simple addition.  
Clearly that wasn't quite right, so I appreciate you explaining as much :)

<br/>

> A function that only accepts a single parameter may accept multiple parameters tomorrow. Having the trailing comma in place reduces the diff where the new arguments are added...

It seems like my suggestion probably won't be added, but I realized I forgot another funny example, and so I wanted to add it on now for the sake of discussion:
``` Py
handle_text(
    "This is a writing style that has come up in my projects on occasion.\n"
    "It allows indentation to be maintained with surrounding code, "
    "And for lines to be formatted in the code differently than in the result.\n"
    f"{' '.join(('You', 'can', 'also', 'use', 'it', 'to', 'split', 'up', 'f-strings'))}.\n"
    "And if you add a comma to the end, it makes the diff larger when you change the text.",
    )
```

Obviously this isn't the end of the world, but I thought it was worth adding to the post.

<br/>

> have you considered to open an issue and asking them to make const function calls python spec compliant so that they accept optional trailing commas?

I think it would definitely be sensible for it to be fixed on the MicroPython side :) It's a rapidly evolving project, so it's not unlikely that this gets fixed at some point. `const` variables are actually 'real' constants that get evaluated at compile time, and so I imagined there could be a really important (but unknown to me) reason that it doesn't understand the comma. But, I don't actually know because I haven't asked!

---

_Comment by @MichaReiser on 2024-09-22 07:58_

That's a fair point. It's a trade-off between optimizing the diff size for adding a new argument or appending to the string. In this case, it's not possible to have both. 

A formatting that avoids this is to assign the message to a variable where `COM812` doesn't apply.

```python
msg = (
    "This is a writing style that has come up in my projects on occasion.\n"
    "It allows indentation to be maintained with surrounding code, "
    "And for lines to be formatted in the code differently than in the result.\n"
    f"{' '.join(('You', 'can', 'also', 'use', 'it', 'to', 'split', 'up', 'f-strings'))}.\n"
    "And if you add a comma to the end, it makes the diff larger when you change the text."
)

handle_text(msg)
```

Or you could use a library like `textwrap` to remove the indent at runtime (with the downside that it comes with a performance penalty).

```python
handle_text(
    textwrap.dedent(f"""
        This is a writing style that has come up in my projects on occasion.
        It allows indentation to be maintained with surrounding code,
        And for lines to be formatted in the code differently than in the result.
        {" ".join(("You", "can", "also", "use", "it", "to", "split", "up", "f-strings"))}.
        And if you add a comma to the end, it makes the diff larger when you change the text.
    """)
)

```

---

_Closed by @zanieb on 2024-09-24 19:49_

---

_Referenced in [astral-sh/ruff#18258](../../astral-sh/ruff/issues/18258.md) on 2025-05-22 17:32_

---

_Referenced in [astral-sh/ruff#21567](../../astral-sh/ruff/pulls/21567.md) on 2025-11-21 18:14_

---
