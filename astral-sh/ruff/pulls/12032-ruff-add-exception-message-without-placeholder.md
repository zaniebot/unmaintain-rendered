```yaml
number: 12032
title: "[`ruff`] Add `exception-message-without-placeholder` rule (#11979)"
type: pull_request
state: open
author: denwong47
labels:
  - rule
  - preview
  - great writeup
assignees: []
base: main
head: exception-message-without-placeholder
created_at: 2024-06-25T19:17:13Z
updated_at: 2024-06-28T04:25:19Z
url: https://github.com/astral-sh/ruff/pull/12032
synced_at: 2026-01-10T21:56:00Z
```

# [`ruff`] Add `exception-message-without-placeholder` rule (#11979)

---

_Pull request opened by @denwong47 on 2024-06-25 19:17_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Addresses #11979 to add a `RUF` rule to check for static exception messages, which does not contain any runtime contextual information.

For example:

```python
from pathlib import Path
settings_file = Path(__file__) / "settings.json"
if not settings_file.exists():
   raise FileNotFoundError("File not found")
```

This exception instance will give out the static error message regardless of where it is looking for `settings_file`. The user could not have known if he misnamed the JSON, or he placed the script in the wrong folder. This could easily be resolved by formatting the message with `settings_file`:

```python
if not settings_file.exists():
   raise FileNotFoundError(f"Settings not found at '{settings_file.resolve().as_posix()}'")
```

This currently covers all built-in exceptions classes except:

- `NotImplementedError` - this, by definition, is not an error related to any runtime states.
- `UnicodeDecodeError`, `UnicodeEncodeError` and `UnicodeTranslateError` - these by its `__init__` already mandates contextual information.

The user will be prompted to either
- Provide a variable in the message, or
- Create a custom Exception class for the specific error condition.

## Test Plan

`cargo test`.

## What is static or not static?

Some examples of static messages:
```python
MY_STATIC_VARIABLE = "This is a static message"
my_other_static_variable = MY_STATIC_VARIABLE
raise ValueError("This is a static message")
raise ValueError(f"This is a {'static':-^11} message even with an f-string")
raise ValueError("This is a {kind} message".format(kind="static"))
raise ValueError(my_other_static_variable)
```

Some examples of dynamic messages:
```python
MY_STATIC_TEMPLATE = "Your error is because of {reasons}"
raise ValueError(MY_STATIC_TEMPLATE.format(reasons="reasons"))

reasons = "reasons"
raise ValueError(f"Your error is because of {reasons}")

raise ValueError("Your error is because of {reasons}".format(reasons=reasons))

raise ValueError("This error" if some_condition() else "That error")
```

The above examples differs from the static messages in that there could be
conditions that change the message at runtime. For example:

```python
MY_STATIC_TEMPLATE = "Your error is because of {reasons}"

if some_condition():
   raise ValueError(MY_STATIC_TEMPLATE.format(reasons="reason 1"))
else:
   raise ValueError(MY_STATIC_TEMPLATE.format(reasons="reason 2"))
```

In this case, the message being formatted is a static value, but subject to
change at runtime, hence it is considered a dynamic message.

## Interaction with other rules

Notable overlapping or similar rules:
- `EM101, EM102, EM103` - these `flake8` rules will look for string literals as Exception messages, and encourage the user to store them in a variable, to reduce duplication in the raised error message. This rule will consider both its condition and its fix to be violations.
- `TRY003` - this checks for exception messages that is a string literal containing any whitespaces, and encourage the user to create a custom exception class instead. Arguably this is a stricter rule than the proposed rule here, with a overlapping goal of making exceptions more specific.

---

_Label `rule` added by @dhruvmanila on 2024-06-28 04:25_

---

_Label `preview` added by @dhruvmanila on 2024-06-28 04:25_

---

_Label `great writeup` added by @dhruvmanila on 2024-06-28 04:25_

---
