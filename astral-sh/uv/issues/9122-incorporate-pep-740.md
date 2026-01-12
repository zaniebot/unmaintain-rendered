```yaml
number: 9122
title: Incorporate PEP 740
type: issue
state: open
author: alex
labels:
  - enhancement
assignees: []
created_at: 2024-11-14T16:30:50Z
updated_at: 2025-07-29T22:29:07Z
url: https://github.com/astral-sh/uv/issues/9122
synced_at: 2026-01-12T15:59:42Z
```

# Incorporate PEP 740

---

_@alex_

These are now available on PyPI: https://blog.pypi.org/posts/2024-11-14-pypi-now-supports-digital-attestations/

Suggestions for ways `uv` could incorporate these:

- When adding a package to `uv.lock`, include the identity of the publisher in the CLI output
- When upgrading a package in `uv.lock`, display (as a warning or error) the identity if it changes, or if it goes from identity->no identity
- Some sort of configuration to allow pinning packages to a given identity (and erroring if they don't come from that)

---

_Comment by @Ravencentric on 2024-11-15 12:45_

[gh-action-pypi-publish](https://github.com/pypa/gh-action-pypi-publish/releases/tag/v1.11.0) now produces and publishes PEP 740 digital attestations to PyPI by default. Would be nice if `uv publish` could do the same so that it can be a full replacement for the action.

---

_Label `enhancement` added by @charliermarsh on 2024-11-16 03:11_

---

_Comment by @kyungjunleeme on 2025-05-06 10:26_

Dear maintainers, @alex @charliermarsh @Ravencentric @zzztimbo @mitsuhiko 

If it would be acceptable, I’d be very grateful for the opportunity to work on this issue.
I’m interested in contributing and would like to take this one on, if no one else is currently assigned.

Please let me know if that would be alright.

Kind regards,
Kyungjun Lee

---

_Comment by @Ravencentric on 2025-05-06 12:05_

I'm not a maintainer of `uv`, I'm just another user.

---

_Comment by @zanieb on 2025-05-06 16:39_

@kyungjunleeme please don't bulk ping people like that, most of those aren't even maintainers of the project. Feel free to open a pull request, and we can take it from there. However, since attestations are security sensitive, note we may take longer to review the pull request or end up implementing it ourseves.

---

_Comment by @kyungjunleeme on 2025-05-07 01:28_

- Ravencentric  // Sorry for mentioning ur name! ㅜㅜ
=============================================
- zanieb 
Thank you for the feedback — I understand. I apologize for the bulk mention and will be more mindful moving forward. I’ll go ahead and open a pull request as suggested.
Totally understand that this may take time to review due to the security sensitivity.

---

_Comment by @dimaqq on 2025-06-27 08:16_

Go for it, @kyungjunleeme 

---

_Comment by @kyungjunleeme on 2025-06-27 08:22_

@dimaqq ok I will take it!

---

_Comment by @matmair on 2025-07-29 22:29_

Running attestation/provenance checks to link with the PEP740 data before install would be very interesting security wise. Typo / Slop-squatting is probably only getting a bigger problem with all those agents writing code now. Being able to limit to a list of approved publishers in uv for example would make the mandatory use of private mirrors in enterprises unnecessary and the tool stack much lighter.

It seems like the proposal to include a similar mechanism in pip is somewhat dormant, sadly.

---
