```yaml
number: 12861
title: "Rule request: abstractmethod appears on normal class"
type: issue
state: open
author: Skylion007
labels:
  - rule
  - accepted
assignees: []
created_at: 2024-08-13T13:30:22Z
updated_at: 2024-09-05T15:28:11Z
url: https://github.com/astral-sh/ruff/issues/12861
synced_at: 2026-01-10T11:09:54Z
```

# Rule request: abstractmethod appears on normal class

---

_Issue opened by @Skylion007 on 2024-08-13 13:30_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
the case where an abstractmethod decorator is used on a class that does not subclass `abc.ABC` uses the `ABC` metaclass, is probably a bug. It would be great to have a rule to detect the obvious case where it is used on the wrong type of class (when all ancestors of the class in the same file for instance).

If there is a rule that already implements this, let me know. :) 

Here is a real example from PyTorch:
```python
class RemoteCacheBackend:
    """
    A backend implementation for accessing a remote/distributed cache.
    """

    def __init__(self, cache_id: str) -> None:
        pass

    @abstractmethod
    def get(self, key: str) -> Optional[object]:
        pass

    @abstractmethod
    def put(self, key: str, data: bytes) -> None:
        pass
```

---

_Comment by @MichaReiser on 2024-08-14 07:21_

@AlexWaygood any thoughts on this?

---

_Comment by @AlexWaygood on 2024-08-14 11:05_

I think this is a good idea for a rule. I agree that if a class that doesn't have `ABCMeta` (or a subclass of `ABCMeta`) as its metaclass, but has `@abstractmethod`-decorated methods, that's probably a bug. Here's a few things we'll need to keep in mind, however:
1. We need to recognise as valid any class that has `metaclass=X`, or any class that inherits from a class that has `metaclass=X`, where `X` is either `abc.ABCMeta` or a subclass of `abc.ABCMeta`.
2. For a class like the example @Skylion007 gives, that doesn't inherit from any classes or have a custom metaclass, it's easy to see that it doesn't have `ABCMeta` as its metaclass. But we'd need multifile analysis to be able to accurately determine whether this class has `ABCMeta` as its metaclass, since it depends on whether `ForeignSuperclass` has `ABCMeta` as its metaclass or not:
   ```py
   from other_module import ForeignSuperclass

   class Klass(ForeignSuperclass): pass
   ```

   What should we do in cases like this? I vote for not emitting the error if a class inherits from any classes that come from external modules, since this is a common pattern, and I think there will be too many false positives otherwise.
3. Stub files need to be excluded from this rule. Legacy code that was written before the introduction of the `abc` module often has classes that are de-facto abstract base classes, even though they don't make any use of the `abc` module. When writing type stubs for legacy code like this, you'll often want to add `@abstractmethod` to the stub methods of these classes, so that type checkers understand them to be abstract methods that need to be overridden in subclasses and emit appropriate errors. However, you won't want to lie about the metaclass in the stub file and pretend that the class has `abc.ABCMeta` as the metaclass, since this can cause spurious type-checker errors about metaclass conflicts in some situations. Adding `@abstractmethod` to a method on a class that does not have `ABCMeta` as the metaclass is fine in a stub file, since type checkers will still recognise the method as being abstract; the metaclass only makes a difference to runtime semantics, and stub files are never executed at runtime.

---

_Label `rule` added by @AlexWaygood on 2024-08-14 11:06_

---

_Label `accepted` added by @AlexWaygood on 2024-08-14 11:06_

---

_Comment by @opk12 on 2024-09-05 15:28_

in fact the @[abc.abstractmethod](https://docs.python.org/3/library/abc.html#abc.abstractmethod) docs says
> Using this decorator requires that the class’s metaclass is [ABCMeta](https://docs.python.org/3/library/abc.html#abc.ABCMeta) or is derived from it.

---
