```yaml
number: 10663
title: unnecessary-lambda fix is unsafe even when there are no side-effects 
type: issue
state: closed
author: arrdem
labels:
  - bug
assignees: []
created_at: 2024-03-29T19:50:38Z
updated_at: 2024-03-30T01:05:06Z
url: https://github.com/astral-sh/ruff/issues/10663
synced_at: 2026-01-12T15:54:50Z
```

# unnecessary-lambda fix is unsafe even when there are no side-effects 

---

_@arrdem_

- **Ruff version:** 0.3.3
- **Linter rule:** unnecessary-lambda / PLW0108

### Example
```
def foo(a, b):
  return a + b

def bar(c, d):
  return c, d

HANDLERS = {
  "foo": foo,
  "bar": lambda a, b: bar(a, b)  # "fixes" to just bar 
}

def handle(verb: str, payload):
  a, b = payload
  return HANDLERS[verb](a=a, b=b)
```

Here we're relying on the lambda to implement renaming of positional arguments. Removing the lambda expression would be incorrect despite an absence of side-effects. A correct refactor would be to rename the arguments to `bar`, or to eliminate the keyword invocation but the linter fix alone is unsafe despite listing itself as such.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-30 00:47_

---

_Label `bug` added by @charliermarsh on 2024-03-30 00:55_

---

_Comment by @charliermarsh on 2024-03-30 00:57_

Thanks. That's... unfortunate, but correct.

---

_Closed by @charliermarsh on 2024-03-30 01:05_

---
