```yaml
number: 1033
title: unresolved-attribute in pyjwt
type: issue
state: closed
author: CharlesPerrotMinot
labels: []
assignees: []
created_at: 2025-08-18T05:02:54Z
updated_at: 2025-08-18T15:34:19Z
url: https://github.com/astral-sh/ty/issues/1033
synced_at: 2026-01-12T15:54:24Z
```

# unresolved-attribute in pyjwt

---

_@CharlesPerrotMinot_

### Summary

We're getting an unresolved-attribute with the pyjwt library
```
error[unresolved-attribute]: Type `<module 'jwt'>` has no attribute `exceptions`
    |
157 |             options={"verify_exp": True},
158 |         )
159 |     except jwt.exceptions.PyJWTError as err:
    |            ^^^^^^^^^^^^^^
160 |         raise Unauthorized from err
161 |     claims = Claims(**claims)
    |
```
Except this module does exist?
https://github.com/jpadilla/pyjwt/blob/master/jwt/exceptions.py

Sample code:
```
def token_issued(token: str) -> bool:
    try:
        claims = jwt.decode(token, options={"verify_signature": False})
    except jwt.exceptions.PyJWTError as err:
        raise Unauthorized from err
    return claims.get("iss") == "platform"
```

No ty configuration, and using uv for dependencies

### Version

[0.0.1-alpha.18](https://github.com/astral-sh/ty/releases/tag/0.0.1-alpha.18)

---

_Comment by @MichaReiser on 2025-08-18 06:25_

Can you share some more details, ideally a minimal reproducible example? 

* the code containing the try..except with the problematic `jwt.exceptins` attribute access
* Your ty configuration (if any)
* What and how you installed your dendencies
* What ty version you're using

---

_Comment by @sharkdp on 2025-08-18 08:43_

Do you have an explicit import for `jwt.exceptions`? If not, this looks like a duplicate of https://github.com/astral-sh/ty/issues/133

---

_Comment by @CharlesPerrotMinot on 2025-08-18 15:31_

I double checked and @sharkdp is right, it's indeed a duplicate of that issue, thanks!

Edit: added the answers to @MichaReiser question just in case if needed in the future

---

_Closed by @CharlesPerrotMinot on 2025-08-18 15:31_

---
