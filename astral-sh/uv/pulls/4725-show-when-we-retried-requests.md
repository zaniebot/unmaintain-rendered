```yaml
number: 4725
title: Show when we retried requests
type: pull_request
state: merged
author: konstin
labels:
  - error messages
  - network
assignees: []
merged: true
base: main
head: konsti/log-retries
created_at: 2024-07-02T09:49:15Z
updated_at: 2024-07-02T17:04:14Z
url: https://github.com/astral-sh/uv/pull/4725
synced_at: 2026-01-10T13:48:28Z
```

# Show when we retried requests

---

_Pull request opened by @konstin on 2024-07-02 09:49_

In #3514 and #2755, users had intermittent network errors, but it was not always clear whether we had already retried these requests or not. Building upon https://github.com/TrueLayer/reqwest-middleware/pull/159, this PR adds the number of retries to the error message, so we can see at first glance where we're missing retries and where we might need to change retry settings.

Example error trace:

```
Could not connect, are you offline?
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://pypi.org/simple/uv/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Name or service not known
  Caused by: failed to lookup address information: Name or service not known
```

This code is ugly since i'm missing a better pattern for attaching context to reqwest middleware errors in https://github.com/TrueLayer/reqwest-middleware/pull/159.

---

_Review requested from @BurntSushi by @konstin on 2024-07-02 09:49_

---

_Review requested from @charliermarsh by @konstin on 2024-07-02 09:49_

---

_Label `network` added by @konstin on 2024-07-02 09:49_

---

_Label `tracing` added by @konstin on 2024-07-02 09:49_

---

_Label `tracing` removed by @konstin on 2024-07-02 09:49_

---

_Label `error messages` added by @konstin on 2024-07-02 09:49_

---

_@charliermarsh approved on 2024-07-02 12:45_

---

_Comment by @zanieb on 2024-07-02 14:53_

Should we wait to merge until we're off the git-deps?

---

_Comment by @konstin on 2024-07-02 16:41_

I haven't gotten any feedback from the maintainers yet and i want to ship this in uv so we get better user reports for network failures.

---

_Comment by @zanieb on 2024-07-02 16:43_

👍 Can we open an issue to track moving them into the Astral org then? 

---

_@zanieb approved on 2024-07-02 16:59_

---

_Merged by @konstin on 2024-07-02 17:04_

---

_Closed by @konstin on 2024-07-02 17:04_

---

_Branch deleted on 2024-07-02 17:04_

---
