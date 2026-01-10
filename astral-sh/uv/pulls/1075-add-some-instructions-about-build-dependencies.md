```yaml
number: 1075
title: Add some instructions about build dependencies
type: pull_request
state: merged
author: konstin
labels:
  - documentation
  - windows
assignees: []
merged: true
base: main
head: konsti/contributing-guide
created_at: 2024-01-24T13:00:32Z
updated_at: 2024-01-24T17:03:56Z
url: https://github.com/astral-sh/uv/pull/1075
synced_at: 2026-01-10T15:39:03Z
```

# Add some instructions about build dependencies

---

_Pull request opened by @konstin on 2024-01-24 13:00_

You need to install cmake on windows, so i added a hint about using `pipx install cmake`, and some more general notes on building and testing puffin.

See #817

---

_Label `documentation` added by @konstin on 2024-01-24 13:00_

---

_Label `windows` added by @konstin on 2024-01-24 13:00_

---

_Comment by @konstin on 2024-01-24 13:02_

I don't know what vistual studio component we exactly require, it worked with what i had installed. Feel free to add instructions for mac. I also intentionally didn't go into detail about installing all the python versions to wait for #1070.

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:5 on 2024-01-24 14:37_

Nit: Oxford comma :)

---

_@charliermarsh reviewed on 2024-01-24 14:37_

---

_@charliermarsh reviewed on 2024-01-24 14:38_

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:7 on 2024-01-24 14:38_

I'd remove "as a faster `cargo test` replacement". `--all-features` will also add a dependency on PyO3, right?

---

_@charliermarsh approved on 2024-01-24 14:39_

---

_Comment by @zanieb on 2024-01-24 15:06_

Related to #817 

---

_@konstin reviewed on 2024-01-24 16:59_

---

_Review comment by @konstin on `CONTRIBUTING.md`:7 on 2024-01-24 16:59_

Yes, is that a problem? (see also https://github.com/astral-sh/puffin/pull/941#pullrequestreview-1829737943)

---

_@konstin reviewed on 2024-01-24 17:03_

---

_Review comment by @konstin on `CONTRIBUTING.md`:5 on 2024-01-24 17:03_

nooo, what are you doing to the german speakers :P

---

_Merged by @konstin on 2024-01-24 17:03_

---

_Closed by @konstin on 2024-01-24 17:03_

---

_Branch deleted on 2024-01-24 17:03_

---
