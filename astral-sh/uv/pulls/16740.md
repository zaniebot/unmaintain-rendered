```yaml
number: 16740
title: Add an integration test for publishing to pyx
type: pull_request
state: merged
author: woodruffw
labels:
  - testing
assignees: []
merged: true
base: main
head: ww/test-publish-pyx
created_at: 2025-11-14T17:10:10Z
updated_at: 2025-11-18T17:13:58Z
url: https://github.com/astral-sh/uv/pull/16740
synced_at: 2026-01-10T05:58:11Z
```

# Add an integration test for publishing to pyx

---

_Pull request opened by @woodruffw on 2025-11-14 17:10_

## Summary

This adds an integration test for publishing to pyx (the staging instance) using an API token.

Separately I need to figure out a good testing strategy for pyx with Trusted Publishing, although that may be blocked on an approach that doesn't involve the current temporary `pyx-auth-action` shim.

## Test Plan

See what happens.

---

_@woodruffw reviewed on 2025-11-14 18:36_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:163 on 2025-11-14 18:36_

Flagging: I've disabled this since it appears to be unreliable, and has been unreliable for several days (based on the CI on main).

Maybe it makes sense to have a "soft fail" knob for these kinds of flaky indices though? Not sure.

---

_@woodruffw reviewed on 2025-11-14 18:37_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:391 on 2025-11-14 18:37_

This feels like a pretty bad hack: this integration script has its own HTTP client that it uses to pull recent version numbers, so we need to give that client read access to pyx. Maybe there's a way we could do this all within uv itself, i.e. have uv provide the list of current versions?

---

_@woodruffw reviewed on 2025-11-14 19:44_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py.lock`:1 on 2025-11-14 19:44_

Noting: I bumped the deps here because the pinned version of httpcore didn't work on 3.14.

---

_@woodruffw reviewed on 2025-11-14 19:45_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:354 on 2025-11-14 19:45_

Noting: I added `env` to `wait_for_index` since we need to propagate the pyx API token in order for `uv pip compile`'s index accesses to succeed.

---

_@woodruffw reviewed on 2025-11-17 15:35_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:254 on 2025-11-17 15:35_

Note: this doesn't affect any of our current tested indices, but PEP 503 doesn't require a direct 200 response -- the index is allowed to redirect.

---

_Marked ready for review by @woodruffw on 2025-11-17 16:32_

---

_Review requested from @konstin by @woodruffw on 2025-11-17 16:32_

---

_Review comment by @woodruffw on `.github/workflows/ci.yml`:1994 on 2025-11-17 16:32_

Noting: I've added this secret to the integration test's environment.

---

_@woodruffw reviewed on 2025-11-17 16:32_

---

_@woodruffw reviewed on 2025-11-17 16:33_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:249 on 2025-11-17 16:33_

I found this useful and it doesn't add much to the log volume (since we already emit a lot), but I'm happy to remove if it's too noisy for others 🙂 

---

_Assigned to @woodruffw by @woodruffw on 2025-11-17 16:42_

---

_Label `testing` added by @woodruffw on 2025-11-17 16:42_

---

_Review comment by @konstin on `scripts/publish/test_publish.py`:163 on 2025-11-18 10:52_

Can you instead remove it from the default index set? Then we can still run it manually, but it doesn't run on CI anymore.

---

_Review comment by @konstin on `scripts/publish/test_publish.py`:249 on 2025-11-18 10:53_

Can you sort them an limit to the latest X versions?

---

_@konstin reviewed on 2025-11-18 10:56_

---

_@woodruffw reviewed on 2025-11-18 16:22_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:249 on 2025-11-18 16:22_

I went ahead and removed it -- it would be easy to re-add if someone needs for debugging in the future 🙂 

---

_@konstin approved on 2025-11-18 17:08_

---

_Merged by @woodruffw on 2025-11-18 17:13_

---

_Closed by @woodruffw on 2025-11-18 17:13_

---

_Branch deleted on 2025-11-18 17:13_

---
