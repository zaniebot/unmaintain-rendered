```yaml
number: 13333
title: Redact credentials when displaying URLs
type: pull_request
state: merged
author: jtfmumm
labels: []
assignees: []
merged: true
base: main
head: jtfm/redact-urls
created_at: 2025-05-07T16:47:17Z
updated_at: 2025-05-12T16:58:28Z
url: https://github.com/astral-sh/uv/pull/13333
synced_at: 2026-01-12T16:10:39Z
```

# Redact credentials when displaying URLs

---

_@jtfmumm_

This PR redacts credentials in displayed URLs. 

It mostly relies on a `redacted_url` function (and where possible `IndexUrl::redacted`). This is a quick way to prevent leaked credentials but it's prone to programmer error when adding new trace statements. A better follow-on would use a `RedactedUrl` type with the appropriate `Display` implementation. This would allow us to still extract credentials from the URL while displaying it securely. On the plus side, the sites where the `redacted_url` function are used serve as easy signposts for where to use the new type in a future PR.

Closes #1714.


---

_@charliermarsh reviewed on 2025-05-07 16:50_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/cache.rs`:97 on 2025-05-07 16:50_

I think this should all happen in the `trace!` macro, so that it doesn't run at all unless `trace!` is enabled (extremely rare).

---

_@charliermarsh reviewed on 2025-05-07 16:51_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/lib.rs`:34 on 2025-05-07 16:51_

Same here.

---

_@charliermarsh reviewed on 2025-05-07 16:51_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/reporters.rs`:304 on 2025-05-07 16:51_

Why not move this to the use site, like line 320/322?

---

_@jtfmumm reviewed on 2025-05-07 16:53_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/reporters.rs`:304 on 2025-05-07 16:53_

The idea is to make it more unlikely for future changes to use the `Url` with credentials. The places I didn't do this were where we were also potentially using the credentials from the `Url`

---

_Review comment by @jtfmumm on `crates/uv-auth/src/cache.rs`:97 on 2025-05-07 17:00_

I can update to use the trace sites. The thinking was that this made it more unlikely a developer would accidentally leak credentials if adding a future trace.

---

_@jtfmumm reviewed on 2025-05-07 17:00_

---

_Closed by @jtfmumm on 2025-05-07 18:48_

---

_Reopened by @jtfmumm on 2025-05-07 18:48_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/cache.rs`:97 on 2025-05-07 20:20_

But does this mean we also need to redact when we _insert_?

---

_@charliermarsh reviewed on 2025-05-07 20:20_

---

_@jtfmumm reviewed on 2025-05-07 21:51_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/cache.rs`:97 on 2025-05-07 21:51_

Yes, it looks like I was only handling the insert into `CREDENTIALS_CACHE` before. Updated. I'm going to add a test for this case since I would have expected it to break.

---

_@charliermarsh reviewed on 2025-05-07 22:21_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:329 on 2025-05-07 22:21_

I'm actually a little more worried about this as a footgun (versus the need to repeatedly call `redacted` in the `tracing` macros). I think we should either remove this gating (always redact), or move the `redacted_url` calls into the `tracing` macros.

---

_@jtfmumm reviewed on 2025-05-09 07:13_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/credentials.rs`:329 on 2025-05-09 07:13_

Ok, I lean toward removing this and always redacting. I will be doing a follow-up using the type system where the redaction will only happen on `Display`. 

---

_Review comment by @jtfmumm on `crates/uv-auth/src/credentials.rs`:329 on 2025-05-09 07:22_

Removed.

---

_@jtfmumm reviewed on 2025-05-09 07:22_

---

_@zanieb reviewed on 2025-05-09 21:24_

---

_Review comment by @zanieb on `crates/uv-auth/src/credentials.rs`:329 on 2025-05-09 21:24_

It looks like this isn't removed, did you need to push still?

---

_@jtfmumm reviewed on 2025-05-09 21:53_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/credentials.rs`:329 on 2025-05-09 21:53_

Pushed

---

_Review comment by @charliermarsh on `crates/uv-git-types/src/lib.rs`:179 on 2025-05-10 14:43_

As a nit, I personally find this easier to read with early returns:

```rust
/// Return a version of the URL with redacted credentials, allowing the generic `git` username (without a password)
/// in SSH URLs, as in, `ssh://git@github.com/...`.
pub fn redacted_url(url: &Url) -> Cow<'_, Url> {
    if url.username().is_empty() && url.password().is_none() {
        return Cow::Borrowed(url);
    }
    if url.scheme() == "ssh" && url.username() == "git" && url.password().is_none() {
        return Cow::Borrowed(url);
    }

    let mut url = url.clone();
    if url.password().is_some() {
        let _ = url.set_password(Some("****"));
    } else if url.username() != "" {
        // A username on its own might be a secret token.
        let _ = url.set_username("****");
    }
    Cow::Owned(url)
}
```

---

_Review comment by @charliermarsh on `crates/uv-git-types/src/lib.rs`:177 on 2025-05-10 14:46_

It's a little confusing that half of our redact methods remove the credentials entirely, and then half replace them with stars. It's also a little strange (though may not matter in practice) that we're using this version for the cache key, so like the cache key path now includes these stars?

It might be nice to use a different name for these methods that replace with stars vs. those that remove the credentials entirely (like `sensitive` vs. `redact` or `without_credentials` or something).

Or just drop the stars entirely and match the existing redact logic for consistency for now.


---

_@charliermarsh approved on 2025-05-10 14:47_

---

_@jtfmumm reviewed on 2025-05-11 12:24_

---

_Review comment by @jtfmumm on `crates/uv-git-types/src/lib.rs`:179 on 2025-05-11 12:24_

Changed

---

_@jtfmumm reviewed on 2025-05-11 12:49_

---

_Review comment by @jtfmumm on `crates/uv-git-types/src/lib.rs`:177 on 2025-05-11 12:49_

It can be helpful to have the stars when debugging but we can defer this decision until after this PR (on `main` we do it in some places and not others). The follow-up PR will put this decision in one place. To move this PR forward, I've removed the masking from `redacted_url` and dropped the stars there.

I've also removed all uses from `uv-auth` (including around the credentials cache). As a result, I've moved `redacted_url` out of `uv-auth` and into a new `uv-redacted` crate. This means we continue to rely on the existing mechanisms for redacting credentials there. This should help move the PR forward and is still a strict improvement.

---

_@charliermarsh reviewed on 2025-05-12 15:34_

---

_Review comment by @charliermarsh on `crates/uv-git-types/src/lib.rs`:177 on 2025-05-12 15:34_

Agreed! Can definitely be helpful, mostly just pointing out that there are two different behaviors using the same terminology and we probably need to be thoughtful about which we use at each site (e.g., `***` for anything in tracing; omitted entirely for anything persisted, like the lockfile).

---

_Comment by @charliermarsh on 2025-05-12 15:34_

(Seems fine to merge.)

---

_Merged by @jtfmumm on 2025-05-12 16:58_

---

_Closed by @jtfmumm on 2025-05-12 16:58_

---

_Branch deleted on 2025-05-12 16:58_

---
