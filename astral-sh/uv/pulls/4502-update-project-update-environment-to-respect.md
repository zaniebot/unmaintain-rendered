```yaml
number: 4502
title: "Update `project::update_environment` to respect reinstall options"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: zb/reinstall-satisfies
created_at: 2024-06-25T01:17:34Z
updated_at: 2024-06-25T21:12:52Z
url: https://github.com/astral-sh/uv/pull/4502
synced_at: 2026-01-10T13:48:28Z
```

# Update `project::update_environment` to respect reinstall options

---

_Pull request opened by @zanieb on 2024-06-25 01:17_

While working on https://github.com/astral-sh/uv/pull/4492 I noticed that `--reinstall-package` was not actually respected by `update_environment`, it exited early due to satisfied requirements.

Before

```

❯ cargo run -q -- tool install black -v --reinstall-package tomli
...
DEBUG All requirements satisfied: black | click>=8.0.0 | mypy-extensions>=0.4.3 | packaging>=22.0 | pathspec>=0.9.0 | platformdirs>=2 | tomli>=1.1.0 ; python_version < '3.11' | typing-extensions>=4.0.1 ; python_version < '3.11'
```

After

```
❯ cargo run -q -- tool install black -v --reinstall-package tomli
...
Uninstalled 1 package in 0.99ms
Installed 1 package in 4ms
 - tomli==2.0.1
 + tomli==2.0.1
```

---

_Label `bug` added by @zanieb on 2024-06-25 01:17_

---

_Label `preview` added by @zanieb on 2024-06-25 01:19_

---

_Label `preview` added by @zanieb on 2024-06-25 01:19_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-25 18:59_

---

_Marked ready for review by @zanieb on 2024-06-25 18:59_

---

_@charliermarsh approved on 2024-06-25 21:12_

---

_Merged by @charliermarsh on 2024-06-25 21:12_

---

_Closed by @charliermarsh on 2024-06-25 21:12_

---

_Branch deleted on 2024-06-25 21:12_

---
