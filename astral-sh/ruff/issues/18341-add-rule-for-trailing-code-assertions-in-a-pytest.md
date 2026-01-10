---
number: 18341
title: Add rule for trailing code/assertions in a pytest.raises block
type: issue
state: closed
author: goodspark
labels:
  - question
assignees: []
created_at: 2025-05-27T18:21:17Z
updated_at: 2025-05-28T16:50:53Z
url: https://github.com/astral-sh/ruff/issues/18341
synced_at: 2026-01-10T01:22:59Z
---

# Add rule for trailing code/assertions in a pytest.raises block

---

_Issue opened by @goodspark on 2025-05-27 18:21_

### Summary

If a unit-under-test is successfully raising an error inside a pytest.raises block, then any trailing code after that call will not be executed and is effectively doing nothing. And given that a function not raising when it should is already an erroneous condition, any other code/assertions are likely not testing the code in the expected state.

Example:
```py
def asd():
  raise ValueError('blah')

def test_asd():
  with pytest.raises(ValueError, match='blah'):
    asd()
    # Code after this line will only be run if the unit-under-test doesn't raise.

    assert something == something_else
```

Given detecting the actual code that should be raising isn't quite obvious, I don't think there's an easy auto-fix for this. But the 'fix' should be:

```py
with pytest.raises(ValueError, match='blah'):
  ... # Code before thing that raises
  ... # Code that raises
  # Anything else moved out of the context manager

assert something == something_else
```

As far as detecting this, the pattern is probably something like:
> Any contiguous block of assertions _at the end_ of a pytest.raises block.

---

_Label `question` added by @MichaReiser on 2025-05-28 07:40_

---

_Comment by @MichaReiser on 2025-05-28 07:40_

I think this is already covered by https://docs.astral.sh/ruff/rules/pytest-raises-with-multiple-statements/  ([playground](https://play.ruff.rs/47ae2fe7-c11b-468c-96bb-ccf9608e6f84))

---

_Comment by @goodspark on 2025-05-28 16:50_

Ah, you're right! Does exactly what I want. Thanks!

---

_Closed by @goodspark on 2025-05-28 16:50_

---
