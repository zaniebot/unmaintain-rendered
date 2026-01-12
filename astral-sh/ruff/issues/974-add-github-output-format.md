```yaml
number: 974
title: Add GitHub output format
type: issue
state: closed
author: edgarrmondragon
labels:
  - configuration
assignees: []
created_at: 2022-12-01T01:50:37Z
updated_at: 2022-12-01T15:22:13Z
url: https://github.com/astral-sh/ruff/issues/974
synced_at: 2026-01-12T15:54:40Z
```

# Add GitHub output format

---

_@edgarrmondragon_

GitHub actions can annotate code locations using so-called [workflow commands](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions).

For example:

```yaml
      - name: Create annotation for build error
        run: echo "::error file=app.js,line=1,col=1::Missing semicolon"
```

If this is deemed valuable, I can submit a PR.



---

_Comment by @charliermarsh on 2022-12-01 01:51_

Oh cool, I've never seen that. Happy to include it!

---

_Label `enhancement` added by @charliermarsh on 2022-12-01 01:51_

---

_Label `configuration` added by @charliermarsh on 2022-12-01 01:51_

---

_Closed by @charliermarsh on 2022-12-01 15:22_

---
