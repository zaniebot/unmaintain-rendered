```yaml
number: 15925
title: Add S3 request signing
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/s3
created_at: 2025-09-18T03:00:16Z
updated_at: 2025-09-22T23:59:53Z
url: https://github.com/astral-sh/uv/pull/15925
synced_at: 2026-01-10T06:36:15Z
```

# Add S3 request signing

---

_Pull request opened by @charliermarsh on 2025-09-18 03:00_

## Summary

This PR enables users to mark a URL as an S3 endpoint, at which point uv will sign requests to that URL by detecting credentials from the standard AWS environment variables, configuration files, etc.

Signing is handled by the [reqsign](https://docs.rs/reqsign/latest/reqsign/) crate, which we can also use in the future to sign requests for other providers.


---

_Review requested from @zanieb by @charliermarsh on 2025-09-18 03:00_

---

_@charliermarsh reviewed on 2025-09-18 03:01_

---

_Review comment by @charliermarsh on `Cargo.lock`:667 on 2025-09-18 03:01_

I'd like to add Jiff support upstream if we can, possibly before merging this \cc @BurntSushi

---

_@charliermarsh reviewed on 2025-09-18 03:02_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:358 on 2025-09-18 03:02_

A lot of churn in this PR comes from this piece. We need a new case, which is: a signer that can sign a request (as opposed to `Credentials`, which can be stored in a file, converted into a header, etc.). I initially implemented this as a new variant on the `Credentials` enum, but it made too many invalid states possible (e.g., `to_header_value` had to return an `Option`; the TOML deserialization had to fail). Instead, I added a new type one level higher on the hierarchy which has an `authenticate` method. I think this is much cleaner but does lead to more churn.


---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:906 on 2025-09-18 03:03_

I don't _think_ the order is very important here? But I no longer `Arc::new` this _before_ calling `uv_auth::store_credentials`, so this just saves us a clone in most cases.

---

_@charliermarsh reviewed on 2025-09-18 03:03_

---

_@zanieb reviewed on 2025-09-18 12:25_

---

_Review comment by @zanieb on `crates/uv-auth/src/credentials.rs`:358 on 2025-09-18 12:25_

I like this, yeah.

---

_@zanieb reviewed on 2025-09-18 12:33_

---

_Review comment by @zanieb on `crates/uv-auth/src/providers.rs`:81 on 2025-09-18 12:33_

Do we just need to add `.without_subdomain` to `Realm`?

---

_Review comment by @zanieb on `crates/uv-auth/src/providers.rs`:87 on 2025-09-18 12:33_

Should be in `EnvVars`

---

_@zanieb reviewed on 2025-09-18 12:33_

---

_Comment by @zanieb on 2025-09-18 12:35_

Pretty easy!

---

_@BurntSushi reviewed on 2025-09-18 13:20_

---

_Review comment by @BurntSushi on `Cargo.lock`:667 on 2025-09-18 13:20_

From a very quick glance, it's probably non-trivial for them to support both. Depending.

Anywho, I opened an issue: https://github.com/apache/opendal-reqsign/issues/631

---

_@charliermarsh reviewed on 2025-09-19 02:05_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/providers.rs`:81 on 2025-09-19 02:05_

Yeah... Something like that? So if the user provides `bar.com`, then we probably want to match _either_ `foo.bar.com` or `bar.com`.


---

_@charliermarsh reviewed on 2025-09-19 02:09_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/providers.rs`:81 on 2025-09-19 02:09_

It turns out that it's non-trivial to strip a subdomain, because you need the list eTLDs in order to figure out where the subdomain starts. For example, the URL could end in `.co.uk`, so it's not just "keep the last two segments".

---

_@charliermarsh reviewed on 2025-09-19 02:37_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/providers.rs`:81 on 2025-09-19 02:37_

I added a `RealmRef::is_subdomain_of` method which does this. (Asking if it's a subdomain of something else is easier than stripping the subdomain, I think.)

---

_@zanieb reviewed on 2025-09-20 15:34_

---

_Review comment by @zanieb on `crates/uv-auth/src/providers.rs`:81 on 2025-09-20 15:34_

Oh interesting. That seems reasonable.

---

_Review requested from @zanieb by @charliermarsh on 2025-09-22 18:49_

---

_Marked ready for review by @charliermarsh on 2025-09-22 18:49_

---

_Label `enhancement` added by @charliermarsh on 2025-09-22 18:49_

---

_Label `preview` added by @charliermarsh on 2025-09-22 18:49_

---

_Comment by @charliermarsh on 2025-09-22 19:04_

I think this is ready for a real review.

---

_@zanieb reviewed on 2025-09-22 22:28_

---

_Review comment by @zanieb on `crates/uv-auth/src/credentials.rs`:448 on 2025-09-22 22:28_

Should we note why this unwrap is safe?

---

_@zanieb approved on 2025-09-22 22:30_

---

_Merged by @charliermarsh on 2025-09-22 23:59_

---

_Closed by @charliermarsh on 2025-09-22 23:59_

---

_Branch deleted on 2025-09-22 23:59_

---
