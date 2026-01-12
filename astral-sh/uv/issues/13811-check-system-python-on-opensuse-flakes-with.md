```yaml
number: 13811
title: "`check system | python on opensuse` flakes with \"context canceled\" while looking for GPG keys"
type: issue
state: open
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2025-06-03T13:57:28Z
updated_at: 2025-06-03T13:57:41Z
url: https://github.com/astral-sh/uv/issues/13811
synced_at: 2026-01-12T16:01:37Z
```

# `check system | python on opensuse` flakes with "context canceled" while looking for GPG keys

---

_@zanieb_

```
Run until zypper install -y python310 which && python3.10 -m ensurepip && mv /usr/bin/python3.10 /usr/bin/python3; do sleep 10; done
Retrieving repository 'openSUSE-Tumbleweed-Non-Oss' metadata [................................
Looking for gpg keys in repository openSUSE-Tumbleweed-Non-Oss.
  gpgkey=http://download.opensuse.org/tumbleweed/repo/non-oss/repodata/repomd.xml.key
context canceled
.......................
Error: The operation was canceled.
```

---

_Label `ci-flake` added by @zanieb on 2025-06-03 13:57_

---

_Comment by @zanieb on 2025-06-03 13:57_

e.g. https://github.com/astral-sh/uv/actions/runs/15419136863/job/43389413121?pr=13808

---
