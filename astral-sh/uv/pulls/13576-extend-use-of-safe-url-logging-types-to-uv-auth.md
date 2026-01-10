```yaml
number: 13576
title: "Extend use of safe URL logging types to `uv-auth` crate"
type: pull_request
state: merged
author: jtfmumm
labels:
  - security
assignees: []
merged: true
base: jtfm/log-safe-url
head: jtfm/log-safe-url-uv-auth
created_at: 2025-05-21T12:37:58Z
updated_at: 2025-05-26T21:43:59Z
url: https://github.com/astral-sh/uv/pull/13576
synced_at: 2026-01-10T11:10:41Z
```

# Extend use of safe URL logging types to `uv-auth` crate

---

_Pull request opened by @jtfmumm on 2025-05-21 12:37_

Extends PR #13560 by using the `DisplaySafeUrl` and new `DisplaySafeUrlRef` types in the `uv-auth` crate. 

The old `tracing_url` function in `middleware.rs` created a string from request URL and credentials. If tracing was enabled, it would also add credentials to that URL and then mask them. In this PR, this function instead creates a `DisplaySafeUrl` and sets its credentials to be equal to the passed in credentials. This removes special-case masking logic and also ensures that downstream functions take a `DisplaySafeUrl` instead of a `&str`. It comes at the cost of one extra clone per request (not including retries) when tracing mode is disabled.

`DisplaySafeUrlRef` was added to be able to work with `reqwest::Request::url()` without having to clone. As an alternative, we could have `DisplaySafeUrl` wrap a `Cow`, but this would require new lifetime annotations across many points in the codebase. Given that the need to wrap a `&Url` is currently limited to a few cases in `uv_auth`, I decided against paying the ergonomic costs. We could also clone the request `Url` in each case and then wrap it in a `DisplaySafeUrl`. This would be a simple change and remove the need for `DisplaySafeUrlRef`, but would require multiple extra clones per request. 

Depends on #13560.


---

_Comment by @codspeed-hq[bot] on 2025-05-21 12:43_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/uv/branches/jtfm%2Flog-safe-url-uv-auth)

### Merging #13576 will **degrade performances by 10.62%**

<sub>Comparing <code>jtfm/log-safe-url-uv-auth</code> (71802f4) with <code>jtfm/log-safe-url</code> (28c59bd)</sub>



### Summary

`❌ 1` regressions  
`✅ 11` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/jtfm%2Flog-safe-url-uv-auth)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` wheelname_tag_compatibility[flyte-long-compatible] `` | 1.8 µs | 2 µs | -10.62% |


---

_Label `security` added by @jtfmumm on 2025-05-24 20:07_

---

_@konstin approved on 2025-05-26 20:18_

---

_Merged by @jtfmumm on 2025-05-26 21:43_

---

_Closed by @jtfmumm on 2025-05-26 21:43_

---

_Branch deleted on 2025-05-26 21:43_

---
