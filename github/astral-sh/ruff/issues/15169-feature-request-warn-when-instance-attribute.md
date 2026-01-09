---
number: 15169
title: "Feature Request: Warn when instance attribute shadows class attribute"
type: issue
state: open
author: alippai
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-12-28T20:59:47Z
updated_at: 2025-12-18T06:58:06Z
url: https://github.com/astral-sh/ruff/issues/15169
synced_at: 2026-01-07T13:12:16-06:00
---

# Feature Request: Warn when instance attribute shadows class attribute

---

_Issue opened by @alippai on 2024-12-28 20:59_

I suggest adding a linting rule to warn when an instance attribute shadows a class attribute of the same name. This can help prevent bugs from unintended overriding of class-level variables.

```python
class A:
    x = 1
    def get_x(self):
        return self.x

a = A()
a.x = 2  # Shadows class attribute 'x'
print(a.get_x())  # Outputs 2, but class attribute 'x' is 1
```

In this example, assigning to a.x creates an instance attribute that overshadows the class attribute x. Accessing self.x in get_x now refers to the instance attribute, which might not be the intended behavior.

Similarly, the subclassing is error prone:
```python
class Parent:
    def __init__(self):
        self.value = 'instance'

    def get_value(self):
        return self.value

class Child(Parent):
    value = 'class'  # Class attribute shadows instance attribute

c = Child()
print(c.get_value())  # Outputs 'instance'
```

---

_Comment by @alippai on 2024-12-28 21:05_

@MichaReiser this might be relevant here: https://github.com/astral-sh/ruff/pull/10891

---

_Label `rule` added by @MichaReiser on 2024-12-30 09:40_

---

_Label `type-inference` added by @MichaReiser on 2024-12-30 09:40_

---

_Comment by @MichaReiser on 2024-12-30 09:46_

Thanks for opening this rule idea. 

The main challenge with this rule is that it probably requires type inference, or at least more advanced analysis than what Ruff does today to be useful:

* Ruff needs to understand that `a` is an instance of `A`
* Ideally, Ruff understands that `a` is an instance of `A` even if `A` is defined in another file or if `A` is the result of a function call
* Ruff needs to understand whether `a.x` is an instance attribute or not (I think that's doable because detecting class attributes is "easy enough).



---

_Comment by @alippai on 2025-05-17 23:17_

Can we use `ty` in ruff to implement this?

---

_Comment by @MichaReiser on 2025-05-18 16:10_

> Can we use `ty` in ruff to implement this?

Not today, no. Using ty in ruff requires fundamental changes to how ruff works internally. But it's on our long-term roadmap to bring the power of ty to ruff lint rules but we don't know yet how exactly we'll do that.

---

_Comment by @alippai on 2025-05-18 17:21_

@MichaReiser thanks! It’s exciting and everything seems to go in the best direction.

---

_Comment by @alippai on 2025-12-18 04:10_

@MichaReiser did ruff or ty change in this question? If there is an example for linting based on type inference, I’d give it a try

---

_Comment by @MichaReiser on 2025-12-18 06:58_

No, Ruff isn't yet using ty's analysis engine

---
