```yaml
number: 11357
title: Add uv release artifact attestations
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: release-attestations
created_at: 2025-02-09T18:10:57Z
updated_at: 2025-10-30T00:43:11Z
url: https://github.com/astral-sh/uv/pull/11357
synced_at: 2026-01-12T16:09:48Z
```

# Add uv release artifact attestations

---

_@samypr100_

## Summary

Similar to https://github.com/astral-sh/uv/pull/8685, this adds attestations for uv release artifacts.

The changes on this PR would add attestations for
* `dist-manifest.json`
* `uv-installer.ps1`
* `uv-installer.sh`
* All `*.tar.gz` and `*.zip` uv binary files

## Test Plan

~(clarifying note: I'm aware this file is managed cargo dist and this will not work without allow-dirty at this time)~

~Currently cargo dist targets generation in `build_local_artifacts` which is not used here, plus we'd ideally want to attest the GH downloads / artifacts.~ (edit: fixed by https://github.com/axodotdev/cargo-dist/pull/2000)

At a glance, this release workflow seems to work successfully:

e.g. Example Run: https://github.com/samypr100/uv/actions/runs/13229100555
e.g. Example Release: https://github.com/samypr100/uv/releases/tag/0.5.29

---

_Assigned to @zanieb by @zanieb on 2025-02-09 23:55_

---

_Comment by @zanieb on 2025-04-16 15:24_

cc @Gankra — seems low priority but want to make sure you're aware of this.

---

_Unassigned @zanieb by @zanieb on 2025-04-16 15:24_

---

_Assigned to @Gankra by @Gankra on 2025-04-16 20:36_

---

_Review requested from @Gankra by @Gankra on 2025-04-16 20:36_

---

_Comment by @Gankra on 2025-04-16 20:41_

idle first thought: we can "just" inline the attestation stuff into the build-binaries subscript, in the same way that it builds tarballs in the exact format cargo-dist "would" if it was running the tasks.

tedious but not the worst.

---

_Comment by @samypr100 on 2025-04-16 22:58_

I also left a proposal here from a pseudo working implementation I started locally, https://github.com/axodotdev/cargo-dist/issues/1754

Although not sure the best approach now with the fork scenario

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/release.yml`:18 on 2025-09-22 22:38_

overly broad permissions

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/172)

---

_@github-advanced-security[bot] reviewed on 2025-09-22 22:38_

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/release.yml`:20 on 2025-09-22 22:38_

overly broad permissions

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/174)

---

_Comment by @samypr100 on 2025-09-22 22:40_

Given we're on dist 0.30 now (which has https://github.com/axodotdev/cargo-dist/pull/2000), we can revive this

---

_Marked ready for review by @samypr100 on 2025-09-22 22:40_

---

_@samypr100 reviewed on 2025-09-22 22:42_

---

_Review comment by @samypr100 on `.github/workflows/release.yml`:18 on 2025-09-22 22:42_

dist's github-attestations implementation sets this, is this something we would want to address in dist?

---

_@samypr100 reviewed on 2025-09-22 22:42_

---

_Review comment by @samypr100 on `.github/workflows/release.yml`:20 on 2025-09-22 22:42_

dist's github-attestations implementation sets this, is this something we would want to address in dist?

---

_Comment by @samypr100 on 2025-10-08 16:31_

@Gankra this should be finally ready

---

_@zanieb reviewed on 2025-10-08 17:31_

---

_Review comment by @zanieb on `.github/workflows/release.yml`:18 on 2025-10-08 17:31_

@Gankra / @woodruffw 

---

_@woodruffw reviewed on 2025-10-08 18:40_

---

_Review comment by @woodruffw on `.github/workflows/release.yml`:18 on 2025-10-08 18:40_

Yeah, I think this should be addressed in dist -- these permissions would ideally be set on the job level, not the cross-job workflow level 🙂 

---

_@samypr100 reviewed on 2025-10-09 01:28_

---

_Review comment by @samypr100 on `.github/workflows/release.yml`:18 on 2025-10-09 01:28_

👍 PR was made https://github.com/axodotdev/cargo-dist/pull/2134 🤞 

---

_Comment by @Gankra on 2025-10-29 20:13_

Apologies for the delay, I'm cutting a cargo-dist release to get your full changes (0.30.0 only had the overly broad ones).

---

_Comment by @Gankra on 2025-10-29 20:13_

https://github.com/axodotdev/cargo-dist/pull/2178

---

_@woodruffw approved on 2025-10-29 20:55_

Awesome 🙂 

---

_Comment by @Gankra on 2025-10-29 21:26_

The PR is now rebased and uses the latest cargo-dist that makes zizmor happy

---

_Merged by @Gankra on 2025-10-30 00:33_

---

_Closed by @Gankra on 2025-10-30 00:33_

---

_Branch deleted on 2025-10-30 00:43_

---
