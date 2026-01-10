```yaml
number: 1425
title: Third-party dataclass-like libraries (pydantic, attrs) will sometimes treat unannotated assignments as dataclass fields
type: issue
state: open
author: franzkurt
labels:
  - dataclasses
  - library
assignees: []
created_at: 2025-10-23T17:22:06Z
updated_at: 2025-12-01T23:38:58Z
url: https://github.com/astral-sh/ty/issues/1425
synced_at: 2026-01-10T01:58:59Z
```

# Third-party dataclass-like libraries (pydantic, attrs) will sometimes treat unannotated assignments as dataclass fields

---

_Issue opened by @franzkurt on 2025-10-23 17:22_

### Summary

For example if my class is a validator like
```python
from pydantic import BaseModel

class ResponseOk(BaseModel):
    status_code = Field(..., alias='$status')
```
`ty` provide the error even enabling the config `allow_population_by_field_name`

```bash
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                                                            error[unknown-argument]: Argument `status_code` does not match any known parameter
 --> /Users/jusbrasil/Desktop/issue.py.py:6:12
  |
4 |     status_code = Field(..., alias='$status')
5 |
6 | ResponseOk(status_code=200)
  |            ^^^^^^^^^^^^^^^
  |
info: rule `unknown-argument` is enabled by default
Found 1 diagnostic
```


### Version

ty 0.0.1a24

---

_Label `dataclasses` added by @AlexWaygood on 2025-10-23 17:22_

---

_Added to milestone `Beta` by @carljm on 2025-10-23 18:36_

---

_Comment by @carljm on 2025-10-23 18:36_

As far as I can see, the `alias` option isn't relevant here; we don't seem to be recognizing `status_code` as a field at all, even if the `alias` option is not used. I'm not sure why, since we support `field_specifiers`; this case doesn't appear to fall under any of the missing features in https://github.com/astral-sh/ty/issues/1327

```py
from pydantic import BaseModel, Field

class ResponseOk(BaseModel):
    status_code = Field()

ex = ResponseOk(status_code=200)  # I get `unknown-argument` here
```

Tentatively putting this in beta milestone pending further clarification/investigation, since it seems important that we support pydantic model fields.

---

_Renamed from "Ty not recognize `pydantic alias` when validating rule `unknown-argument`" to "Not recognizing pydantic model fields" by @carljm on 2025-10-23 18:38_

---

_Comment by @sharkdp on 2025-10-23 18:53_

I think we probably just treat *annotated* attributes as fields.

I started to write a Pydantic-compatibility test suite, so hopefully we can soon get a clearer picture of what is missing.

---

_Comment by @AlexWaygood on 2025-10-24 10:52_

I agree that it would be great if we could do better here, but I think doing so will require pydantic-specific special casing that goes above and beyond the `dataclass_transform` spec. Both mypy and pyright also fail to recognise the unannotated field as a dataclass field here:

```sh
~/dev % uvx --with=pydantic pyright foo.py                                                                          
Installed 7 packages in 143ms
/Users/alexw/dev/foo.py
  /Users/alexw/dev/foo.py:4:19 - error: Dataclass field without type annotation will cause runtime exception (reportGeneralTypeIssues)
  /Users/alexw/dev/foo.py:6:12 - error: No parameter named "status_code" (reportCallIssue)
2 errors, 0 warnings, 0 informations
~/dev [1] % uvx --with=pydantic mypy foo.py   
Installed 8 packages in 27ms
foo.py:6: error: Unexpected keyword argument "status_code" for "ResponseOk"  [call-arg]
Found 1 error in 1 file (checked 1 source file)
```

This makes sense if all you've implemented is the `dataclass_transform` spec, since unannotated assignments do not constitute fields in the stdlib `dataclasses` module, and `dataclass_transform` implementations are basically assumed by type checkers to work mostly the same way as the stdlib `dataclasses` module:

```pycon
>>> from dataclasses import dataclass, field
>>> @dataclass
... class A:
...     x = field(default=0)
...     
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    @dataclass
     ^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/dataclasses.py", line 1305, in dataclass
    return wrap(cls)
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/dataclasses.py", line 1295, in wrap
    return _process_class(cls, init, repr, eq, order, unsafe_hash,
                          frozen, match_args, kw_only, slots,
                          weakref_slot)
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/dataclasses.py", line 1032, in _process_class
    raise TypeError(f'{name!r} is a field but has no type annotation')
TypeError: 'x' is a field but has no type annotation
>>> @dataclass
... class B:
...     x = 42
...     
>>> B(X=56)
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    B(X=56)
    ~^^^^^^
TypeError: B.__init__() got an unexpected keyword argument 'X'
```

Pyrefly does not emit the same error as mypy/pyright/ty on this snippet, but from chatting with them at PyCon UK I believe they've been working quite hard on implementing custom support for pydantic/django/similar libraries, so this doesn't surprise me too much.

---

_Label `dataclasses` removed by @AlexWaygood on 2025-10-24 10:53_

---

_Label `library` added by @AlexWaygood on 2025-10-24 10:53_

---

_Comment by @MichaReiser on 2025-10-24 10:56_

[Docs about Pyrefly's pydantic support](https://pyrefly.org/en/docs/pydantic/)

---

_Renamed from "Not recognizing pydantic model fields" to "Not recognizing unannotated pydantic model fields" by @AlexWaygood on 2025-10-24 11:48_

---

_Comment by @carljm on 2025-10-24 15:57_

Thanks for the analysis! Totally missed the missing annotation. Moving this back out of beta milestone for now; it seems like what we are doing follows the spec. We may want to add further dedicated pydantic support, but we should discuss this, and it doesn't seem like a blocker for beta.

---

_Removed from milestone `Beta` by @carljm on 2025-10-24 15:57_

---

_Label `dataclasses` added by @sharkdp on 2025-10-27 12:43_

---

_Comment by @dhermes on 2025-10-27 17:21_

This appears to be a regression in `0.0.1a24`. I was using `0.0.1a21` and had no issue and now on `0.0.1a24` this issue appears. I just checked `0.0.1a23` and the issue didn't occur there either. E.g. for

```python
class FirebaseClaims(pydantic.BaseModel):
    model_config = pydantic.ConfigDict(extra="ignore", populate_by_name=True)

    subject: str = pydantic.Field(alias="sub")
    audience: str = pydantic.Field(alias="aud")
    exp: int
    iat: int
    name: str | None = pydantic.Field(default=None)
    email: str | None = pydantic.Field(default=None)
    email_verified: bool | None = pydantic.Field(default=None)

    @property
    def issued_at(self) -> datetime.datetime:
        return datetime.datetime.fromtimestamp(self.iat, datetime.UTC)

    @property
    def expires_at(self) -> datetime.datetime:
        return datetime.datetime.fromtimestamp(self.exp, datetime.UTC)
```

the following code

```python
    expected = middleware.FirebaseClaims(
        subject="7vH1VaTSXC2tTM67LooOkYmS78qc",
        audience="manticore-474331",
        exp=now + 1800,
        iat=now - 1800,
        name="Terry Crews 833",
        email="terry.crews833@mail.invalid",
        email_verified=True,
    )
```

now fails with:

```
error[missing-argument]: No arguments provided for required parameters `sub`, `aud`
   --> tests/unit/api/test_middleware.py:312:16
    |
310 |       middleware._require_firebase_claims(request, verify_firebase_token)
311 |       claims = middleware.get_firebase_claims(request)
312 |       expected = middleware.FirebaseClaims(
    |  ________________^
313 | |         subject="7vH1VaTSXC2tTM67LooOkYmS78qc",
314 | |         audience="manticore-474331",
315 | |         exp=now + 1800,
316 | |         iat=now - 1800,
317 | |         name="Terry Crews 833",
318 | |         email="terry.crews833@mail.invalid",
319 | |         email_verified=True,
320 | |     )
    | |_____^
321 |       assert claims == expected
    |
info: rule `missing-argument` is enabled by default

error[unknown-argument]: Argument `subject` does not match any known parameter
   --> tests/unit/api/test_middleware.py:313:9
    |
311 |     claims = middleware.get_firebase_claims(request)
312 |     expected = middleware.FirebaseClaims(
313 |         subject="7vH1VaTSXC2tTM67LooOkYmS78qc",
    |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
314 |         audience="manticore-474331",
315 |         exp=now + 1800,
    |
info: rule `unknown-argument` is enabled by default

error[unknown-argument]: Argument `audience` does not match any known parameter
   --> tests/unit/api/test_middleware.py:314:9
    |
312 |     expected = middleware.FirebaseClaims(
313 |         subject="7vH1VaTSXC2tTM67LooOkYmS78qc",
314 |         audience="manticore-474331",
    |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^
315 |         exp=now + 1800,
316 |         iat=now - 1800,
    |
info: rule `unknown-argument` is enabled by default
```

---

_Comment by @sharkdp on 2025-10-27 18:03_

@dhermes what you are seeing is a distinct issue, please see #1438.

---

_Comment by @AlexWaygood on 2025-11-29 14:32_

It seems like `attrs` will also treat some unnanotated assignments in the class body as creating dataclass fields, and we will similarly fail to correctly understand these classes currently:

```py
import attr

@attr.attrs
class VersionInfo:
    year = attr.attrib(type=int)

VersionInfo(year=42)  # error[unknown-argument] Argument `year` does not match any known parameter
```

If you change the `year` assignment to `year: int = attr.attrib(type=int)`, the false-positive error goes away.

This came up in https://github.com/astral-sh/ruff/pull/21685.

---

_Renamed from "Not recognizing unannotated pydantic model fields" to "Third-party dataclass-like libraries (pydantic, attrs) will sometimes treat unannotated assignments as dataclass fields" by @AlexWaygood on 2025-11-29 14:33_

---

_Added to milestone `Stable` by @carljm on 2025-12-01 23:38_

---
