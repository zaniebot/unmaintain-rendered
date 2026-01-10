---
number: 2350
title: "Auto classes from transformers library \"from_pretrained\" marked as possibly missing"
type: issue
state: closed
author: c-vongpaseut
labels:
  - needs-info
assignees: []
created_at: 2026-01-05T17:10:09Z
updated_at: 2026-01-06T17:10:44Z
url: https://github.com/astral-sh/ty/issues/2350
synced_at: 2026-01-10T01:51:14Z
---

# Auto classes from transformers library "from_pretrained" marked as possibly missing

---

_Issue opened by @c-vongpaseut on 2026-01-05 17:10_

### Summary

Auto classes, e.g. AutoModel, from huggingface transformers library `from_pretrained` method raises `warning[possibly-missing-attribute]`. 
These class method are defined, see implementation [here](https://github.com/huggingface/transformers/blob/c9b0498f2e5ab0ef83ff6d6226025bec7c23aaec/src/transformers/models/auto/auto_factory.py#L469)
_______________________________________________

To reproduce

_snippet_
```py
from transformers import AutoModel

model = AutoModel.from_pretrained("bert-base-uncased")
```

_command_
```
uv add transformer==4.51.0
uvx ty@0.0.9 check .
```

### Version

0.0.9

---

_Comment by @carljm on 2026-01-05 20:22_

I'm not able to reproduce this. In an empty directory, I `uv init .` and `uv add transformers` (it installs transformers 4.57.3), and put the above code snippet in a file `test_automodel.py`, then `uvx ty@0.0.9 check .` reports no errors.

Can you re-check and clarify your reproduction instructions?

---

_Label `needs-info` added by @carljm on 2026-01-05 20:22_

---

_Comment by @c-vongpaseut on 2026-01-06 08:22_

Thanks for checking, I am using `transformers==4.51.0`

---

_Comment by @carljm on 2026-01-06 17:10_

With `transformers==4.51.0` I can reproduce. This is the same issue as https://github.com/astral-sh/ty/issues/2244. Since it already seems resolved in a more recent version of transformers library, I don't think we need a separate issue open to track this for `transformers` specifically, so I'll close this as duplicate. Thanks for the report!

---

_Closed by @carljm on 2026-01-06 17:10_

---
