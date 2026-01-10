```yaml
number: 12651
title: Use index URL instead of package URL for keyring credential lookups
type: pull_request
state: merged
author: jtfmumm
labels:
  - network
  - breaking
assignees: []
merged: true
base: release/070
head: jtfm/index-url-auth
created_at: 2025-04-03T14:05:57Z
updated_at: 2025-04-19T07:41:04Z
url: https://github.com/astral-sh/uv/pull/12651
synced_at: 2026-01-10T11:10:40Z
```

# Use index URL instead of package URL for keyring credential lookups

---

_Pull request opened by @jtfmumm on 2025-04-03 14:05_

Some registries (like Azure Artifact) can require you to authenticate separately for every package URL if you do not authenticate for the /simple endpoint. These changes make the auth middleware aware of index URL endpoints and attempts to fetch keyring credentials for such an index URL when making a request to any URL it's a prefix of.

The current uv behavior is to cache credentials either at the request URL or realm level. But with these changes, we also need to cache credentials at the index level. Note that when uv does not detect an index URL for a request URL, it will continue to apply the old behavior.

Addresses part of #4056
Closes #4583
Closes #11236
Closes #11391
Closes #11507


---

_Label `network` added by @jtfmumm on 2025-04-03 14:09_

---

_Review requested from @zanieb by @jtfmumm on 2025-04-03 14:41_

---

_Comment by @jtfmumm on 2025-04-03 15:09_

In this version, when an index URL prefix is found, we will use that instead of the full package URL to look up credentials. This is probably the expected behavior, but is it possible some users now rely on the old behavior? If this is a concern, another option is to first check one and then the other, though keyring lookup can be slow.

---

_@zanieb reviewed on 2025-04-03 17:17_

---

_Review comment by @zanieb on `crates/uv-auth/src/lib.rs`:13 on 2025-04-03 17:17_

I'd probably call this `indexes` to match the existing naming scheme

---

_@zanieb reviewed on 2025-04-03 17:18_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:61 on 2025-04-03 17:18_

Nit: Similar to https://github.com/astral-sh/uv/pull/12651/files#r2027441564, it seems fine to drop the `auth_` prefix here since these are the only indexes which exist in this context.

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:61 on 2025-04-03 17:18_

(I think this applies broadly)

---

_@zanieb reviewed on 2025-04-03 17:18_

---

_@zanieb reviewed on 2025-04-03 17:20_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:492 on 2025-04-03 17:20_

This drops the check for the full URL match, should we do that? Are you worried about breakage? What if a credential provider changes its behavior based on the accessed resource?

---

_@zanieb reviewed on 2025-04-03 17:22_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index_url.rs`:420 on 2025-04-03 17:22_

if we need more information anyway, can we just store the `Index` or `IndexUrl` type directly? Why do we decompose to `index_url` and `policy_url` eagerly?

---

_@zanieb reviewed on 2025-04-03 17:26_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:492 on 2025-04-03 17:26_

I missed your comment at https://github.com/astral-sh/uv/pull/12651#issuecomment-2776117709

> In this version, when an index URL prefix is found, we will use that instead of the full package URL to look up credentials. This is probably the expected behavior, but is it possible some users now rely on the old behavior? If this is a concern, another option is to first check one and then the other, though keyring lookup can be slow.

Thanks for highlighting this change.

I'm not sure what's best here. If it was fast, I'd expect to check the exact URL first then a child URL (e.g., for the index). I'd be _surprised_ if this change broke someone's setup but it does seem plausible. 🤔 

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:480 on 2025-04-03 17:29_

I think there's more that needs to change for the authentication middleware to become index-aware. We calculate the cache key above

```rust
        // Fetches can be expensive, so we will only run them _once_ per realm and username combination
        // All other requests for the same realm will wait until the first one completes
        let key = (
            Realm::from(url),
            Username::from(
                credentials
                    .map(|credentials| credentials.username().unwrap_or_default().to_string()),
            ),
        );
```

but we should cache at the index level instead of the realm level.

It may be plausible to ship that separately? But it's part of the objective of #4583 

---

_@zanieb reviewed on 2025-04-03 17:29_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:360 on 2025-04-03 22:47_

This looks a little awkward, can't the `client.with(auth_middleware);` bit be outside the `else` and
`auth_middleware = auth_middleware.with_auth_indexes(auth_indexes.clone())` be in the `if`? (I presume if the borrow checker doesn't get in the way it could also be done with a `.map` instead?)

---

_@zanieb reviewed on 2025-04-03 22:47_

---

_@zanieb reviewed on 2025-04-03 22:48_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index_url.rs`:420 on 2025-04-03 22:48_

Then we could avoid having to re-explain the meaning of the `root()` URL up in the `AuthIndex` docs too.

---

_Comment by @zanieb on 2025-04-03 22:49_

Excited that you tackled this!

---

_@jtfmumm reviewed on 2025-04-04 07:18_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:420 on 2025-04-04 07:18_

The first reason is that using `Index` or `IndexUrl` would cause a crate dependency cycle. But because we only use a small subset of that data here, it didn't seem like a good idea to refactor our dependency graph to allow this.

But it also made sense to me to pay the small memory cost of storing both URLs to avoid recalculating the URLs every time. And I think there's value in making it clear in the docs in this crate what the root URL means in this context (namely that it is the point at which we apply the auth policy).

---

_@jtfmumm reviewed on 2025-04-04 07:24_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:492 on 2025-04-04 07:24_

I guess one rationale for doing both checks is that the performance cost is only paid in the failure case. That is, if the credentials exist for the index URL we only do one check. If not, then either there are credentials for the child URL (which the user would want us to check) or there are no credentials. So I'll update it and we can always revisit.

---

_@jtfmumm reviewed on 2025-04-04 07:54_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:360 on 2025-04-04 07:54_

I was trying to keep `auth_middleware` immutable while also not repeating the `.with_keyring` and `.with_only_authenticated` code within the if/else. 

This new version configures `auth_middleware` first and then applies it to the client.

---

_@jtfmumm reviewed on 2025-04-04 08:10_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/lib.rs`:13 on 2025-04-04 08:10_

Yeah this naming isn't ideal, but since we can't depend on `IndexLocations` here without a circular crate dependency, the `AuthIndex` is used in the context of `IndexLocations` to construct an `AuthIndexes`.

A solution is to use the fully qualified name `uv_auth::Index` in the context of `IndexLocations`. So I'll do that.

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:480 on 2025-04-04 08:29_

Yeah, I agree this needs to be done. The main question is whether we still need to support realm-level caching for URLs not tied to an index. I think so, given that the `BaseClient` can be used for multiple purposes. If not, this work would entail reworking our credential cache to operate at the index URL level instead of the realm level. If so, we'll need to support both.

I'm ok with adding that to the scope of this PR, but I also think this PR could be merged independently without causing surprising behavior (and would strictly be an improvement). My inclination is to merge separately. 

---

_@jtfmumm reviewed on 2025-04-04 08:29_

---

_@zanieb reviewed on 2025-04-04 16:08_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:480 on 2025-04-04 16:08_

> The main question is whether we still need to support realm-level caching for URLs not tied to an index. I think so, given that the BaseClient can be used for multiple purposes.

Yeah I think so too.

> [...] I also think this PR could be merged independently without causing surprising behavior (and would strictly be an improvement). My inclination is to merge separately.

Works for me.

---

_@zanieb reviewed on 2025-04-04 16:14_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:360 on 2025-04-04 16:14_

I think I'd prefer just making `auth_middleware` mutable to make it clearer that we're unconditionally applying the auth middleware. We could also not make it mutable and unconditionally do (pseudocode) `auth_middleware.with_auth_indexes(self.auth_indexes.unwrap_or_default())`? Stepping up a level, why are we storing `Option<Indexes>` in the parent type and `Indexes` in the child type? If those were consistent, there wouldn't be a need to resolve the `Option`.

---

_@zanieb reviewed on 2025-04-04 16:19_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:492 on 2025-04-04 16:19_

The only problem there is we usually go from more specific -> less specific so...

Exact URL -> Index -> Realm

but Index is the most likely match here. 

---

_@zanieb reviewed on 2025-04-04 16:24_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index_url.rs`:420 on 2025-04-04 16:24_

Ah, the cycle is unfortunate. 

I looked around a bit... but I don't see an obvious path forward. What would the refactor look like? e.g., would a `uv-index` crate resolve the problem? Generally our bar for refactoring the crate dependencies is not high.

> But it also made sense to me to pay the small memory cost of storing both URLs to avoid recalculating the URLs every time. 

I have no qualms about the memory cost. It's the indirection that worries me (from a maintenance / understanding perspective). I think we might want to explore a different strategy for comparing the URLs so we don't need to depend on storing a mutated version — but that can happen separately.

---

_Comment by @zanieb on 2025-04-04 16:33_

I'd retitle to be more relevant to users (otherwise, I just have to do it on release) e.g.:

> Use index URL instead of package URL for keyring credential lookups 

---

_@jtfmumm reviewed on 2025-04-04 16:55_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:492 on 2025-04-04 16:55_

If we know there's a corresponding index URL, I think we should prefer it since that's the expected behavior. I'm not sure there's an advantage to starting from the most specific URL in that case. Really the only reason to check the package URL if credentials aren't found for the index URL is in case any users are depending on the previous behavior.

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:360 on 2025-04-04 17:01_

Are you commenting on the latest version of this?

```
                        let auth_middleware = if let Some(auth_indexes) = &self.auth_indexes {
                            AuthMiddleware::new().with_auth_indexes(auth_indexes.clone())
                        } else {
                            AuthMiddleware::new()
                        }
                        .with_keyring(self.keyring.to_provider())
                        .with_only_authenticated(true);

                        client = client.with(auth_middleware);
       

---

_@jtfmumm reviewed on 2025-04-04 17:01_

---

_@jtfmumm reviewed on 2025-04-04 17:05_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:360 on 2025-04-04 17:05_

The `BaseClientBuilder` has an `Option` because there might be no auth indexes for that `BaseClient`, which seemed like useful information. But `AuthMiddleware`, which is constructed later, does at least have an empty one. We could have `AuthMiddleware.with_auth_indexes` take an `Option`, simplifying the above code, but that's not standard.

---

_Renamed from "Fetch keyring credentials from index URL in auth middleware" to "Use index URL instead of package URL for keyring credential lookups" by @jtfmumm on 2025-04-04 17:06_

---

_@zanieb reviewed on 2025-04-04 17:06_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:360 on 2025-04-04 17:06_

Sorry, no. I hadn't reviewed the code again — I was just commenting on the discussion (and I misread "This new version configures auth_middleware first and then applies it to the client.").

This seems like an improvement, but I still wonder about the need for the `Option` type at all? (edit: we commented concurrently here)

---

_@jtfmumm reviewed on 2025-04-04 17:08_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:420 on 2025-04-04 17:08_

If we copy the root() logic in the `uv_auth` crate then we can just pass (url, policy) pairs into the `uv_auth::Indexes` constructor. Then we will manage the URLs close to where they're used.

---

_@zanieb reviewed on 2025-04-04 17:08_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:360 on 2025-04-04 17:08_

> The BaseClientBuilder has an Option because there might be no auth indexes for that BaseClient, which seemed like useful information. But AuthMiddleware, which is constructed later, does at least have an empty one. We could have AuthMiddleware.with_auth_indexes take an Option, simplifying the above code, but that's not standard.

Why is it useful information though? It seems surprising that we care about representing the `Option` in one place but not the other (i.e., in `AuthMiddleware` we just initialize an empty set of URLs). There are a few other `AuthMiddleware` methods that take an `Option`, it just depends on the type it's storing internally. 

---

_@zanieb reviewed on 2025-04-04 17:11_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index_url.rs`:420 on 2025-04-04 17:11_

I'm hesitant to copy the `root()` logic. My goal is to keep the meaning of `Index::root` in a single place, which is why I prefer to refer to `Index` directly within the authentication logic.

---

_@zanieb reviewed on 2025-04-04 17:13_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:360 on 2025-04-04 17:13_

Concretely, since `AuthMiddleware` is always constructed here, then there shouldn't be a case where a `None` for `AuthIndexes` avoids overwriting some pre-inserted value in `AuthMiddleware`, right?

---

_@jtfmumm reviewed on 2025-04-04 18:47_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:420 on 2025-04-04 18:47_

This will require some significant crate changes. `Index` depends on both `uv_auth::AuthPolicy` and `uv_auth::Credentials`. We might need to move `AuthMiddleware` into `uv_client` and have it depend on both `uv_auth` and `uv_distribution_types`. But the credentials cache code doesn't expose everything as `pub`, so we'll need to think about where that belongs. Possibly we'll need to divide up `uv_auth`. 

Do you consider this a blocking issue for this PR?

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:360 on 2025-04-04 18:57_

I've updated so that `BasicClientBuilder` no longer uses an `Option` and then simplified this code.

---

_@jtfmumm reviewed on 2025-04-04 18:57_

---

_@zanieb reviewed on 2025-04-04 18:59_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index_url.rs`:420 on 2025-04-04 18:59_

No, not blocking. I'm just trying to build a better understanding of the problem now. I would feel differently if it required less refactoring but I think it's an okay trade-off to leave it as is. Maybe leave a TODO in there?

---

_@zanieb reviewed on 2025-04-04 19:05_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:360 on 2025-04-04 19:05_

Thank you, appreciate it.

---

_@jtfmumm reviewed on 2025-04-04 19:07_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:420 on 2025-04-04 19:07_

Added

---

_@zanieb reviewed on 2025-04-04 19:09_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:492 on 2025-04-04 19:09_

> I'm not sure there's an advantage to starting from the most specific URL in that case.

The primary advantages are that:

1. It's consistent with our general authentication strategy
2. It's definitely backwards compatible — if a keyring provider has different handling for child resources in an index, this is a breaking change as-is

I actually think there's probably no benefit to falling back to checking for the full URL if there aren't credentials at the index level, that seems even more unlikely than the case in (2).



---

_@zanieb reviewed on 2025-04-04 19:10_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:492 on 2025-04-04 19:10_

I'm tempted to say we should just "do the breaking change" here? We could release the behavior change under preview then stabilize in 0.7.0? or just hold off until 0.7.0?

---

_Label `breaking` added by @jtfmumm on 2025-04-04 19:13_

---

_Added to milestone `v0.7.0` by @jtfmumm on 2025-04-04 19:13_

---

_@jtfmumm reviewed on 2025-04-04 19:14_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:492 on 2025-04-04 19:14_

That's fine by me. I've updated the code, added a `breaking` label, and added this to the milestone.

---

_@jtfmumm reviewed on 2025-04-07 14:35_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:480 on 2025-04-07 14:35_

I've implemented this in #12717. We could either merge that into this PR if approved or merge it after this one.

---

_@zanieb approved on 2025-04-07 23:06_

---

_@zanieb reviewed on 2025-04-09 15:21_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:5435 on 2025-04-09 15:21_

Sorry if I'm just missing this, but... is there a test case here where the keyring has credentials for the index level and we _don't_ fallback to the realm request?

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:5435 on 2025-04-09 15:22_

Similarly, is there a test case where the keyring has credentials for the full URL but not at the index or realm level?

(These would be achieved by tweaking `EnvVars::KEYRING_TEST_CREDENTIALS, r#"{"pypi-proxy.fly.dev": {"other": "heron"}}"`)

---

_@zanieb reviewed on 2025-04-09 15:22_

---

_@jtfmumm reviewed on 2025-04-09 17:34_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/pip_install.rs`:5435 on 2025-04-09 17:34_

> Sorry if I'm just missing this, but... is there a test case here where the keyring has credentials for the index level and we _don't_ fallback to the realm request?

That test is in the follow-on PR #12717 (which we can merge into this one).


> Similarly, is there a test case where the keyring has credentials for the full URL but not at the index or realm level?
> 
> (These would be achieved by tweaking `EnvVars::KEYRING_TEST_CREDENTIALS, r#"{"pypi-proxy.fly.dev": {"other": "heron"}}"`)

I don't have this test, so I can add it.

---

_@jtfmumm reviewed on 2025-04-10 15:41_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/edit.rs`:10270 on 2025-04-10 15:41_

@zanieb Here is the full URL test. As discussed in another thread, we don't actually check the full URL in the keyring now for indexes, which is reflected here.

---

_Comment by @zanieb on 2025-04-18 21:27_

Is this waiting on anything?

---

_Comment by @jtfmumm on 2025-04-18 21:38_

> Is this waiting on anything?

No it’s ready

---

_Comment by @zanieb on 2025-04-18 21:40_

I think it needs to be based on release/070 instead?

---

_Comment by @jtfmumm on 2025-04-18 21:43_

Yeah I can do that this weekend

---

_Merged by @jtfmumm on 2025-04-19 07:41_

---

_Closed by @jtfmumm on 2025-04-19 07:41_

---

_Branch deleted on 2025-04-19 07:41_

---
