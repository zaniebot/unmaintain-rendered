---
number: 13226
title: Implement HTTP Digest authentication
type: issue
state: open
author: eolo999
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-04-30T11:50:41Z
updated_at: 2025-04-30T12:59:24Z
url: https://github.com/astral-sh/uv/issues/13226
synced_at: 2026-01-07T13:12:18-06:00
---

# Implement HTTP Digest authentication

---

_Issue opened by @eolo999 on 2025-04-30 11:50_

### Summary

It would be useful to be able to use the HTTP Digest method to authenticate to private indexes.

### Example

_No response_

---

_Label `enhancement` added by @eolo999 on 2025-04-30 11:50_

---

_Comment by @konstin on 2025-04-30 12:20_

Which private index is using HTTP digest, and why does regular auth not work for you?

---

_Comment by @eolo999 on 2025-04-30 12:42_

It's an internal index I am bringing up for my company. We are using HTTP Digest authentication already for other stuff and a lot of user passwords are already stored in permissions files. Making the index use Basic Auth would require quite a lot of work to create permissions files.
I also think that, given Digest authentication it's a standard method as defined [here](https://www.rfc-editor.org/rfc/rfc2617) it would be a good addition to the project despite my use case. thanks.

---

_Label `wish` added by @konstin on 2025-04-30 12:59_

---
