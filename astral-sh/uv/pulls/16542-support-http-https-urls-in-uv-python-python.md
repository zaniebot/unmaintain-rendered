```yaml
number: 16542
title: "Support http/https URLs in `uv python --python-downloads-json-url`"
type: pull_request
state: merged
author: MeitarR
labels: []
assignees: []
merged: true
base: main
head: remote-python-downloads-json-url
created_at: 2025-10-31T21:51:37Z
updated_at: 2025-11-14T22:51:48Z
url: https://github.com/astral-sh/uv/pull/16542
synced_at: 2026-01-10T05:58:11Z
```

# Support http/https URLs in `uv python --python-downloads-json-url`

---

_Pull request opened by @MeitarR on 2025-10-31 21:51_

continuation PR based on #14687

---

_Comment by @MeitarR on 2025-10-31 23:17_

I successfully rebased the old PR on the updated main. Later, I will also try to fix the CR comments from there.

---

_Marked ready for review by @MeitarR on 2025-11-07 19:28_

---

_Comment by @MeitarR on 2025-11-07 19:30_

@konstin, I added a WireMock test and fixed the confusing comment. It should be ready for review.

Tell me if the test is what you expected

---

_Review requested from @Gankra by @Gankra on 2025-11-10 20:16_

---

_Review comment by @Gankra on `docs/reference/environment.md`:540 on 2025-11-10 22:11_

```suggestion
This variable can be set to a local path or URL pointing to
```

(I don't think the specificity is needed -- also there's an extra `/` here if you want to preserve this text)

---

_Review comment by @Gankra on `crates/uv-static/src/env_vars.rs`:403 on 2025-11-10 22:14_

```suggestion
    /// This variable can be set to a local path or URL pointing to
```
(oh right this is the actual canonical text, the other one was generated)

---

_Review comment by @Gankra on `crates/uv-python/src/installation.rs`:134 on 2025-11-10 22:16_

```suggestion
        // default retries to avoid the middleware performing uncontrolled retries.
```

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:1024 on 2025-11-10 22:22_

Should we be adding context to these errors..?

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:1020 on 2025-11-10 22:23_

Preallocating a 1MB buffer seems a bit aggressive? Is that a pattern copied from somewhere else?

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:1033 on 2025-11-10 22:24_

Interesting nod to future-compat, sure.

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:1036 on 2025-11-10 22:25_

lol

---

_@Gankra approved on 2025-11-10 22:26_

Wow this is great! I have a few nits but this seems solid.

---

_@MeitarR reviewed on 2025-11-11 20:42_

---

_Review comment by @MeitarR on `crates/uv-python/src/downloads.rs`:1024 on 2025-11-11 20:42_

How would we do that?

I see that `read_url` already has different error types.

(BTW, I see that there is another usage of `read_url` later in the file that does the same)

---

_Review comment by @MeitarR on `crates/uv-python/src/downloads.rs`:1020 on 2025-11-11 20:48_

From reading the code (which @geofft wrote), I see that the 1MB is only relevant when we can't get the size of the buffer, and the only real case (if I understand correctly) is when in the HTTP response there where no `content-length` header.

Maybe it is somehow related to https://github.com/astral-sh/uv/pull/14687#issuecomment-3085800753 

---

_@MeitarR reviewed on 2025-11-11 20:48_

---

_@konstin reviewed on 2025-11-12 13:14_

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:1020 on 2025-11-12 13:14_

`.bytes()`, the non-streaming version, has a better collector for this, but it doesn't really fit with the current `read_url`

---

_@Gankra reviewed on 2025-11-12 13:53_

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:1024 on 2025-11-12 13:53_

It's probably fine.

---

_@Gankra approved on 2025-11-12 13:56_

Should be mergeable, but this has a non-trivial rebase to do :(

---

_@konstin reviewed on 2025-11-13 10:43_

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:1024 on 2025-11-13 10:43_

You can test this by running with an arbitrary large file (so you can abort before it finished), and turning the network off mid-download:

```
$ UV_HTTP_TIMEOUT=1 cargo run python install 3.14 --python-downloads-json-url https://files.pythonhosted.org/packages/a9/8c/3da60787bcf70add986c4ad485993026ac0ca74f2fc21410bc4eb1bb7695/torch-2.9.1-cp314-cp314t-manylinux_2_28_x86_64.whl
error: error decoding response body
  Caused by: request or response body error
  Caused by: operation timed out
```

We should make sure that we show at least the URL in the error message. 

For example, this is the error message when network isn't available when starting the download:

```
$ UV_HTTP_TIMEOUT=1 cargo run python install 3.14 --python-downloads-json-url https://files.pythonhosted.org/packages/a9/8c/3da60787bcf70add986c4ad485993026ac0ca74f2fc21410bc4eb1bb7695/torch-2.9.1-cp314-cp314t-manylinux_2_28_x86_64.whl
error: Failed to download https://files.pythonhosted.org/packages/a9/8c/3da60787bcf70add986c4ad485993026ac0ca74f2fc21410bc4eb1bb7695/torch-2.9.1-cp314-cp314t-manylinux_2_28_x86_64.whl
  Caused by: error sending request for url (https://files.pythonhosted.org/packages/a9/8c/3da60787bcf70add986c4ad485993026ac0ca74f2fc21410bc4eb1bb7695/torch-2.9.1-cp314-cp314t-manylinux_2_28_x86_64.whl)
  Caused by: client error (Connect)
  Caused by: dns error
  Caused by: failed to lookup address information: Name or service not known
```

We're also not retrying network failures as we do else, because we're using the python download client that has automatic retries deactivated, but don't implement our own retry loop, and don't do any caching. `read_url` fits badly here, `CachedClient` has the features we want: Retries, serde integration and caching.

---

_@konstin reviewed on 2025-11-13 11:52_

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:1024 on 2025-11-13 11:52_

Regarding the retrying, can you add a test to `network.rs` for `--python-downloads-json-url`?

---

_@MeitarR reviewed on 2025-11-14 22:04_

---

_Review comment by @MeitarR on `crates/uv-python/src/downloads.rs`:1024 on 2025-11-14 22:04_

Added context for the errors there

```

$ UV_HTTP_TIMEOUT=1 cargo run python install 3.14 --python-downloads-json-url https://files.pythonhosted.org/packages/a9/8c/3da60787bcf70add986c4ad485993026ac0ca74f2fc21410bc4eb1bb7695/torch-2.9.1-cp314-cp314t-manylinux_2_28_x86_64.whl
error: Error while fetching remote python downloads json from 'https://files.pythonhosted.org/packages/a9/8c/3da60787bcf70add986c4ad485993026ac0ca74f2fc21410bc4eb1bb7695/torch-2.9.1-cp314-cp314t-manylinux_2_28_x86_64.whl'
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: operation timed out
```


About the other comments, I will address them in a later PR, as this PR is very conflict-prone

---

_Merged by @Gankra on 2025-11-14 22:51_

---

_Closed by @Gankra on 2025-11-14 22:51_

---

_@Gankra reviewed on 2025-11-14 22:51_

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:1024 on 2025-11-14 22:51_

(Agreed to have this as a followup)

---
