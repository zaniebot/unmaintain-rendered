---
number: 12178
title: "[Rule request] Flag unsupported characters in logging method calls"
type: issue
state: open
author: huonw
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-07-04T04:47:57Z
updated_at: 2024-07-04T08:22:37Z
url: https://github.com/astral-sh/ruff/issues/12178
synced_at: 2026-01-07T13:12:15-06:00
---

# [Rule request] Flag unsupported characters in logging method calls

---

_Issue opened by @huonw on 2024-07-04 04:47_

There's two rules (F509, PLE1300) that detect when a `%` formatted string include an invalid format character. However, these don't seem to flag format strings passed to logging functions (`debug`, `info`, etc.). These strings use `%` for formatting in the same way.

(References: "The message attribute of the record is computed using msg % args" https://docs.python.org/3/library/logging.html#logging.Formatter.format , https://github.com/python/cpython/blob/9728ead36181fb3f0a4b2e8a7291a3e0a702b952/Lib/logging/__init__.py#L391-L401).

Reproducer: https://play.ruff.rs/36d5756e-44c7-48a1-8d8e-ae45a29ee480

```python
"""x."""
import logging

x = "flagged: %A" % 1 # error: F509, PLE1300
logging.error("ignored: %A", 1) # no error
```

Settings (note `"select": ["ALL"]`)

```json
{
  "preview": false,
  "builtins": [],
  "target-version": "py312",
  "line-length": 88,
  "indent-width": 4,
  "lint": {
    "allowed-confusables": [],
    "dummy-variable-rgx": "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$",
    "extend-select": [],
    "extend-fixable": [],
    "external": [],
    "ignore": [],
    "select": [
      "ALL"
    ]
  },
  "format": {
    "indent-style": "space",
    "quote-style": "double"
  }
}
```

To confirm that this is an error, comment out the `x = ...` line and run the code. It prints:

```
--- Logging error ---
Traceback (most recent call last):
  File "/Users/huon/.pyenv/versions/3.10.4/lib/python3.10/logging/__init__.py", line 1100, in emit
    msg = self.format(record)
  File "/Users/huon/.pyenv/versions/3.10.4/lib/python3.10/logging/__init__.py", line 943, in format
    return fmt.format(record)
  File "/Users/huon/.pyenv/versions/3.10.4/lib/python3.10/logging/__init__.py", line 678, in format
    record.message = record.getMessage()
  File "/Users/huon/.pyenv/versions/3.10.4/lib/python3.10/logging/__init__.py", line 368, in getMessage
    msg = msg % self.args
ValueError: unsupported format character 'A' (0x41) at index 10
Call stack:
  File "/private/tmp/test.py", line 6, in <module>
    logging.error("ignored: %A", 1)  # no error
Message: 'ignored: %A'
Arguments: (1,)
```

Issues that are (somewhat) related:

- https://github.com/astral-sh/ruff/issues/7248
- https://github.com/astral-sh/ruff/issues/11403


Thanks for ruff, very speedy!

---

_Comment by @dhruvmanila on 2024-07-04 04:58_

Thank you for the detailed issue!

I think it's a little bit complicated than that. The formatting is decided by the [Formatter objects](https://docs.python.org/3/library/logging.html#formatter-objects) which can use `%` but it can also use other formatting style which is determined by the `style` parameter and it can be set separately for each logger or can be set through the global config.

Or, maybe we can just check if the string uses `%`-style format characters and make an assumption that it uses the `%` style.

---

_Label `rule` added by @dhruvmanila on 2024-07-04 04:58_

---

_Label `needs-decision` added by @dhruvmanila on 2024-07-04 04:58_

---

_Comment by @huonw on 2024-07-04 08:22_

Thanks for the quick reply!

I'm a little confused by the ins-and-outs of `logging` at times... but, I think there's two layers of formatting:

1. Rendering of the individual `LogRecord`'s message, using the values passed to `info`/`error`/etc (`msg` & `args`) to set `message`.
2. Constructing the final output from the message and other attributes (time etc), using a `Formatter` like you say. (And its `style` arg to customise.)

For 1, I believe this happens unconditionally using `%`, unless there's some way to use a `LogRecord` subclass/replacement. See code: https://github.com/python/cpython/blob/9728ead36181fb3f0a4b2e8a7291a3e0a702b952/Lib/logging/__init__.py#L391-L401 

In addition, there's existing linting rules that explicitly suggest turning f-strings and `.format` calls into `%`, so "use `%` for logging" is an assumption with precedent:

- G001: https://docs.astral.sh/ruff/rules/logging-string-format/ (and variants G002, G003, G004)
- PLE1205: https://docs.astral.sh/ruff/rules/logging-too-many-args/ (and variant PLE1206)

What do you think?

---
