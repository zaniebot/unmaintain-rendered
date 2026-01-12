```yaml
number: 1517
title: RET504 - false positive
type: issue
state: closed
author: Czaki
labels:
  - bug
assignees: []
created_at: 2022-12-31T18:59:08Z
updated_at: 2022-12-31T23:04:13Z
url: https://github.com/astral-sh/ruff/issues/1517
synced_at: 2026-01-12T15:54:41Z
```

# RET504 - false positive

---

_@Czaki_

I have checked that same problem also occurs in `flake8-return` so it is not a bug in logic translation, but definitely there is a problem with this rule. I also see #1035 with similar problems related to this plugin/set of rules.  

The following code is reported as breaking RET504.

```
    def get_changes(self):
        ret = self.changes
        self.changes = []
        return ret
```

On one side, it fits in the description, on the other side, I do not see any better way to write such code. 
Similarly to #1035 I do not use `flake8-return` earlier but would like to try these rules when seeing the description in the ruff readme. 

---

_Comment by @charliermarsh on 2022-12-31 19:03_

That makes sense. We could cause assignments to invalidate the rule. I think we already do that for function calls.

---

_Label `bug` added by @charliermarsh on 2022-12-31 19:03_

---

_Closed by @charliermarsh on 2022-12-31 23:04_

---
