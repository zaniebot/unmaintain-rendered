```yaml
number: 16007
title: "[D214, D405, D417] False positive with arg keyword `attributes`"
type: issue
state: closed
author: JP-Ellis
labels:
  - bug
  - docstring
assignees: []
created_at: 2025-02-07T00:16:20Z
updated_at: 2025-02-11T17:05:30Z
url: https://github.com/astral-sh/ruff/issues/16007
synced_at: 2026-01-10T11:09:57Z
```

# [D214, D405, D417] False positive with arg keyword `attributes`

---

_Issue opened by @JP-Ellis on 2025-02-07 00:16_

## Summary

If using Google style and putting the argument's description on a new line, then if one argument is `attributes`, it raises D214 and D405.

I raised a similar issue previously (#8457), and this appears to another related edge case.

## Example

<details>
<summary><code>ruff.toml</code></summary>

```toml
[lint.pydocstyle]
convention = "google"
```

</details> 

```python
def send(payload: str, attributes: dict[str, Any]) -> None:
    """
    Send a message.

    Args:
        payload:
            The message payload.

        attributes:
            Additional attributes to be sent alongside the message.
    """
```

```console
❯ ruff check --config config.toml --select ALL ruff.py 
ruff.py:1:5: D417 Missing argument description in the docstring for `send`: `attributes`
  |
1 | def send(payload: str, attributes: dict[str, Any]) -> None:
  |     ^^^^ D417
2 |     """
3 |     Send a message.
  |
ruff.py:9:9: D405 [*] Section name should be properly capitalized ("attributes")
   |
 7 |             The message payload.
 8 |
 9 |         attributes:
   |         ^^^^^^^^^^ D405
10 |             Additional attributes to be sent alongside the message.
11 |     """
   |
   = help: Capitalize "attributes"

ruff.py:9:9: D214 [*] Section is over-indented ("attributes")
   |
 7 |             The message payload.
 8 |
 9 |         attributes:
   |         ^^^^^^^^^^ D214
10 |             Additional attributes to be sent alongside the message.
11 |     """
   |
   = help: Remove over-indentation from "attributes"

Found 6 errors.
```

_Note that I omitted a couple of expected errors (such as an undefined 'Any')_

<details>
<summary><code>ruff --version</code></summary>

```
ruff 0.9.3
```

</details>

## Related

This is related (nearly identical) to #8457.

---

_Label `bug` added by @ntBre on 2025-02-07 04:08_

---

_Label `docstring` added by @ntBre on 2025-02-07 04:08_

---

_Assigned to @ntBre by @ntBre on 2025-02-07 14:21_

---

_Comment by @ntBre on 2025-02-08 16:40_

I realized I never responded here, but thank you for the report! This looks like a bug, and I have a PR open to address it.

---

_Closed by @ntBre on 2025-02-11 17:05_

---

_Closed by @ntBre on 2025-02-11 17:05_

---
