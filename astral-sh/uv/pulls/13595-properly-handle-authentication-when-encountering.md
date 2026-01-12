```yaml
number: 13595
title: Properly handle authentication when encountering redirects
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: feature/redirect-authentication
head: jtfm/fix-redirect-handling
created_at: 2025-05-22T15:18:02Z
updated_at: 2025-06-18T08:52:41Z
url: https://github.com/astral-sh/uv/pull/13595
synced_at: 2026-01-12T16:10:45Z
```

# Properly handle authentication when encountering redirects

---

_@jtfmumm_

This is mostly restoring #13215. It also includes a one-line fix for #13208 (which resulted from that PR). In particular, Azure was returning 303s which were not being correctly handled. 

I have also opened another PR (#13754) that refactors and improves the redirect handling here. It also supersedes the fix here. There are some tests failing here but they all pass there.

This PR depends on #13615, which adds a script for testing against registries. The test fails for Azure when running against the restored #13215 alone and passes with the fix. It also passes for AWS CodeArtifact, GCP Artifact Registry, JFrog Artifactory, GitLab, and Gemfury in both cases. I also plan to test against Cloudsmith and Nexus.

---

_@jtfmumm reviewed on 2025-05-23 10:56_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:560 on 2025-05-23 10:56_

This is the change that fixes the Azure bug

---

_Label `bug` added by @jtfmumm on 2025-05-24 20:06_

---

_Assigned to @konstin by @zanieb on 2025-05-27 18:13_

---

_Comment by @codspeed-hq[bot] on 2025-05-30 21:54_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/jtfm%2Ffix-redirect-handling)

### Merging #13595 will **not alter performance**

<sub>Comparing <code>jtfm/fix-redirect-handling</code> (77960e9) with <code>main</code> (499c8aa)</sub>



### Summary

`✅ 12` untouched benchmarks  





---

_Marked ready for review by @jtfmumm on 2025-05-31 14:45_

---

_@konstin reviewed on 2025-06-04 15:50_

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:1228 on 2025-06-04 15:50_

nit: import sorting

---

_@konstin reviewed on 2025-06-04 16:06_

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:70 on 2025-06-04 16:06_

Can you write more on how to read those values, which middleware is calling which and what do we skip

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:560 on 2025-06-04 16:45_

Do we know what reqwest does here? (i briefly looked but couldn't find it)

---

_@konstin reviewed on 2025-06-04 16:45_

---

_@konstin approved on 2025-06-04 16:47_

LGTM, we do need the test script to verify against though

---

_@jtfmumm reviewed on 2025-06-04 17:00_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:560 on 2025-06-04 17:00_

The updates to the redirect implementation in #13754 are closer to what reqwest does  

---

_Comment by @jtfmumm on 2025-06-04 17:03_

This PR depends on #13615, which adds the test script I’ve used to locally verify the cloud providers. Before merging I will also test a number of other registries. We still have to decide if local testing with the script is enough for merging or if we want to make it part of CI

---

_@jtfmumm reviewed on 2025-06-04 17:04_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:560 on 2025-06-04 17:04_

But reqwest handles this same list of status codes

---

_Comment by @konstin on 2025-06-05 12:44_

CI integration would be good but not critical, and it also depends on credential handling and such, for me a single command local testing would be crucial, so every team member can validate future changes.

---

_Comment by @jenshnielsen on 2025-06-16 12:34_

@jtfmumm I can confirm that this seems to work correctly for me installing packages from an Azure feed build locally from source on windows 11 as of 8a8566796d1a2ec06f319fe57c4f8fbb640e52e7

e.g. running a random command such as `cargo run pip install --upgrade numpy` into an environment with an older version of numpy correctly installs numpy from the internal feed.



---

_Comment by @jtfmumm on 2025-06-16 12:37_

Thanks for your help @jenshnielsen!

---

_Comment by @jtfmumm on 2025-06-16 12:50_

@jenshnielsen Would you mind also testing #13754? I initially linked this PR (#13595) in the request I cc'd you on, but it would also be helpful to see if #13754 works for you.

---

_Comment by @jenshnielsen on 2025-06-16 13:07_

@jtfmumm Done. It works see comments in #13754 

---

_Merged by @jtfmumm on 2025-06-18 08:49_

---

_Closed by @jtfmumm on 2025-06-18 08:49_

---

_Branch deleted on 2025-06-18 08:49_

---
