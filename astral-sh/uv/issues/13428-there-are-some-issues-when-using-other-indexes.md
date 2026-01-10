---
number: 13428
title: There are some issues when using other indexes
type: issue
state: open
author: zlpmetyou
labels:
  - needs-mre
assignees: []
created_at: 2025-05-13T10:08:25Z
updated_at: 2025-09-05T12:58:36Z
url: https://github.com/astral-sh/uv/issues/13428
synced_at: 2026-01-10T01:25:33Z
---

# There are some issues when using other indexes

---

_Issue opened by @zlpmetyou on 2025-05-13 10:08_

### Summary

create a new project
```
uv init demo
```
install flask with default
```
cd demo
uv add flask
```
uv.lock like as

![Image](https://github.com/user-attachments/assets/ccf5b7cc-e8eb-4412-9f1d-a8f9d3ca77ee)

it used default PyPI, it's ok

Then i want to install a package from private index, i add index to `pyproject.toml`

![Image](https://github.com/user-attachments/assets/dce1fc6d-60d8-4f99-a48e-2a21ce882951)

install package
```
uv add 
```
### issue 1:

![Image](https://github.com/user-attachments/assets/cc4f7d39-573b-417a-bd8c-b00407e7170b)
in `uv.lock` , all packages source have been replaced by private index, Does this mean that all packages are installed from a private index？

then i search from uv documentation

![Image](https://github.com/user-attachments/assets/80bbf0dd-a873-4bb0-a8fd-6f9ccb1557ec)

do it!  remove package and index first
```
uv remove configserver-python
```

![Image](https://github.com/user-attachments/assets/903f6262-9d8b-49f6-947d-24db5e9abc41)


### issue 2:

![Image](https://github.com/user-attachments/assets/397a373e-fdfc-4e46-b1c9-e5097ca863ef)
all packages source in `uv.lock` still my private index

uv lock? do it
```
uv lock
```
![Image](https://github.com/user-attachments/assets/8e91178f-444f-4431-a971-97899501cb62)
nothing happened, still private index
uv lock --refresh? 
```
uv lock --refresh
```

![Image](https://github.com/user-attachments/assets/9b979019-8a89-45e0-b816-0b2cbd7de6c3)

still private index.

ok, remove it . redo all.

![Image](https://github.com/user-attachments/assets/c495f333-6aad-43a1-b404-93224f38da39)

```
uv add flask configserver-python
```


### Platform

Ubuntu24.04

### Version

uv 0.7.3

### Python version

Python 3.12.3

---

_Label `bug` added by @zlpmetyou on 2025-05-13 10:08_

---

_Comment by @charliermarsh on 2025-05-15 02:38_

To clarify, if you remove the `uv.lock` and then run `uv lock`, do you end up in the desired state?

---

_Label `bug` removed by @charliermarsh on 2025-05-18 21:27_

---

_Label `needs-mre` added by @charliermarsh on 2025-05-18 21:27_

---

_Comment by @zlpmetyou on 2025-06-26 09:39_

@charliermarsh When I specify the private index explicit=true, the problem is that the private package I am about to install depends on another package that only exists in the private index, but UV will search in the public index and will prompt an error message stating that the package cannot be found. Can we make the package that the private package depends on also search in the private index

---

_Comment by @konstin on 2025-09-05 12:58_

This is the same problem as in https://github.com/astral-sh/uv/issues/8253, we currently don't apply explicit indexes transitively.

---
