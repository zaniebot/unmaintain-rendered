```yaml
number: 7866
title: "Unable to detect `runtime-evaluated-base-classes` across files"
type: issue
state: open
author: sujuka99
labels:
  - type-inference
assignees: []
created_at: 2023-10-09T09:15:14Z
updated_at: 2025-12-12T20:45:07Z
url: https://github.com/astral-sh/ruff/issues/7866
synced_at: 2026-01-10T11:09:50Z
```

# Unable to detect `runtime-evaluated-base-classes` across files

---

_Issue opened by @sujuka99 on 2023-10-09 09:15_

`TCH001` seems to incorrectly trigger on the example below despite the settings only when `from __future__ import annotations` is present.

**base_model.py**
```python
from pydantic import BaseModel

class Base(BaseModel):
    ...
```

**field_model.py**
```python
from pydantic import BaseModel

class F(BaseModel):
    ...
```

**example_model.py**
```python
from __future__ import annotations  # Remove to avoid the false positive

from minimal_example_ruff_tch.base_model import Base
from minimal_example_ruff_tch.field_model import F  # Falsely flagged by `TCH001`

class ExampleModel(Base):
    field: F
```

Running `ruff check path/to/modules --select "TCH001" --fix` results in the following:

**example_model.py**
```python
from __future__ import annotations
from typing import TYPE_CHECKING
from minimal_example_ruff_tch.base_model import Base

if TYPE_CHECKING:
    from minimal_example_ruff_tch.field_model import F

class ExampleModel(Base):
    field: F
```

Removing the `annotations` import from **example_model.py** or putting `Base` and `F` in the same file (**field_model.py** for example) avoids the false positive.

Current Ruff settings:
```toml
[tool.ruff.flake8-type-checking]
runtime-evaluated-base-classes = ["pydantic.BaseModel"]
```
Current Ruff version: `ruff 0.0.292`


---

_Comment by @charliermarsh on 2023-10-09 11:59_

So, I think there are two things going on here.

The main issue is that we don't support making these kinds of inferences across files. So when you use `from minimal_example_ruff_tch.base_model import Base` and then `class ExampleModel(Base):`, Ruff is unable to detect that `ExampleModel` is a `pydantic.BaseModel`.

Instead, you'd need to add `minimal_example_ruff_tch.base_model.Base` to your `runtime-evaluated-base-classes`, or add `pydantic.BaseModel` as an additional subclass to `ExampleModel` (so it'd be `ExampleModel(Base, pydantic.BaseModel)` or similar). These are unfortunate but known limitations that'll be lifted in time.

The second thing is that -- ignore Pydantic entirely -- Python has some unintuitive behavior when it comes to whether annotations are required to be available at runtime or not, and this is where `from __future__ import annotations` comes into play.

If you omit `from __future__ import annotations`, then when you do:

```python
from minimal_example_ruff_tch.field_model import F

class ExampleModel:
    field: F
```

Python actually _does_ require that `F` is available at runtime. If you move `from minimal_example_ruff_tch.field_model import F` into an `if TYPE_CHECKING:` block, the code will error at runtime.

However, if you either use `from __future__ import annotations` _or_ quote the annotation (like `field: "F")`, Ruff will correctly flag and move the import.

So, I think the known bug here is that we don't yet support multi-file analysis for these rules. In general, I don't know that I'd recommend using them with libraries like Pydantic that depend on the runtime evaluation of types. It's possible but not totally straightforward. Hopefully we'll improve there over time.


---

_Label `multifile-analysis` added by @charliermarsh on 2023-10-09 11:59_

---

_Renamed from "TCH001: False positive" to "Unable to detect `runtime-evaluated-base-classes` across files" by @charliermarsh on 2023-10-09 12:00_

---

_Comment by @disrupted on 2023-10-09 12:51_

I noticed a similar issue of Ruff having trouble evaluating base classes. It seems it doesn't reliably detect the bases – even within the same file.

given this example
```python
from abc import ABC
from pydantic import BaseModel

class Model(BaseModel):
    x: str


class Empty(BaseModel):
    ...


class Abstract(Empty, ABC):
    inner: Model
```

Ruff evaluates the following classes and their bases (I simply placed some log statements in this function)
https://github.com/astral-sh/ruff/blob/a1509dfc7c2b16f3762545fc802d49f5f03726e2/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L42

```log
class Model  bases: [Some(["pydantic", "BaseModel"])]
class Abstract  bases: [None, Some(["abc", "ABC"])]
```
notice the absence of `Empty` and how `Abstract` then doesn't get detected as `BaseModel`

If you think it should be a separate issue, I'll gladly create a new one for this.

---

_Comment by @charliermarsh on 2023-10-09 19:09_

@disrupted - Yeah this is separate -- we don't follow subclass chains _within_ files. That one is solvable (not a one-line fix, but doesn't require any major changes). You're welcome to open another issue for it!

---

_Comment by @BobVitorBob on 2024-08-23 16:13_

Any updates on this? Ran into the same problem here.

Also, shouldn't `runtime-evaluated-base-classes = ["example_model.ExampleModel"]` tell ruff that any class references used for typing inside `ExampleModel` are required on runtime? I tried doing something along these lines but it didn't work.

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---

_Comment by @vkbo on 2025-08-27 07:56_

I ran into this issue today with a FastAPI route endpoint function with an Annotated parameter where pydantic will error if the annotated type is inside the `TYPE_CHECKING` block. It would be helpful if it was possible to define a set of imports that were excluded from the TC001-3 rules. Adding the import (as reported by the ruff error) to `runtime-evaluated-base-classes` did not make any difference.

Edit: Too quick there. I see `exempt-modules` also exists as a setting. Still, this is a little tricky to get to work.

---

_Comment by @9en9i on 2025-12-12 11:07_

Are there any updates? Python 3.14 has been stable for several months now, and the restrictions of this rule prevent it from being used normally when there are several inherited base classes from pydantic that cannot be determined.

---

_Comment by @ntBre on 2025-12-12 18:22_

There hasn't been an update to Ruff's multi-file analysis abilities. ty is able to analyze multiple files, and we hope to reuse that infrastructure in Ruff someday, but the single-file limitation is still in place.

It sounds like a couple of the comments here are reporting issues with single-file subclass chains. If that's the case, I'd be happy to take a look, as I believe that should work after #8768, but a minimal example, ideally in the [playground](https://play.ruff.rs/), would be very helpful!

---

_Comment by @9en9i on 2025-12-12 20:45_

@ntBre Thank you very much for your reply! Unfortunately, my problem is that the classes are located in different files. I look forward to this functionality becoming available.

---
