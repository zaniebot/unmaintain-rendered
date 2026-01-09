---
number: 3103
title: Pattern matching throws false positive F821 when the value comes from the pattern
type: issue
state: closed
author: Vegemash
labels: []
assignees: []
created_at: 2023-02-21T22:26:25Z
updated_at: 2023-02-21T22:28:04Z
url: https://github.com/astral-sh/ruff/issues/3103
synced_at: 2026-01-07T13:12:14-06:00
---

# Pattern matching throws false positive F821 when the value comes from the pattern

---

_Issue opened by @Vegemash on 2023-02-21 22:26_

Woo pattern matching! Thank you for supporting 99% of our codebase, here's some bugs :P

```python
class TheEyesHaveIt:
    def __init__(self, i=None, eye=None):
        self.i = i
        self.eye = eye


def match_the_thing(thing: TheEyesHaveIt) -> None:
    match thing:
        case TheEyesHaveIt(i=value) if value:
            print(f"The i's have it at {value}")
        case TheEyesHaveIt(eye=value) if value:
            print(f"The eyes have it at {value}")


a = TheEyesHaveIt(eye="foo")
b = TheEyesHaveIt(i="bar")
match_the_thing(a)
match_the_thing(b)
```

All those `value`s are marked as undefined names "F821 Undefined name \`value\`".



---

_Comment by @charliermarsh on 2023-02-21 22:28_

I think these were fixed by #3098, but I'll double check.

I'll probably cut a new release today to get that out.

---

_Closed by @charliermarsh on 2023-02-21 22:28_

---
