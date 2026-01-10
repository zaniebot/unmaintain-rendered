```yaml
number: 10300
title: "Feature Request: Rule to avoid module coupling"
type: issue
state: open
author: ndrluis
labels:
  - rule
  - needs-design
assignees: []
created_at: 2024-03-08T15:30:30Z
updated_at: 2024-08-09T07:28:01Z
url: https://github.com/astral-sh/ruff/issues/10300
synced_at: 2026-01-10T11:09:52Z
```

# Feature Request: Rule to avoid module coupling

---

_Issue opened by @ndrluis on 2024-03-08 15:30_

Hello, I'm not a Pythonist, but I'm working on some Python projects and having a problem with how to avoid some import coupling. 

For example:
```python
# module_a.py

def func_a():
    pass

# module_b.py

from module_a import func_a

def func_b():
    func_a()

# module_c.py

from module_b import func_a  # This line should import func_a directly from module_a instead

def func_c():
    func_a()
```    
  
We want to avoid having `func_a` imported from `module_b`; instead, we aim to import it directly from the original module where `func_a` is defined. Is it possible to create a rule that warns us?

Any thoughts on this?

---

_Comment by @mkniewallner on 2024-03-09 19:59_

I believe that the feature would be similar to what https://github.com/seddonym/import-linter provides, right? Although less powerful than the library, in Ruff you should be able to achieve something similar with https://docs.astral.sh/ruff/rules/banned-api/.

https://github.com/astral-sh/ruff/issues/1912 exists to implement `import-linter` in Ruff as well.

---

_Comment by @eli-schwartz on 2024-03-10 04:25_

I recall searching for this feature and coming across import-linter, then disregarding it since import-linter seems to do something else entirely -- and require significant configuration to do so.

The problem is not actually about allowing or banning specific APIs from being used. The problem is that if you have a file that includes `import os`, "os" is a validly existing symbol reference inside your module and "import mymodule.os" works at runtime...

... but no one would actually consider this a reasonable usage of the "os" module. Because it's an import.

This can be trivially detected in a heuristic manner, you don't need to define rulesets for specific examples you don't want to use. All you need to do is check whether your module's public interface is being obeyed. It's intrinsically tied to `__all__`.

It turns out that this works well together with static typing -- in particular, stub files will tend to never define an API you don't publicly provide, even if the implementation itself happens to import stdlib modules ;) ;) ;) -- so mypy includes an option for it: https://mypy.readthedocs.io/en/stable/config_file.html#confval-implicit_reexport

I enabled the mypy option and caught a looooooot of cases where we were doing it in our codebase. :D

---

_Comment by @eli-schwartz on 2024-03-10 04:27_

It sounds potentially interesting for ruff to implement that either in addition to or instead of import-linter. Unlike import-linter it has the advantage of by design not requiring any sort of configuration.

---

_Label `rule` added by @zanieb on 2024-03-11 16:55_

---

_Label `needs-design` added by @zanieb on 2024-03-11 16:56_

---

_Comment by @ashrub-holvi on 2024-08-09 07:28_

Looks like there is an alternative to import-linter written in Rust - https://github.com/gauge-sh/tach/

---
