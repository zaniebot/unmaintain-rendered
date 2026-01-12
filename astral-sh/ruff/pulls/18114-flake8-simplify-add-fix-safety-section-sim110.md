```yaml
number: 18114
title: "[`flake8-simplify`] add fix safety section (`SIM110`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-safety-reimplemented_builtin
created_at: 2025-05-15T06:53:27Z
updated_at: 2025-05-19T20:38:08Z
url: https://github.com/astral-sh/ruff/pull/18114
synced_at: 2026-01-12T15:56:12Z
```

# [`flake8-simplify`] add fix safety section (`SIM110`)

---

_@VascoSch92_

The PR add the `fix safety` section for rule `SIM110` (#15584 )

### Unsafe Fix Example

```python
def predicate(item):
    global called
    called += 1
    if called == 1:
    # after first call we change the method
        def new_predicate(_): return False
        globals()['predicate'] = new_predicate
    return True

def foo():
    for item in range(10):
        if predicate(item):
            return True
    return False

def foo_gen():
    return any(predicate(item) for item in range(10))

called = 0
print(foo())      # true – returns immediately on first call

called = 0
print(foo_gen())  # false – second call uses new `predicate`
```

### Note

I notice that [here](https://github.com/astral-sh/ruff/blob/46be305ad243a5286d4269b1f8e5fd67623d38c2/crates/ruff_linter/src/rules/flake8_simplify/rules/reimplemented_builtin.rs#L60) we have two rules, `SIM110` & `SIM111`. The second one seems not anymore active. Should I delete `SIM111`?


---

_Comment by @github-actions[bot] on 2025-05-15 07:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @ntBre on 2025-05-15 20:28_

---

_Comment by @ntBre on 2025-05-15 20:42_

That's quite an exotic example! I'm not sure if it's doing exactly what you think, though. I tried switching the order in which I call `foo` and `foo_gen`, and the output was still `True` and then `False`:

### Original order

```pycon
>>> def predicate(item):
...     global called
...     called += 1
...     if called == 1:
...     # after first call we change the method
...         def new_predicate(_): return False
...         globals()['predicate'] = new_predicate
...     return True
...
... def foo():
...     for item in range(10):
...         if predicate(item):
...             return True
...     return False
...
... def foo_gen():
...     return any(predicate(item) for item in range(10))
...
... called = 0
... print(foo())      # true – returns immediately on first call
...
... called = 0
... print(foo_gen())  # false – second call uses new `predicate`
...
True
False
```

### Swapped order

```pycon
>>> def predicate(item):
...     global called
...     called += 1
...     if called == 1:
...     # after first call we change the method
...         def new_predicate(_): return False
...         globals()['predicate'] = new_predicate
...     return True
...
... def foo():
...     for item in range(10):
...         if predicate(item):
...             return True
...     return False
...
... def foo_gen():
...     return any(predicate(item) for item in range(10))
...
... called = 0
... print(foo_gen())      # true – returns immediately on first call
...
... called = 0
... print(foo())  # false – second call uses new `predicate`
...
True
False
```

I think both versions use the same `predicate` for the whole loop, but `called` isn't actually getting reset between calls.

So removing comments might be the main reason for unsafety here.

---

_Comment by @VascoSch92 on 2025-05-18 20:13_

@ntBre You are right. :-)

I just added the fact it could remove comment. 

Other question: we could make the fix _safe_ when it doesn't remove comments right?

---

_@ntBre approved on 2025-05-19 20:37_

Thanks!

> Other question: we could make the fix safe when it doesn't remove comments right?

Yes, I think that's right since we couldn't come up with any other reason for the unsafety.

---

_Merged by @ntBre on 2025-05-19 20:38_

---

_Closed by @ntBre on 2025-05-19 20:38_

---
