```yaml
number: 4786
title: "Use `flask` in lieu of `black` for tool tests"
type: pull_request
state: closed
author: charliermarsh
labels:
  - testing
assignees: []
base: main
head: charlie/flask
created_at: 2024-07-03T18:28:16Z
updated_at: 2024-07-09T20:01:33Z
url: https://github.com/astral-sh/uv/pull/4786
synced_at: 2026-01-12T16:06:27Z
```

# Use `flask` in lieu of `black` for tool tests

---

_@charliermarsh_

## Summary

On my machine, this reduces the test time from ~12 seconds to 4-5 seconds, since the package is so much smaller and the cold starts aren't as dramatic as `black`.



---

_Label `testing` added by @charliermarsh on 2024-07-03 18:28_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-03 18:28_

---

_Comment by @zanieb on 2024-07-03 18:36_

Let's just create a project in `astral-test`? It should be trivial to publish a package with some entry points. I can do it this week.

---

_Comment by @charliermarsh on 2024-07-03 18:42_

Ok, but I need this like... ASAP. The tests are so slow for me. I suppose I can do it now.

---

_Comment by @zanieb on 2024-07-03 18:48_

I don't mind if you merge this in the meantime but it feels like a bit of a hack and I don't love the extra being included. 🤷‍♀️ 

---

_Comment by @charliermarsh on 2024-07-03 18:51_

Can I change it to Flask in the meantime?

---

_Renamed from "Use `python-dotenv` in lieu of `black` for tool tests" to "Use `flask` in lieu of `black` for tool tests" by @charliermarsh on 2024-07-03 18:56_

---

_@zanieb approved on 2024-07-03 22:30_

---

_Closed by @charliermarsh on 2024-07-09 20:01_

---
