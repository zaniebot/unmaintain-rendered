```yaml
number: 21039
title: Disable npm caching for playground
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ci
assignees: []
merged: true
base: main
head: micha/disable-playground-caching
created_at: 2025-10-23T07:44:08Z
updated_at: 2025-10-23T09:51:31Z
url: https://github.com/astral-sh/ruff/pull/21039
synced_at: 2026-01-12T15:57:15Z
```

# Disable npm caching for playground

---

_@MichaReiser_

## Summary

https://github.com/astral-sh/ruff/pull/20999 broke the ty playground deployment because it 
added npm caching without specifying the path to the package.json

I don't think the NPM caching gives us that much (this isn't a workflow that runs for every PR),
especially not if it also introduces us to cache poisoning (while the playground isn't critical, 
it's still something that many Astral employees and users access eveyr day). 

## Test Plan

Test in prod ;)


---

_Label `playground` added by @MichaReiser on 2025-10-23 07:44_

---

_Review requested from @woodruffw by @MichaReiser on 2025-10-23 07:44_

---

_Label `ci` added by @MichaReiser on 2025-10-23 07:44_

---

_@sharkdp approved on 2025-10-23 08:22_

---

_Merged by @MichaReiser on 2025-10-23 09:51_

---

_Closed by @MichaReiser on 2025-10-23 09:51_

---

_Branch deleted on 2025-10-23 09:51_

---
