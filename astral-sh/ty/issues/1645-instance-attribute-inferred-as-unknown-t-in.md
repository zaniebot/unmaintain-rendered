```yaml
number: 1645
title: "Instance attribute inferred as `Unknown | T` in method despite being assigned concrete type in `__init__`"
type: issue
state: closed
author: sinon
labels:
  - question
  - attribute access
assignees: []
created_at: 2025-11-26T11:53:18Z
updated_at: 2025-11-26T14:35:51Z
url: https://github.com/astral-sh/ty/issues/1645
synced_at: 2026-01-12T15:54:25Z
```

# Instance attribute inferred as `Unknown | T` in method despite being assigned concrete type in `__init__`

---

_@sinon_

### Summary

#### Problem

When investigating adopting `ty` in an existing codebase which is currently checked by `mypy` and `pyright`. I get following odd case where an sttribute defined in `__init__` has the expected type but when this attribute is then accessed in a method it gains an `| Unknown` which then seems to trigger `missing-argument` error as it seems to get confused about whether the `self` is present.

##### Expected Behavior
`self._jwk_client` should have type `PyJWKClient` in both `__init__` and `_get_signing_key`.

##### Actual Behavior
- In `__init__`: type is correctly `PyJWKClient`
- In `_get_signing_key`: type becomes `Unknown | PyJWKClient`
- This causes spurious `missing-argument` errors


#### MRE

```toml
[project]
name = "mre-pyjwt"
version = "0.1.0"
description = "MRE for ty issue"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [  "pyjwt[crypto]>=2.10.0",]
```

```py
from typing import reveal_type

import jwt


class JWTVerifier:
    def __init__(
        self,
        *,
        jwk_set_url: str,
    ) -> None:
        self._jwk_client = jwt.PyJWKClient(
            jwk_set_url,
        )
        reveal_type(self._jwk_client)  # PyJWKClient

    def _get_signing_key(self, token: str) -> jwt.PyJWK:
        kid = jwt.get_unverified_header(token)["kid"]
        reveal_type(self._jwk_client)  # Unknown | PyJWKClient
        self._jwk_client.get_signing_key(kid) # error[missing-argument] No argument provided for required parameter `kid` of function `get_signing_key`
        return self._jwk_client.get_signing_key(kid=kid) # error[missing-argument] No argument provided for required parameter `self` of function `get_signing_key`
```

### Expected Behavior
`self._jwk_client` should have type `PyJWKClient` in both `__init__` and `_get_signing_key`.

### Actual Behavior
- In `__init__`: type is correctly `PyJWKClient`
- In `_get_signing_key`: type becomes `Unknown | PyJWKClient`
- This causes spurious `missing-argument` errors

It's present in all versions from 0.0.1-alpha.24 onwards, where the `self._jwk_client` goes from being `Unknown` / `Unknown` to `PyJWKClient` / `PyJWKClient | Unknown`

```sh
➜ uv tool run ty@0.0.1-alpha.23 check . --output-format concise
main.py:15:21: info[revealed-type] Revealed type: `Unknown`
main.py:19:21: info[revealed-type] Revealed type: `Unknown`
Found 2 diagnostics

➜ uv tool run ty@0.0.1-alpha.24 check . --output-format concise
main.py:15:21: info[revealed-type] Revealed type: `PyJWKClient`
main.py:19:21: info[revealed-type] Revealed type: `Unknown | PyJWKClient`
main.py:20:9: error[missing-argument] No argument provided for required parameter `kid` of function `get_signing_key`
main.py:21:16: error[missing-argument] No argument provided for required parameter `self` of function `get_signing_key`
Found 4 diagnostics
```

I assume this isn't strictly related to the upstream library specifically as `ty` is correctly detecting the `PyJWKClient` in the `__init__` 🤔 (my naive attempts to reproduce in ty playground without pyjwk did fail that but that is potentially a skill issue on my part)

Please let me know if I can provide any further details to help debug, apologies if this has already been reported searching on `missing-argument` didn't turn up anything that quite fit.

### Version

0.0.1-alpha.24-28

---

_Comment by @sharkdp on 2025-11-26 12:09_

Thank you for reporting this.

*Within* `__init__`, you see the locally narrowed type of `PyJWKClient`. But since `_jwk_client` is not annotated, it might be externally mutated (I guess we could consider making exceptions for "private" attributes?). This is why we union with `Unknown` for the type of the attribute when accessed in other methods (see https://github.com/astral-sh/ty/issues/1515#issuecomment-3512306787 for a more detailed explanation).

You can either annotate `_jwk_client: jwt.PyJWKClient` on the class body, or annotate the attribute assignment (`self._jwk_client: jwt.PyJWKClient = jwt.PyJWKClient(…)`) to fix this.

See https://github.com/astral-sh/ty/issues/1515 for a similar discussion.

---

_Label `question` added by @sharkdp on 2025-11-26 12:09_

---

_Label `attribute access` added by @sharkdp on 2025-11-26 12:09_

---

_Comment by @sinon on 2025-11-26 12:20_

I had tried specifying the type explicitly (even adding a cast) but still get the same errors

```py
class JWTVerifier:
    def __init__(
        self,
        *,
        jwk_set_url: str,
    ) -> None:
        self.jwk_client: jwt.PyJWKClient = jwt.PyJWKClient(
            jwk_set_url,
        )
        reveal_type(self.jwk_client)  # PyJWKClient

    def get_signing_key(self, token: str) -> jwt.PyJWK:
        kid = jwt.get_unverified_header(token)["kid"]
        reveal_type(self.jwk_client)  # PyJWKClient
        self.jwk_client = cast(jwt.PyJWKClient, self.jwk_client)  # warning[redundant-cast] Value is already of type `PyJWKClient`
        self.jwk_client.get_signing_key(kid)  # error[missing-argument] No argument provided for required parameter `kid` of function `get_signing_key`
        return self.jwk_client.get_signing_key(kid=kid)  # error[missing-argument] No argument provided for required parameter `self` of function `get_signing_key`
```
or
```py
class JWTVerifier:
    def __init__(
        self,
        *,
        jwk_set_url: str,
    ) -> None:
        self.jwk_client: jwt.PyJWKClient = jwt.PyJWKClient(
            jwk_set_url,
        )
        reveal_type(self.jwk_client)  # PyJWKClient

    def get_signing_key(self, token: str) -> jwt.PyJWK:
        kid = jwt.get_unverified_header(token)["kid"]
        reveal_type(self.jwk_client)  # PyJWKClient
        self.jwk_client = cast(jwt.PyJWKClient, self.jwk_client)
        client: jwt.PyJWKClient = self.jwk_client
        client.get_signing_key(kid)
        return client.get_signing_key(kid=kid)
```

So they get rid of the `Unknown` but not the errors 🤔 


In terms of other typecheckers (for the original MRE not above code) I see following behaviour:
`mypy` - they are actually both revealed as `Any` (though this might just not be picking up the venvs packages when running with `uv tool`)
`basedpyright` (easier to run to `uv tool`) reports `PyJWKClient` for each reveal (various infos/warns due to additional default rules but not the error `ty` experiences)
`pyrefly` - has `PyJWKClient` and `PyJWKClient | Unknown` but no errors 


---

_Comment by @sharkdp on 2025-11-26 14:25_

Oh, I just now recognized `PyJWKClient`. I actually saw the same error in another project yesterday.

The problem is that the `get_signing_key` method is overwritten here: https://github.com/jpadilla/pyjwt/blob/f7b872edd08c2584f06efe90715495c401684063/jwt/jwks_client.py#L50-L52

We thus infer `jwks_client.get_signing_key` as a union of the bound method and the plain function:
```
(bound method PyJWKClient.get_signing_key(kid: str) -> PyJWK) | (def get_signing_key(self, kid: str) -> PyJWK)
```

So I think this is actually a bug in ty, see https://github.com/astral-sh/ty/issues/350.

---

_Closed by @sharkdp on 2025-11-26 14:25_

---

_Comment by @sinon on 2025-11-26 14:35_

> Oh, I just now recognized `PyJWKClient`. I actually saw the same error in another project yesterday.
> 
> The problem is that the `get_signing_key` method is overwritten here: https://github.com/jpadilla/pyjwt/blob/f7b872edd08c2584f06efe90715495c401684063/jwt/jwks_client.py#L50-L52
> 
> We thus infer `jwks_client.get_signing_key` as a union of the bound method and the plain function:
> 
> ```
> (bound method PyJWKClient.get_signing_key(kid: str) -> PyJWK) | (def get_signing_key(self, kid: str) -> PyJWK)
> ```
> 
> So I think this is actually a bug in ty, see [#350](https://github.com/astral-sh/ty/issues/350).

Ah, nice thank you @sharkdp! Good to see this is scheduled for Stable milestone so I can safely link #350 when suppressing the error 

---
