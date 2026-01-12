```yaml
number: 16918
title: Allow setting proxy variables via global / user configuration
type: pull_request
state: merged
author: seemethere
labels:
  - configuration
assignees: []
merged: true
base: main
head: seemethere/add_proxy_configuration
created_at: 2025-12-01T22:24:16Z
updated_at: 2026-01-09T20:14:24Z
url: https://github.com/astral-sh/uv/pull/16918
synced_at: 2026-01-12T16:12:31Z
```

# Allow setting proxy variables via global / user configuration

---

_@seemethere_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This allows users to set the HTTP, HTTPS, and no proxy variables via the configuration files like ~pyproject.toml~ and uv.toml.

Users can set like so:

`uv.toml`
```toml
https-proxy = "http://my_cool_proxy:10500"
http-proxy = "http://my_cool_proxy:10500"
no-proxy = [
  "dontproxyme.com",
  "localhost",
]
```

Resolves https://github.com/astral-sh/uv/issues/9472

## Test Plan

<!-- How was it tested? -->
It also adds a new integration test for the proxy support in `uv-client`.

This was tested on some of our developer machines with our proxy setup using `~/.config/uv/uv.toml` with values like:

```toml
https-proxy = "http://my_cool_proxy:10500"
http-proxy = "http://my_cool_proxy:10500"
no-proxy = [
  "dontproxyme.com",
  "localhost",
]

```


---

_Comment by @samypr100 on 2025-12-02 02:22_

~~@seemethere Thanks for the contribution, please see [CONTRIBUTING.md](https://github.com/astral-sh/uv/blob/main/CONTRIBUTING.md#finding-ways-to-help) to help guide which issues are open to contributions.~~ (edit: initial discussions already happened in a separate medium)

For reference on this one, see https://github.com/astral-sh/uv/issues/9461#issuecomment-2509801588 which I believe will need to be addressed first before we can commit to a design for this issue.

---

_Comment by @zanieb on 2025-12-02 03:32_

I think `uv.toml` support without `pyproject.toml` support feels reasonable? Without looking at the pull request in detail yet, I had a similar concern to @samypr100. We're generally avoiding system-level options in the `pyproject.toml`.

---

_Comment by @zanieb on 2025-12-02 03:33_

Would that work for your use-case?

---

_Comment by @seemethere on 2025-12-02 05:08_

> Would that work for your use-case?

Yup this still works for my use case!

---

_Comment by @seemethere on 2025-12-02 05:09_

> I think `uv.toml` support without `pyproject.toml` support feels reasonable? Without looking at the pull request in detail yet, I had a similar concern to @samypr100. We're generally avoiding system-level options in the `pyproject.toml`.

I'll fix this up to do that! Will ping when it's ready!

---

_Comment by @seemethere on 2025-12-02 16:55_

Okay should be updated now errors should look like:

<img width="1024" height="61" alt="Screenshot 2025-12-02 at 10 55 06 AM" src="https://github.com/user-attachments/assets/fa15e379-2541-4fe1-ba2c-ef3f6132d850" />

```
~/work/uv/scratch seemethere/add_proxy_configuration*
❯ ../target/debug/uv sync
error: Failed to parse: `pyproject.toml`. The `http-proxy` field is not allowed in a `pyproject.toml` file. `http-proxy` is a system-level setting and should be placed in a `uv.toml` file instead.
```

Also added a test case to validate this in show_settings.rs


---

_Comment by @seemethere on 2025-12-05 00:47_

Rebased this to fix the merge conflicts!

---

_Renamed from "Allow setting proxy variables via configuration" to "Allow setting proxy variables via global / user configuration" by @seemethere on 2025-12-05 00:47_

---

_@zanieb reviewed on 2025-12-09 21:26_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:552 on 2025-12-09 21:26_

We can't panic on user-facing input here. We should validate these values much earlier and raise errors instead.

---

_Review comment by @zanieb on `docs/reference/settings.md`:1334 on 2025-12-09 21:30_

It seems problematic to show these `pyproject.toml` examples in the settings reference if we're not going to allow it, we might need to invest in infrastructure to omit that before this can land.

---

_@zanieb reviewed on 2025-12-09 21:30_

---

_@zanieb reviewed on 2025-12-09 21:33_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:308 on 2025-12-09 21:33_

I started looking at this and I feel like we might need a `validate_pyproject_toml` function like `validate_uv_toml`? We won't hard deny any of these options in the `pyproject.toml` toda.

I'm surprised that we allow `allow_insecure_host` in the `pyproject.toml` today, which might mean my and @samypr100's points aren't really in scope for this work.

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:308 on 2025-12-09 21:38_

Related

- https://github.com/astral-sh/uv/issues/15835
- https://github.com/astral-sh/uv/issues/14500

---

_@zanieb reviewed on 2025-12-09 21:38_

---

_@zanieb reviewed on 2025-12-09 21:41_

---

_Review comment by @zanieb on `docs/reference/settings.md`:1334 on 2025-12-09 21:41_

See https://github.com/astral-sh/uv/pull/16918#discussion_r2604447761 though

---

_@zanieb reviewed on 2025-12-09 21:42_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:308 on 2025-12-09 21:42_

I think my understanding of what we weren't allowing today is wrong and it probably doesn't make sense to block this on fixing that.

---

_@samypr100 reviewed on 2025-12-10 01:09_

---

_Review comment by @samypr100 on `crates/uv-settings/src/lib.rs`:308 on 2025-12-10 01:09_

> I'm surprised that we allow allow_insecure_host in the pyproject.toml today

Gasp 😓 

---

_@seemethere reviewed on 2025-12-10 04:41_

---

_Review comment by @seemethere on `crates/uv-client/src/base_client.rs`:552 on 2025-12-10 04:41_

Okay! Added a ProxyUrl type to handle this, this should surface errors a lot better.

Errors (from user POV) should look like:

```
invalid proxy URL scheme `ftp` in `ftp://proxy.example.com`: expected http, https, socks5, or socks5h
```

```
invalid proxy URL: relative URL without a base
```

---

_@seemethere reviewed on 2025-12-10 04:42_

---

_Review comment by @seemethere on `crates/uv-settings/src/lib.rs`:308 on 2025-12-10 04:42_

It's fine, I added a utility that should be able to allow us to do this, I can also submit a follow-up PR to not include the other ones (like insecure_host) as well if we want.

---

_@zanieb reviewed on 2025-12-10 13:21_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:557 on 2025-12-10 13:21_

Can you explain why we're not using the `Proxy` type directly for validation? Why do we add a wrapper type?

---

_Review comment by @seemethere on `crates/uv-client/src/base_client.rs`:557 on 2025-12-10 15:52_

The wrapper type includes a `Deserialize` implementation which `reqwest::Proxy` does not, as well the error messaging is a bit more informative than the generic `error building client: builder error` you might get from `reqwest`.

Happy to rework this though if we feel like it's overkill.

---

_@seemethere reviewed on 2025-12-10 15:52_

---

_@zanieb reviewed on 2025-12-10 15:54_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:557 on 2025-12-10 15:54_

Why does it wrap `Url` instead of `Proxy`?

I'm just trying to understand the pattern here as something feels a bit off.

---

_@seemethere reviewed on 2025-12-10 16:11_

---

_Review comment by @seemethere on `crates/uv-client/src/base_client.rs`:557 on 2025-12-10 16:11_

I think it mostly comes down to Proxy being a suitable type for the runtime http client while being an unsuitable type for expressing user configuration. 

I think in general what we're expressing with `http_proxy` and `https_proxy` are URLs that have routing rules expressed through their variable names.

We also get the benefits of inheriting some of the things from Url like `PartialEq`, `Serialize`, etc. which `Proxy` doesn't have.

---

_@seemethere reviewed on 2025-12-10 16:14_

---

_Review comment by @seemethere on `crates/uv-client/src/base_client.rs`:557 on 2025-12-10 16:14_

I do think that perhaps this would be something that should be upstreamed to reqwest since it might also be generally useful for other people too.

---

_Assigned to @zanieb by @zanieb on 2025-12-11 15:37_

---

_Comment by @zanieb on 2025-12-11 16:02_

I'm out sick today but my next step is to futz with this locally and propose some tweaks based on that.

---

_Comment by @seemethere on 2025-12-11 18:10_

> I'm out sick today but my next step is to futz with this locally and propose some tweaks based on that.

Sounds good! I hope you feel better!

---

_Comment by @zanieb on 2026-01-06 00:05_

Alright sorry for the delay from the holiday time off :)

I've pushed a commit that does a few things

1. Moves conversion into the `reqwest::Proxy` type into our wrapper `ProxyUrl` type to consolidate the safety logic
2. Adds curl / reqwest compatible `http://` scheme assumption
3. Adds CLI-level integration tests

and I've resolved the conflict with `main`.

Please give it a look over and let me know if you have any questions or concerns.

---

_Comment by @seemethere on 2026-01-06 16:40_

> Alright sorry for the delay from the holiday time off :)
> 
> I've pushed a commit that does a few things
> 
> 1. Moves conversion into the `reqwest::Proxy` type into our wrapper `ProxyUrl` type to consolidate the safety logic
> 2. Adds curl / reqwest compatible `http://` scheme assumption
> 3. Adds CLI-level integration tests
> 
> and I've resolved the conflict with `main`.
> 
> Please give it a look over and let me know if you have any questions or concerns.

LGTM!

---

_@zanieb approved on 2026-01-06 17:13_

---

_Merged by @zanieb on 2026-01-06 17:13_

---

_Closed by @zanieb on 2026-01-06 17:13_

---

_Label `configuration` added by @zanieb on 2026-01-06 17:14_

---

_Comment by @charliermarsh on 2026-01-09 20:14_

This is live in v0.9.23.

---
