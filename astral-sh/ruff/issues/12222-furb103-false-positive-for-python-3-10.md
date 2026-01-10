```yaml
number: 12222
title: "`FURB103` false positive for python <3.10"
type: issue
state: closed
author: danieleades
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-07-07T07:17:23Z
updated_at: 2024-07-09T02:43:33Z
url: https://github.com/astral-sh/ruff/issues/12222
synced_at: 2026-01-10T11:09:54Z
```

# `FURB103` false positive for python <3.10

---

_Issue opened by @danieleades on 2024-07-07 07:17_

`FURB103` has a false positive if the 'newline' argument is used in python <3.10

consider the following code snippet:

```python
 with open(outfile, 'w', encoding='utf-8', newline=newline) as f:
     f.write(rendered_file)
```

the lint suggests using

```python
Path(outfile).write_text(rendered_file, encoding='utf-8', newline=newline)
```

`Path.write_text` only supports the newline argument for python 3.10+, but this lint suggest the change even if you're targeting an earlier version of Python. This means the lint has to be manually suppressed. see https://github.com/cookiecutter/cookiecutter/pull/2061/files#diff-02c88bfba84f4a832d0e00474c6fac0000b940456bad66d925eaebf5839d94deR256

---

_Label `bug` added by @MichaReiser on 2024-07-07 09:01_

---

_Label `help wanted` added by @MichaReiser on 2024-07-07 09:01_

---

_Comment by @evanrittenhouse on 2024-07-08 01:03_

Feel free to assign this to me

---

_Assigned to @evanrittenhouse by @charliermarsh on 2024-07-08 01:10_

---

_Closed by @charliermarsh on 2024-07-09 02:43_

---
