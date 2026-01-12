```yaml
number: 16473
title: Add support for SSL_CERT_DIR
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
assignees: []
merged: true
base: main
head: ssl-cert-dir
created_at: 2025-10-27T16:26:58Z
updated_at: 2025-11-19T02:08:55Z
url: https://github.com/astral-sh/uv/pull/16473
synced_at: 2026-01-12T16:12:16Z
```

# Add support for SSL_CERT_DIR

---

_@samypr100_

## Summary

Closes https://github.com/astral-sh/uv/issues/16414

Adds support for the standard [SSL_CERT_DIR](https://docs.openssl.org/3.6/man3/SSL_CTX_load_verify_locations) which has gained recent proper support from [rustls-native-certs](https://github.com/rustls/rustls-native-certs/pull/187) in v0.8.2.

In addition, this PR clarifies documentation around `SSL_CERT_FILE` and `SSL_CERT_DIR` when used in combination with `UV_NATIVE_TLS` as mentioned in https://github.com/astral-sh/uv/issues/16412#issuecomment-3434927201 

## Test Plan

Manually tested with custom cert chains in multiple directories and loading them via SSL_CERT_DIR. We didn't have tests for `SSL_CERT_FILE` or `SSL_CERT_DIR` environment variables so I added a basic one using our own test-only certificate generation and dummy https server. I also moved some things around for better reuse.

---

_Comment by @samypr100 on 2025-10-27 17:04_

Please ignore the docker failures as its due to my depot OIDC configuration.

---

_Marked ready for review by @samypr100 on 2025-10-27 17:04_

---

_@samypr100 reviewed on 2025-10-29 17:33_

---

_Review comment by @samypr100 on `crates/uv-client/src/base_client.rs`:425 on 2025-10-29 17:33_

Note, this logic is similar to SSL_CERT_FILE I added back in https://github.com/astral-sh/uv/pull/2401

---

_@samypr100 reviewed on 2025-10-29 17:35_

---

_Review comment by @samypr100 on `crates/uv-client/tests/it/http_util.rs`:222 on 2025-10-29 17:35_

This was moved from user_agent_version into it's own function for reusability. No changes were done. The logic is effectively the same as I originally added in https://github.com/astral-sh/uv/pull/2136

---

_@samypr100 reviewed on 2025-10-29 17:36_

---

_Review comment by @samypr100 on `crates/uv-client/tests/it/http_util.rs`:382 on 2025-10-29 17:36_

This mimics `start_http_user_agent_server` except for support for TLS. I will add another test later for mTLS that I added back https://github.com/astral-sh/uv/pull/4171 in a future PR.

---

_Review comment by @samypr100 on `crates/uv-client/tests/it/ssl_certs.rs`:96 on 2025-10-29 17:39_

I think asserting that it's an error is sufficient (as it can't even establish a connection due to TLS handshake failing), but I can downcast and assert on the actual errors as well if desired.

---

_@samypr100 reviewed on 2025-10-29 17:39_

---

_@samypr100 reviewed on 2025-10-29 20:30_

---

_Review comment by @samypr100 on `crates/uv-client/tests/it/ssl_certs.rs`:96 on 2025-10-29 20:30_

Done in 27b7b76d9472885fd983a59ac769eff58f527215, I can revert if we prefer the less verbose

---

_@samypr100 reviewed on 2025-10-30 03:21_

---

_Review comment by @samypr100 on `crates/uv-client/tests/it/http_util.rs`:382 on 2025-10-30 03:21_

Ended up adding the mTLS test in this PR in e0816e27f6448c4e9c87f8f6ab0247a4fac4ddee

---

_@samypr100 reviewed on 2025-10-30 18:10_

---

_Review comment by @samypr100 on `Cargo.toml`:225 on 2025-10-30 18:10_

@konstin I just noticed your concern with regards to rustls in the comment https://github.com/astral-sh/uv/pull/16245#discussion_r2432205290

This combination should not affect our current rustls features (no `aws-lc-rs` yet😄), I was paying close attention to lock file changes.

I made sure the features didn't change or affected us (outside of what we needed to tests in the new tests).

---

_Review requested from @konstin by @samypr100 on 2025-10-30 18:13_

---

_Label `enhancement` added by @samypr100 on 2025-10-31 03:55_

---

_@samypr100 reviewed on 2025-11-02 17:23_

---

_Review comment by @samypr100 on `crates/uv-client/tests/it/http_util.rs`:222 on 2025-11-02 17:23_

This was refactored to use a builder as part of 6470d3c774fa112312a00b3bafc3ffbb98b3ce07

---

_@samypr100 reviewed on 2025-11-02 17:23_

---

_Review comment by @samypr100 on `crates/uv-client/tests/it/http_util.rs`:382 on 2025-11-02 17:23_

Refactored a bit as part of 6470d3c774fa112312a00b3bafc3ffbb98b3ce07

---

_Review comment by @konstin on `Cargo.lock`:1096 on 2025-11-05 21:29_

The crate self-describes as
>  Proof of concept ranged integers in Rust.

which doesn't inspire confidence.

---

_Review comment by @konstin on `Cargo.lock`:3353 on 2025-11-05 21:31_

It's definitely better than with pulling in the other crypto library too, but I still have mixed feelings on those dependencies, we're newly adding legacy crates. It's seem that that's mostly because there's not much activity on rcgen? For example, the rest of our dependency tree uses jiff, having migrated from chrono, while this library adds the time crate.

---

_Review comment by @konstin on `.config/nextest.toml`:17 on 2025-11-05 21:35_

If we got test isolation, why do those tests need to be serial?

---

_Comment by @codspeed-hq[bot] on 2025-11-10 06:17_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/samypr100%3Assl-cert-dir?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16473 will **not alter performance**

<sub>Comparing <code>samypr100:ssl-cert-dir</code> (013385f) with <code>main</code> (aec4254)</sub>



### Summary

`✅ 6` untouched  





---

_@zanieb reviewed on 2025-11-11 20:09_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:391 on 2025-11-11 20:09_

```suggestion
                warn_user_once!("Ignoring invalid `SSL_CERT_DIR`. None of the entries exist.");
```

I would also split between "None of the entries exist" and "The directory does not exist" depending on the length of `missing`.

---

_@zanieb reviewed on 2025-11-11 20:10_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:384 on 2025-11-11 20:10_

We should filter the empty string, right?

---

_@zanieb reviewed on 2025-11-11 20:11_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:386 on 2025-11-11 20:11_

```suggestion
            // Parse `SSL_CERT_DIR`, with support for multiple entries using a platform-specific delimiter (`:` on Unix, `;` on Windows)
```

I think the latter half of this comment is just explaining the already clear code

---

_@zanieb reviewed on 2025-11-11 20:12_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:407 on 2025-11-11 20:12_

As noted in https://github.com/astral-sh/uv/pull/16473/files#r2515602680 I'd split based on one or more since one is going to be the common case

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:593 on 2025-11-11 20:12_

Let's note the support for multiple entries

---

_@zanieb reviewed on 2025-11-11 20:12_

---

_@zanieb approved on 2025-11-11 20:13_

---

_@samypr100 reviewed on 2025-11-11 20:55_

---

_Review comment by @samypr100 on `crates/uv-client/src/base_client.rs`:384 on 2025-11-11 20:55_

I kept it the same as rustls-native-certs to align on behavior https://github.com/rustls/rustls-native-certs/blob/813790a297ad4399efe70a8e5264ca1b420acbec/src/lib.rs#L206

Are you thinking of the `SSL_CERT_DIR=":some\dir:"` scenario?

---

_@zanieb reviewed on 2025-11-11 20:59_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:384 on 2025-11-11 20:59_

I'm thinking of `SSL_CERT_DIR=""` which in uv generally means we ignore the variable entirely but here we'll throw a warning? I think

>  warn_user_once!("Ignoring invalid `SSL_CERT_DIR`. None of the entries exists.");

I also think empty paths _within_ the variable could be weird and I'd probably ignore them entirely but 🤷‍♀️ 

---

_@samypr100 reviewed on 2025-11-11 21:01_

---

_Review comment by @samypr100 on `crates/uv-client/src/base_client.rs`:391 on 2025-11-11 21:01_

Is b7405d76f952da1c3388c6e85671b317d2d842f1 what you had in mind?

---

_@samypr100 reviewed on 2025-11-11 21:01_

---

_Review comment by @samypr100 on `crates/uv-client/src/base_client.rs`:407 on 2025-11-11 21:01_

See b7405d76f952da1c3388c6e85671b317d2d842f1, is it better?

---

_@samypr100 reviewed on 2025-11-11 21:01_

---

_Review comment by @samypr100 on `crates/uv-client/src/base_client.rs`:386 on 2025-11-11 21:01_

Done in b7405d76f952da1c3388c6e85671b317d2d842f1

---

_@samypr100 reviewed on 2025-11-11 21:04_

---

_Review comment by @samypr100 on `crates/uv-static/src/env_vars.rs`:593 on 2025-11-11 21:04_

Good point, done in b7405d76f952da1c3388c6e85671b317d2d842f1

---

_@samypr100 reviewed on 2025-11-11 21:08_

---

_Review comment by @samypr100 on `crates/uv-client/src/base_client.rs`:384 on 2025-11-11 21:08_

Fair, not sure what's best behavior for everyone. Personally, I'd like to know if I messed up setting the env var contents. I'll proceed to add the empty scenario. Not sure what rustls does on the former case.

---

_@samypr100 reviewed on 2025-11-11 21:19_

---

_Review comment by @samypr100 on `crates/uv-client/src/base_client.rs`:384 on 2025-11-11 21:19_

Done in 6f9065b9e7c723ceb01b8af473476c7b678665b8

We don't do this for SSL_CERT_FILE, do we want to align there?

---

_@konstin reviewed on 2025-11-12 13:11_

---

_@samypr100 reviewed on 2025-11-12 13:53_

---

_Review comment by @samypr100 on `Cargo.lock`:3353 on 2025-11-12 13:53_

Fair, although I'd consider rcgen "stable". Last activity in rcgen was today. Ading Jiff support to it may be possible if the rustls maintainers are willing to.

Note, we're no longer adding rcgen in this PR as it was added in another.

---

_Review comment by @samypr100 on `.config/nextest.toml`:17 on 2025-11-12 14:07_

Good catch, fixed in 2b94775dca8e5ba819d22fd6e56cad011909b179

---

_@samypr100 reviewed on 2025-11-12 14:07_

---

_@zanieb reviewed on 2025-11-12 14:56_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:384 on 2025-11-12 14:56_

I guess so! Seems fine to open an issue to track it — it'd be a good first issue.

---

_@zanieb reviewed on 2025-11-12 14:57_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:389 on 2025-11-12 14:57_

I don't think a warning is appropriate here, we've gotten complaints about this before — people want to be able to unset variables by using an empty string.

We usually just use https://github.com/astral-sh/uv/blob/63ab24776566bb76d68751e2ee5180b6cec34090/crates/uv/src/settings.rs#L107

It seems fine to fail on a value that's whitespace.

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:404 on 2025-11-12 14:58_

Why don't we show the values like we do for the `!missing.is_empty()` block? (just wondering)

---

_@zanieb reviewed on 2025-11-12 14:58_

---

_@samypr100 reviewed on 2025-11-12 20:36_

---

_Review comment by @samypr100 on `crates/uv-client/src/base_client.rs`:389 on 2025-11-12 20:36_

Funny, I wrote that line 😆 

Changed in d3316733008bedb95a1008a3a04047adc10db99d

---

_@samypr100 reviewed on 2025-11-12 20:37_

---

_Review comment by @samypr100 on `crates/uv-client/src/base_client.rs`:404 on 2025-11-12 20:37_

Good point, changed in d3316733008bedb95a1008a3a04047adc10db99d it's consistent with SSL_CERT_FILE as well.

---

_@samypr100 reviewed on 2025-11-12 20:45_

---

_Review comment by @samypr100 on `crates/uv-client/src/base_client.rs`:384 on 2025-11-12 20:45_

Issue made https://github.com/astral-sh/uv/issues/16712

---

_@samypr100 reviewed on 2025-11-12 20:46_

---

_Review comment by @samypr100 on `crates/uv-client/src/base_client.rs`:384 on 2025-11-12 20:46_

Changed to no-trimming / no warning version in  d3316733008bedb95a1008a3a04047adc10db99d

---

_Merged by @zanieb on 2025-11-16 17:48_

---

_Closed by @zanieb on 2025-11-16 17:48_

---

_Branch deleted on 2025-11-16 18:01_

---

_Comment by @michael-o on 2025-11-18 17:17_

This really looks like openssl-probe reinvented. It does already the env var magic. I am confused why uv has to reproduce it?! uv does use openssl-probe.

---

_Comment by @samypr100 on 2025-11-19 02:07_

> This really looks like openssl-probe reinvented. It does already the env var magic. I am confused why uv has to reproduce it?! uv does use openssl-probe.

I'm don't think this is correct as stated as uv does not use openssl-probe by default. It's opt-in when the `--native-tls` is passed and `SSL_CERT_DIR` or `SSL_CERT_FILE` are not set. We delegate to [rustls-native-certs](https://github.com/rustls/rustls-native-certs/blob/main/src/lib.rs#L118-L125) and primarily proxy the supported environment variables to provide a better user experience when common user errors are present in the values of these.

---
