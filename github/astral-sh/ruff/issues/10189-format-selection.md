---
number: 10189
title: Format selection
type: issue
state: closed
author: eljeffeg
labels:
  - question
assignees: []
created_at: 2024-03-01T21:06:23Z
updated_at: 2024-03-02T13:49:31Z
url: https://github.com/astral-sh/ruff/issues/10189
synced_at: 2026-01-07T13:12:15-06:00
---

# Format selection

---

_Issue opened by @eljeffeg on 2024-03-01 21:06_

Is there a way to preserve the `indent-style`, `line-ending`, and `quote-style` while ignoring the other black formatting?

---

_Comment by @eljeffeg on 2024-03-01 21:31_

And is there a way to ignore the **formatting** of `isort`?
This first block is sorted and formatted perfectly fine. I don't want it converted to the below format (in fact, this is what we don't like about black).
```py
from ipaddress import (AddressValueError, IPv4Address, IPv4Network,
                       IPv6Address, IPv6Network, NetmaskValueError)
```
```py
from ipaddress import (
    AddressValueError,
    IPv4Address,
    IPv4Network,
    IPv6Address,
    IPv6Network,
    NetmaskValueError,
)
```


---

_Comment by @charliermarsh on 2024-03-01 21:34_

> Is there a way to preserve the indent-style, line-ending, and quote-style while ignoring the other black formatting?

Sorry, can you say a bit more about what you mean by this? You _want_ indents and quotes to be corrected? Or you want indents and quotes to be left as-is, but other formatting to change?

> And is there a way to ignore the formatting of isort?

No, we don't expose a formatting option to ignore imports. The intent is to be compatible with Black on this front.


---

_Label `question` added by @charliermarsh on 2024-03-01 21:34_

---

_Comment by @eljeffeg on 2024-03-01 21:39_

Correct, I want indents and quotes to be corrected without the more aggressive black formatting.

If there was an option to ignore the other black formatting changes, assuming isort uses `format`, that may solve both of my issues.  isort would just sort.

---

_Comment by @MichaReiser on 2024-03-02 13:07_

No, this sort of configuration isn't supported by `isort` or the formatter. 

You can give [`flake8-quotes`](https://docs.astral.sh/ruff/rules/#flake8-quotes-q) a try if all you want is to enforce a specific quote style and probably a mixture of [`pycodestyle`](https://docs.astral.sh/ruff/rules/#pycodestyle-e-w) rules to enforce indentation. I don't think there's any rule to enforce a consistent line ending

---

_Closed by @charliermarsh on 2024-03-02 13:49_

---
