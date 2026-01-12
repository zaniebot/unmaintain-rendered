```yaml
number: 19767
title: Contents of docstrings receive Python formatting rules
type: issue
state: closed
author: bepri
labels:
  - question
assignees: []
created_at: 2025-08-05T17:33:45Z
updated_at: 2025-11-06T13:30:36Z
url: https://github.com/astral-sh/ruff/issues/19767
synced_at: 2026-01-12T15:54:57Z
```

# Contents of docstrings receive Python formatting rules

---

_@bepri_

### Summary

One of our repositories uses rST in its docstrings. Given the right conditions, this rST appears to be interpreted as Python for some reason and has Python formatting rules applied to it.

```py
# min_repro.py
def foo():
    """An example.

    Example rST::

        LONG_LONG_LONG_STRING = "please do not break me up with formatting, I must be kept together!"
    """
```

With the above file, run `ruff format min_repro.py` and see that it breaks up the string into this:

```py
# min_repro.py
def foo():
    """An example.

    Example rST::

        LONG_LONG_LONG_STRING = (
            "please do not break me up with formatting, I must be kept together!"
        )
    """
```

### Version

ruff 0.12.5 (d13228ab8 2025-07-24)

---

_Comment by @ntBre on 2025-08-05 18:22_

Thanks for the report!

The [docs](https://docs.astral.sh/ruff/formatter/#docstring-formatting) say this should be opt-in behavior in the formatter. Have you tried setting [docstring-code-format](https://docs.astral.sh/ruff/settings/#format_docstring-code-format) to `false`?

I can only reproduce this in the [playground](https://play.ruff.rs/fb9861ea-4b50-4a48-a87b-2804fda420f2) with this option set to `true`.

---

_Label `question` added by @ntBre on 2025-08-05 18:22_

---

_Comment by @bepri on 2025-08-05 18:50_

This is embarrassing - sorry, I didn't realize that was enabled on our particular project.

In our particular use-case, we wanted to show the contents of an `os-release` file in the docstring (I know this is a strange docstring, I've actually removed it but still found this behavior surprising enough to open this issue). We wanted to put it into a codeblock to separate it out from the rest of the text, but that codeblock wasn't necessarily Python output. `::` accomplished that.

I think it is weird behavior to assume the language type of `::` since rST doesn't do that, but I also don't think it's an unreasonable assumption since it _is_ a Python docstring after all. I'm not sure that there's a satisfying answer to this, the current behavior pleases one crowd and my proposed behavior pleases a different one.

---

_Comment by @MichaReiser on 2025-08-06 06:16_

Ruff currently formats all [literal blocks](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#toc-entry-22) (what you have here) and [code blocks](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-code-block) that are valid Python syntax. Assuming that all literal blocks are Python code is probably a bit of a stretch but it's probably more often correct than wrong? 

You can disable the formatter for this one docstring or the one doc line using `fmt: skip` ([playground](https://play.ruff.rs/806f2b2f-866f-449b-83cc-beda06c3ebdb)). Ruff, unfortunately, doesn't support any docstring specific suppression comments (so that you could skip the literal block without it appearing in the rendered documentation)

---

_Closed by @MichaReiser on 2025-11-06 13:30_

---
