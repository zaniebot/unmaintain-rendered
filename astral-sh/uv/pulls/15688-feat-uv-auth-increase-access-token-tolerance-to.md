```yaml
number: 15688
title: "feat(uv-auth): increase access token tolerance to 30 minutes"
type: pull_request
state: closed
author: woodruffw
labels:
  - enhancement
assignees: []
base: main
head: ww/increase-access-token-tolerance
created_at: 2025-09-04T18:18:18Z
updated_at: 2025-09-04T19:00:47Z
url: https://github.com/astral-sh/uv/pull/15688
synced_at: 2026-01-10T06:44:33Z
```

# feat(uv-auth): increase access token tolerance to 30 minutes

---

_Pull request opened by @woodruffw on 2025-09-04 18:18_

## Summary

pyx access tokens are valid for an hour at a time, so our current default tolerance of 5m means that we pessimistically refresh the access token up to 12 times per hour. 

This increases to tolerance to 30m, meaning that we'll now pessimistically refresh only twice an hour. This has two main benefits:

1. End users should see slightly faster average requests, since they'll do implicit token refreshes less often;
2. The pyx backend sees less chaff volume from token refreshes (these aren't a load issue at the moment, but in the distant future they could be at current tolerances).

## Test Plan

Backend testing in pyx only, most likely? I'm not sure there's a great way to test this (ATM) in uv itself.

---

_Review requested from @charliermarsh by @woodruffw on 2025-09-04 18:18_

---

_Review requested from @zanieb by @woodruffw on 2025-09-04 18:18_

---

_Assigned to @woodruffw by @woodruffw on 2025-09-04 18:18_

---

_Label `enhancement` added by @woodruffw on 2025-09-04 18:18_

---

_@zanieb approved on 2025-09-04 18:30_

---

_@zanieb requested changes on 2025-09-04 18:44_

I don't actually see how this would have the desired effect. It seems like it would do the opposite.

---

_Comment by @woodruffw on 2025-09-04 18:59_

> I don't actually see how this would have the desired effect. It seems like it would do the opposite.

Yeah, turns out I completely misread the tolerance condition -- I read it backwards as "time _from_ issuance", not "time _from_ expiry" 🤦 

---

_Comment by @woodruffw on 2025-09-04 19:00_

I'm gonna go ahead and close this -- I don't think it makes sense in any current iteration. 5m tolerance strikes me as reasonable in terms of the actual behavior here.

---

_Closed by @woodruffw on 2025-09-04 19:00_

---

_Branch deleted on 2025-09-04 19:00_

---
