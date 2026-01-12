```yaml
number: 404
title: Support for argument unpacking
type: issue
state: closed
author: stfwn
labels: []
assignees: []
created_at: 2025-05-15T09:12:31Z
updated_at: 2025-05-15T09:20:07Z
url: https://github.com/astral-sh/ty/issues/404
synced_at: 2026-01-12T15:54:23Z
```

# Support for argument unpacking

---

_@stfwn_

### Summary

Thanks for making `ty`, it's very fast! :)

It would be great to have some level of support for argument unpacking. This seems pretty complex to me the more I think about it, but I thought it would be worth opening an issue for it anyway.

For example:

```python
def foo(a: int) -> None:
    pass


kwargs = {"a": 1}
foo(**kwargs)

args = [1]
foo(*args)
```

Currently gives:

```
error[missing-argument]: No argument provided for required parameter `a` of function `foo`
 --> test.py:6:1
  |
5 | kwargs = {"a": 1}
6 | foo(**kwargs)
  | ^^^^^^^^^^^^^
7 |
8 | args = [1]
  |
info: rule `missing-argument` is enabled by default

error[missing-argument]: No argument provided for required parameter `a` of function `foo`
 --> test.py:9:1
  |
8 | args = [1]
9 | foo(*args)
  | ^^^^^^^^^^
  |
info: rule `missing-argument` is enabled by default

Found 2 diagnostics
```

Intuitively it might not be possible to support all cases of argument unpacking (who knows what's in a dict/iterable at any given time!). But for context, I ran into this when I was doing this common pattern:

```
if __name__ == '__main__':
  parser = ...
  parser.add_argument(...)
  args = parser.parse_args()
  main(**vars(args))
```

And perhaps this pattern is a candidate to support as a special case.

### Version

ty 0.0.1-alpha.2 (59c45cc60 2025-05-14)

---

_Comment by @sharkdp on 2025-05-15 09:20_

Thank you for your report. Please see #247.

---

_Closed by @sharkdp on 2025-05-15 09:20_

---
