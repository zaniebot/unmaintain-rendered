```yaml
number: 15820
title: "Revert \"feat(ci): build loongarch64 binaries in CI (#15387)\""
type: pull_request
state: merged
author: woodruffw
labels: []
assignees: []
merged: true
base: main
head: ww/revert-loong64
created_at: 2025-09-12T21:01:06Z
updated_at: 2025-09-16T16:48:56Z
url: https://github.com/astral-sh/uv/pull/15820
synced_at: 2026-01-12T16:11:57Z
```

# Revert "feat(ci): build loongarch64 binaries in CI (#15387)"

---

_@woodruffw_

This reverts commit 2fd9e53b258e5cbf3ce683c613ae228b7f66bcec.

## Summary

TL;DR: This build currently relies on an unofficial upstream Debian image build, which means that it doesn't provide the stability or provenance guarantees that we'd like to accompany first-party builds of uv.

## Test Plan

See what happens in CI.

---

_Review requested from @zanieb by @woodruffw on 2025-09-12 21:01_

---

_Comment by @woodruffw on 2025-09-12 21:05_

Integration test on GCP appears to be an unrelated flake.

---

_@zanieb approved on 2025-09-12 21:07_

cc @SkyBird233 

Unfortunately, we don't feel comfortable using the community built trixie image here. We spent some time auditing it today and decided need stronger guarantees around the provenance of a build platform for uv.

---

_Merged by @woodruffw on 2025-09-12 21:21_

---

_Closed by @woodruffw on 2025-09-12 21:21_

---

_Branch deleted on 2025-09-12 21:21_

---

_Comment by @SkyBird233 on 2025-09-16 09:32_

Hi @zanieb, the lack of support of LoongArch with Docker is one of the reasons why I haven't added support for `build-docker.yml`. But if I didn't get it wrong, the Debian Trixie image is just for testing the wheel and running a `--help` command in a LoongArch environment, which is not that related to the building process. Also, it seems that Docker is not going to add LoongArch support (and build official images) in the foreseeable future (docker-library/official-images#16404), so I think that image is suitable here.
If that's still not enough, should we move another image or does it mean thar we can only wait for an official one?

---

_Comment by @woodruffw on 2025-09-16 14:28_

> If that's still not enough, should we move another image or does it mean thar we can only wait for an official one?

I think we'd need to use an official image here -- the lack of an official image means that we have trouble establishing provenance for the various build changes needed to produce the image here. Similarly, without an official image we're concerned about ABI and other environmental stability.

(Your point about the image just being used for testing is fair, but unfortunately we need to apply a high standard for *anything* that enters our CI, since being trusted to run on every PR means a significant commitment to both stability and security on our end.)


---

_Comment by @zanieb on 2025-09-16 14:48_

It's plausible we could just drop testing of the loongarch artifacts.

---

_Comment by @SkyBird233 on 2025-09-16 16:47_

Thanks for your detailed reply. If it's accaptable to skip tests for LoongArch for now, should I update my changes and re-submit the pull requests?

---
