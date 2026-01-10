```yaml
number: 14052
title: RET504 false-positive when nested functions reference the variable
type: issue
state: closed
author: Enegg
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-11-01T19:00:28Z
updated_at: 2025-07-10T20:10:24Z
url: https://github.com/astral-sh/ruff/issues/14052
synced_at: 2026-01-10T11:09:55Z
```

# RET504 false-positive when nested functions reference the variable

---

_Issue opened by @Enegg on 2024-11-01 19:00_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I've searched for `RET504` before opening the issue.

```py
def outer() -> list[object]:
    @register
    async def inner() -> None:
        print(layout)

    layout = [...]
    return layout  # noqa: RET504
```
The rule reports the assignment to `layout` as unnecessary, and provides a fix by rewriting it as `return [...]`, which will break the `inner` function.

The rule doesn't trigger when `layout` is assigned to above the definition of the `inner` function, however in my use-case a decorator replaces the function with an object, which is then used to define `layout`, mandating the assignment to layout happens below the function definition.

---
```toml
[tool.ruff.lint]
preview = true
explicit-preview-rules = true
select = [ "ALL" ]
```

VSCode extension (`v2024.54.0`), `ruff==0.7.1`

---

_Label `bug` added by @MichaReiser on 2024-11-01 20:38_

---

_Label `help wanted` added by @MichaReiser on 2024-11-01 20:38_

---

_Comment by @sbrugman on 2024-11-04 18:00_

@Enegg thanks for opening the issue. Could you please provide a more realistic (minimal) example of how you would use layout here? For instance it would help illustrate why the inner function is defined before `layout = [...]`

---

_Comment by @Enegg on 2024-11-04 21:45_

Sure. Here, note how the function `my_button` is replaced with the component passed to the decorator:
```py
from collections import abc
from dataclasses import dataclass, field
from typing import TypeAlias

# --------- library code ---------

@dataclass
class Component:
    custom_id: str
    disabled: bool = False


Layout: TypeAlias = abc.Sequence[abc.Sequence[Component]]


class Interaction:
    async def respond(self, content: str | None = None, *, components: Layout = ()) -> None: ...


Callback: TypeAlias = abc.Callable[[Interaction], abc.Awaitable[None]]


@dataclass
class Store:
    ids_to_callbacks: dict[str, Callback] = field(default_factory=dict)

    def register(self, component: Component, /) -> abc.Callable[[Callback], Component]:
        def wraps(callback: Callback) -> Component:
            self.ids_to_callbacks[component.custom_id] = callback
            return component
        return wraps

    def make_id(self) -> str: ...
    async def listen(self) -> None: ...

# --------- user code ---------

def create_layout(store: Store) -> Layout:
    @store.register(Component(custom_id=store.make_id()))
    async def my_button(inter: Interaction) -> None:
        my_button.disabled = True
        await inter.respond("Hello World", components=layout)

    layout = [[my_button]]
    return layout


async def command(inter: Interaction) -> None:
    store = Store()
    layout = create_layout(store)
    await inter.respond(components=layout)
    await store.listen()
 ```
And here are a few [examples](https://github.com/Enegg/discord-ui-store/blob/bf60916b10b5fe5cf0231e0de1c9746c65f7fc1d/examples/basic.py) from my library relying on this setup.

---

_Comment by @joaoe on 2025-05-23 08:39_

The presented example is quite obscure to properly detect. Like, how can ruff know if the inner function goes out of scope or is kept globally ? 

---

_Comment by @Enegg on 2025-05-23 16:25_

Whether a reference to the inner function is stored somewhere is not relevant to the issue.

The rule seems unaware of the fact a variable can be referenced by a closure before it is defined.

---

_Closed by @ntBre on 2025-07-10 20:10_

---
