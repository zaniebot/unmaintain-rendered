```yaml
number: 12969
title: Obfuscate username in tracing URL
type: pull_request
state: merged
author: jtfmumm
labels:
  - security
assignees: []
merged: true
base: main
head: jtfm/obfuscate-username
created_at: 2025-04-18T14:06:58Z
updated_at: 2025-04-19T09:11:43Z
url: https://github.com/astral-sh/uv/pull/12969
synced_at: 2026-01-12T16:10:29Z
```

# Obfuscate username in tracing URL

---

_@jtfmumm_

A URL username can be a secret token, so we should avoid logging it.

---

_Label `security` added by @jtfmumm on 2025-04-18 14:06_

---

_Comment by @zanieb on 2025-04-18 16:08_

I think we might need a way to recover this, because it can be important for debugging to see which username is being used (this is frequently relevant since the username is used downstream, e.g., in keyring fetches).

Separately, should we only obfuscate the username if we don't have a password? That's the only case where it can be a token, right?

---

_Comment by @jtfmumm on 2025-04-18 17:39_

> I think we might need a way to recover this, because it can be important for debugging to see which username is being used (this is frequently relevant since the username is used downstream, e.g., in keyring fetches).

> Separately, should we only obfuscate the username if we don't have a password? That's the only case where it can be a token, right?

It's possible but probably pretty uncommon for people to accidentally switch the two. For now, I could update it to only obfuscate the username when it's on its own (since it's still more secure than `main`).

---

_Comment by @jtfmumm on 2025-04-18 18:07_

I've updated to only mask the username when it's on its own and added a test for the various scenarios.

---

_@zanieb reviewed on 2025-04-18 20:54_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:521 on 2025-04-18 20:54_

A comment would probably be helpful here, re. tokens

---

_@zanieb approved on 2025-04-18 20:54_

---

_Merged by @jtfmumm on 2025-04-19 09:11_

---

_Closed by @jtfmumm on 2025-04-19 09:11_

---

_Branch deleted on 2025-04-19 09:11_

---
