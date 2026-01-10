```yaml
number: 7897
title: "New Linting Rule: Prefer `setdefault` Over Conditional Key Checks for Dictionary Assignment"
type: issue
state: open
author: danparizher
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-10-10T15:10:26Z
updated_at: 2023-10-12T05:21:54Z
url: https://github.com/astral-sh/ruff/issues/7897
synced_at: 2026-01-10T11:09:50Z
```

# New Linting Rule: Prefer `setdefault` Over Conditional Key Checks for Dictionary Assignment

---

_Issue opened by @danparizher on 2023-10-10 15:10_

# Proposed Linting Rule

The proposed new linting rule is to prefer the use of `setdefault` over over conditional checks when assigning a value to a dictionary key. This change is suggested because `setdefault` is a more concise and efficient way to check if a key exists in a dictionary and assign a value to it if it doesn't.

## Reason for Preference

The reason for this preference is that `setdefault` combines the check for the existence of the key and the assignment of the value into a single operation. This makes the code more readable and slightly more efficient, as it reduces the number of dictionary lookups. Additionally,  `setdefault` is atomic, whereas something like `if "x" in y: y["x"] = z` isn't thread safe. Moreover, it aligns with the Pythonic way of writing clean and efficient code.

## Applicability

This rule can be applied to any situation where you need to check if a key exists in a dictionary and assign a value to it if it doesn't. This includes but is not limited to:

- Default values for function arguments
- Default values for class attributes
- Default values for dictionary keys

In each of these cases, `setdefault` provides a more efficient and Pythonic way to handle the situation.

## Examples

### 1. Grouping items while filling a dictionary

Before:

```py
new = {}
for key, value in data:
    if key in new:
        new[key].append(value)
    else:
        new[key] = [value]
```

After:

```py
new = {}
for key, value in data:
    new.setdefault(key, []).append(value)
```

In this scenario, `setdefault` is used to group items by their keys in a dictionary. If a key already exists in the dictionary, it appends the value to the list of values for that key. If the key does not exist, it creates a new key with a list containing the value as its value.

### 2. Setting default values for function arguments

Before:

```py
def notify(self, level, *pargs, **kwargs):
    if "persist" not in kwargs:
        kwargs["persist"] = level >= DANGER
    self.__defcon.set(level, **kwargs)
```

After:

```py
def notify(self, level, *pargs, **kwargs):
    kwargs.setdefault("persist", level >= DANGER)
    self.__defcon.set(level, **kwargs)
```

In this scenario, `setdefault` is used to set a default value for a keyword argument in a function. If the "persist" keyword is not in `kwargs`, it sets it to `level >= DANGER`.

### 3. Setting default values for dictionary keys

Before:

```py
headers = parse_headers(msg)  # parse the message, get a dict
for headername, defaultvalue in optional_headers:
    if headername not in headers:
        headers[headername] = defaultvalue
```

After:

```py
headers = parse_headers(msg)  # parse the message, get a dict
for headername, defaultvalue in optional_headers:
    headers.setdefault(headername, defaultvalue)
```

In this scenario, `setdefault` is used to ensure that specific keys exist in a dictionary after it is created. If a key is not in the dictionary, it adds the key with a specified default value.

## References

- [Python Docs: dict.setdefault](https://docs.python.org/3/library/stdtypes.html#dict.setdefault)


---

_Renamed from "New Linting Rule: Prefer `setdefault` Over `if not in` for Dictionary Assignment" to "New Linting Rule: Prefer `setdefault` Over Conditional Key Checks for Dictionary Assignment" by @danparizher on 2023-10-10 15:16_

---

_Label `rule` added by @dhruvmanila on 2023-10-12 05:21_

---

_Label `needs-decision` added by @dhruvmanila on 2023-10-12 05:21_

---
