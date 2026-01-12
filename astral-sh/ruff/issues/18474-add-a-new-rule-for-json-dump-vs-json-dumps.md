```yaml
number: 18474
title: Add a new rule for json.dump VS json.dumps
type: issue
state: closed
author: stmlange
labels: []
assignees: []
created_at: 2025-06-05T06:32:05Z
updated_at: 2025-06-05T07:17:04Z
url: https://github.com/astral-sh/ruff/issues/18474
synced_at: 2026-01-12T15:54:56Z
```

# Add a new rule for json.dump VS json.dumps

---

_@stmlange_

### Summary

Hello,
first of all - thank you for the amazing ruff linter!

I would kindly request a new rule for "json.dump" as I always happen to confuse it with "json.dumps".

The key difference is that the dump expects a fp argument while the dump*s* does not:
- json.dump(obj, fp) -- https://docs.python.org/3/library/json.html#json.dump
- json.dumps(obj) -- https://docs.python.org/3/library/json.html#json.dumps

Minimal example:
```python
"""Example that demonstrates an issue with json.dump VS json.dumps."""

import json


def main() -> None:
    """Execute the main application logic."""
    json_result = {"foo": "bar"}
    json.dump(json_result, indent=4, ensure_ascii=False)


if __name__ == "__main__":
    main()
```

Running the code fails with:
```
TypeError: dump() missing 1 required positional argument: 'fp'
```

Pylint seems to have checks for this:
```
λ pylint example.py
************* Module example
example.py:11:10: E1120: No value for argument 'fp' in function call (no-value-for-parameter)

λ pylint --version
pylint 3.2.7
astroid 3.2.4
```

Ruff does not detect the issue:
```
λ ruff check --select ALL example.py
[incompatible selected rules warning goes here]
All checks passed!

λ ruff --version
ruff 0.11.12
```

Thank you for your time and happy linting :-)

---

_Comment by @MichaReiser on 2025-06-05 07:17_

I think this is something a type checker can catch well and doesn't need to be a specific lint rule. 

For example, our in-progress type checker ty catches this: https://play.ty.dev/0a7d986f-9810-48a0-a95a-922f7862af92

---

_Closed by @MichaReiser on 2025-06-05 07:17_

---

_Closed by @MichaReiser on 2025-06-05 07:17_

---
