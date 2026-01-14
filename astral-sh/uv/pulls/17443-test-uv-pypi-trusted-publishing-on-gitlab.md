```yaml
number: 17443
title: Test uv+PyPI Trusted Publishing on Gitlab
type: pull_request
state: open
author: woodruffw
labels:
  - internal
assignees: []
base: main
head: ww/pypi-tp-gl-test
created_at: 2026-01-13T19:14:18Z
updated_at: 2026-01-14T13:17:19Z
url: https://github.com/astral-sh/uv/pull/17443
synced_at: 2026-01-14T13:42:35Z
```

# Test uv+PyPI Trusted Publishing on Gitlab

---

_@woodruffw_

## Summary

~~WIP.~~

The basic idea here is to invoke a Gitlab Pipeline via GitHub. That pipeline generates an OIDC token which it saves as an artifact, which the GitHub workflow can then read. We then use that OIDC token to impersonate the identity of the Gitlab Pipeline for Trusted Publishing purposes.

Guinea pig project: https://test.pypi.org/project/astral-test-pypi-trusted-publishing-gitlab/

See #17438 for motivating context.

## Test Plan

Implement the above within `test_publish.py` and `ci.yml`.

---

_Assigned to @woodruffw by @woodruffw on 2026-01-13 19:14_

---

_@woodruffw reviewed on 2026-01-13 22:11_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:1 on 2026-01-13 22:11_

Apologies for the churn in this file -- I realized that I was having trouble tracking the state through the different sub-tests, so what I've done is add a "plan" abstraction that pre-plans some test state. We can then pass that state around consistently via a `Plan` object rather than sharing it piecemeal.

---

_@woodruffw reviewed on 2026-01-13 22:11_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:244 on 2026-01-13 22:11_

This is the main operative change 🙂 

---

_@woodruffw reviewed on 2026-01-13 22:15_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:562 on 2026-01-13 22:15_

Flagging: the nuance here is that GitLab, unlike GitHub and other TP providers, doesn't have an "active" OIDC source: the runner gets a single OIDC credential at startup. As a result of that PyPI (and pyx) consider the OIDC credential "spent" after its first use, meaning that our duplicate/conflict/etc. tests all fail for an unrelated reason (the index is rejecting our credential as reused).

There's no great way around this, it's seemingly an architectural limitation of GitLab. But the good news is that it won't affect typical user workflows, it only dings us here because we're intentionally re-using an OIDC credential across separate `uv publish` invocations 🙂 

---

_@woodruffw reviewed on 2026-01-13 22:16_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:562 on 2026-01-13 22:16_

(This is true for the other tests below as well.)

---

_@woodruffw reviewed on 2026-01-13 22:18_

---

_Review comment by @woodruffw on `.github/workflows/ci.yml`:259 on 2026-01-13 22:18_

Flagging: this adds ~30s of sync runtime to the publishing tests, since this action needs to block while the Gitlab pipeline completes. That's not ideal, but the silver lining is that it'll be amortized as we add more Gitlab publishing tests (e.g. for pyx too), since we can use the same pipeline invocation to generate multiple ID tokens.

---

_Marked ready for review by @woodruffw on 2026-01-13 22:19_

---

_Review requested from @zanieb by @woodruffw on 2026-01-13 22:19_

---

_Review requested from @konstin by @woodruffw on 2026-01-13 22:19_

---

_Review requested from @zsol by @woodruffw on 2026-01-13 22:19_

---

_Label `internal` added by @woodruffw on 2026-01-13 23:18_

---

_Review comment by @woodruffw on `.github/workflows/ci.yml`:259 on 2026-01-13 23:22_

(Also noting that I reviewed this action's source. I considered trying to open-code this with GitLab's `glab` CLI instead, but this seemed simpler. We could always switch to that instead, though.)

---

_@woodruffw reviewed on 2026-01-13 23:22_

---

_@zsol reviewed on 2026-01-14 08:20_

---

_Review comment by @zsol on `scripts/publish/test_publish.py`:562 on 2026-01-14 08:20_

Would this mean that two subsequent uv publish commands in the same job in a user's workflow fail the same way? If so, that's quite a severe limitation 

---

_@zsol approved on 2026-01-14 08:24_

This looks ok to me, but maybe it would be _slightly_ better if we would have a separate, parallel job for the non-github trusted publishing tests, so if they do fail at the point of getting the oidc token the rest of the tests would still run 

---

_@woodruffw reviewed on 2026-01-14 12:52_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:562 on 2026-01-14 12:52_

Yep, exactly. There's ways we could potentially work around that within uv itself (e.g. we could stash the minted credential for reuse between invocations), but the limitation on OIDC token reuse is architectural/inside PyPI.

In practice this doesn't generally bite users because all they do is a single `uv publish` invocation, plus must CI providers don't have this limitation (GitHub and all the others can acquire additional OIDC creds at runtime.)

---

_Comment by @woodruffw on 2026-01-14 13:17_

> so if they do fail at the point of getting the oidc token the rest of the tests would still run

Yeah, I was thinking about either sharding this across jobs with a matrix *or* changing it to use a more "traditional" pytest setup, so that we could run the entire suite without failing fast. I can look at that with a follow up.

---
