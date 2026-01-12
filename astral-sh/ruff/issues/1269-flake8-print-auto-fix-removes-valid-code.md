```yaml
number: 1269
title: flake8-print auto fix removes valid code
type: issue
state: closed
author: serjflint
labels:
  - bug
assignees: []
created_at: 2022-12-17T08:10:31Z
updated_at: 2022-12-19T05:10:09Z
url: https://github.com/astral-sh/ruff/issues/1269
synced_at: 2026-01-12T15:54:41Z
```

# flake8-print auto fix removes valid code

---

_@serjflint_

```python
with open(output_filepath, "w") as out:
        for row in result:
            print(json.dumps(row), file=out)
```

becomes

```python
with open(output_filepath, "w") as out:
        for _row in result:
            pass
```

---

_Comment by @charliermarsh on 2022-12-17 15:05_

Is the idea here that we should ignore when writing with a file keyword?

---

_Comment by @charliermarsh on 2022-12-17 15:17_

(I wonder if those should even be flagged as errors?)

---

_Label `bug` added by @charliermarsh on 2022-12-17 15:57_

---

_Comment by @squiddy on 2022-12-17 17:00_

> (I wonder if those should even be flagged as errors?)

`flake8-print` doesn't really give away the intention with respect to the file keyword, but I feel like it is more towards removing superfluous debug statements, rather than completely denying any use of print/pprint.

So I would not even flag these cases as errors. 

---

_Comment by @charliermarsh on 2022-12-17 17:52_

I agree.

---

_Comment by @charliermarsh on 2022-12-17 20:54_

I can fix this, unless @squiddy has started on it.

---

_Comment by @squiddy on 2022-12-18 06:48_

Go ahead @charliermarsh :)

---

_Closed by @charliermarsh on 2022-12-19 05:10_

---
