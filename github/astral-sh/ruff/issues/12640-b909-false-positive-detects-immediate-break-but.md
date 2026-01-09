---
number: 12640
title: "`B909` False-positive: Detects immediate `break` but not `return`"
type: issue
state: closed
author: Avasam
labels:
  - bug
assignees: []
created_at: 2024-08-02T18:46:04Z
updated_at: 2024-08-02T22:14:18Z
url: https://github.com/astral-sh/ruff/issues/12640
synced_at: 2026-01-07T13:12:15-06:00
---

# `B909` False-positive: Detects immediate `break` but not `return`

---

_Issue opened by @Avasam on 2024-08-02 18:46_

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
B909

* A minimal code snippet that reproduces the bug.

```python
def __pop_image_type(split_image: Iterable[AutoSplitImage], image_type: ImageType):
    for image in split_image:
        if image.image_type == image_type:
            split_image.remove(image)
            return image

    return None
```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
`ruff check . --isolated --select=B909 --preview`
* The current Ruff version (`ruff --version`).
ruff 0.5.6

See the reproducer above, I'd expect that to pass, given that the below code also passes, but is much less cleaner:
```python
def __pop_image_type(split_image: list[AutoSplitImage], image_type: ImageType):
    for image in split_image:
        if image.image_type == image_type:
            split_image.remove(image)
            break
    else:
        return None

    return image
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-02 21:42_

---

_Label `bug` added by @charliermarsh on 2024-08-02 21:42_

---

_Referenced in [astral-sh/ruff#12646](../../astral-sh/ruff/pulls/12646.md) on 2024-08-02 21:43_

---

_Closed by @charliermarsh on 2024-08-02 22:14_

---

_Closed by @charliermarsh on 2024-08-02 22:14_

---
