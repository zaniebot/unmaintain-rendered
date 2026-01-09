---
number: 15618
title: "uv publish: create attestations"
type: issue
state: open
author: alex
labels:
  - enhancement
assignees: []
created_at: 2025-09-01T11:31:13Z
updated_at: 2025-11-12T22:58:38Z
url: https://github.com/astral-sh/uv/issues/15618
synced_at: 2026-01-07T13:12:19-06:00
---

# uv publish: create attestations

---

_Issue opened by @alex on 2025-09-01 11:31_

### Summary

It'd be useful if `uv publish` was able to create attestations (PEP 740) when uploading to PyPI.

We currently create them with https://github.com/pypa/gh-action-pypi-publish/blob/unstable/v1/attestations.py (as part of their action), but it'd be nice to let `uv` handle them.

### Example

_No response_

---

_Label `enhancement` added by @alex on 2025-09-01 11:31_

---

_Referenced in [pyca/cryptography#11548](../../pyca/cryptography/issues/11548.md) on 2025-09-01 11:35_

---

_Comment by @zanieb on 2025-09-01 14:34_

See also https://github.com/astral-sh/uv/issues/9122

---

_Comment by @alex on 2025-09-01 14:36_

Indeed (also filed by me :-)), despite the open ended name that one focuses
on the install side, rather than upload

All that is necessary for evil to succeed is for good people to do nothing.

On Mon, Sep 1, 2025, 10:34 AM Zanie Blue ***@***.***> wrote:

> *zanieb* left a comment (astral-sh/uv#15618)
> <https://github.com/astral-sh/uv/issues/15618#issuecomment-3242596413>
>
> See also #9122 <https://github.com/astral-sh/uv/issues/9122>
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/15618#issuecomment-3242596413>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAAAGBACWQANIXUM42VNBH33QRKP3AVCNFSM6AAAAACFKFQMN2VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTENBSGU4TMNBRGM>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @zanieb on 2025-09-01 14:40_

We intend to support this, it's just not trivial and we want to do something security-sensitive properly...

---

_Comment by @konstin on 2025-09-01 14:43_

CC @woodruffw who has the most knowledge here

---

_Comment by @woodruffw on 2025-09-01 20:29_

Thanks for the ping!

Yeah, this is something we should support directly within uv (and with `uv publish` directly), but it's unfortunately nontrivial in its "ideal" form:

1. We'll need a stable-enough Rust implementation of Sigstore;
2. We'll need a Rust equivalent of the Python `pypi-attestations` package;
3. We'll need the equivalent of twine's `--attestations` flag.

(1) in particular is pretty heavy, since Sigstore is a somewhat hefty spec that builds on top of equally hefty specs (RFC 5280, RFC 6962). 

A stop-gap solution that might be acceptable in the mean time:

* Tell users to invoke `actions/attest` or `sigstore/gh-action-sigstore-python` in their CI/CD
* Write an "adaptor" action that takes the outputs from above and turns them into the appropriate PEP 740 shape
* Implement `--attestations` per (3) above

That would make `uv publish` feature-consistent with `twine`, but would push the bulk of the actual Sigstore/PEP 740 implementation outside of uv itself (similar to how `twine` keeps it outside and relies on `gh-action-pypi-publish` to do it).

---

_Comment by @alex on 2025-09-01 20:38_

FWIW, from my perspective, if there were a documented and clearly supported way to externally attest things and then upload with `uv`, that'd be a great start. Not as good as all in one, but a start.

---

_Assigned to @woodruffw by @woodruffw on 2025-11-12 22:56_

---

_Comment by @woodruffw on 2025-11-12 22:58_

> FWIW, from my perspective, if there were a documented and clearly supported way to externally attest things and then upload with `uv`, that'd be a great start. Not as good as all in one, but a start.

Yeah, for this I'm thinking that `uv publish` should probably support `--attestations`, similarly to how `twine upload` does. I'll be poking at that.

---

_Referenced in [astral-sh/uv#16731](../../astral-sh/uv/pulls/16731.md) on 2025-11-13 21:34_

---
