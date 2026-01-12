```yaml
number: 2349
title: "Object of type `PreTrainedTokenizerFast` is callable"
type: issue
state: closed
author: c-vongpaseut
labels:
  - needs-info
assignees: []
created_at: 2026-01-05T16:58:57Z
updated_at: 2026-01-06T17:28:25Z
url: https://github.com/astral-sh/ty/issues/2349
synced_at: 2026-01-12T15:54:26Z
```

# Object of type `PreTrainedTokenizerFast` is callable

---

_@c-vongpaseut_

### Summary

Object of type `PreTrainedTokenizerFast` is callable (see documentation h[ere](https://huggingface.co/docs/transformers/v5.0.0rc1/main_classes/tokenizer#transformers.TokenizersBackend.__call__)) but raise `error[call-non-callable]`.

________________________________
To reproduce

_snippet_
````
from transformers import AutoTokenizer, PreTrainedTokenizerFast


def tokenize(tokenizer: PreTrainedTokenizerFast):
    return tokenizer("blabla")

t = AutoTokenizer.from_pretrained("bert-base-uncased")
tokenize(t)
````

_command_
````
uv add transformer==4.51.0
uvx ty@0.0.9 check .
````

### Version

0.0.9

---

_Comment by @carljm on 2026-01-05 19:58_

I'm not able to reproduce this. When I `uv init .` in an empty directory, `uv add transformers`, and then put the above code snippet in a file, `uvx ty@0.0.9 check .` does not report any errors for me.

Can you double-check your exact reproduction steps?

---

_Label `needs-info` added by @carljm on 2026-01-05 19:58_

---

_Comment by @c-vongpaseut on 2026-01-06 08:17_

Thanks for checking, I am using `transformers==4.51.0`

---

_Comment by @carljm on 2026-01-06 17:28_

It looks like in `transformers==4.51.0` there's a complex fallback in `transformers/__init__.py` where dummy objects are used in case some optional dependency is not installed. So ty understands that `PreTrainedTokenizerFast` might be the real class, or it might be the dummy object (which is not callable).

It seems like more recent versions of transformers library have fixed this and removed the dummy objects, which helps type checkers better understand these types.

I think there's not really anything to fix in ty here -- the best option would be to upgrade your version of transformers.

---

_Closed by @carljm on 2026-01-06 17:28_

---
