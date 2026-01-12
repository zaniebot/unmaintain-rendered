```yaml
number: 2724
title: "PLE1307: false positives"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-02-10T18:00:49Z
updated_at: 2023-02-10T19:26:00Z
url: https://github.com/astral-sh/ruff/issues/2724
synced_at: 2026-01-12T15:54:43Z
```

# PLE1307: false positives

---

_@spaceone_

```sh
$ echo "import random; '%d' % (random.randint(0, 100),)" | ruff --isolated --select PLE1307 -
-:1:16: PLE1307 Format type does not match argument type
```

further ones:
`"-%f" % time.time()`
`"%r" % (object['dn'],)`
`r'\%03o' % (ord(c),)`
`('%02X' % int(_) for _ in o)`
`"%s;range=%d-*" % (attr, upper + 1)`
`"%d" % (len(foo),)`

---

_Renamed from "PLE1307: false positive" to "PLE1307: false positives" by @spaceone on 2023-02-10 18:02_

---

_Comment by @charliermarsh on 2023-02-10 18:10_

Cool, we'll either fix these or disable the rule for now prior to the next release.

---

_Label `bug` added by @charliermarsh on 2023-02-10 18:10_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-10 18:32_

---

_Closed by @charliermarsh on 2023-02-10 19:26_

---
