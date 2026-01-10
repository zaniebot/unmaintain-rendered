```yaml
number: 2206
title: Error for arg but type is correct
type: issue
state: closed
author: vincenzon
labels: []
assignees: []
created_at: 2025-12-24T15:16:00Z
updated_at: 2025-12-24T21:45:39Z
url: https://github.com/astral-sh/ty/issues/2206
synced_at: 2026-01-10T01:56:41Z
```

# Error for arg but type is correct

---

_Issue opened by @vincenzon on 2025-12-24 15:16_

### Summary

Using openai client with ty, vscode is marking the following as a typing error but you can see the type is correct.

The argument is defined as:

```py
from openai import OpenAI
from openai.types.responses import EasyInputMessageParam

client = OpenAI(api_key=...)

input: list[EasyInputMessageParam] = [ ... ]

response = client.responses.parse( model="gpt-5.1", input=input). <--- input marked as wrong type
```

The error message shows the type is correct:

```
Argument to bound method `parse` is incorrect: Expected `str | list[EasyInputMessageParam | Message | ResponseOutputMessageParam | ... omitted 23 union elements] | Omit`, found `list[EasyInputMessageParam]`ty[invalid-argument-type](https://ty.dev/rules#invalid-argument-type)
responses.py(1116, 9): Method defined here
responses.py(1123, 9): Parameter declared here
list[EasyInputMessageParam]
```

<img width="760" height="171" alt="Image" src="https://github.com/user-attachments/assets/593d56a0-5b59-436d-9bcf-58bd38975560" />

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-12-24 18:54_

Hey, thanks for the report! However, I think ty's diagnostic is correct here.

It looks like if you want to pass a list to this function, the list needs to have type `list[EasyInputMessageParam | Message | ResponseOutputMessageParam | ... omitted 23 union elements]`. You've passed in an object of type `list[EasyInputMessageParam]`. You might think that `list[EasyInputMessageParam]` would be assignable to `list[EasyInputMessagrParam | Message | ... etc.]`, but that's not the case, because `list` is an invariant type.

We don't have great docs on invariance yet, but you can see mypy's docs for some explanation here:
- https://mypy.readthedocs.io/en/stable/generics.html#variance-of-generics
- https://mypy.readthedocs.io/en/stable/common_issues.html#invariance-vs-covariance

It seems likely to me that the underlying issue is that the annotations for `client.responses.parse` in openai's Python API are too precise. Rather than annotating this parameter with `list`, it would probably be much more ergonomic for their users if they annotated the parameter with a covariant type such as `Sequence`.

Mypy, pyright and pyrefly all issue the same diagnostic as ty here.

---

_Closed by @AlexWaygood on 2025-12-24 18:54_

---

_Comment by @AlexWaygood on 2025-12-24 18:55_

I modified the snippet you posted to this, which is a more reproducible snippet for testing with different type checkers, before running mypy/pyrefly/pyright/ty on it and comparing their outputs:

```py
from openai import OpenAI
from openai.types.responses import EasyInputMessageParam

def f(client: OpenAI):
    input: list[EasyInputMessageParam] = []
    response = client.responses.parse(model="gpt-5.1", input=input)  # <--- input marked as wrong type
```

---

_Comment by @vincenzon on 2025-12-24 21:45_

Thanks Alex, yes, I see. 

---
