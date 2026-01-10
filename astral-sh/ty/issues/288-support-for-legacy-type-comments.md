```yaml
number: 288
title: support for legacy type comments?
type: issue
state: closed
author: arjenzorgdoc
labels:
  - question
assignees: []
created_at: 2025-05-09T08:41:14Z
updated_at: 2025-05-09T08:44:59Z
url: https://github.com/astral-sh/ty/issues/288
synced_at: 2026-01-10T02:34:09Z
```

# support for legacy type comments?

---

_Issue opened by @arjenzorgdoc on 2025-05-09 08:41_

### Question

Some code still has legacy type comments: 

```python
x = dict()  # type: dict[str, str]
```

See:

https://play.ty.dev/f129a7ef-a780-41ce-9123-5eb3403c2fd6

vs:

https://pyright-play.net/?pythonVersion=3.13&code=GYJw9gtgBALgngBwJYDsDmUkQWEMogCmAboQIYA2A%2BvAoQFCNlQC8UA3gL5RQDEsiQgC4oAEyQBjGAG0AzjBAAaKPJABdekVKUaggBRkAdGkIw9AIgBG5gJQ2gA

and

https://mypy-play.net/?mypy=latest&python=3.12&gist=79b0621299b667376a61d84c666c3113

Any plans for supporting this?

### Version

_No response_

---

_Label `question` added by @arjenzorgdoc on 2025-05-09 08:41_

---

_Comment by @AlexWaygood on 2025-05-09 08:44_

I'm afraid we don't plan on supporting these. It would require quite far-reaching changes to our model, and, as you say, these are mostly a legacy feature that has been supplanted by newer syntax in more recent years.

---

_Closed by @AlexWaygood on 2025-05-09 08:44_

---
