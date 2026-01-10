```yaml
number: 10050
title: Fix mirror script to handle newer metadata format.
type: pull_request
state: merged
author: bw513
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-mirror-script-arch-filter
created_at: 2024-12-20T09:46:04Z
updated_at: 2024-12-20T13:42:29Z
url: https://github.com/astral-sh/uv/pull/10050
synced_at: 2026-01-10T12:00:01Z
```

# Fix mirror script to handle newer metadata format.

---

_Pull request opened by @bw513 on 2024-12-20 09:46_

## Summary

The architecture details in `crates/uv-python/download-metadata.json` is now a dictionary with family and variant data, whereas it used to be a string. This patches the architecture filter in `scripts/create-python-mirror.py` to support both scenarios.

## Test Plan

Tested manually using `uv run ./scripts/create-python-mirror.py --name cpython --arch x86_64 --os linux --from-all-history`

---

_Review comment by @nathanscain on `scripts/create-python-mirror.py`:94 on 2024-12-20 09:57_

`match` added in 3.10 - script claims to support >=3.8 in metadata at top of file.

---

_Review comment by @nathanscain on `scripts/create-python-mirror.py`:97 on 2024-12-20 10:02_

Can't use because of other issue with supporting 3.8 and 3.9 - but for future reference keep in mind that this does not test that the input to `match` is a string. It simply assigns str as a variable to the input - which should be flagged as reassigning over a default name.

To test that something is a string do:

```python
match x:
    case variable if isinstance(variable, str):
        print(f"input was a string: {variable}")
    case _:
        print(f"input was NOT a string: {x}"
```

---

_@nathanscain reviewed on 2024-12-20 10:03_

---

_Review comment by @nathanscain on `scripts/create-python-mirror.py`:98 on 2024-12-20 10:04_

per above, this line is unreachable

---

_@nathanscain reviewed on 2024-12-20 10:05_

---

_@bw513 reviewed on 2024-12-20 10:58_

---

_Review comment by @bw513 on `scripts/create-python-mirror.py`:94 on 2024-12-20 10:58_

Thanks, I have switch away from using match :)

---

_Label `bug` added by @charliermarsh on 2024-12-20 13:27_

---

_Merged by @charliermarsh on 2024-12-20 13:37_

---

_Closed by @charliermarsh on 2024-12-20 13:37_

---

_Branch deleted on 2024-12-20 13:38_

---

_Comment by @nathanscain on 2024-12-20 13:40_

@charliermarsh is there a reason nothing in the pipeline flagged that `match` was used in a script claiming to support Python 3.8+? Are these scripts excluded from linting?

---

_Comment by @bw513 on 2024-12-20 13:42_

Thank you :)

---
