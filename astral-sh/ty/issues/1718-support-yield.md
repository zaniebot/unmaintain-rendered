```yaml
number: 1718
title: "Support `yield`"
type: issue
state: open
author: MichaReiser
labels:
  - type-inference
assignees: []
created_at: 2025-12-02T13:34:21Z
updated_at: 2025-12-02T18:53:03Z
url: https://github.com/astral-sh/ty/issues/1718
synced_at: 2026-01-12T15:54:25Z
```

# Support `yield`

---

_@MichaReiser_

Stop inferring `Todo` for:

```py
async def test():
    a = yield 10

```

https://github.com/astral-sh/ruff/blob/7b1e530a6d424c967588ffdffac89b3d5924c88c/crates/ty_python_semantic/src/types/infer/builder.rs#L8420

https://play.ty.dev/f25888e2-1782-469d-9e60-4367d30452a8

---

_Label `type-inference` added by @MichaReiser on 2025-12-02 13:34_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-02 13:34_

---

_Comment by @carljm on 2025-12-02 16:29_

The return type of a generator function (any function containing `yield`) is a `typing.Generator` (or `typing.AsyncGenerator` if its an `async def`); this type must be assignable to the annotated return type.

The type of a `yield` expression inside the function is the second type argument of the annotated `Generator` or `AsyncGenerator` return type, for example:

```py
from typing import Generator

def unannotated():
    x = yield 10
    reveal_type(x)  # revealed: Unknown

def default_specialized() -> Generator:
    x = yield 10
    # The `_SendT_contra` typevar on `Generator` has `default=None`
    reveal_type(x)  # revealed: None

def partially_specialized() -> Generator[int]:
    x = yield 10
    # The first type parameter of `Generator` is the yielded type (so `10` here must be assignable to it)
    # The second type parameter is the "send" type, which is still default-specialized to `None` here:
    reveal_type(x)  # revealed: None

def fully_specialized() -> Generator[int, str]:
    x = yield 10
    reveal_type(x)  # revealed: str

def iterator() -> Iterator[int]:
    x = yield 10
    # The `Iterator` return type here is a supertype of `Generator`.
    # It has no "send" type, nor a send method, so this should always be `None`
    reveal_type(x)  # revealed: None
```

All those examples would look the same for an `async def`, except the return type would need to be `AsyncGenerator`, not `Generator`.

---

_Comment by @sharkdp on 2025-12-02 16:33_

If someone implements this, take a look at `Type::generator_return_type`, which does something similar for the return type instead of the send type.

---

_Comment by @AlexWaygood on 2025-12-02 16:35_

> The return type of a generator function (any function containing `yield`) must be assignable to `typing.Generator` (or `typing.AsyncGenerator` if its an `async def`).

Really? No other type checker appears to enforce this rule. They all allow generator functions to return `Iterator` (which is a supertype of `Generator`, not a subtype). [Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=ab5ec9cd5ab6e817e95790342db2d8e3), [pyright](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&reportUnreachable=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoCSMApiAIYy4BQVAJscFMABQAeAlFALQB8hJ5SiADaqGAF0AXFSiyocKAF55SYgBtaUACwAmGXJDEYAVxAp5VIA), [pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN0BJBjEqoGvADropmGGDpgAFAEo6AWgB8g4aPGUA2qwYBdRFLoW6pOgF4rEGFEx0ALACZzlyjAYBXSuhWUiAANCC%2BDNBwJOSIIADEdACqkVAQTAq%2B6ADGkbjocFIycgq8NGIA%2Bui%2BNNgiiviInOgMqpp0cAyUZoFePv6BYBIgAHI1dd10wPgAvsPBYWTeYFCkhOI0UBSJAAqky6sdGDgEdNn5kGz%2BYhD5hFKJAMowMHQAFgwMxHCIAPS-Szkq0IvDYvxg6F%2BmFw2Tgv3O6Eu1zykNKlDoqAAbqhoKhsLAzhcIFddLdArhiCjolIyAw3vk1JiRHAybY6MMAMyEACMHhAwRmYVQuQgTIAYtAYBQ0Fg8EQyCAZkA).

---

_Comment by @AlexWaygood on 2025-12-02 16:37_

Did you mean to say that for any generator function, `types.GeneratorType` (or `types.AsyncGeneratorType` if it's an `async def` function) must be assignable to the return type of the function?

---

_Comment by @AlexWaygood on 2025-12-02 16:48_

> Did you mean to say that for any generator function, `types.GeneratorType` (or `types.AsyncGeneratorType` if it's an `async def` function) must be assignable to the return type of the function?

(we already implement this check: https://play.ty.dev/e79542b2-1f5e-4c15-bc82-fd8ecf182c1f)

---

_Comment by @carljm on 2025-12-02 16:50_

Yes, that bit was just supposed to be context for the rest, not describing what needs to be implemented still. Fixed the wording and added an example with `Iterator`.

---
