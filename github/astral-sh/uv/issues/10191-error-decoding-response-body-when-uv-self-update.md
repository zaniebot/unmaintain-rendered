---
number: 10191
title: "\"error decoding response body\" when \"uv self update\" (uv version: 0.5.12)"
type: issue
state: closed
author: AxelThePop
labels:
  - bug
assignees: []
created_at: 2024-12-27T10:03:51Z
updated_at: 2024-12-27T14:03:35Z
url: https://github.com/astral-sh/uv/issues/10191
synced_at: 2026-01-07T13:12:18-06:00
---

# "error decoding response body" when "uv self update" (uv version: 0.5.12)

---

_Issue opened by @AxelThePop on 2024-12-27 10:03_

I've updated to the "0.5.12 (351d602d8 2024-12-26)" with "uv self update". If I hit "uv self update" now, I get:
"error: error decoding response body
	Caused by: there are extra bytes after body has been decompressed"

Windows 10 with all the latest updates, Powershell 7.4.6

---

_Renamed from ""error decoding response body" When "uv self update"" to ""error decoding response body" when "uv self update" (uv version: 0.5.12)" by @AxelThePop on 2024-12-27 10:21_

---

_Comment by @monk-time on 2024-12-27 13:02_

This seems pretty serious. If v0.5.12 [broke all network requests](https://github.com/astral-sh/uv/issues/10186) and it can no longer self update to v0.5.13 that contains a fix for that, how can all v0.5.12 users upgrade to it? ISTM v0.5.12 should be recalled as soon as possible and instructions for alternative updating route should be made highly visible. 

Also it seems strange how testing didn't catch a breakdown of such core functionality as networking.

---

_Comment by @charliermarsh on 2024-12-27 13:36_

Sorry -- this is a bug in `reqwests` that I fixed last night (#10187), but didn't realize that the subsequent release failed. I'll get it out now.

---

_Comment by @charliermarsh on 2024-12-27 13:40_

The reason it wasn't caught in testing is that it's specific to specific registries. All the reports in https://github.com/astral-sh/uv/issues/10190 are from custom registries (like CodeArtifact, Gitlab, etc.).

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-27 13:48_

---

_Label `bug` added by @charliermarsh on 2024-12-27 13:48_

---

_Comment by @charliermarsh on 2024-12-27 14:02_

This should be fixed in v0.5.13.

If you're on v0.5.12, you may need to reinstall with cURL rather than via `uv self update`:

```shell
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Sorry for the inconvenience! It's my mistake for going to bed without double-checking the release pipeline.

---

_Closed by @charliermarsh on 2024-12-27 14:02_

---

_Comment by @AxelThePop on 2024-12-27 14:03_

Well done!
success: Upgraded uv from v0.5.12 to v0.5.13! https://github.com/astral-sh/uv/releases/tag/0.5.13
success: You're on the latest version of uv (v0.5.13)

Thanks.

---

_Referenced in [astral-sh/uv#10196](../../astral-sh/uv/issues/10196.md) on 2024-12-27 16:35_

---
