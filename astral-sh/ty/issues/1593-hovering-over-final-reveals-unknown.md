```yaml
number: 1593
title: "Hovering over `Final` reveals `Unknown`"
type: issue
state: open
author: MichaReiser
labels:
  - bug
  - server
assignees: []
created_at: 2025-11-19T15:03:40Z
updated_at: 2025-12-05T19:27:56Z
url: https://github.com/astral-sh/ty/issues/1593
synced_at: 2026-01-12T15:54:25Z
```

# Hovering over `Final` reveals `Unknown`

---

_@MichaReiser_

If you hover over the `Final` in `X: Final`, it shows `Unknown`

```py
from typing import Final, reveal_type

X: Final = ["foo", "bar"]
```

Go to definition works. 



https://play.ty.dev/64da8236-5cf7-43d4-bc71-6cd7b42773c4

---

_Label `bug` added by @MichaReiser on 2025-11-19 15:03_

---

_Label `server` added by @MichaReiser on 2025-11-19 15:03_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-19 15:04_

---

_Comment by @Gankra on 2025-12-05 14:04_

I think this is a quirk of how the typechecker records types and the resulting AST. 

We handle `x: Final[int]` fine, but notably if you hover `x` you will get the type `int` -- `Final` is implemented to evaporate! Similarly if you hover the `x` in `x: Final` you get `Unknown` because we have suddenly invented a syntactically valid way in python to declare a variable exists but without assigning a value or giving it an actual type! (i.e. I think `x: Unknown` is a reasonable/correct inference).

Now in `x: Final[int]` I believe there are 3 annotation exprs here: `Final`, `int`, and `Final[int]`, which have the following inferred types:

* `Final`: `<special form 'typing.Final'>`
* `int`: `int`
* `Final[int]`: Unknown

So when you hover `Final` we grab the inner expression and happily report `<special form 'typing.Final'>`.

However in `x: Final` there's only one expr: `Final`! And yet it is simultaneously `<special form 'typing.Final'>` and `Unknown` -- with the latter being the "final" type and therefore the one assigned to that expr. So when we hover, all you get is `Unknown`.

---

_Comment by @carljm on 2025-12-05 19:27_

I think it is possible to change this behavior in the type checker without introducing any great inconsistency -- it's sort of an arbitrary choice what type we assign to the annotation expression in this case. Given that you can already get the inferred type of `x` by hovering over `x`, I think we may as well assign the special-form type to the bare `Final` in `x: Final = 1`.

---
