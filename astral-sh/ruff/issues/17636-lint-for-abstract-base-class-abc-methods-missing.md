```yaml
number: 17636
title: "Lint for Abstract Base Class (ABC) Methods Missing `@abstractmethod` Decorator"
type: issue
state: closed
author: d33bs
labels:
  - rule
assignees: []
created_at: 2025-04-25T20:44:10Z
updated_at: 2025-04-28T12:15:46Z
url: https://github.com/astral-sh/ruff/issues/17636
synced_at: 2026-01-12T15:54:56Z
```

# Lint for Abstract Base Class (ABC) Methods Missing `@abstractmethod` Decorator

---

_@d33bs_

### Summary

First, I just want to say thank you for the amazing work on Ruff! I appreciate the quality and thoughtfulness throughout the project.

I'd like to propose a feature that would extend Ruff’s capabilities around Python abstract base classes (ABCs).

## 📚 Scenario

When writing Python code that uses `abc.ABC`, it's easy to mistakenly define a method intended to be abstract but forget to decorate it with `@abstractmethod`.
Today, Ruff does not currently flag this to my knowledge — it can lead to design inconsistencies that static analysis could help prevent earlier.

For example:

```python
from abc import ABC, abstractmethod

class MyInterface(ABC):
    def should_be_abstract(self):  # Missing @abstractmethod
        pass

    @abstractmethod
    def correctly_marked(self):
        pass

class ConcreteImplementation(MyInterface):
    def correctly_marked(self):
        print("Implemented correctly")

# The mistake: ConcreteImplementation can now be instantiated
# even though should_be_abstract was not overridden!
obj = ConcreteImplementation()
obj.should_be_abstract()  # Oops — empty method executes
```

In this example:

- `should_be_abstract` looks like it should be abstract (empty body), but it's missing `@abstractmethod`.
- `ConcreteImplementation` can be instantiated without ever overriding `should_be_abstract`, defeating the point of having an interface.
- This could easily lead to subtle bugs or design errors.

## ✅ What Ruff could do

- Detect methods in `ABC` subclasses that have "empty" bodies (only `pass`, `...`, or raising `NotImplementedError`) but lack the `@abstractmethod` decorator.
- Suggest adding `@abstractmethod` to such methods.
- Raise a warning like ABC001 (hypothetical) — e.g., "Method in ABC subclass has empty body but is not decorated with `@abstractmethod.`"

Optionally, it could allow configuration (some teams might prefer to permit empty methods without enforcing abstractness).

---

_Renamed from "Lint for Abstract Base Class (ABC) Methods Missing @abstractmethod Decorator" to "Lint for Abstract Base Class (ABC) Methods Missing `@abstractmethod` Decorator" by @d33bs on 2025-04-25 20:44_

---

_Comment by @MichaReiser on 2025-04-28 07:38_

Thanks for the kind words.

This is the opposite of https://github.com/astral-sh/ruff/issues/12861

---

_Label `rule` added by @MichaReiser on 2025-04-28 07:38_

---

_Comment by @MichaReiser on 2025-04-28 07:40_

This is already covered by [B027](https://docs.astral.sh/ruff/rules/empty-method-without-abstract-decorator/): [playground](https://play.ruff.rs/36e89fb9-8e29-49c1-bfad-9d18b7d9ff1d)

---

_Closed by @MichaReiser on 2025-04-28 07:40_

---

_Comment by @d33bs on 2025-04-28 12:15_

Thanks @MichaReiser !

---
