```yaml
number: 2347
title: "`ty` missing `unresolved-attribute` on classes that inherit from `sqlalchemy.DeclarativeBase`"
type: issue
state: closed
author: rodda-kyusu
labels:
  - needs-info
assignees: []
created_at: 2026-01-05T15:46:18Z
updated_at: 2026-01-05T21:07:00Z
url: https://github.com/astral-sh/ty/issues/2347
synced_at: 2026-01-10T01:56:41Z
```

# `ty` missing `unresolved-attribute` on classes that inherit from `sqlalchemy.DeclarativeBase`

---

_Issue opened by @rodda-kyusu on 2026-01-05 15:46_

### Summary

Loving the beta!  Our type check time went from > 2 minutes to under 2 seconds.

We are experiencing `ty` not throwing the `unresolved-attribute` rule in cases where it seems it should.

Minimum example:

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import DeclarativeBase


class SampleModel(DeclarativeBase):
    pass


SampleModel.non_existent_attribute  # does not throw unresolved-attribute


class Sample:
    pass


Sample.non_existent_attribute   # throws unresolved-attribute
```

Believe I have a clean environment (can reproduce the above in a standalone file with no configuration etc):

```$ ty --version
ty 0.0.9 (f1652f05d 2026-01-05)
```

Thanks for the help!

(added after original posting)

Saw this issue https://github.com/astral-sh/ty/issues/384, but think it handles `ClassVar` behavior — didn't see anything else regarding declarative base.


### Version

ty 0.0.9 (f1652f05d 2026-01-05)

---

_Comment by @carljm on 2026-01-05 20:30_

Hmm, that's odd -- I can't reproduce this:

```
➜ mkdir issue2347 && cd issue2347 && uv init .
Initialized project `issue2347` at `/Users/carlmeyer/projects/ruff-examples/issue2347`

✘130 ➜ rm -f main.py && cat >main.py
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import DeclarativeBase


class SampleModel(DeclarativeBase):
    pass


SampleModel.non_existent_attribute

✘1 ➜ uv add sqlalchemy
Using CPython 3.13.2
Creating virtual environment at: .venv
Resolved 4 packages in 2ms
Installed 2 packages in 4ms
 + sqlalchemy==2.0.45
 + typing-extensions==4.15.0

➜ uvx ty@0.0.9 check .
error[unresolved-attribute]: Class `SampleModel` has no attribute `non_existent_attribute`
 --> main.py:9:1
  |
9 | SampleModel.non_existent_attribute
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```

Do your reproduction steps differ from the above? What version of SQLAlchemy are you using?

---

_Label `needs-info` added by @carljm on 2026-01-05 20:31_

---

_Comment by @rodda-kyusu on 2026-01-05 20:43_

Ah that's it; was on:

`sqlalchemy==2.0.35` and `typing-extensions==4.12.2`

Confirmed that upgrading fixed this, thanks!

---

_Closed by @rodda-kyusu on 2026-01-05 20:44_

---

_Comment by @rodda-kyusu on 2026-01-05 21:07_

Did a bit more investigation (for those who stumble upon this in the
future), as even though the example started working the code the example
was derived from remained to be not working.

Believe it's because that code was using `db.Model`, a pattern from
`flask-sqlalchemy`, which is defined here (
https://github.com/pallets-eco/flask-sqlalchemy/blob/168cb4b7b50fe5176307a10d873781bfafc6eeda/src/flask_sqlalchemy/extension.py#L244-L246).
This won't work I don't think regardless of version.  We are migrating to
inheriting from `DeclarativeBase` directly as a result — which is what's
recommend in SQLAlchemy 2.x anyways.



On Mon, Jan 5, 2026 at 3:31 PM Carl Meyer ***@***.***> wrote:

> *carljm* left a comment (astral-sh/ty#2347)
> <https://github.com/astral-sh/ty/issues/2347#issuecomment-3711986013>
>
> Hmm, that's odd -- I can't reproduce this:
>
> ➜ mkdir issue2347 && cd issue2347 && uv init .
> Initialized project `issue2347` at `/Users/carlmeyer/projects/ruff-examples/issue2347`
>
> ✘130 ➜ rm -f main.py && cat >main.py
> from sqlalchemy import Column, Integer, String
> from sqlalchemy.orm import DeclarativeBase
>
>
> class SampleModel(DeclarativeBase):
>     pass
>
>
> SampleModel.non_existent_attribute
>
> ✘1 ➜ uv add sqlalchemy
> Using CPython 3.13.2
> Creating virtual environment at: .venv
> Resolved 4 packages in 2ms
> Installed 2 packages in 4ms
>  + sqlalchemy==2.0.45
>  + typing-extensions==4.15.0
>
> ➜ uvx ***@***.*** check .
> error[unresolved-attribute]: Class `SampleModel` has no attribute `non_existent_attribute`
>  --> main.py:9:1
>   |
> 9 | SampleModel.non_existent_attribute
>   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
>   |
> info: rule `unresolved-attribute` is enabled by default
>
> Found 1 diagnostic
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/ty/issues/2347#issuecomment-3711986013>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/BCPHULU6VOXWVBQNUSDWWQ34FLCZLAVCNFSM6AAAAACQXRW5SSVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTOMJRHE4DMMBRGM>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---
