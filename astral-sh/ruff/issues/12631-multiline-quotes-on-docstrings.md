```yaml
number: 12631
title: Multiline-quotes on docstrings
type: issue
state: closed
author: karolyi
labels:
  - question
  - docstring
assignees: []
created_at: 2024-08-02T14:22:06Z
updated_at: 2024-09-09T12:13:53Z
url: https://github.com/astral-sh/ruff/issues/12631
synced_at: 2026-01-10T11:09:54Z
```

# Multiline-quotes on docstrings

---

_Issue opened by @karolyi on 2024-08-02 14:22_

Hello,

I started experimenting around with ruff to see if it can suit my needs and coding style. I've dropped it and went back to pylsp because while I can enforce strings to be single-quoted (docstrings included), I can't enforce that _multiline docstrings_ be double-quoted, as in:

```python
def single_docstring():
   'some docstring that lies in a single line'

def multiline_docstring():
    """
    Some docstring
    that for some reason,
    will have to be broken into multiple lines.
    """
```

It seems that with the current configurable options that is not to be achieved. The only ones I could find about this current state are:
```toml
[lint.flake8-quotes]
docstring-quotes = 'single'
multiline-quotes = 'double'
inline-quotes = 'single'
```

And we haven't even talked about the autoformatting part.

I have some more qualms but this is the single most one that made me drop ruff, unfortunately. Let me know if or when this is/will-be achieavable.

---

_Comment by @charliermarsh on 2024-08-02 21:36_

To be clear, you want _all_ strings to be single-quoted, except for triple-quoted docstrings, which should be double-quoted?

---

_Label `question` added by @charliermarsh on 2024-08-02 21:40_

---

_Label `docstring` added by @charliermarsh on 2024-08-02 21:40_

---

_Comment by @karolyi on 2024-08-02 21:42_

No. I'd like to have the `multiline-quotes` setting apply to docstrings as well, because it doesn't now. Or maybe to have a `multiline-docstring-quotes` setting? Everything regarding the quote style is okay with me so far, but I didn't go deeper into using ruff yet.

The basic rule is: if a docstring has to be broken into multiple lines, it should be triple quoted with double-quotes.

Is that doable?

---

_Comment by @MichaReiser on 2024-08-03 07:38_

No, I don't think the current settings support this specific linting. 
It is technically doable to add support for it, it shouldn't be too hard actually but we should consider this carefully because it would introduce a new setting/option that needs to be kept in sync with the formatter setting (only compatible when using `quote-style=preserve`). 

---

_Comment by @karolyi on 2024-08-03 09:13_

Indeed.

Yes it violates pep257, but enables more characters in one-line docstrings where I have set 72 chars as maximum limit (that is also a pep convention). Often times it wouldn't fit with 3 quotes.

That said you can take it or leave it, it was only a question if you would do it or not, it's absolutely not a must. I can keep continuing to use the current tooling I have.

---

_Closed by @MichaReiser on 2024-09-09 12:03_

---

_Comment by @karolyi on 2024-09-09 12:05_

@MichaReiser can I get a quick rundown on how this got completed, if it really is?

I'd like to try ruff with that new setting.

---

_Comment by @MichaReiser on 2024-09-09 12:12_

Sorry. I should have added a comment. 

I marked it as not-planned because we want to hold back on new options/rules that create incompatibilities with the formatter.

---

_Closed by @MichaReiser on 2024-09-09 12:12_

---

_Comment by @karolyi on 2024-09-09 12:13_

Thanks.

Please let me know by commenting in this issue, if ever this changes.

---
