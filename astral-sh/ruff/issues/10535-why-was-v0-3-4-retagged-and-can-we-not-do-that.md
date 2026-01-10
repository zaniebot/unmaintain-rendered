```yaml
number: 10535
title: Why was v0.3.4 retagged and can we not do that again please?
type: issue
state: closed
author: alerque
labels: []
assignees: []
created_at: 2024-03-23T13:53:54Z
updated_at: 2024-03-23T21:30:59Z
url: https://github.com/astral-sh/ruff/issues/10535
synced_at: 2026-01-10T11:09:52Z
```

# Why was v0.3.4 retagged and can we not do that again please?

---

_Issue opened by @alerque on 2024-03-23 13:53_

The v0.3.4 release was tagged at least 2 days ago. I packaged and released it for Arch Linux on the 21st. Now there is a [new tag/release](https://github.com/astral-sh/ruff/releases/tag/v0.3.4) using the same version identifier and the old tag point is gone.

This is a big no-no. If something went wrong with the release cycle please increment the patch number before trying again. The tag was pushed and released already. Now downstream maintainers have to figure out what happened, why, whether it was malicious (something checksum changes trigger a need to research), and figure out what to do next.

---

_Comment by @alerque on 2024-03-23 13:58_

Hmmm, this might be more of a bug in the GitHub user interface than anything else. It is showing me the date of the tag as the date of the *release* (that got added per #10534), but the GH release doesn't matter, just the tag. I'm still looking to see if the tag actually changed.

---

_Comment by @alerque on 2024-03-23 14:01_

Yes, sorry for the noise. This is a GitHub UI issue. The release is on 5062572aca9413670aafd018cb65037bcb4d6acb which is the same commit I already packaged, so just the UI is dating things wrong.

---

_Closed by @alerque on 2024-03-23 14:01_

---

_Comment by @charliermarsh on 2024-03-23 21:30_

Yeah, I just published the release (which was in draft) two days after it was created. I forgot to promote it from draft at the time, but the tag should be unchanged.

---
