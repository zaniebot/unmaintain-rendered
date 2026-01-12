```yaml
number: 2463
title: ty trips up on Union of TypedDicts
type: issue
state: open
author: adamjstewart
labels: []
assignees: []
created_at: 2026-01-12T12:31:46Z
updated_at: 2026-01-12T15:20:13Z
url: https://github.com/astral-sh/ty/issues/2463
synced_at: 2026-01-12T15:54:26Z
```

# ty trips up on Union of TypedDicts

---

_@adamjstewart_

### Summary

I have a rather complex hierarchy of optionals, unions, typed dicts, and data classes (from [here](https://github.com/Lightning-AI/pytorch-lightning/blob/2.6.0/src/lightning/pytorch/utilities/types.py)) that I need to use for type hints:
```python
OptimizerLRScheduler = Optional[
    Union[
        Optimizer,
        Sequence[Optimizer],
        tuple[Sequence[Optimizer], Sequence[Union[LRSchedulerTypeUnion, LRSchedulerConfig]]],
        OptimizerConfig,
        OptimizerLRSchedulerConfig,
        Sequence[OptimizerConfig],
        Sequence[OptimizerLRSchedulerConfig],
    ]
]
```
The following code:
```python
from lightning.pytorch.utilities.types import OptimizerLRScheduler
from torch.optim import Adam
from torch.optim.lr_scheduler import ReduceLROnPlateau

def configure_optimizers() -> OptimizerLRScheduler:
    optimizer = Adam({})
    scheduler = ReduceLROnPlateau(optimizer)
    return {
        'optimizer': optimizer,
        'lr_scheduler': {'scheduler': scheduler},
    }
```
results in an error:
```console
> ty check test.py
error[invalid-return-type]: Return type does not match returned value
  --> test.py:5:31
   |
 3 |   from torch.optim.lr_scheduler import ReduceLROnPlateau
 4 |
 5 |   def configure_optimizers() -> OptimizerLRScheduler:
   |                                 -------------------- Expected `Optimizer | Sequence[Optimizer] | tuple[Sequence[Optimizer], Sequence[LRScheduler | LRSchedulerConfig]] | ... omitted 4 union elements` because of return type
 6 |       optimizer = Adam({})
 7 |       scheduler = ReduceLROnPlateau(optimizer)
 8 |       return {
   |  ____________^
 9 | |         'optimizer': optimizer,
10 | |         'lr_scheduler': {'scheduler': scheduler},
11 | |     }
   | |_____^ expected `Optimizer | Sequence[Optimizer] | tuple[Sequence[Optimizer], Sequence[LRScheduler | LRSchedulerConfig]] | ... omitted 4 union elements`, found `dict[Unknown | str, Unknown | Adam | dict[Unknown | str, Unknown | ReduceLROnPlateau]]`
   |
info: rule `invalid-return-type` is enabled by default

Found 1 diagnostic
```
However, if I use a narrower type from that union:
```python
from lightning.pytorch.utilities.types import OptimizerLRSchedulerConfig
from torch.optim import Adam
from torch.optim.lr_scheduler import ReduceLROnPlateau

def configure_optimizers() -> OptimizerLRSchedulerConfig:
    optimizer = Adam({})
    scheduler = ReduceLROnPlateau(optimizer)
    return {
        'optimizer': optimizer,
        'lr_scheduler': {'scheduler': scheduler},
    }
```
Ty is happy:
```console
> ty check test2.py 
All checks passed!
```
I'm confused how something can satisfy type `Foo` but not `Optional[Union[Foo, ...]]`.

### Version

ty 0.0.11

---
