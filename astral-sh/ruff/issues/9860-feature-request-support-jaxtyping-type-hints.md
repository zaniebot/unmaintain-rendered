```yaml
number: 9860
title: "Feature request: support jaxtyping type hints (currently valid type hints flagged incorrectly)"
type: issue
state: open
author: jaanli
labels:
  - bug
assignees: []
created_at: 2024-02-06T16:52:59Z
updated_at: 2025-12-21T07:52:38Z
url: https://github.com/astral-sh/ruff/issues/9860
synced_at: 2026-01-10T11:09:52Z
```

# Feature request: support jaxtyping type hints (currently valid type hints flagged incorrectly)

---

_Issue opened by @jaanli on 2024-02-06 16:52_

<img width="506" alt="image" src="https://github.com/astral-sh/ruff/assets/5317244/1314bebe-b77f-4b30-aacb-93fa57af32f9">

Minimal example that incorrectly flags a valid type hint using the `jaxtyping` type hint library (https://github.com/patrick-kidger/jaxtyping):
```
import numpy as np
from jaxtyping import Int
from beartype import beartype

@beartype
def predict_labels_for_encounter(
    encounter_index: np.int32,
    val_sentence_neighbor_idx: Int[np.ndarray, "num_nearest_neighbors"]):
    pass
```


---

_Label `bug` added by @charliermarsh on 2024-02-06 19:08_

---

_Comment by @crypdick on 2024-02-17 01:55_

I found a [work-around](https://github.com/beartype/beartype/issues/326) to this problem by changing Ruff's typing modules. However, it would be awesome if Ruff would ship this alias within the Ruff codebase. cc @charliermarsh @leycec

---

_Comment by @charliermarsh on 2024-02-17 02:01_

We may be able to add some of these as first-class members. I need to look into the relationship between jaxtyping and beartype though.

---

_Comment by @leycec on 2024-02-17 05:41_

**@beartype lead @leycec is summoned.** Thanks so much for the kind consideration, @charliermarsh. Likewise, thanks for pinging me here, @crypdick. Let's also ping `jaxtyping` lead @patrick-kidger. This is his baby.

> I need to look into the relationship between jaxtyping and beartype though.

Tenuous at best. @patrick-kidger could probably fill us in on all the details. Briefly:

* `jaxtyping` type-checks `jaxtyping`-specific type hints (e.g., `jaxtyping.Int[...]` in the above example).
* `jaxtyping` then loosely couples with a more general-purpose type-checker to type-check the remaining `jaxtyping`-agnostic type hints (e.g., `numpy.int32` in the above example). Because the coupling is *very* loose indeed, `jaxtyping` literally supports *all* possible general-purpose type-checkers satisfying the standard decorator protocol. This includes:
  * @beartype, of course. Yay! :partying_face: 
  * `typeguard`, of course. Boo! Kidding. Only kidding. Everybody loves `typeguard` here. :eyes: 

Honestly, this issue is *probably* just about @beartype. As awesome as `jaxtyping` and `typeguard` are, this really has nothing to do with either of those packages – I think, anyway. Let us know if @beartype can do anything to assist Ruff here.

---

_Comment by @patrick-kidger on 2024-02-17 09:00_

Hey folks! jaxtyping is my package. This is a known issue, and [I recommend just disabling the F722 check
](https://docs.kidger.site/jaxtyping/faq/#flake8-or-ruff-are-throwing-an-error) on this.

(@leycec - this issue is purely jaxtyping-specific, and unrelated to beartype, you'll be pleased to know!)

What's happening is that in type annotations, strings are either (a) just strings or (b) forward references. Ruff is assuming that only the second case is true, trying to resolve our strings as forward references, and failing. This is consistent with flake8.

To be fair, Python itself offers no clear way of distinguishing these cases either. Most static type checkers have to special-case `Annotated` so that `Annotated[Foo, "bar"]` is still valid. We actually use `if TYPE_CHECKING: from typing import Annotated as Float` to make things work with them, but it's not clear if tools like ruff should respect `TYPE_CHECKING` themselves.

If you'd like to special-case ruff support for jaxtyping that would be awesome. Conversely I recognise you may not wish to add special support to a third-party package, in which case I think no action is needed from ruff. :)

---

_Comment by @tpgillam on 2025-09-12 15:35_

Is it possible that technology from ty would allow more general detection of types that are equivalent to `typing.Annotated` at type-checking time? And hence avoid a need to special-case jaxtyping.

e.g. in the following, `moo3` seems like it would be "easier" to make work, insofar as the alias is contained in the same file. But presumably ty would understand the more general case where the alias happens within `jaxtyping._indirection`, when `TYPE_CHECKING` is set at least. (But at that point I'm not sure if this check would belong in ty itself, rather than ruff.)

```python
from __future__ import annotations

import typing

if typing.TYPE_CHECKING:
    import jaxtyping
    import torch


def moo1(x: typing.Annotated[int, "fsdfds fskdjfl skl"]) -> None:  # OK
    del x


def moo2(x: jaxtyping.Float[torch.Tensor, "m n"]) -> None:  # Flags F722
    del x


Thing = typing.Annotated

def moo3(x: Thing[int, "fsdfds fskdjfl skl"]) -> None:  # Flags F722
    del x
```

---

_Comment by @ntBre on 2025-09-12 16:55_

Yeah there are a couple of related issues for `typing.Annotated` aliases, both labeled `type-inference`. I think the infrastructure from ty will help here one day!

https://github.com/astral-sh/ruff/issues/17386, https://github.com/astral-sh/ruff/issues/11378

---

_Comment by @TimWalter on 2025-12-18 11:56_

While disabling F722 works for most annotations it does not for symbolic annotations e.g.
```python
import torch
from torch import Tensor
from jaxtyping import Float, jaxtyped
from beartype import beartype

@jaxtyped(typechecker=beartype)
def test(num_samples) -> Float[Tensor, "{num_samples}+1"]:
    return torch.rand(num_samples+1)


if __name__ == "__main__":
    test(100)
```
gives "Undefined name `num_samples` Ruff(undefined-name)"

---

_Comment by @patrick-kidger on 2025-12-18 12:50_

I think `Float[Tensor, " {num_samples}+1"]` (leading space) should work.

---

_Comment by @leycec on 2025-12-21 07:52_

**...lol.** Why does Ruff expect a `num_samples` attribute to exist in the common case of `Float[Tensor, "{num_samples}+1"]`, anyway? That's not an `f`-string. Doesn't make sense, honestly. Ruff wouldn't complain about a comparable PEP 586-compliant `typing.Literal["{num_samples}+1"]` type hint, would it? That's a valid type hint – even if it looks and smells suspicious. Doesn't matter whether that string contains a `{`- and `}`-delimited format placeholder or not. All possible strings are equally valid.

<sup>Or am I truly as dumb as my cats think I am... 😅</sup>

Unrelatedly: **Merry Christmas, everybody!** Hope everyone had an awesome year. 2025 sure has been a ride. Here's to QA and hoofprints on the roof bringing fat presents. 🎅 

---
