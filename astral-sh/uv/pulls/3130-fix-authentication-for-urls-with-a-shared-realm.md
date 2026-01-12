```yaml
number: 3130
title: Fix authentication for URLs with a shared realm
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/auth-401
created_at: 2024-04-18T22:42:54Z
updated_at: 2024-04-22T18:06:58Z
url: https://github.com/astral-sh/uv/pull/3130
synced_at: 2026-01-12T16:05:26Z
```

# Fix authentication for URLs with a shared realm

---

_@zanieb_

In #2976 I made some changes that led to regressions:

- We stopped tracking URLs that we had not seen credentials for in the cache
    - This means the cache no longer returns a value to indicate we've seen a realm before
- We stopped seeding the cache with URLs 
    - Combined with the above, this means we no longer had a list of locations that we would never attempt to fetch credentials for
- We added caching of credentials found on requests
    - Previously the cache was only populated from the seed or credentials found in the netrc or keyring
    - This meant that the cache was populated for locations that we previously did not cache, i.e. GitHub artifacts(?)

Unfortunately this unveiled problems with the granularity of our cache. We cache credentials per realm (roughly the hostname) but some realms have mixed authentication modes i.e. different credentials per URL or URLs that do not require credentials. Applying credentials to a URL that does not require it can lead to a failed request, as seen in #3123 where GitHub throws a 401 when receiving credentials.

To resolve this, the cache is expanded to supporting caching at two levels:

- URL, cached URL must be a prefix of the request URL
- Realm, exact match required

When we don't have URL-level credentials cached, we attempt the request without authentication first. On failure, we'll search for realm-level credentials or fetch credentials from external services. This avoids providing credentials to new URLs unless we know we need them.

Closes https://github.com/astral-sh/uv/issues/3123

---

_Label `bug` added by @zanieb on 2024-04-18 22:42_

---

_Renamed from "zb/auth 401" to "Fix authentication for URLs with a shared realm" by @zanieb on 2024-04-18 22:43_

---

_@zanieb reviewed on 2024-04-18 22:47_

---

_Review comment by @zanieb on `crates/uv-auth/src/cache.rs`:13 on 2024-04-18 22:47_

This doesn't seem ideal, we need to scan all of the authenticated URLs on each request? I presume we could make this a `BTreeSet` and use `Range` to help search?

---

_@zanieb reviewed on 2024-04-18 22:48_

---

_Review comment by @zanieb on `crates/uv-auth/src/cache.rs`:13 on 2024-04-18 22:48_

We only insert new URLs into the cache though if we fail to find authentication in the cache already though, so hopefully the size is relatively small?

---

_@charliermarsh reviewed on 2024-04-18 22:57_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:110 on 2024-04-18 22:57_

Why do we perform the request twice?

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:110 on 2024-04-18 22:59_

I believe this matches pip's behavior. We can't _know_ if we're supposed to send authentication for a URL unless it fails. For example, if you try to install two GitHub projects one private and one public, we can attach authentication from the private project to the public project which will fail.

See https://github.com/astral-sh/uv/issues/3123#issuecomment-2064706021

---

_@zanieb reviewed on 2024-04-18 22:59_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/cache.rs`:65 on 2024-04-18 23:02_

`url.as_str().starts_with(cached_url.as_str())` feels off to me. (1) Why are those the right semantics? And (2) can we do this by matching on the _data_ rather than the string representation?

---

_@charliermarsh reviewed on 2024-04-18 23:02_

---

_@charliermarsh reviewed on 2024-04-18 23:04_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:273 on 2024-04-18 23:04_

So does every URL that we visit get inserted into the cache?

---

_@charliermarsh reviewed on 2024-04-18 23:05_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:273 on 2024-04-18 23:05_

Why do we need to track on a per-URL basis?

---

_@charliermarsh reviewed on 2024-04-18 23:13_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/cache.rs`:65 on 2024-04-18 23:13_

What would happen if I made a request to:

`https://index.org/simple/botocore/?format=v4+application/json`

And it hits the netrc to grab the credentials. Then we cache the credentials under `https://index.org/simple/botocore/?format=v4+application/json`.

That path then returns a relative URL to an artifact, like `https://index.org/artifacts/foo-123.whl`. How would that latter URL get authenticated, since it wouldn't match here? Would we fetch the auth again? Would it go through the realm cache?


---

_@charliermarsh reviewed on 2024-04-18 23:15_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:144 on 2024-04-18 23:15_

So in this path, we can use the realm because we know we need credentials?

---

_@zanieb reviewed on 2024-04-18 23:28_

---

_Review comment by @zanieb on `crates/uv-auth/src/cache.rs`:65 on 2024-04-18 23:28_

We'd cache under `https://index.org/simple/botocore/` — we strip the query string when writing to the cache. But yeah the artifact would have to fall back to the realm cache since it's not a prefix.

The reason we use a prefix is a part of the RFC for caching credentials as well as a heuristic, e.g. credentials for `https://index.org/simple/botocore/` should be valid for `https://index.org/simple/botocore/foo/bar`.

The string representation of the URL is cached on the type so it's not expensive. It seems like the simplest way to check for a prefix-match. I could investigate further though?

---

_@zanieb reviewed on 2024-04-18 23:30_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:144 on 2024-04-18 23:30_

Yeah we know that authentication is needed so we don't need to "try" the request first. We could check for cached URL-level credentials first, that seems more "correct" but I feel like it'd be surprising to have the same username for multiple URLs in the same realm.

---

_@charliermarsh reviewed on 2024-04-18 23:32_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/cache.rs`:13 on 2024-04-18 23:32_

I think you want a trie?

---

_@charliermarsh reviewed on 2024-04-18 23:34_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:196 on 2024-04-18 23:34_

In what cases will we need to do this? Like, when would we expect this to happen, and when would we expect that we can retrieve from the cache?

---

_@charliermarsh reviewed on 2024-04-18 23:34_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:196 on 2024-04-18 23:34_

Maybe to put it differently: when would we expect to see cache misses for URLs on the same domain?

---

_@charliermarsh reviewed on 2024-04-18 23:35_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:110 on 2024-04-18 23:35_

Wouldn't the prefix-based URL matching avoid us failing on the two-GitHubs case though?

---

_Comment by @zanieb on 2024-04-19 17:17_

The following new tests fail on `main`:

```
uv-auth middleware::tests::test_credentials_from_keyring_mixed_authentication_in_realm
uv-auth middleware::tests::test_credentials_in_url_mixed_authentication_in_realm
```

---

_@zanieb reviewed on 2024-04-19 17:23_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:110 on 2024-04-19 17:23_

I'm sorry, I don't understand this comment.

---

_@zanieb reviewed on 2024-04-19 17:25_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:273 on 2024-04-19 17:25_

It allows us to cache credentials more precisely than the realm, so we can handle realms with mixed credentials.

---

_@charliermarsh reviewed on 2024-04-19 17:26_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:110 on 2024-04-19 17:26_

In the "two GitHubs" case, what is an example of the two URLs and the credentials that would be attached to them by the user? Like, the URL for the private project won't be a prefix of the public project, right? So we wouldn't try to reuse the credentials anyway...?

---

_@zanieb reviewed on 2024-04-19 17:27_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:196 on 2024-04-19 17:27_

If you have `example.com/foo` and it provides a link to `example.com/bar` we will not match in the URL-level cache and must attempt an unauthorized request first because `bar` may not need credentials and could fail if provided. On failure, we'll find credentials in the realm-level cache and try them.

---

_@zanieb reviewed on 2024-04-19 17:27_

---

_Review comment by @zanieb on `crates/uv-auth/src/cache.rs`:13 on 2024-04-19 17:27_

Yeah maybe. I'm questioning complexity vs performance here.

---

_@zanieb reviewed on 2024-04-19 18:00_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:110 on 2024-04-19 18:00_

So.. if we make an authenticated request we will not perform a retry. The second request is only made if we failed to find cached credentials for the prefix. This ensures that we only make a request with cached realm-level credentials if necessary. 

Let's say we see `github.com/private`, we'll cache credentials at the realm and URL-level for this.
Subsequent requests to `github.com/private/foo` will use the cached URL-level credentials, yay we avoided sending a bad request to realize we needed credentials.
If a subsequent request to `github.com/private/bar` fails, we won't perform a second request.

Now if we see `github.com/other-private`, we'll have a URL cache miss. We don't know it's private so we attempt a request. This request fails so we try the cached credentials for the realm — these work yay.

Now we see `github.com/public`. We'll have a URL cache miss but the request just succeeds — yay.

---

_@zanieb reviewed on 2024-04-19 18:08_

---

_Review comment by @zanieb on `crates/uv-auth/src/cache.rs`:65 on 2024-04-19 18:08_

Note there is a bug here because `example.com/foo-2` would match `example.com/foo` but we should treat these as distinct resources e.g. `example.com/foo-2/` and `example.com/foo/`

---

_@zanieb reviewed on 2024-04-19 19:44_

---

_Review comment by @zanieb on `crates/uv-auth/src/cache.rs`:134 on 2024-04-19 19:44_

Thank you @BurntSushi!

I modified an implementation authored by Andrew to match prefixes with chunks of URL path segments rather than raw bytes. This prevents e.g. `example/foo1` from matching `example/foo` 

---

_@zanieb reviewed on 2024-04-19 19:44_

---

_Review comment by @zanieb on `crates/uv-auth/src/cache.rs`:65 on 2024-04-19 19:44_

Resolved in the trie implementation

---

_Marked ready for review by @zanieb on 2024-04-19 19:51_

---

_@zanieb reviewed on 2024-04-19 20:42_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1337 on 2024-04-19 20:42_

As far as I can tell, these don't fail on main but seem nice to have regardless.

---

_@zanieb reviewed on 2024-04-19 20:54_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:150 on 2024-04-19 20:54_

I considered not caching in this case to reduce the size of the cache, but it's kind of confusing behavior and we use a trie to ensure cache performance is reasonable with many URLs.

---

_Review requested from @charliermarsh by @zanieb on 2024-04-22 15:57_

---

_Review comment by @BurntSushi on `crates/uv-auth/src/cache.rs`:14 on 2024-04-22 16:51_

I realize this was previously using a `Mutex`, but have you considered using `RwLock` for both of these? It looks the `get_*` methods below are read-only and _could_ be executed in parallel with other read-only calls.

I don't have a good sense of the read/write workload here so I don't know if this will be an overall benefit, but it looks like it fits here.

---

_Review comment by @BurntSushi on `crates/uv-auth/src/middleware.rs`:260 on 2024-04-22 17:04_

Maybe more of a personal preference thing, but I find long if blocks trailed by a short/trivial else block to be more difficult to read. I'm more of a fan of using early returns to simplify (which are used above here). So in this case, maybe:

```rust
if !matches!(
    response.status(),
    StatusCode::FORBIDDEN | StatusCode::NOT_FOUND | StatusCode::UNAUTHORIZED
) {
    return Ok(response);
}

trace!(
    "Request for {url} failed with {}, checking for credentials",
    response.status()
);
// and so on...
```

Basically the idea is that when you're reading the code in a linear fashion, you can check the condition, note the early return and then move on. But with the code written this way, you kinda have to keep the condition in mind as you go on, and then we you get to the `else`, I find I usually have to go back up to remind myself of what the condition is.

The other upshot of this is that you get less nesting.

---

_Review comment by @BurntSushi on `crates/uv-auth/src/middleware.rs`:292 on 2024-04-22 17:05_

Continuing on my early return crusade, I'd write this as:

```rust
let Some(credentials) = credentials else {
    return next.run(request, extensions).await;
};
let url = request.url().clone();
// and so on...
```

---

_Review comment by @BurntSushi on `crates/uv-auth/src/middleware.rs`:1192 on 2024-04-22 17:07_

Nice tests. :)

---

_@BurntSushi approved on 2024-04-22 17:07_

Really nice work. This looked pretty annoying to deal with. :)

---

_@zanieb reviewed on 2024-04-22 17:11_

---

_Review comment by @zanieb on `crates/uv-auth/src/cache.rs`:14 on 2024-04-22 17:11_

Thanks! I'd happily explore this in a follow-up to avoid increasing scope here.

---

_@zanieb reviewed on 2024-04-22 17:38_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:292 on 2024-04-22 17:38_

Ah I forget about `let else`

---

_@zanieb reviewed on 2024-04-22 17:39_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:260 on 2024-04-22 17:39_

THanks!

---

_Merged by @zanieb on 2024-04-22 18:06_

---

_Closed by @zanieb on 2024-04-22 18:06_

---

_Branch deleted on 2024-04-22 18:06_

---
