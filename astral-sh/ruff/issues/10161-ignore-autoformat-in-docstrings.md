```yaml
number: 10161
title: Ignore autoformat in docstrings
type: issue
state: closed
author: aawilson
labels:
  - question
  - formatter
assignees: []
created_at: 2024-02-29T07:00:59Z
updated_at: 2024-02-29T17:10:43Z
url: https://github.com/astral-sh/ruff/issues/10161
synced_at: 2026-01-12T15:54:50Z
```

# Ignore autoformat in docstrings

---

_@aawilson_

I'd like an option to prevent ruff from changing any formatting inside of particular docstrings. This is because sometimes I desire trailing whitespace for readability that ruff wants to remove--for example, in this `ply`-based code (which implements a `yacc` parser, hence the context-free grammar syntax):

```
def p_measured_time(p):
    """measured_time :'(' time ')'
                                |'(' time ',' distance ')'
    """
    # function impl follows
```

Ruff autoformats to this (note the lack of alignment between the `:` of the first line and the `|` of the second:
```
def p_measured_time(p):
    """measured_time :'(' time ')'
    |'(' time ',' distance ')'
    """
    # function impl follows
```

If this functionality does exist somehow, then I apologize for missing it, I spent some time with https://docs.astral.sh/ruff/rules/ looking for something likely but didn't find anything.


---

_Comment by @MichaReiser on 2024-02-29 07:34_

Hey. You can disable the formatting of individual docstring by [using a formatter suppression comment](https://docs.astral.sh/ruff/formatter/#format-suppression). In this case, you can add a `# fmt: skip` comment after the docstring, see https://play.ruff.rs/d2cd2a6f-127b-4bf8-9609-16424079f96e

---

_Label `question` added by @MichaReiser on 2024-02-29 07:34_

---

_Label `formatter` added by @MichaReiser on 2024-02-29 07:34_

---

_Comment by @aawilson on 2024-02-29 17:10_

And today I learned fmt: skip exists, thanks for this, I'll close the issue.

---

_Closed by @aawilson on 2024-02-29 17:10_

---
