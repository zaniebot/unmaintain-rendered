```yaml
number: 574
title: "Type `<module 'requests'>` has no attribute `exceptions`"
type: issue
state: closed
author: RubenVanEldik
labels:
  - imports
assignees: []
created_at: 2025-06-03T09:48:34Z
updated_at: 2025-06-03T10:05:24Z
url: https://github.com/astral-sh/ty/issues/574
synced_at: 2026-01-12T15:54:23Z
```

# Type `<module 'requests'>` has no attribute `exceptions`

---

_@RubenVanEldik_

### Summary

Hi ty developers!

I am trying ty out in my own projects. It is blazingly fast, but I get a lot of `unresolved-import` errors. I get these errors on a range of modules, both external dependencies and internal modules. I am not sure if this is due the early alpha stage ty is in, or because I am doing something wrong.

A minimum working example is:

1. Install the dependencies:
```sh
pip install ty requests
```

2. Create main.py:
```py
import requests

print(requests.exceptions.RequestException)
```

3. Run ty:
```sh
ty check main.py

WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-attribute]: Type `<module 'requests'>` has no attribute `exceptions`
 --> main.py:3:7
  |
1 | import requests
2 |
3 | print(requests.exceptions.RequestException)
  |       ^^^^^^^^^^^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic

```



Am I making a very obvious mistake, or is this a known bug in ty that will be solved before the general release?

### Version

ty 0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Comment by @sharkdp on 2025-06-03 09:51_

Thank you for reporting this.

This looks like https://github.com/astral-sh/ty/issues/133? See also the last item in https://github.com/astral-sh/ty/issues/445.

---

_Label `imports` added by @sharkdp on 2025-06-03 09:51_

---

_Closed by @sharkdp on 2025-06-03 09:52_

---

_Comment by @AlexWaygood on 2025-06-03 09:52_

Thanks for the kind words!

The issue here is that ty does not yet model some of the side effects of Python's import system. As @sharkdp says, this is #133 -- the `requests.exceptions` submodule is loaded as an attribute onto the `requests` module at import time due to this import in `requests/__init__.py`: https://github.com/psf/requests/blob/7341690e842a23cf18ded0abd9229765fa88c4e2/src/requests/__init__.py#L45

---

_Comment by @RubenVanEldik on 2025-06-03 10:05_

Hi @sharkdp and @AlexWaygood,

Thanks for the quick response!

It is indeed the same issue as #133. I came across that issue when looking for similar problems, but I did not read it carefully enough and thought it was unrelated. Apologies!

For future readers, a summary of #133 is that `requests.exceptions.RequestException` completely works at runtime, but it uses a quirk in Python which is not a good practice. In my example, I could solve it by replacing it with `requests.RequestException`.

---
