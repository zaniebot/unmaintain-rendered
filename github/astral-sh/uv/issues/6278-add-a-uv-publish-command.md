---
number: 6278
title: "Add a `uv publish` command"
type: issue
state: closed
author: zanieb
labels:
  - projects
  - cli
assignees: []
created_at: 2024-08-20T21:54:23Z
updated_at: 2024-10-15T15:31:25Z
url: https://github.com/astral-sh/uv/issues/6278
synced_at: 2026-01-07T13:12:17-06:00
---

# Add a `uv publish` command

---

_Issue opened by @zanieb on 2024-08-20 21:54_

e.g., to publish the project to PyPI.

This is a pretty large, but well isolated project — if you've contributed previously and are interested in working on it let me know.

---

_Label `help wanted` added by @zanieb on 2024-08-20 21:54_

---

_Label `projects` added by @zanieb on 2024-08-20 21:54_

---

_Label `cli` added by @zanieb on 2024-08-20 21:54_

---

_Comment by @pplmx on 2024-08-21 07:13_

Add `uv build` and `uv test` too?

---

_Comment by @zanieb on 2024-08-22 00:20_

Those are both out of scope for this issue — though `uv build` should be trivial once `uv publish` is set up. See https://github.com/astral-sh/uv/issues/1510 for the relevant tracking issue there.

---

_Referenced in [FinanceData/FinanceDataReader#225](../../FinanceData/FinanceDataReader/issues/225.md) on 2024-08-23 08:46_

---

_Referenced in [astral-sh/uv#5507](../../astral-sh/uv/issues/5507.md) on 2024-08-25 21:26_

---

_Comment by @Zaf4 on 2024-08-25 22:41_

I was going to open the same issue I have been using poetry for publishing. Would be great to fully switch to uv for everything. 

---

_Comment by @woodruffw on 2024-08-26 17:28_

Hi all! I'm neither a maintainer of PyPI nor `twine`, but I'm an active contributor to both who has touched a lot of the components related to uploading/API tokens/etc. It's very exciting to see `uv` consider adding publishing support!

A couple of things that might be worth considering as part of an implementation here:

* PyPI and TestPyPI both support [trusted publishing](https://docs.pypi.org/trusted-publishers/), i.e. credentialless package publishing via OIDC. This is handled transparently at the API token issuance layer to avoid any changes to PyPI's authentication parameters, but a future `uv publish` command could avoid the need for a separate token exchange step by performing the token exchange internally.
* PyPI and TestPyPI are nearing the completion of initial support for [PEP 740](https://peps.python.org/pep-0740/), i.e. index support for digital attestations over packages. `twine` has support for detecting and uploading attestations; a future `uv publish` command could consider adding support as well 🙂 

I'm happy to provide more details on the above or anything else related to publishing, to those interested. Just let me know!

---

_Comment by @andrlik on 2024-08-28 13:37_

Adding this comment to the correct issue. 🤦 

Would be good to mimic the `rye publish` command where it prompts you to encrypt your PyPI token with a password.

---

_Referenced in [jorenham/mainpy#45](../../jorenham/mainpy/issues/45.md) on 2024-09-08 17:02_

---

_Assigned to @konstin by @konstin on 2024-09-13 15:33_

---

_Label `help wanted` removed by @konstin on 2024-09-13 15:34_

---

_Comment by @konstin on 2024-09-24 16:32_

`uv publish` is now implemented. It supports token authentication (for PyPI), username/password authentication, keyring integration and trusted publishing support for GitHub Actions + PyPI (publishing without manually configuration a secret in GitHub Actions). In the next release, it will ship in preview mode (pass `--preview` to use it), as we might still need to make minor adjustments to the CLI outside of our usual stability policy.

If you are running into any problems, please open an issue! Especially for authentication with private registries feedback would be invaluable.

---

_Closed by @konstin on 2024-09-24 16:32_

---

_Unassigned @konstin by @konstin on 2024-09-24 16:32_

---

_Comment by @juan-abia on 2024-10-15 15:31_

Related @konstin:
https://github.com/astral-sh/uv/issues/8221

---
