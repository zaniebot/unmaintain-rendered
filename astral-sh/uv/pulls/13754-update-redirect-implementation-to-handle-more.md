```yaml
number: 13754
title: Update redirect implementation to handle more cases and be more testable
type: pull_request
state: merged
author: jtfmumm
labels:
  - enhancement
  - security
assignees: []
merged: true
base: feature/redirect-authentication
head: jtfm/update-redirect-handling
created_at: 2025-05-31T13:46:52Z
updated_at: 2025-06-18T08:57:38Z
url: https://github.com/astral-sh/uv/pull/13754
synced_at: 2026-01-12T16:10:50Z
```

# Update redirect implementation to handle more cases and be more testable

---

_@jtfmumm_

This PR factors out and updates the redirect handling logic from #13595. It handles a few new cases:
* If a 303 redirect is received for any method other than GET or HEAD, converts it to a GET. Unlike `reqwest`, it does not do this conversion for 301s or 302s (which is not required by RFC 7231 and was not the original intention of the spec).
* If the original request did not have a Referer header, does not include a Referer header in the redirect request.
* If the redirect is a cross-origin request, removes sensitive headers to avoid leaking credentials to untrusted domains. 
* * This change had the side effect of breaking mock server tests that redirected from `localhost` to `pypi-proxy.fly.dev`. I have added a `CrossOriginCredentialsPolicy` enum with a `#[cfg(test)]`-only `Insecure` variant. This allows existing tests to continue to work while still making it impossible to propagate credentials on cross-origin requests outside of tests.
* * I've updated the main redirect integration test to check if cross-origin requests fail (there is, by design, no way to configure an insecure cross-origin policy from the command line). But critically, netrc credentials for the new location can still be successfully fetched on a cross-origin redirect (tested in `pip_install_redirect_with_netrc_cross_origin`).
* One of the goals of the refactor was to make the redirect handling logic unit-testable. This PR adds a number of unit tests checking things like proper propagation of credentials on redirects on the same domain (and removal on cross-origin) and HTTP 303 POST-to-GET conversion.

The following table illustrates the different behaviors on current `main`, the initial (reverted) redirect handling PR (#12920), the PR that restores #12920 and fixes the 303s bug (#13595), and this PR (#13754). We want to propagate credentials on same-origin but not cross-origin redirects, and we want to look up netrc credentials on redirects.

| Behavior                                                   | main | reverted #12920 | fix #13595  | update #13754 |
|---------------------------------------|------|--------------|-------------|----------------|
| Propagate credentials on same-origin redirects  | No    | Yes                 | Yes          | Yes                        |
| Propagate credentials on cross-origin redirects | No    | Yes                 | Yes          | No                         |
| Look up netrc credentials on redirects  | No    | Yes                 | Yes          | Yes                        |
| Handle 303s without failing                  | Yes   | No                   | Yes          | Yes                        |

Depends on #13595.


---

_Label `enhancement` added by @jtfmumm on 2025-05-31 13:46_

---

_Label `security` added by @jtfmumm on 2025-05-31 13:46_

---

_@jtfmumm reviewed on 2025-05-31 14:17_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:224 on 2025-05-31 14:17_

I've added this same warning in a few places to make sure this invariant isn't broken in the future.

---

_@jtfmumm reviewed on 2025-05-31 14:35_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/edit.rs`:11865 on 2025-05-31 14:35_

On a cross-origin request (which is what happens with the mock server here), no username is propagated. It would be possible to use `reqwest_middleware` `Extensions` to do this and special-case the handling of a username propagated in that way, but we have decided to avoid the complexity around this refactor for now. Currently, this means that uv will not successfully find credentials in the keyring for cross-origin requests.

---

_Marked ready for review by @jtfmumm on 2025-05-31 14:39_

---

_Closed by @jtfmumm on 2025-05-31 14:42_

---

_Reopened by @jtfmumm on 2025-05-31 14:42_

---

_Renamed from "Update redirect implementation" to "Update redirect implementation to handle more cases and be more testable" by @jtfmumm on 2025-05-31 14:43_

---

_@konstin approved on 2025-06-05 14:08_

Can you add a table to the description showing what behaviors we have on main and how they are with the two PRs for the relevant cases? This makes easier to follow along what we're changing and that we're handling everything we want to handle.

---

_Comment by @jtfmumm on 2025-06-06 01:08_

> Can you add a table to the description showing what behaviors we have on main and how they are with the two PRs for the relevant cases? This makes easier to follow along what we're changing and that we're handling everything we want to handle.

I've added a table

---

_Comment by @jenshnielsen on 2025-06-16 13:06_

@jtfmumm Tested this against a local azure feed. As of c9c44e36f8f7aa44df77553c653a65dc8325f39e this works correctly for me and I can confirm that I can install packages as expected. 

---

_Comment by @jtfmumm on 2025-06-16 13:07_

Thanks for the confirmation @jenshnielsen!

---

_Comment by @thomasaarholt on 2025-06-16 15:03_

EDIT: Both ways worked great. No issues.

I checked out the PR, cd'd into a new folder, added a `uv.toml` file as per below, created a venv and ran `cargo run -- pip install llms` to install our custom `llms` package there. It ran fine. [Here are the -v logs](https://gist.github.com/thomasaarholt/f47790391e946cf0760d9447cdc82284).

```
# uv.toml
[pip]
keyring-provider = "subprocess"
index-url = "https://VssSessionToken@<our-internal-feed>/pypi/simple/"
```

The (really cool looking!) uvx command gets stuck on the last compilation step.  EDIT: It finished, just took quite a bit longer than compiling it myself. I then created a new folder for it, ran `uv init`, and ran the command again successfully.

---

_Comment by @jtfmumm on 2025-06-16 16:15_

Thanks @thomasaarholt! Yeah that command can be slow, but also nice to have.



---

_Merged by @jtfmumm on 2025-06-18 08:57_

---

_Closed by @jtfmumm on 2025-06-18 08:57_

---

_Branch deleted on 2025-06-18 08:57_

---
