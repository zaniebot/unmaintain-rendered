```yaml
number: 11979
title: new rule - enforce that strings passed to exceptions have a variable in them
type: issue
state: open
author: DetachHead
labels:
  - rule
assignees: []
created_at: 2024-06-22T00:10:21Z
updated_at: 2024-06-24T21:43:50Z
url: https://github.com/astral-sh/ruff/issues/11979
synced_at: 2026-01-12T15:54:51Z
```

# new rule - enforce that strings passed to exceptions have a variable in them

---

_@DetachHead_

there have been countless times where i've encountered unhelpful error messages in software that don't provide enough information to the user. i think this is something that can be addressed with a lint rule. for example:

```py
if not path.exists():
    raise Exception("file not found") # error: use a relevant variable in your error message to provide more information to the user
```
error messages like this are extremely common and are very frustrating when exposed to a user. what file was it looking for?

in this case, the fix would be to include the `path` variable in the message:

```py
if not path.exists():
    raise Exception(f"file not found: {path}")
```

obviously there will be cases where this is intentional, so here are a couple ideas to help reduce the number of false positives:

- the error could only be reported on certain exception types (eg. `Exception`, `FileNotFoundError`). ideally this would be user-configurable.
- only report the error if the raised exception is being passed a literal string. ie. `raise FooException()` would not report the error but `raise FooException("something failed")` would

---

_Label `rule` added by @charliermarsh on 2024-06-23 17:32_

---

_Comment by @denwong47 on 2024-06-24 21:43_

I can give this a go as it seems quite similar to the other PR (#11981) I just worked on, and I personally like long and detailed Exceptions!

I am proposing to only deal with Built-in Exceptions - as User defined exceptions can have particular contextual meaning already, and a variable is not necessarily required.

There are also 4 notable exceptions:
- `NotImplementedError` - this, by definition, is not an error related to any runtime states.
- `UnicodeDecodeError`, `UnicodeEncodeError` and `UnicodeTranslateError` - these by its `__init__` already mandates contextual information.

I'll set this up to check against the remaining 52 types.

---
