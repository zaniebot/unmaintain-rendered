```yaml
number: 19285
title: "Suggestion to edit #PLC0415 ruff rules"
type: issue
state: closed
author: lighting9999
labels:
  - question
assignees: []
created_at: 2025-07-11T13:30:56Z
updated_at: 2025-07-14T07:50:29Z
url: https://github.com/astral-sh/ruff/issues/19285
synced_at: 2026-01-10T11:09:59Z
```

# Suggestion to edit #PLC0415 ruff rules

---

_Issue opened by @lighting9999 on 2025-07-11 13:30_

### Summary

While top-level imports are generally preferred, PEP 8 explicitly allows exceptions for:
> "Imports [...] can be placed inside functions to avoid circular import problems" 
 
#[PEP 8: Imports](https://peps.python.org/pep-0008/#imports)
However, local imports are necessary as they can shorten code execution time. For example, in Doctests, loading imports at the top is **unnecessary and may slow down code execution**.  
So I suggest edit #PLC0415 rules

---

_Renamed from "Suggestion to readme #PLC0415 ruff rules" to "Suggestion to edit #PLC0415 ruff rules" by @lighting9999 on 2025-07-11 13:32_

---

_Comment by @ntBre on 2025-07-11 13:35_

Do you have a suggested edit in mind? I think this is already covered in the [rule documentation](https://docs.astral.sh/ruff/rules/import-outside-top-level/):

> An import statement would typically be placed within a function only to avoid a circular dependency, to defer a costly module load, or to avoid loading a dependency altogether in a certain runtime environment.

---

_Label `question` added by @ntBre on 2025-07-11 13:35_

---

_Comment by @lighting9999 on 2025-07-12 09:14_

But I think you can edit.
Because:
Impact:  
Local imports must be evaluated because unnecessary top-level imports significantly slow down execution. This is critical in contexts like doctests where:  
1. **Startup latency** matters (milliseconds compound across test suites)  
2. **Module-load overhead** becomes measurable in hot code paths  
3. **Resource consumption** scales with unused imports"
For example:
```python  
# Slower: 15ms per doctest (100 tests = 1500ms)  
import heavy_library  

def test_function():  
    """  
    >>> test_function()  
    Result  
    """  
    return heavy_library.process("data")  

# Faster: 2ms per doctest (100 tests = 200ms)  
def test_function():  
    """  
    >>> test_function()  
    Result  
    """  
    import heavy_library  # Local import  
    return heavy_library.process("data")  
```  
    So I suggest to edit this rules.

---

_Comment by @MichaReiser on 2025-07-14 07:49_

The case you describe is already outlined in the documentation (see the documentation that @ntBre). 

In the case of a heavy import, use a noqa comment or you can disable the rule alltogether if you disagree with it

---

_Closed by @MichaReiser on 2025-07-14 07:49_

---
