```yaml
number: 11140
title: Docs on how to verify uv docker image attestations
type: pull_request
state: merged
author: mjpieters
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc-docker-attestations
created_at: 2025-01-31T19:09:23Z
updated_at: 2025-02-04T23:01:56Z
url: https://github.com/astral-sh/uv/pull/11140
synced_at: 2026-01-12T16:09:42Z
```

# Docs on how to verify uv docker image attestations

---

_@mjpieters_

As [requested by @zanieb](https://github.com/astral-sh/uv/pull/8685#issuecomment-2627556992).

---

_@mjpieters reviewed on 2025-01-31 19:19_

---

_Review comment by @mjpieters on `docs/guides/integration/docker.md`:524 on 2025-01-31 19:19_

The specific image digest and workflow commit hashes are those for the latest release, which hasn't been signed, of course. This means that the exact example shown here will actually _not_ succeed.

If a new release is pushed before this PR merges, I'll update these values with some that will actually check out.

---

_Assigned to @zanieb by @zanieb on 2025-01-31 19:50_

---

_Review comment by @samypr100 on `docs/guides/integration/docker.md`:524 on 2025-01-31 19:54_

Maybe worth omitting some of the surrounding information and just leave `✓ Verification succeeded!` 

---

_@samypr100 reviewed on 2025-01-31 19:54_

---

_@mjpieters reviewed on 2025-01-31 20:01_

---

_Review comment by @mjpieters on `docs/guides/integration/docker.md`:524 on 2025-01-31 20:01_

I rather not as I think it is important to highlight what kind of claims the command verifies.

---

_@samypr100 reviewed on 2025-01-31 20:02_

---

_Review comment by @samypr100 on `docs/guides/integration/docker.md`:524 on 2025-01-31 20:02_

Fair point, I said so from the perspective that these hashes will likely not be kept up to date 😅 when using :latest: tag.

---

_@mjpieters reviewed on 2025-01-31 21:56_

---

_Review comment by @mjpieters on `docs/guides/integration/docker.md`:524 on 2025-01-31 21:56_

It's meant to be an example. I'll update it once to have it work for that release. But I don't think it needs to be up-to-date for every future release to be effective.

---

_@samypr100 reviewed on 2025-01-31 22:50_

---

_Review comment by @samypr100 on `docs/guides/integration/docker.md`:524 on 2025-01-31 22:50_

Sounds good, thanks for clarifying :) 

---

_Comment by @zanieb on 2025-02-03 23:35_

https://github.com/astral-sh/uv/releases/tag/0.5.27 should have the attestations

---

_Comment by @mjpieters on 2025-02-04 15:58_

Indeed, here they all are: https://github.com/astral-sh/uv/attestations

and they check out too:

```console
% gh attestation verify --owner astral-sh oci://ghcr.io/astral-sh/uv:0.5.27
Loaded digest sha256:5adf09a5a526f380237408032a9308000d14d5947eafa687ad6c6a2476787b4f for oci://ghcr.io/astral-sh/uv:0.5.27
Loaded 1 attestation from GitHub API

The following policy criteria will be enforced:
- OIDC Issuer must match:................... https://token.actions.githubusercontent.com
- Source Repository Owner URI must match:... https://github.com/astral-sh
- Predicate type must match:................ https://slsa.dev/provenance/v1
- Subject Alternative Name must match regex: (?i)^https://github.com/astral-sh/

✓ Verification succeeded!

sha256:5adf09a5a526f380237408032a9308000d14d5947eafa687ad6c6a2476787b4f was attested by:
REPO          PREDICATE_TYPE                  WORKFLOW
astral-sh/uv  https://slsa.dev/provenance/v1  .github/workflows/build-docker.yml@refs/heads/main
```

I'll update the examples now.

---

_Review requested from @samypr100 by @mjpieters on 2025-02-04 16:20_

---

_Comment by @mjpieters on 2025-02-04 16:21_

As far as I am concerned I think this is ready to merge now.

---

_@zanieb approved on 2025-02-04 21:38_

---

_Merged by @zanieb on 2025-02-04 21:38_

---

_Closed by @zanieb on 2025-02-04 21:38_

---

_Label `documentation` added by @zanieb on 2025-02-04 21:38_

---

_Comment by @zanieb on 2025-02-04 21:38_

Thanks again!

---

_Branch deleted on 2025-02-04 23:01_

---
