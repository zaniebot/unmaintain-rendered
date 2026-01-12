```yaml
number: 6389
title: Add support for defining credentials for uv in an environment variable
type: pull_request
state: closed
author: zanieb
labels:
  - enhancement
  - configuration
assignees: []
base: main
head: zb/auth-urls
created_at: 2024-08-21T23:24:51Z
updated_at: 2024-09-24T16:58:11Z
url: https://github.com/astral-sh/uv/pull/6389
synced_at: 2026-01-12T16:07:21Z
```

# Add support for defining credentials for uv in an environment variable

---

_@zanieb_

Adds `UV_AUTH_URLS` which defines credentials that should be used for URLs if we make a request there.

This is helpful for cases where, e.g., you want to provide credentials for an index but you don't want to change the index URL semantics by setting `UV_INDEX_URL`.

Implemented by seeding the URL authentication cache with the value of the variable.

Supports things like:

- `UV_AUTH_URLS="username:password@hostname"`
- `UV_AUTH_URLS="username@hostname"`
- `UV_AUTH_URLS="username:password@hostname username:password@other-hostname"`
- `UV_AUTH_URLS="username:password@hostname/prefix"`

There's a small caveat here, that if you use an index URL like `foo@hostname/simple` and you provide a password `bar:password@hostname` — the password should not be used but requests to fetch wheels from a different, parent path e.g.`hostname/files` can allow use of `bar:password`. This requires #4583 to address. In the meantime, credentials should just be properly scoped.

---

_@zanieb reviewed on 2024-08-22 18:55_

---

_Review comment by @zanieb on `crates/uv-auth/src/lib.rs`:95 on 2024-08-22 18:55_

@BurntSushi recommendations here?

---

_@zanieb reviewed on 2024-08-22 18:56_

---

_Review comment by @zanieb on `crates/uv-auth/src/lib.rs`:95 on 2024-08-22 18:56_

(We can't try parsing and reparse on a missing scheme error because `public:test@foo.com` treats `public` as a scheme.)

---

_@BurntSushi reviewed on 2024-08-22 19:01_

---

_Review comment by @BurntSushi on `crates/uv-auth/src/lib.rs`:95 on 2024-08-22 19:01_

What's wrong with what's here?

---

_@zanieb reviewed on 2024-08-22 20:50_

---

_Review comment by @zanieb on `crates/uv-auth/src/lib.rs`:95 on 2024-08-22 20:50_

Uhh, well if you do `http://foo` we don't want to support that but we'll fail by parsing `https://http://foo` which seems weird. I guess that's fine though?

---

_@zanieb reviewed on 2024-08-22 22:41_

---

_Review comment by @zanieb on `crates/uv-auth/src/lib.rs`:95 on 2024-08-22 22:41_

I'll just special case http for now.

---

_Comment by @zanieb on 2024-08-22 23:07_

I want to write documentation for this and make sure it's actually solving people's problems, but the implementation is ready for review.

---

_Marked ready for review by @zanieb on 2024-08-22 23:07_

---

_Label `enhancement` added by @zanieb on 2024-08-22 23:20_

---

_Label `configuration` added by @zanieb on 2024-08-22 23:20_

---

_@BurntSushi reviewed on 2024-08-23 11:54_

---

_Review comment by @BurntSushi on `crates/uv-auth/src/lib.rs`:95 on 2024-08-23 11:54_

Yeah I don't have any better ideas here, especially if URL parsing is treating things as schemes that we don't want to be treated as schemes. So I'd guess you'd probably need to write a custom URL parser to deal with this in a way that is completely satisfying. Special casing `http` seems fine.

---

_Review comment by @BurntSushi on `crates/uv-auth/src/lib.rs`:53 on 2024-08-23 12:11_

```suggestion
/// added to the realm-level cache to ensure that a URL with a path is not used unless a request
```

---

_Review comment by @BurntSushi on `crates/uv-auth/src/lib.rs`:87 on 2024-08-23 12:12_

Should this be a `warn!`?

---

_@BurntSushi approved on 2024-08-23 12:13_

---

_@zanieb reviewed on 2024-08-23 12:44_

---

_Review comment by @zanieb on `crates/uv-auth/src/lib.rs`:87 on 2024-08-23 12:44_

Can't use our `warn_user` macros here, but we could use a warn tracing level — just doesn't mean much yet. Seems reasonable though.

---

_@zanieb reviewed on 2024-08-23 12:54_

---

_Review comment by @zanieb on `crates/uv-auth/src/lib.rs`:87 on 2024-08-23 12:54_

```suggestion
        warn!("Ignoring insecure URL in `UV_AUTH_URLS`: {url}");
```

---

_@zanieb reviewed on 2024-08-23 12:54_

---

_Review comment by @zanieb on `crates/uv-auth/src/lib.rs`:3 on 2024-08-23 12:54_

```suggestion
use tracing::{debug, trace, warn};
```

---

_@zanieb reviewed on 2024-08-23 12:55_

---

_Review comment by @zanieb on `crates/uv-auth/src/lib.rs`:75 on 2024-08-23 12:55_

```suggestion
            warn!("Ignoring URL without credentials in `UV_AUTH_URLS`: {url}");
```

---

_@charliermarsh approved on 2024-08-26 19:22_

---

_Comment by @charliermarsh on 2024-08-26 19:22_

Maybe `UV_BASIC_AUTH_URLS`? Feel free to ignore though.

---

_Comment by @zanieb on 2024-08-26 19:23_

I don't love my name, so maybe. Will consider...

---

_Review comment by @mkniewallner on `crates/uv/tests/pip_install.rs`:3857 on 2024-09-03 11:47_

```suggestion
fn install_package_basic_auth_from_uv_basic_auth_urls() {
```
grep and replace gone a bit too wild I guess 😄 

---

_@mkniewallner reviewed on 2024-09-03 11:48_

---

_Comment by @mkniewallner on 2024-09-03 11:50_

Since auto-merge was enabled, apart from test and `clippy` failures, is there anything preventing the merge of this PR, or was it maybe just forgotten?

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:3857 on 2024-09-03 17:56_

Haha thanks

---

_@zanieb reviewed on 2024-09-03 17:56_

---

_Comment by @zanieb on 2024-09-03 17:57_

I'm not getting a ton of signal about how useful it would be so it's relatively low priority.

More relevant though, I was on vacation and am just now noticing auto-merge failed.

---

_Comment by @charliermarsh on 2024-09-03 18:00_

@zanieb -- My gut is to punt on this until we see more demand, but ultimately defer to you.

---

_Comment by @mkniewallner on 2024-09-03 19:11_

> I'm not getting a ton of signal about how useful it would be so it's relatively low priority.

Definitely not high priority IMO. Personally I can leave without it, since we can pass the entire index including credentials with `UV_INDEX_URL` and `UV_EXTRA_INDEX_URL`. So it's really just like a convenient thing to have, as I like the idea of having the index explicitly set in `[tool.uv]`. On projects where a lot of developers work on, having this explicit configuration makes things more obvious (although in fact, even without the feature, you can still explicitly set the index in `[tool.uv]` and override the entire thing with the env var, but this kinda repeats something that is already set).

Do you think the feature would also fit what is planned in https://github.com/astral-sh/uv/issues/171, or are we thinking about providing another mechanism to provide credentials for this other feature (e.g. what [Poetry does](https://python-poetry.org/docs/repositories/#configuring-credentials), where the credentials are set for a source name by including the name in the header)? If we're not sure yet, maybe we can hold off on merging this PR until the other feature is implemented, to try to come up with something that would ideally fit both use cases?

---

_Comment by @zanieb on 2024-09-03 19:38_

@charliermarsh is going to start working on #171 soon and I think that will probably supersede this and include a design for index credentials.

---

_Closed by @zanieb on 2024-09-24 16:58_

---
