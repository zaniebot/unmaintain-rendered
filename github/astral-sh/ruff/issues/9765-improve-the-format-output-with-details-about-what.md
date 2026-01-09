---
number: 9765
title: Improve the format output with details about what needs to be formatted.
type: issue
state: closed
author: vlkorsakov
labels:
  - question
  - formatter
assignees: []
created_at: 2024-02-01T23:15:02Z
updated_at: 2024-02-23T21:32:22Z
url: https://github.com/astral-sh/ruff/issues/9765
synced_at: 2026-01-07T13:12:15-06:00
---

# Improve the format output with details about what needs to be formatted.

---

_Issue opened by @vlkorsakov on 2024-02-01 23:15_

Please provide a more detailed output, including the line number and a description of what I need to format as in flake8. I think you can update the output for use with the --check flag only.

**code example:**
```python
def foo():
    pass
def bar():
    pass
```

**flake8 output:**
```shell
my_awesome_project/main.py:3:1: E302 expected 2 blank lines, found 0
my_awesome_project/main.py:4:9: W292 no newline at end of file
```

**ruff format --check output:**
```shell
Would reformat: my_awesome_project/main.py
1 file would be reformatted
```

---

_Label `formatter` added by @AlexWaygood on 2024-02-01 23:17_

---

_Comment by @zanieb on 2024-02-01 23:21_

Sounds like you're looking for #7352 with `ruff check` for those specific rules.

Note that the formatter and linter are different, but if you want to see the format changes, run `ruff format --diff`. See [this document](https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules) for information on how the linter and formatter overlap. It's unlikely that we'll be able to provide discrete "codes" or descriptions of what needs to be changed by the formatter. It works in a different way than lint rules and implementing it would be very challenging.

---

_Label `question` added by @zanieb on 2024-02-01 23:21_

---

_Comment by @vlkorsakov on 2024-02-01 23:46_

> run `ruff format --diff`

This gives such a complex result. flake8's output is easier to understand. I think if you add more colours to diff (like in git diff), the output might become easier.

In general, I would like the result to be the same as "check" only because part of the pycodestyle rules are not supported right now (waiting for #9266), and I can only see what I missed by using the formatter. Thanks for the answer.

---

_Comment by @zanieb on 2024-02-01 23:53_

You can get colors with something like `ruff format --diff | ydiff` (there are a bunch of different tools out there for viewing diffs). I agree that's not ideal though we should probably emit colored diffs by default!

> In general, I would like the result to be the same as "check"

Yeah I agree this would be nice if it were feasible.

---

_Comment by @MichaReiser on 2024-02-23 21:32_

We plan to add color support to our diffs. See https://github.com/astral-sh/ruff/issues/9984 I'll close this in favour of that issue. 

---

_Closed by @MichaReiser on 2024-02-23 21:32_

---
