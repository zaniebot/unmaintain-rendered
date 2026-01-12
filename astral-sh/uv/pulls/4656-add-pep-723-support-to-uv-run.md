```yaml
number: 4656
title: Add PEP 723 support to uv run
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/pep723
created_at: 2024-06-29T23:57:26Z
updated_at: 2024-07-01T12:20:27Z
url: https://github.com/astral-sh/uv/pull/4656
synced_at: 2026-01-12T16:06:22Z
```

# Add PEP 723 support to uv run

---

_@charliermarsh_

Closes #3096 

## Summary

Enables `uv run foo.py` to execute PEP 723-compatible scripts.

For example, given:

```python
# /// script
# requires-python = ">=3.11"
# dependencies = [
#   "requests<3",
#   "rich",
# ]
# ///

import requests
from rich.pretty import pprint

resp = requests.get("https://peps.python.org/api/peps.json")
data = resp.json()
pprint([(k, v["title"]) for k, v in data.items()][:10])
```

![Screenshot 2024-06-29 at 7 23 52 PM](https://github.com/astral-sh/uv/assets/1309177/c60f2415-4874-4b15-b9f5-dd8c8c35382e)


---

_Review requested from @zanieb by @charliermarsh on 2024-06-29 23:57_

---

_Label `preview` added by @charliermarsh on 2024-06-29 23:57_

---

_Comment by @charliermarsh on 2024-06-29 23:58_

There are some refactors I want to do as a follow-up.

---

_Comment by @charliermarsh on 2024-06-29 23:58_

Should we suppress output here...?

---

_Review requested from @konstin by @charliermarsh on 2024-06-29 23:58_

---

_@charliermarsh reviewed on 2024-06-29 23:59_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:79 on 2024-06-29 23:59_

I will DRY this up separately, I need to audit the usages.

---

_@zanieb reviewed on 2024-06-30 03:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:118 on 2024-06-30 03:41_

Ah thank you! I need a guide on our various requirement types haha

---

_@zanieb reviewed on 2024-06-30 03:42_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:178 on 2024-06-30 03:42_

Because I don't think there's coverage for this, can you add a test for running a non PEP 723 script?

---

_@charliermarsh reviewed on 2024-06-30 14:45_

---

_Review comment by @charliermarsh on `crates/uv/tests/run.rs`:178 on 2024-06-30 14:45_

Done!

---

_@charliermarsh reviewed on 2024-06-30 14:46_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:118 on 2024-06-30 14:46_

I'm gonna audit the usages in these commands, all good.

---

_Review requested from @zanieb by @charliermarsh on 2024-06-30 14:46_

---

_@charliermarsh reviewed on 2024-06-30 14:46_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:118 on 2024-06-30 14:46_

(But, as a separate PR.)

---

_@konstin approved on 2024-07-01 08:06_

---

_Merged by @charliermarsh on 2024-07-01 12:20_

---

_Closed by @charliermarsh on 2024-07-01 12:20_

---

_Branch deleted on 2024-07-01 12:20_

---
