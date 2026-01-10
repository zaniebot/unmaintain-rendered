```yaml
number: 2614
title: uv pip compile fails with conflicting urls (sha vs tag) across dependencies
type: issue
state: closed
author: fraenkel
labels:
  - bug
assignees: []
created_at: 2024-03-22T14:04:35Z
updated_at: 2024-04-02T23:36:37Z
url: https://github.com/astral-sh/uv/issues/2614
synced_at: 2026-01-10T05:40:32Z
```

# uv pip compile fails with conflicting urls (sha vs tag) across dependencies

---

_Issue opened by @fraenkel on 2024-03-22 14:04_

I am issuing the following `pipenv requirements | uv pip compile --generate-hashes -o requirements.txt -` which fails with:
```
error: There are conflicting URLs for package `common`:
- git+ssh://git@github.com/cerebrotech/platform-python-common.git@565787e8abc8a81b18ea49770f98df09453987e6
- git+ssh://git@github.com/cerebrotech/platform-python-common.git@v3.0.1
```

git shows the following for the v3.0.1 tag: 
`commit 565787e8abc8a81b18ea49770f98df09453987e6 (HEAD -> develop, tag: v3.0.1, origin/develop, origin/HEAD)`

The current package has the following in its requirements.txt
`common@ git+ssh://git@github.com/cerebrotech/platform-python-common.git@565787e8abc8a81b18ea49770f98df09453987e6`

However there is a dependency on another package `platform-python-catalog` which has in its pyproject.toml, 
`"common @ git+ssh://git@github.com/cerebrotech/platform-python-common.git@v3.0.1"`

According to the debug, the failure occurs when resolving
`DEBUG Searching for a compatible version of catalog @ git+ssh://git@github.com/cerebrotech/platform-python-catalog.git@11866ba07919d174c77b8a161c0f10b0073003c8 (*)`

---

_Comment by @zanieb on 2024-03-22 14:22_

Related https://github.com/astral-sh/uv/issues/1903 / #2285

---

_Label `bug` added by @zanieb on 2024-03-22 14:23_

---

_Comment by @charliermarsh on 2024-03-22 14:24_

The common way to support this is to add `git+ssh://git@github.com/cerebrotech/platform-python-common.git@v3.0.1` as a constraint. (I'm not certain whether we'll support it in general without the constraint.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-02 00:04_

---

_Closed by @charliermarsh on 2024-04-02 23:36_

---
