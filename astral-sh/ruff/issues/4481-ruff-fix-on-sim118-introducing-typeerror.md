```yaml
number: 4481
title: Ruff --fix on SIM118 introducing TypeError 
type: issue
state: open
author: fennerm
labels:
  - bug
  - type-inference
  - help wanted
assignees: []
created_at: 2023-05-17T21:27:18Z
updated_at: 2025-07-28T09:45:49Z
url: https://github.com/astral-sh/ruff/issues/4481
synced_at: 2026-01-10T11:09:47Z
```

# Ruff --fix on SIM118 introducing TypeError 

---

_Issue opened by @fennerm on 2023-05-17 21:27_

(Running a large repo through ruff --fix today, so might have a few of these tickets for you 😬)

Ruff --fix on the SIM118 rule is introducing a bug in our code (ruff=v0.0.267). The rule seems to flag any case where a `keys()` method is used. This works for dicts but doesn't work for classes with a `keys()` method but no `__iter__`.

Minimal example:
```
class Foo:
    def __init__(self) -> None:
        self._keys = [1,2,3]

    def keys(self) -> list[int]:
        return self._keys


foo = Foo()

for k in foo.keys():
    print(k)
```

Ruff error: `SIM118 [*] Use k in foo instead of k in foo.keys()`

`ruff --fix` converts this to `for k in foo:` which is a TypeError.

Thanks!

---

_Comment by @charliermarsh on 2023-05-17 21:28_

Haha no worries :)

We don't have a reliable way to avoid this right now, other than to make this a suggested (and not an automatic) fix.

---

_Label `bug` added by @charliermarsh on 2023-05-17 21:28_

---

_Label `type-inference` added by @charliermarsh on 2023-05-17 21:28_

---

_Comment by @owenlamont on 2024-06-05 02:42_

I ran into this case too. Thanks for raising this issue. Shame the rule can't be designed to only match Python dictionaries.

In my case the class implemented ```keys()``` and ```__iter__()``` but the ```__iter__()``` iterated the values of the object, not the key values.

---

_Comment by @LordAro on 2025-06-13 13:00_

To give an actual example "in the wild", [`xml.etree.ElementTree.Element.keys`](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree.Element.keys) & [`lxml.etree._Element.keys`](https://lxml.de/api/lxml.etree._Element-class.html#keys) methods exist, and the objects are otherwise very un-dict-like

---

_Comment by @bradezard131 on 2025-07-25 07:22_

The rule in the docs states that it is marked a safe fix [when the object is known to be a dict](https://docs.astral.sh/ruff/rules/in-dict-keys/). Can we have configuration to only have this same control for the lint itself? i.e. only warn when it is known to be a dict

---

_Comment by @MichaReiser on 2025-07-28 09:45_

We could use ruff's limited type inference here. That means trading false positives with false negatives but this is an approach we've taken before. The ecosystem changes will show us if this is a feasible approach for SIM118 or not



---

_Label `help wanted` added by @MichaReiser on 2025-07-28 09:45_

---
