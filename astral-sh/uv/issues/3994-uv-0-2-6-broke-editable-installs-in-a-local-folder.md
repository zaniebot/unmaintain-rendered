```yaml
number: 3994
title: uv 0.2.6 broke editable installs in a local folder
type: issue
state: closed
author: gibsondan
labels:
  - bug
assignees: []
created_at: 2024-06-03T18:56:31Z
updated_at: 2024-06-04T19:37:07Z
url: https://github.com/astral-sh/uv/issues/3994
synced_at: 2026-01-12T15:58:47Z
```

# uv 0.2.6 broke editable installs in a local folder

---

_@gibsondan_

MRE:

```
git clone --depth 1 https://github.com/dagster-io/dagster.git
cd dagster/python_modules
uv pip install -e dagster -e dagster-pipes
```

on uv 0.2.5:

```
(dagster-3.11.7) dgibson@MacBook-Pro python_modules % uv pip install -e dagster -e dagster-pipes
Resolved 44 packages in 529ms
   Built dagster @ file:///Users/dgibson/dagster/python_modules/dagster
   Built dagster-pipes @ file:///Users/dgibson/dagster/python_modules/dagster-pipes                                                                                                                                                      Downloaded 2 packages in 589ms
Uninstalled 2 packages in 2ms
Installed 2 packages in 1ms
 - dagster==1!0+dev (from file:///Users/dgibson/dagster/python_modules/dagster)
 + dagster==1!0+dev (from file:///Users/dgibson/dagster/python_modules/dagster)
 - dagster-pipes==1!0+dev (from file:///Users/dgibson/dagster/python_modules/dagster-pipes)
 + dagster-pipes==1!0+dev (from file:///Users/dgibson/dagster/python_modules/dagster-pipes)
```

On uv 0.2.6:

```
(dagster-3.11.7) dgibson@MacBook-Pro python_modules % uv pip install -e dagster -e dagster-pipes
error: Editable `dagster` must refer to a local directory
```


---

_Comment by @charliermarsh on 2024-06-03 18:57_

Thanks, taking a look.

---

_Comment by @charliermarsh on 2024-06-03 19:00_

Apologies, you can do `uv pip install -e ./dagster -e ./dagster-pipes`. I thought I had tested that `pip` also required a path separator for this. Will fix.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-03 19:00_

---

_Label `bug` added by @charliermarsh on 2024-06-03 19:00_

---

_Comment by @charliermarsh on 2024-06-03 19:07_

I actually wonder if pip will disallow this in the future... Because `pip install dagster` does _not_ install `./dagster`.

---

_Comment by @charliermarsh on 2024-06-03 19:23_

Fixed in https://github.com/astral-sh/uv/pull/3995.

---

_Comment by @gibsondan on 2024-06-03 19:24_

Thanks! We'll add the path separator either way, seems more explicit if nothing else.

---

_Comment by @charliermarsh on 2024-06-03 19:58_

I think that's smart -- it's the most forwards-compatible.

---

_Closed by @charliermarsh on 2024-06-04 19:37_

---
