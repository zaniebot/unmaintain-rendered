```yaml
number: 2285
title: "Narrowing across `with` statements is unsound"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - control flow
assignees: []
created_at: 2025-12-31T03:19:39Z
updated_at: 2025-12-31T11:03:13Z
url: https://github.com/astral-sh/ty/issues/2285
synced_at: 2026-01-12T15:54:26Z
```

# Narrowing across `with` statements is unsound

---

_@MeGaGiGaGon_

### Summary

While messing around trying to find general unsoundness, I found this. Both pyright and mypy correctly flag both functions, but ty misses the second one:
https://play.ty.dev/22b76c09-25ad-4e16-9733-edc338b69a26
```py
def generate_str() -> str:
    raise NotImplementedError

def uses_try(x: int) -> str:
    y: int | str = x
    try:
        y = generate_str()
    except NotImplementedError:
        pass
    return y  # Errors with invalid-return-type as expected

class MySuppressor:
    def __enter__(self) -> None: ...
    def __exit__(self, exc_type, exc_value, traceback) -> bool:
        return True

def uses_custom(x: int) -> str:
    y: int | str = x
    with MySuppressor():
        y = generate_str()
    return y  # No error, should also be invalid-return-type
```
[pyright playground](https://pyright-play.net/?pythonVersion=3.12&code=CYUwZgBA5iB2ICcCGAXEB9AzihAKAlBALQB8E2CAXAFAR0TICWmIEAcgPYoCSAtgA4AbELzhpgAUQQIOCatVCQAri0zocAT1wAPShEawUhUuRw16EDXoMoIAH1MIIAXgjba9TeYv0NL6HCIqBgUBB50INoAxiD8tpw8AsKihiCS0rLePhD8SJiY4QwgKEoIsJZ0AMQQUjIImBAA7owoABb6sABuSIKMwEQIxaWwRCga-Kx5EJETUeLyUYJ5DQCyGgDKSvz8g-mZhYoQ6OhiiMe4LIJgxmSc8HoAdE8H4Ecn2i3nl2AANNPR6nGID%2BkSi6G6giUwIgOCQMQARnCANY3CDwjgcQRZHyDEplCAAFQQUPkhxUIDUURUKA4vB01kMqIo2KsHVsDgo-ncFmabQga02212mFkBGxFj8rhg8GQaCwODCFlxwwqEGqnGmGQQf0wrQ4SkEwAgPRFaNYBghfQGQzKoyB1CAA)
https://mypy-play.net/?mypy=latest&python=3.12&gist=decd8c0156ecb1ab8e42ea823f5573aa


### Version

ty 0.0.8 (aa7559db8 2025-12-29) playground 9dadf2724

---

_Label `control flow` added by @mtshiba on 2025-12-31 03:32_

---

_Assigned to @mtshiba by @mtshiba on 2025-12-31 03:32_

---

_Comment by @AlexWaygood on 2025-12-31 11:03_

Thanks! This is #152

---

_Closed by @AlexWaygood on 2025-12-31 11:03_

---
