```yaml
number: 2946
title: Add ecosystem test for homeassistant
type: pull_request
state: closed
author: zanieb
labels:
  - testing
assignees: []
base: main
head: zb/ecosystem-test
created_at: 2024-04-09T21:02:27Z
updated_at: 2024-04-10T16:42:30Z
url: https://github.com/astral-sh/uv/pull/2946
synced_at: 2026-01-12T16:05:19Z
```

# Add ecosystem test for homeassistant

---

_@zanieb_

Extends #2942 with another repository.

This one might be too big it takes 5 minutes haha — but it's faster than Windows still!

---

_Label `testing` added by @zanieb on 2024-04-09 21:02_

---

_@charliermarsh approved on 2024-04-09 23:53_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:243 on 2024-04-10 08:26_

I think we can skip the parentheses

```suggestion
          - repo: "prefecthq/prefect",
            command: "uv pip install --upgrade -e '.[dev]'",
            python: "3.9",
          - repo: "home-assistant/core",
            command: "uv pip install -r requirements_all.txt",
            python: "3.12",
```

---

_Review comment by @konstin on `.github/workflows/ci.yml`:242 on 2024-04-10 08:26_

Do we need to perform a checkout?

---

_Review comment by @konstin on `.github/workflows/ci.yml`:258 on 2024-04-10 08:28_

We can speed this up with depth 1, `actions/checkout@v4` should do this by default

---

_@konstin reviewed on 2024-04-10 08:28_

---

_@zanieb reviewed on 2024-04-10 13:50_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:242 on 2024-04-10 13:50_

Nope! :D

```suggestion
```

---

_Comment by @konstin on 2024-04-10 14:53_

Home assistant uses a custom index, no build and a musl based python 3.12 for their deploys, i think we need to replicate this if we want to use it as a test

---

_Closed by @zanieb on 2024-04-10 16:42_

---
