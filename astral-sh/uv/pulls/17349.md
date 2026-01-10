```yaml
number: 17349
title: Enable uploads via pre-signed URLs
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/direct-s3
created_at: 2026-01-07T18:24:13Z
updated_at: 2026-01-09T14:39:24Z
url: https://github.com/astral-sh/uv/pull/17349
synced_at: 2026-01-10T05:49:14Z
```

# Enable uploads via pre-signed URLs

---

_Pull request opened by @charliermarsh on 2026-01-07 18:24_

## Summary

For pyx, we can allow uploads that bypass the registry and send the file directly to S3. This is an opt-in feature, enabled via the `--direct` flag.


---

_Label `preview` added by @charliermarsh on 2026-01-07 18:24_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:7596 on 2026-01-07 18:26_

The latter part of this seems wrong.

---

_@zanieb reviewed on 2026-01-07 18:26_

---

_@charliermarsh reviewed on 2026-01-07 18:28_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:7596 on 2026-01-07 18:28_

How so?

---

_@charliermarsh reviewed on 2026-01-07 18:28_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:7596 on 2026-01-07 18:28_

Like it's not _required_?

---

_@zanieb reviewed on 2026-01-07 19:08_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:7596 on 2026-01-07 19:08_

Yeah, it's not required

---

_@zanieb reviewed on 2026-01-07 19:17_

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:774 on 2026-01-07 19:17_

Since this is a streaming request, does it need to have an outer retry wrapper?

---

_@zanieb approved on 2026-01-07 19:19_

This looks reasonable though @woodruffw or @konstin should be final reviewers.

---

_@konstin reviewed on 2026-01-08 12:48_

---

_Review comment by @konstin on `crates/uv/src/commands/publish.rs`:253 on 2026-01-08 12:48_

nit: we should rename this client

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:774 on 2026-01-08 12:50_

That's the client with the regular settings wrt to retries and timeouts, and with no auth middleware

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:740 on 2026-01-08 12:53_

Is that URL confidential, should we redact it in logs?

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:761 on 2026-01-08 12:55_

We need a retry loop here, streaming uploads can't be retried otherwise

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:697 on 2026-01-08 12:56_

That's the client without retries, I think that should be the regular client, plus/minus auth settings?

---

_@konstin reviewed on 2026-01-08 12:56_

---

_Review comment by @woodruffw on `crates/uv-cli/src/lib.rs`:7595 on 2026-01-08 15:16_

Seems reasonable to me, although noting that we could in principle do this without a flag by having uv send an `Accept: ...` header that indicates support, to which pyx would then respond with the pre-signed URL.

(That might be nice for enabling this implicit/by default in the future, but seems like not worth doing initially!)

---

_Review comment by @woodruffw on `crates/uv-publish/src/lib.rs`:712 on 2026-01-08 15:18_

Q, minor: should we use HTTP 409 (Conflict) for this, given that we're not tied to PyPI's response code semantics for our own upload scheme?

---

_@woodruffw approved on 2026-01-08 15:22_

LGTM, one note and one comment but neither is blocking IMO. I'll follow up with some comments on the pyx side too.

---

_@charliermarsh reviewed on 2026-01-08 15:26_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:712 on 2026-01-08 15:26_

Would 409 be for the case in which it already exists and isn't the same file? Or already exists and _is_ the same file? Here I'm using 200 for "already exists and is the same" (and 201 for "created, now proceed to upload").

---

_@woodruffw reviewed on 2026-01-08 15:38_

---

_Review comment by @woodruffw on `crates/uv-publish/src/lib.rs`:712 on 2026-01-08 15:38_

Oh, good point. 409 would be for "exists but with different content" I believe, 200 is semantically correct for "exists with the same content already."

---

_Marked ready for review by @charliermarsh on 2026-01-08 17:03_

---

_Comment by @codspeed-hq[bot] on 2026-01-08 18:03_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
### Merging this PR will **not alter performance**




### Summary

`✅ 5` untouched benchmarks  



---

<sub>Comparing <code>charlie/direct-s3</code> (fa48051) with <code>main</code> (25d691e)</sub>

<a href="https://codspeed.io/astral-sh/uv/branches/charlie%2Fdirect-s3?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>



---

_Merged by @charliermarsh on 2026-01-09 02:26_

---

_Closed by @charliermarsh on 2026-01-09 02:26_

---

_Branch deleted on 2026-01-09 02:26_

---

_@konstin reviewed on 2026-01-09 11:13_

---

_Review comment by @konstin on `crates/uv/src/commands/publish.rs`:146 on 2026-01-09 11:13_

For the OIDC client, we need retries (GitHub's networking is too unreliable) and ideally the default timeouts, the S3 is separate in that it uses its own retry loop, upload timeouts and no auth middleware.

---

_@charliermarsh reviewed on 2026-01-09 14:32_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/publish.rs`:146 on 2026-01-09 14:32_

Fixing.

---

_@charliermarsh reviewed on 2026-01-09 14:39_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/publish.rs`:146 on 2026-01-09 14:39_

https://github.com/astral-sh/uv/pull/17379

---
