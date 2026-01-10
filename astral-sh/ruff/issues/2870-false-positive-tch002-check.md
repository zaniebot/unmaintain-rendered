---
number: 2870
title: False positive TCH002 check
type: issue
state: closed
author: Olegt0rr
labels: []
assignees: []
created_at: 2023-02-13T21:23:58Z
updated_at: 2023-02-13T21:43:31Z
url: https://github.com/astral-sh/ruff/issues/2870
synced_at: 2026-01-10T01:22:41Z
---

# False positive TCH002 check

---

_Issue opened by @Olegt0rr on 2023-02-13 21:23_

Since I import `cast` and use it in the code - `TCH002` rule stop working...

## Wrong behaviour (with casting)

```python
from typing import cast

from aiogram.types import Message, User


async def handle_start(message: Message) -> None:
    """Handle /start command."""
    user = cast(User, message.from_user)
    await message.answer(f"Hi, {user.full_name}")

```

There's no `TCH002` error about `Message`


## Right behaviour (without casting)

```python
from aiogram.types import Message


async def handle_start(message: Message) -> None:
    """Handle /start command."""
    user = message.from_user
    await message.answer(f"Hi, {user.full_name}")

```

TCH002 Move third-party import `aiogram.types.Message` into a type-checking block

---

_Comment by @spaceone on 2023-02-13 21:29_

I think that is correct:
```python
from typing import TYPE_CHECKING, cast


if TYPE_CHECKING:
        from aiogram.types import Message, User


def handle_start(message: "Message") -> None:
    """Handle /start command."""
    user = cast(User, message.from_user)
    message.answer(f"Hi, {user.full_name}")


handle_start(type('foo', (object,), {'answer:': lambda m: print(m)}))
```

```
$ python3 foo.py
Traceback (most recent call last):
  File "foo.py", line 14, in <module>
    handle_start(type('foo', (object,), {'answer:': lambda m: print(m)}))
  File "foo.py", line 10, in handle_start
    user = cast(User, message.from_user)
NameError: name 'User' is not defined
```

---

_Comment by @Olegt0rr on 2023-02-13 21:31_

You forgot to quote it...

try this:

```python
from typing import TYPE_CHECKING, cast
from aiogram.types import User

if TYPE_CHECKING:
    from aiogram.types import Message


def handle_start(message: "Message") -> None:
    """Handle /start command."""
    user = cast(User, message.from_user)
    message.answer(f"Hi, {user.full_name}")
```

---

_Comment by @spaceone on 2023-02-13 21:37_

alright, only `Message` should be in the `TYPE_CHECKING` block.

---

_Comment by @Olegt0rr on 2023-02-13 21:38_

So in my first message in `Wrong behaviour` section there's no TCH002 error

---

_Comment by @charliermarsh on 2023-02-13 21:39_

@Olegt0rr - This is because we have "strict mode" disabled by default. So if you import multiple members from a module, and at least one of them is required at runtime, we don't flag the others.

You can change this behavior like:

```toml
[tool.ruff.flake8-type-checking]
strict = true
```

Which gives you:

```
foo.py:3:27: TCH002 Move third-party import `aiogram.types.Message` into a type-checking block
```

---

_Closed by @charliermarsh on 2023-02-13 21:39_

---

_Comment by @Olegt0rr on 2023-02-13 21:41_

@charliermarsh, thanks for the explanation!

---

_Comment by @charliermarsh on 2023-02-13 21:43_

@Olegt0rr - No prob! Let me know if you run into issues with it. The reasoning, IIRC, is that if you're _already_ importing `aiogram.types` to get `User`, it's no more expensive to import `Message` too at runtime, so it's often not considered worthwhile to enforce.

---
