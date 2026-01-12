```yaml
number: 15916
title: "Feature Request: Detect Unbound Local Variables in Conditional Branches"
type: issue
state: closed
author: rodolfo-nobrega
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-02-03T18:35:44Z
updated_at: 2025-02-03T22:47:26Z
url: https://github.com/astral-sh/ruff/issues/15916
synced_at: 2026-01-12T15:54:55Z
```

# Feature Request: Detect Unbound Local Variables in Conditional Branches

---

_@rodolfo-nobrega_

### Description

#### **Description**  
Currently, Ruff does not detect cases where a variable might be referenced before being assigned due to a missing branch in a conditional statement. This can lead to runtime errors such as `UnboundLocalError`.  

#### **Example of the Issue**  
Consider the following function:  

```python
def test(x):
    if x == 1:
        a = 3
    return a  # Potential UnboundLocalError
```

If `x != 1`, then `a` is never assigned, causing an error when the function tries to return `a`:  

```
UnboundLocalError: cannot access local variable 'a' where it is not associated with a value
```

This kind of error can be subtle and hard to debug in larger codebases, making it a good candidate for static analysis.  

#### **Proposed Solution**  
Ruff should provide a warning when a variable is used before being assigned in all possible execution paths. This check could function similarly to how other linters (e.g., PyLint’s `used-before-assignment` warning) identify potential uninitialized variables.  

#### **Example of Expected Behavior**  
Ruff should warn about the unbound variable in the following function:  

```python
def test(x):
    if x == 1:
        a = 3
    return a  # Warning: Variable 'a' might be used before assignment
```

#### **Why This Would Be Useful**  
- Prevents runtime errors due to uninitialized variables.  
- Helps catch logical issues in conditionally assigned variables.  
- Improves code safety and maintainability.  

Would this be feasible to implement in Ruff? Thanks for considering this request! 🚀  

---

_Comment by @MichaReiser on 2025-02-03 19:26_

Thanks for the well written issue. 

We're working on an improve semantic model (red knot) and a type checker. Both support the above but it will take a while before the improvements make it back into ruff. 

This is related to https://github.com/astral-sh/ruff/issues/13296



---

_Label `rule` added by @MichaReiser on 2025-02-03 19:26_

---

_Label `type-inference` added by @MichaReiser on 2025-02-03 19:26_

---

_Comment by @rodolfo-nobrega on 2025-02-03 22:47_

Thanks for the update! I think Ruff is incredible, and it’s amazing to see the power of Rust in action for this kind of application. I really appreciate all the work you’re putting into it and look forward to seeing these improvements in the future!

---

_Closed by @rodolfo-nobrega on 2025-02-03 22:47_

---
