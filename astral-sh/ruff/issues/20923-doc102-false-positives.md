```yaml
number: 20923
title: DOC102 false positives
type: issue
state: open
author: ntBre
labels:
  - rule
assignees: []
created_at: 2025-10-16T14:45:25Z
updated_at: 2025-10-16T21:16:28Z
url: https://github.com/astral-sh/ruff/issues/20923
synced_at: 2026-01-10T11:10:00Z
```

# DOC102 false positives

---

_Issue opened by @ntBre on 2025-10-16 14:45_

There were two remaining false positives found in the [ecosystem check](https://github.com/astral-sh/ruff/pull/20376#issuecomment-3288875538) on #20376.

+ [disnake/client.py:814:9:](https://github.com/DisnakeDev/disnake/blob/613e01d45d8eb0034a4f3fd524b64752e55b0b38/disnake/client.py#L814) DOC102 Documented parameter `Example` is not in the function's signature
  We look for a section called Example**s**, but I think it would be worth allowing singular Example too. I think this one is not specific to DOC102 and can affect other rules.
+ [superset/async_events/api.py:57:11:](https://github.com/apache/superset/blob/2f8657f122f4a40c1a8cddb6be989aead77fa752/superset/async_events/api.py#L57) DOC102 Documented parameter `responses` is not in the function's signature
  This and several other superset errors are from docstrings like
  ```py
          """
        Read off of the Redis async events stream, using the user's JWT token and
        optional query params for last event received.
        ---
        get:
          summary: Read off of the Redis events stream
          description: >-
            Reads off of the Redis events stream, using the user's JWT token and
            optional query params for last event received.
          parameters:
          - in: query
            name: last_id
            description: Last ID received by the client
            schema:
                type: string
          responses:
            200:
  ```
  which I guess looks like YAML. I feel like we could do something like not process sub-sections under an unknown heading to filter this out, but I haven't taken a closer look.

---

_Label `rule` added by @ntBre on 2025-10-16 14:45_

---

_Comment by @augustelalande on 2025-10-16 17:55_

FYI for the second case, it is not an unknown heading. The problem is that we recognize `parameters:` as a valid heading.

---

_Comment by @pep-sanwer on 2025-10-16 21:12_

A few more false positives related to less commonly used `numpy` [docstring sections](https://numpydoc.readthedocs.io/en/latest/format.html#sections)

From my testing it looks like the following valid sections are not being recognized as sections:
- `Receives`
- `Warns`
- `Warnings`

---

Given `example.py` like:
```python
def bad_doc102(x: int) -> int:
    """
    Show a `Warnings` DOC102 false positive.

    Parameters
    ----------
    x : int

    Warnings
    --------
    This function demonstrates a DOC102 false positive

    Returns
    -------
    int
    """
    return x
```
results in the false positive
```shell
✗ ruff check example.py
example.py:
  9:5 DOC102 Documented parameter `Warnings` is not in the function's signature

Found 1 error.
```

---

Given `example.py` like:
```python
def bad_doc102(x: int) -> int:
    """
    Show another `Warnings` DOC102 false positive.

    Parameters
    ----------
    x : int

    Raises
    ------
    ValueError
        If `x` is zero

    Warnings
    --------
    This function demonstrates a DOC102 false positive

    Returns
    -------
    int
    """
    if x == 0:
        msg = "x cannot be zero"
        raise ValueError(msg)

    return x
```
results in the false positive
```shell
✗ ruff check example.py
example.py:
  2:5 DOC502 Raised exceptions are not explicitly raised: `Warnings`, `--------`, `This function demonstrates a DOC102 false positive`

Found 1 error.
```

---

_Comment by @augustelalande on 2025-10-16 21:16_

I can make a quick PR to add those

---
