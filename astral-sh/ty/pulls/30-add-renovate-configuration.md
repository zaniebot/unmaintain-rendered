```yaml
number: 30
title: Add renovate configuration
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: renovate
created_at: 2025-05-05T14:55:44Z
updated_at: 2025-07-08T10:39:21Z
url: https://github.com/astral-sh/ty/pull/30
synced_at: 2026-01-12T15:54:27Z
```

# Add renovate configuration

---

_@MichaReiser_

## Summary

Adds a basic renovate configuration. I only enabled github actions and pre-commit for now. We can enable more managers once we have the need for them in ty

Closes https://github.com/astral-sh/ty/issues/19

## Test plan

```bash
npx --yes --package renovate -- renovate-config-validator
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully
```


---

_@AlexWaygood approved on 2025-05-05 14:56_

---

_Comment by @AlexWaygood on 2025-05-05 14:58_

We'll also need to grant the renovate app access to this repo

---

_Comment by @MichaReiser on 2025-05-05 15:00_

> We'll also need to grant the renovate app access to this repo

That's good to know. Let me figure out how I do that

---

_Comment by @zanieb on 2025-05-05 16:19_

Done!

---

_Merged by @zanieb on 2025-05-05 16:20_

---

_Closed by @zanieb on 2025-05-05 16:20_

---

_Branch deleted on 2025-07-08 10:39_

---
