```yaml
number: 8388
title: less brackets than black in multiline assert
type: issue
state: open
author: peterjc
labels:
  - documentation
  - formatter
assignees: []
created_at: 2023-10-31T19:41:41Z
updated_at: 2023-10-31T21:52:26Z
url: https://github.com/astral-sh/ruff/issues/8388
synced_at: 2026-01-10T11:09:50Z
```

# less brackets than black in multiline assert

---

_Issue opened by @peterjc on 2023-10-31 19:41_

Excerpt from https://github.com/peterjc/thapbi-pict/blob/v1.0.3/thapbi_pict/prepare.py formatted by black (e.g. black version 23.10.1):

```python
assert (
    parse_flash_stdout(
        """\
...
[FLASH] Read combination statistics:
[FLASH]     Total pairs:      6105
[FLASH]     Combined pairs:   5869
...
"""
    )
    == (6105, 5869)
)
```

Output from ruff version 1.0.3 (which black reverts to the above):

```python
assert parse_flash_stdout(
    """\
...
[FLASH] Read combination statistics:
[FLASH]     Total pairs:      6105
[FLASH]     Combined pairs:   5869
...
"""
) == (6105, 5869)
```

I prefer the ruff version (one less level of indentation and no redundant parenthesis).

I'm unsure if this is https://github.com/astral-sh/ruff/blob/main/docs/formatter/black.md#call-chain-calls-break-differently or #8180 or #8331.

---

_Comment by @stinodego on 2023-10-31 20:21_

I noticed this too in the Polars test suite. It's one of the nice improvements of the ruff formatter, in my opinion! Though I'm not sure it's intended.

---

_Comment by @MichaReiser on 2023-10-31 21:52_

Thanks for reporting. We're glad you like it :) 

This is related to https://github.com/astral-sh/ruff/issues/6938, which we need to document properly. It is a partial implementation of Black's [improved multiline string handling](https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#improved-multiline-string-handling) that we got for free, and fixing it backward felt like wasted work, considering how rare it is. Which is why we kept it. I'll keep this issue open to track the documentation of the deviation.

---

_Label `documentation` added by @MichaReiser on 2023-10-31 21:52_

---

_Label `formatter` added by @MichaReiser on 2023-10-31 21:52_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-31 21:52_

---
