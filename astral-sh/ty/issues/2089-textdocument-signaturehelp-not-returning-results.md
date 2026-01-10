```yaml
number: 2089
title: "`textDocument/signatureHelp` not returning results where Pylance does"
type: issue
state: closed
author: KevinMusgrave
labels:
  - server
assignees: []
created_at: 2025-12-18T21:38:55Z
updated_at: 2025-12-18T21:52:31Z
url: https://github.com/astral-sh/ty/issues/2089
synced_at: 2026-01-10T01:54:00Z
```

# `textDocument/signatureHelp` not returning results where Pylance does

---

_Issue opened by @KevinMusgrave on 2025-12-18 21:38_

## Summary

`ty` doesn't return a signature for [`litellm.completion`](https://github.com/BerriAI/litellm/blob/main/litellm/main.py#L989):

<img width="285" height="74" alt="Image" src="https://github.com/user-attachments/assets/0ee93ac7-e0ad-4534-8c73-ad8d7c463207" />


But `pylance` does:

<img width="604" height="323" alt="Image" src="https://github.com/user-attachments/assets/295045ef-e5fc-4c82-9c96-25f1343a72e0" />

I've confirmed that `ty` is functioning because it does provide signature help for `torch.nn.CrossEntropyLoss`:

<img width="649" height="121" alt="Image" src="https://github.com/user-attachments/assets/a4d8a215-4a7b-4544-a38c-bdd2c9913d11" />




## Steps to reproduce

1. Install `ty` in VS Code as an extension.
2. Set `"python.languageServer": "None"` in `.vscode/settings.json`.
3. `uv pip install litellm==1.80.10`
4. Create a test file:
```python
import litellm

litellm.completion()
```

Here is the definition of `litellm.completion`: https://github.com/BerriAI/litellm/blob/5230e97448db5b6c207f1fb85b5669e1a64769d1/litellm/main.py#L989


## ty playground

I'm not sure how to add custom packages to the ty playground. If it's possible, please let me know how and I can provide a reproduction.

### Version

2025.74.0

---

_Label `server` added by @AlexWaygood on 2025-12-18 21:39_

---

_Comment by @carljm on 2025-12-18 21:52_

The issue here is that `completion` is decorated with two untyped decorators which we don't currently see through, so we just see its type as `Unknown`. We could do #1971 to just assume that those decorators have no effect on the signature, which is what I think pylance is doing.

---

_Closed by @carljm on 2025-12-18 21:52_

---
