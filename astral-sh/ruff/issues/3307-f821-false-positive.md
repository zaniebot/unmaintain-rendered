```yaml
number: 3307
title: F821 false positive
type: issue
state: closed
author: JonathanPlasse
labels:
  - bug
assignees: []
created_at: 2023-03-02T14:40:18Z
updated_at: 2023-03-03T23:07:32Z
url: https://github.com/astral-sh/ruff/issues/3307
synced_at: 2026-01-12T15:54:43Z
```

# F821 false positive

---

_@JonathanPlasse_

```python
import bpy

class LIGHTSHOW_OT_letter_creation(bpy.types.Operator):
    bl_label = "Create Character"
    bl_idname = "lightshow.letter_creation"

    filepath: bpy.props.StringProperty(subtype="FILE_PATH")
```

Ruff raises `F821` unused variable. It should not because `"FILE_PATH"` is not a forward reference as it is in parentheses and not in brackets.
[Link to the playground](https://play.ruff.rs/#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksufkoirhwFbGQqemZDbmw-CgYlLgaPAAWugAcQ43FpeVCbKIAojHTUABikGyVojxKGig88jXEnHi6kAAO0gDMAIwu86L4sNAA1ihDGNC46Rt8eIJgoBCiMlOGH4nB2mR4hFqunwcGKsVExEIx2OmWIxAw4iksk2PxhsDhC0giORqPR6VwXXBkNw-A00NhKHhUDgiCQRQ2ilemleuGk9PxjMJzQyXSI8FkhFwPGkx3Q-OKbAAvo07o9nhgyG80KDdH9-pB+ko0Dg0OgMDwJMcMZxMmUMnypv99QB6C3HG5OqDOhSKV2Wj1OyDO01yZ3EfoSFZOtb6rD9FBYB7mmVm+JYFDHL64eUoJUq+5PF4cDRkFBKXWE6qFThSQgbChdLAsn5MPO5VWFjVcXj8H560Qcbh8ATAtIijrESYMRht24F9U4S2ZeMCA4CCuBlnIa1lbDNjBIUH9DBPaRIDKYTaSJI8XEM2dQDvqmqKCTEOl0ftQCTQLTs5S9N0SR9IMdAuA+kBPi8ACOhAIIkfZCrgHQUhgsHwVQRxoAghANgG36ED2KFdOhCFYTheFMmICBYJOAEaGhcFkXQ1G4ZQ+EwHICCcJgVBNrKug8IohC5hAyrtvOLzjPgG76sKmQYHUEiYY6nqQEYP5PPhohGNAxDamU2lQEYmTHPc6ZGep+CcCUaDUEyOnWbZRimjChG3lG-wzmJ+Zqi8fBoLINbHBkHmfoSmq4OoJTQHwchdMFoU-CcSg3vZEVvBSl7HJwuqKhBUHJrK2DxomtKyQiQmcIZdB4gSgbxCglo7BICDiJQLYLOsMrlQs3lgOJc5+cClodFgoL-tgeBNlOX4wPA24SO53CdIJwmiQNvmdolig7DguDxVKa6IZuHR6SpvxdVAxxanpRzHGglmnEoijIPd0BPYgPCwNIAB08QolQyXHDoDlQMU5AZNmLHEOuYOQAUxAZHcb0sTwMnw7gkinEcuDuvDP48GZ8EdGQRyjZZhPE99nBkL9pzU-dvBPQg9yKMDsBPW8JT3dm8NwBsNpHALln9KzCByDZSDJf0cieWJBWSRgpwITsk7SOx4WBtZCSQl0N2ZFK8bFD8QkiVRBvQMpVW2F0SlUEVWRo0i7EW0oVvbMotvyHAInotKAksRO5Ru4oHs210CiwH76jbgHTtQBCZlZFRYecCb0WwTaXSE3GGD4Bk2RXZAABC50zFo6aZmulkV1XWaWQAar7KAzIor2KJZADyADKbcd5ZACSXf94X8MzAdNp4MpUqj538NI4m2z-e3hd9an0Dpw7VaYJkWcKbnx4F53QgxgCShJgbN7GxdZsbYNj5K5KJBmleM9hZdgbyZHSgqAZikezvAKCC6dQoVUfBkdMB4w5WjgCoE2OYqLHygfsXoahiKIMJKgjQ6DOgYDTNHfY3w2iBkXGQPBekRohV2kA+q+piBmQmngc0acUIMUXD+Na5tCQXhqBqWQ8cuGCm1pAroSNdoHiPPwIoCYsy0OEfqZBXQeAIHNAgd0p8qIPHSEgKK1l2Y7ANtKEh+ptHICigMG0mAjEOmnFonRUVEBNlgPnVmpoT52MrAkMO7JbqKEwKTMOihbE5EDJke4cUEqWiShgXhHjIKQgGFQHYKjsCIBNiHQke8uCZBSNEmhJjRBYHuGiC6oTYzfA2FKFsZ8oA+hUA2GpVF0jdE6OiUsx8E7lNEMRdE0B8CJE5DtD+GALhUV6RqbYqAkiOx+C4JBojMDFEtokCoEEJBYCbKWcBkAfx-kXMnLQoJbFXAgrKY4LwlLlS1nJMcCl7adU9ODbYABVfGxdEhKAACLmMssUHgbyADCJT0pPIRmWRQPzdHAr0qCtS-y3kAFk2qERTh8iFULcDIvamisFelpC4CwD3V57y8XEAJVgAAKhi35C8SXUsnF82KH14Ywm4HrOuGYG7w0QL0RFQNoBbEpr+L5nB8AyXXoSYpsLrZi0wKaHAYcVHsyLmC6VaJZVtXlmAWpRJPjVU1fKhMGRYoZEefC-VWBDXav6g-E40gTTJI1gnOa39YnxUULyhiGxiBJg4WlHMZzpDYVotKTWn9bktC6Aqk1yqalBpQrNQkW42Q-g0NVH20dlEpnNYGOilkyDSDIpKwMeyNRh0JcbXQFwABMVEy1UlqD8AAbPW38XINA-AAKxtr-JOWKTUA10C7eBHyuRThIg0GHU0OyngZnUJKPgykirXLAHVe+IBFSRG3cMsAZBTggBAOq4gYAAAyg8ADiAAJSlPcr1dwAOpGC7pSowlAeCDKMFgTIsU1wGH3X9eOxBfpd1lEqjIYQaALAoG+8gJQwAAF4oCAp-YkMAgLDRhzKDUbVMGeL20Q1ADoGh+i3jFkgX677P3frLA3Q9TprKUBugMOgAH6avWOMBnuVVegAAUOM1GlAYREZB44IcgIsQep6ZhGF4-YSlV7IBhBAEAA)

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `bug` added by @charliermarsh on 2023-03-02 15:19_

---

_Comment by @charliermarsh on 2023-03-02 15:19_

That is interesting... Will look into it.

---

_Comment by @JonathanPlasse on 2023-03-02 15:31_

It will also raise `F722` if the string is `""` or `"A value ..."`.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-02 18:19_

---

_Comment by @charliermarsh on 2023-03-02 21:49_

I don't really know how to reason about this. Sometimes arguments in parentheses _should_ be treated as forward references, e.g., `ForwardRef(...)`. We encode a bunch of those as special-cases, but I'm wondering if the _default_ should be _not_ treat those as references.

---

_Comment by @charliermarsh on 2023-03-02 22:05_

I don't think it's legal to have function calls in type definitions like that. At least, Mypy and Pyright don't allow it.

---

_Comment by @charliermarsh on 2023-03-02 22:24_

Pyright gives you: `error: Illegal type annotation: call expression not allowed (reportGeneralTypeIssues)`.

I guess we could try to avoid false positives here, but it's a little strange because it seems like it might be an error.

---

_Comment by @JonathanPlasse on 2023-03-03 05:20_

FYI, This is taken from the Blender API.

---

_Comment by @onerandomusername on 2023-03-03 05:38_

> I don't think it's legal to have function calls in type definitions like that. At least, Mypy and Pyright don't allow it.

While they do complain it isn't illegal. [`typing.Annotated`](https://docs.python.org/3/library/typing.html?highlight=annotated#typing.Annotated) allows it, and anything that's user implemented can allow it too. 

---

_Comment by @JonathanPlasse on 2023-03-03 09:18_

`typing.Annotated` is a particular case as the call is inside brackets.

This is correct
```python
x: Annotated[int, ValueRange(-10, 5)]
```
This is invalid
```python
x: func(x, y, z)
```

As this issue is about non-standard Python, you can close this issue. I will add `noqa`s where it is needed.

---

_Comment by @charliermarsh on 2023-03-03 15:45_

I think it'd be nice not to error here. I'll keep it open for now.

---

_Comment by @charliermarsh on 2023-03-03 22:18_

Actually seems like Mypy doesn't complain (as in, doesn't call it invalid) for any callables within subscripts:

```py
def f(x: Foo[int, Bar(-10, 5)]):
    pass
```

---

_Closed by @charliermarsh on 2023-03-03 23:07_

---
