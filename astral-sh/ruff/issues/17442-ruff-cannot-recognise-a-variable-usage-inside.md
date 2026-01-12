```yaml
number: 17442
title: Ruff Cannot Recognise a Variable Usage inside typing.cast
type: issue
state: closed
author: barmanroys
labels:
  - bug
assignees: []
created_at: 2025-04-17T04:15:49Z
updated_at: 2025-04-23T07:26:01Z
url: https://github.com/astral-sh/ruff/issues/17442
synced_at: 2026-01-12T15:54:55Z
```

# Ruff Cannot Recognise a Variable Usage inside typing.cast

---

_@barmanroys_

### Summary

[Play ground link](https://play.ruff.rs/7abdedee-b394-4ce3-a5b4-ecc1904fbfa7). 

##### Context
A test method within a `unittest` class. 

##### Code
```python3
from typing import cast, Tuple
...

def test_run(self):
    """Use the sample initial states and action choices for testing."""
    initial_input: int = 1 # Ruff warns this variable is defined but not used 
    actions: Tuple[int, ...] = (199, 213, 111, 333) # Ruff warns this variable is defined but not used 
    result = cast(
        typ=Tuple[Any],
        val=self.interactor.run(llm_input=initial_input, action_sequence=actions),
    )
    self.assertTupleEqual(tuple1=result, tuple2=(1, 0, True, {}))
    logging.info(msg="Multi-round interactor passed testing.")
```

##### Problem
Ruff Cannot recognise the usage of a variable inside `typing.cast`. Even though it _is_ being used. 

##### System Details
* ruff 0.11.5
* uv 0.5.13
* Operating system: Linux mint 22

### Version

0.11.5

---

_Comment by @sharkdp on 2025-04-17 06:16_

Here's a smaller example that shows the same problem. It seems to depend on the fact that keyword arguments are used for `typing.cast`:
```py
from typing import cast

def f():
    x = 1
    print(cast(typ=int, val=x))
```

---

_Comment by @barmanroys on 2025-04-17 06:24_

@sharkdp thanks for reducing my example to a minimal variant. 

So, as `typing.cast` accepts keyword arguments, should this behaviour be fixed on the _ruff_-end of things (perdon the pun)? 

Also, weird enough, the bug/warning from ruff is absent when the `cast` is being called at the top level of the module. So the following code works okay. 

```python3
from typing import cast
x = 1
print(cast(typ=int, val=x))
```


---

_Comment by @sharkdp on 2025-04-17 07:22_

> So, as `typing.cast` accepts keyword arguments, should this behaviour be fixed on the _ruff_-end of things

Yes, I think so.



> Also, weird enough, the bug/warning from ruff is absent when the `cast` is being called at the top level of the module

[F841](https://docs.astral.sh/ruff/rules/unused-variable/) only applies to unused variables in function scopes. If you define an variable in module-global scope, it could be imported from another module. So there's no way to tell if it is used or not (in a single-file analysis).

---

_Label `bug` added by @ntBre on 2025-04-17 13:10_

---

_Comment by @ntBre on 2025-04-17 13:23_

I think we're only visiting the positional args for `cast` (and the other `typing` imports in this block?):

https://github.com/astral-sh/ruff/blob/d4695feccf28b46883cb1303a811f147b812ab72/crates/ruff_linter/src/checkers/ast/mod.rs#L1541-L1553

---

_Comment by @dhruvmanila on 2025-04-17 19:48_

> I think we're only visiting the positional args for `cast` (and the other `typing` imports in this block?):

Yeah, this seems like a clear issue. We should probably use `find_argument_value` or `find_argument` which finds an argument via both position and name.

---

_Closed by @MichaReiser on 2025-04-23 07:26_

---
