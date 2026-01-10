```yaml
number: 1095
title: Classproperty returns as an object and not the type
type: issue
state: closed
author: brentonmcs
labels:
  - needs-info
assignees: []
created_at: 2025-08-25T01:41:57Z
updated_at: 2025-08-27T00:08:20Z
url: https://github.com/astral-sh/ty/issues/1095
synced_at: 2026-01-10T02:06:24Z
```

# Classproperty returns as an object and not the type

---

_Issue opened by @brentonmcs on 2025-08-25 01:41_

### Summary

Hi team,

I've got a class property on an "ExtendedEnum" class to list the enums as a list[str]. When I use this I get "Expected `Iterable[Unknown]`, found `object`"

```python
class ExtendedEnum(Enum):
    """
    An extended Enum class that provides additional functionality, such as a class
    property to get a list of all enum values as strings.
    """

    @classproperty
    def list_values(cls) -> list[str]:
        """
        Returns a list of all enum values as strings.

        This method is decorated with @classproperty, allowing it to be accessed as a
        property on the class itself, rather than on an instance of the class.

        Returns:
            list[str]: A list of all enum values as strings.
        """
        return [item.value for item in cls]
```

### Version

ty 0.0.1-alpha.19 (e9cb838b3 2025-08-19)

---

_Comment by @sharkdp on 2025-08-25 08:11_

Thank you for reporting this.

Can you please share a reproducible example? Where does `classproperty` come from? Is that function a method of a class? Where exactly does the error show up?

---

_Label `needs-mre` added by @sharkdp on 2025-08-25 08:11_

---

_Label `needs-mre` removed by @sharkdp on 2025-08-25 08:11_

---

_Label `needs-info` added by @sharkdp on 2025-08-25 08:11_

---

_Comment by @brentonmcs on 2025-08-27 00:08_

Sorry! Thought @classproperty was a built-in thing! Turns out I don't know my own codebase!

---

_Closed by @brentonmcs on 2025-08-27 00:08_

---
