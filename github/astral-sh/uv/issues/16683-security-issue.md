---
number: 16683
title: Security Issue
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2025-11-11T04:39:18Z
updated_at: 2025-11-11T17:05:32Z
url: https://github.com/astral-sh/uv/issues/16683
synced_at: 2026-01-07T13:12:19-06:00
---

# Security Issue

---

_Issue opened by @ghost on 2025-11-11 04:39_

Found GITHUB_PAT in UV git history after conducting analysis on files:

remote:       —— GitHub Personal Access Token ——————————————————————        
remote:        locations:        
remote:          - blob id: 01fecac90209cbf4fdb1222e5ae092d8b4ff478a        
remote:          - blob id: 055d84f36300567c9239428e701263032e816b3f        
remote:          - blob id: 00d025a450b6ad4cde12a6108105173cbe16fe7c        
remote:          - blob id: 039261b24aebdee375e71b0770ae983f114934fc        
remote:          - blob id: 00a233c5f03af4bb53fee0ae450ced2bc7b4006d   

The specific GitHub Commit in the history that these may be discovered is located at SHA: c2eb32164d0b5ddd4a1bb0c495bb34ac726134bf (Note: this is simply the date of all the files, the PAT was not uploaded in this commit) (However, the files during this time have the PAT)

To locate GITHUB_PAT I suggest running (run this on all the BLOB ID's from the time of the commit I mentioned)

git log --all -m --find-object=<BLOB_ID>

---

_Renamed from "Security Issue" to "Security Issue GitHub PAT" by @ghost on 2025-11-11 04:39_

---

_Renamed from "Security Issue GitHub PAT" to "Security Issue" by @ghost on 2025-11-11 04:39_

---

_Comment by @ghost on 2025-11-11 04:46_

Renamed with reduced information for security reasons

---

_Comment by @ghost on 2025-11-11 05:11_

My email client started working, for security reasons I am closing this issue and contacting: security@astral.sh

---

_Closed by @ghost on 2025-11-11 05:11_

---

_Locked by @woodruffw on 2025-11-11 12:55_

---

_Comment by @zanieb on 2025-11-11 17:05_

(This PAT is intentionally committed for integration testing and only has read access to a dedicated test organization)

---
