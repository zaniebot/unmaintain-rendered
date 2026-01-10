```yaml
number: 12805
title: Make uv’s first-index strategy more secure by default by failing early on authentication failure
type: pull_request
state: merged
author: jtfmumm
labels:
  - enhancement
  - network
  - breaking
assignees: []
merged: true
base: release/070
head: jtfm/first-index-secure-by-default
created_at: 2025-04-10T12:38:11Z
updated_at: 2025-04-30T13:06:32Z
url: https://github.com/astral-sh/uv/pull/12805
synced_at: 2026-01-10T11:10:40Z
```

# Make uv’s first-index strategy more secure by default by failing early on authentication failure

---

_Pull request opened by @jtfmumm on 2025-04-10 12:38_

uv’s default index strategy was designed with dependency confusion attacks in mind. [According to the docs](https://docs.astral.sh/uv/configuration/indexes/#searching-across-multiple-indexes), “if a package exists on an internal index, it should always be installed from the internal index, and never from PyPI”. Unfortunately, this is not true in the case where authentication fails on that internal index. In that case, uv will simply try the next index (even on the `first-index` strategy). This means that uv is not secure by default in this common scenario.

This PR causes uv to stop searching for a package if it encounters an authentication failure at an index. It is possible to opt out of this behavior for an index with a new `pyproject.toml` option `ignore-error-codes`. For example:

```
[[tool.uv.index]]
name = "my-index"
url = "<index-url>"
ignore-error-codes = [401, 403]
```

This will also enable users to handle idiosyncratic registries in a more fine-grained way. For example, PyTorch registries return a 403 when a package is not found. In this PR, we special-case PyTorch registries to ignore 403s, but users can use `ignore-error-codes` to handle similar behaviors if they encounter them on internal registries. 

Depends on #12651

Closes #9429
Closes #12362

---

_Label `enhancement` added by @jtfmumm on 2025-04-10 12:38_

---

_Label `network` added by @jtfmumm on 2025-04-10 12:38_

---

_Label `breaking` added by @jtfmumm on 2025-04-10 12:38_

---

_Renamed from "Exit indexes search on auth failure to make uv's first-index strategy more secure by default" to "Make uv’s first-index strategy more secure by default by failing early on authentication failure" by @jtfmumm on 2025-04-10 13:15_

---

_@zanieb reviewed on 2025-04-10 19:45_

---

_Review comment by @zanieb on `crates/uv-auth/src/index.rs`:76 on 2025-04-10 19:45_

I love watching this comment jump around :D

---

_@zanieb reviewed on 2025-04-16 18:58_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/status_code_strategy.rs`:40 on 2025-04-16 18:58_

Why `expect` here? How do we know they're valid? Should this just return an `Err`?

---

_@zanieb reviewed on 2025-04-16 19:00_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:237 on 2025-04-16 19:00_

Does this mean we don't allow overriding our default behavior for given index URLs by defining an empty set `ignore-error-codes = []`?

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:393 on 2025-04-16 19:01_

Why not just store `StatusCode` directly instead of the u16?

---

_@zanieb reviewed on 2025-04-16 19:01_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:520 on 2025-04-16 19:02_

Can this use `.map_or`?

---

_@zanieb reviewed on 2025-04-16 19:02_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:289 on 2025-04-16 19:03_

Is this part of this pull request? Does this need rebase?

---

_@zanieb reviewed on 2025-04-16 19:03_

---

_@zanieb reviewed on 2025-04-16 19:06_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index_url.rs`:595 on 2025-04-16 19:06_

Can you add a doc?

---

_@jtfmumm reviewed on 2025-04-17 09:59_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:289 on 2025-04-17 09:59_

Yeah, need a rebase

---

_@jtfmumm reviewed on 2025-04-17 10:02_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:393 on 2025-04-17 10:02_

I tried that first, but `StatusCode` doesn't implement `Serialize`. 

---

_@jtfmumm reviewed on 2025-04-17 10:10_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:237 on 2025-04-17 10:10_

The new default behavior is to only ignore 404s, which we do no matter what `IndexStatusCodeStrategy` is specified. I'm not sure we want to allow disabling that. 

---

_@jtfmumm reviewed on 2025-04-17 10:29_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:595 on 2025-04-17 10:29_

Added

---

_@zanieb reviewed on 2025-04-17 13:38_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:393 on 2025-04-17 13:38_

Why does it need to implement `Serialize`?

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:237 on 2025-04-17 13:43_

I'm referring to the `from_index_url` behavior here, not the default behavior. Let's say I'm using pytorch but don't want to ignore 403s (e.g., because something has changed and our default is incorrect), that's not possible with `ignore-error-codes = []`, right? You'd need `ignore-error-codes = [XXX]` where `XXX` is some arbitrary code.

Separately, regarding the fallback to the default behavior... what do we do if a user specifies `ignore-error-codes = [404]`?  We'd ignore the 404, right?

---

_@zanieb reviewed on 2025-04-17 13:43_

---

_@jtfmumm reviewed on 2025-04-17 13:51_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:237 on 2025-04-17 13:51_

Ah, ok that wasn't clear. That's true for the pytorch case. I can update it to treat an explicit empty list as overriding the default.

If you specified just `[404]`, then uv would behave exactly as it does with the default.

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:237 on 2025-04-17 14:01_

> I can update it to treat an explicit empty list as overriding the default.

I like the idea of allowing the user to opt out of our inference.

> If you specified just [404], then uv would behave exactly as it does with the default.

Ah yeah sorry that was a silly question, the 404 is _ignored_ by default. Confusing myself :)

Is the motivation for this that you don't want people to need to specify `404` once they start adding `ignore-error-codes`? And / or that you think people should _never_ be allowed to fail on 404s?

---

_@zanieb reviewed on 2025-04-17 14:01_

---

_@zanieb reviewed on 2025-04-17 14:03_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:237 on 2025-04-17 14:03_

(fwiw we had a similar discussion around extension of default values in https://github.com/astral-sh/uv/pull/7136#discussion_r1750399976)

---

_@jtfmumm reviewed on 2025-04-17 14:21_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:237 on 2025-04-17 14:21_

Yeah, I agree that allowing them to opt-out is usefully flexible. But I don't think failing on 404s makes sense. The whole point of the index strategies is to determine what to do when a package is missing. If someone requested the ability to fail on 404s, I'd recommend that they set a single index they want to search as the default. Does that make sense?

---

_@jtfmumm reviewed on 2025-04-17 14:22_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:237 on 2025-04-17 14:22_

(updated by the way)

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:393 on 2025-04-17 15:29_

Both `Serialize` and `Deserialize` are derived on `Index` and `StatusCode` doesn't implement either. We could do `#[serde(skip)]` but that seemed incorrect to me.

---

_@jtfmumm reviewed on 2025-04-17 15:29_

---

_@zanieb reviewed on 2025-04-17 16:57_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:393 on 2025-04-17 16:57_

Stepping up a level, why is `Index` serializable? For writing it to the TOML file? If that's it, why _would_ we need this field to be serializable? A user can't construct it via the CLI. If truly necessary, why not use a wrapper type to implement serializability?

---

_@zanieb reviewed on 2025-04-17 17:01_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:237 on 2025-04-17 17:01_

Alright, that seems sensible. Let's just make sure it's clearly documented that 404s are always "ignored". Can you also add a section to https://docs.astral.sh/uv/configuration/indexes/

---

_@jtfmumm reviewed on 2025-04-17 18:44_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:393 on 2025-04-17 18:44_

If we don’t need it, I can use #[serde(skip)]

---

_@jtfmumm reviewed on 2025-04-17 19:13_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:393 on 2025-04-17 19:13_

I guess it's going to be more complicated than that. We can't use skip when we need a `deserialize_with` to parse the numbers in the TOML file. I'll need to implement a wrapper type around `StatusCode` for deriving `schemars::JsonSchema` as well as `De/Serialize` if we want to do it this way. 

---

_@zanieb reviewed on 2025-04-17 20:18_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:393 on 2025-04-17 20:18_

It seems nice for type safety throughout and straightforward to implement? I don't feel super strongly, but the current approach of validating at the CLI level, but retaining the u16, then revalidating later seems quite weird.

---

_@jtfmumm reviewed on 2025-04-17 20:30_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:393 on 2025-04-17 20:30_

Yeah that’s why originally it was validated once on ingest with an expect down the line when converted to a StatusCode. But I can also create the wrapper

---

_@charliermarsh reviewed on 2025-04-17 20:53_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/status_code_strategy.rs`:22 on 2025-04-17 20:53_

Should we compare the `Realm` here instead? Or use `endswith`?

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/status_code_strategy.rs`:27 on 2025-04-17 20:54_

I don't think `vec!` is necessary here.

---

_@charliermarsh reviewed on 2025-04-17 20:54_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/status_code_strategy.rs`:75 on 2025-04-17 20:54_

This can be `Copy`.

---

_@charliermarsh reviewed on 2025-04-17 20:54_

---

_@charliermarsh reviewed on 2025-04-17 20:56_

---

_Review comment by @charliermarsh on `crates/uv-client/src/error.rs`:240 on 2025-04-17 20:56_

Can we retain the type? In other words, should this be `#[from] IndexSourceError`? Why `String`?

---

_@charliermarsh reviewed on 2025-04-17 20:56_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/Cargo.toml`:33 on 2025-04-17 20:56_

Can we make this a dev dependency? (We typically want to avoid using `anyhow` in library crates.)

---

_@jtfmumm reviewed on 2025-04-18 07:46_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/Cargo.toml`:33 on 2025-04-18 07:46_

Removed with refactor

---

_Review comment by @jtfmumm on `crates/uv-client/src/error.rs`:240 on 2025-04-18 07:47_

Removed with refactor

---

_@jtfmumm reviewed on 2025-04-18 07:47_

---

_Comment by @jtfmumm on 2025-04-18 08:28_

@zanieb I've created a wrapper type for `StatusCode` that implements `JsonSchema` and `De/Serialize` on [this commit](https://github.com/astral-sh/uv/pull/12805/commits/4da1534f68bf482714232db449877aa855db774c).

---

_@jtfmumm reviewed on 2025-04-18 09:09_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:237 on 2025-04-18 09:09_

Added

---

_@zanieb reviewed on 2025-04-18 21:14_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/status_code_strategy.rs`:112 on 2025-04-18 21:14_

Should this just be a deref implementation? (we do that pretty often)

---

_@zanieb reviewed on 2025-04-18 21:15_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/status_code_strategy.rs`:147 on 2025-04-18 21:15_

Ah it seems nice to have this in the schema

---

_@zanieb reviewed on 2025-04-18 21:16_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:10665 on 2025-04-18 21:16_

This looks like a regression, e.g., that `help: ` message is gone.

This is an example of what I was concerned about (e.g., over in https://github.com/astral-sh/uv/pull/12667#discussion_r2029115491) by exiting eagerly instead of relying existing error handling.

---

_@zanieb reviewed on 2025-04-18 21:18_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:7527 on 2025-04-18 21:18_

This also looks like a regression, we no longer are explaining _why_ `iniconfig` was required (though it _is_ an improvement that we say which index we failed to fetch it from — I think that's just the missing hint I flagged in https://github.com/astral-sh/uv/pull/12667#discussion_r2029183230 though).

---

_@zanieb reviewed on 2025-04-18 21:19_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:216 on 2025-04-18 21:19_

I'd link to the strategy here, if we can

---

_@zanieb reviewed on 2025-04-18 21:23_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:217 on 2025-04-18 21:23_

Elsewhere, we stylize error codes as "HTTP XXX" instead of "`XXX NAME`". I do think the name is nice, I might do "HTTP 403 Forbidden" though? I'd probably omit the backticks, since you refer to the error codes in the subsequent text without them.

You can probably just say "status code" instead of "response status code".

Generally, I'd avoid using "it" to refer to uv if it's easy. So... "uv will stop searching if a ... is encountered" rather than "uv will stop searching if it encounters ...".



---

_@zanieb reviewed on 2025-04-18 21:24_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:220 on 2025-04-18 21:24_

We avoid referring to "Users" like this in the documentation. Instead, I'd say "You can configure which error codes..." or, more typically, "To ignore additional error codes for an index, use the `...` setting."

---

_@jtfmumm reviewed on 2025-04-18 21:37_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/status_code_strategy.rs`:112 on 2025-04-18 21:37_

I’m pretty sure you can’t match against e.g. StatusCode::FORBIDDEN that way (which is what we need this for)

---

_@zanieb reviewed on 2025-04-18 21:43_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/status_code_strategy.rs`:112 on 2025-04-18 21:43_

Wouldn't you just deref it where you're currently calling this method?

---

_@jtfmumm reviewed on 2025-04-19 07:45_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/status_code_strategy.rs`:112 on 2025-04-19 07:45_

Oh yeah, I was thinking of the transparent deref, but you're right you can just call `deref()`. Updated

---

_@jtfmumm reviewed on 2025-04-21 09:10_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/edit.rs`:10665 on 2025-04-21 09:10_

I've created a PR off this branch to address this problem (see #13016). 

---

_@jtfmumm reviewed on 2025-04-22 14:53_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/lock.rs`:7527 on 2025-04-22 14:53_

Addressed in #13016

---

_Comment by @jtfmumm on 2025-04-22 14:59_

@zanieb Now that #13016 is merged into this PR and I've rebased against `release/070`, I don't think there is unaddressed feedback.

---

_@charliermarsh reviewed on 2025-04-25 02:46_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:477 on 2025-04-25 02:46_

Are these changes intended as part of this PR?

---

_@charliermarsh reviewed on 2025-04-25 02:48_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/cache.rs`:21 on 2025-04-25 02:48_

As a point of data modeling, I think this should be an enum, not a string -- is that possible? I recommend always approaching `String` with skepticism by default, since casting down to `String` means you've lost ~all type safety and type information. If we know this is either a realm or an index URL, then it makes sense to model the data that way, right?

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:387 on 2025-04-25 02:50_

Personally, I'd just call this `index_url`, since `maybe` is implied by the type.

---

_@charliermarsh reviewed on 2025-04-25 02:50_

---

_@charliermarsh reviewed on 2025-04-25 02:50_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:379 on 2025-04-25 02:50_

Why is this not `status_code_strategy_for`?

---

_@charliermarsh reviewed on 2025-04-25 02:51_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:998 on 2025-04-25 02:51_

I think we should document these variants, do you mind?

---

_@charliermarsh reviewed on 2025-04-25 02:52_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:353 on 2025-04-25 02:52_

Should this variant include the status code? Should we log this interaction here?

---

_@charliermarsh reviewed on 2025-04-25 02:54_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:521 on 2025-04-25 02:54_

Do we now lose this error? Does it ever get surfaced to the user?

---

_@charliermarsh reviewed on 2025-04-25 02:54_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:353 on 2025-04-25 02:54_

Having trouble following a bit, how does this error propagate upwards?

---

_@charliermarsh reviewed on 2025-04-25 02:56_

---

_Review comment by @charliermarsh on `docs/configuration/indexes.md`:217 on 2025-04-25 02:56_

What does "stop searching" mean, for a user? Similarly, what does it mean to "keep" searching?

---

_@jtfmumm reviewed on 2025-04-25 07:25_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/tool/run.rs`:477 on 2025-04-25 07:25_

No, just needed to rebase against the newly rebased `release/070`

---

_@jtfmumm reviewed on 2025-04-25 07:43_

---

_Review comment by @jtfmumm on `crates/uv-client/src/registry_client.rs`:379 on 2025-04-25 07:43_

I've added a comment. For unsafe index strategies, we ignore authentication failures and just find whatever matches we can across the indexes.

---

_@jtfmumm reviewed on 2025-04-25 07:47_

---

_Review comment by @jtfmumm on `crates/uv-client/src/registry_client.rs`:353 on 2025-04-25 07:47_

> Having trouble following a bit, how does this error propagate upwards?

It's the same as before. On a 401 or 403 that our strategy does not ignore, we set that in the `IndexCapabilities`. When you break here, that effectively ends the search and a `PackageNotFound` error will be returned at the end of this function.

---

_@jtfmumm reviewed on 2025-04-25 08:03_

---

_Review comment by @jtfmumm on `crates/uv-client/src/registry_client.rs`:521 on 2025-04-25 08:03_

It no longer gets surfaced to the user if it's an error with a response status code. That's because the new interface gives the user the ability to ignore any status code they want. 

But now I've updated it so that if the strategy check fails on anything other than a 401 or 403, we just return the original error. This still provides the flexibility but preserves the old error information.

---

_@jtfmumm reviewed on 2025-04-25 08:05_

---

_Review comment by @jtfmumm on `docs/configuration/indexes.md`:217 on 2025-04-25 08:05_

I thought it was clear from the section title, but I'll make it more explicit.

It now reads "stop searching across indexes" and "continue searching across indexes"

---

_@jtfmumm reviewed on 2025-04-25 08:48_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/cache.rs`:21 on 2025-04-25 08:48_

Updated

---

_Closed by @jtfmumm on 2025-04-25 08:50_

---

_Reopened by @jtfmumm on 2025-04-25 08:50_

---

_Closed by @jtfmumm on 2025-04-25 09:11_

---

_Reopened by @jtfmumm on 2025-04-25 09:11_

---

_@jtfmumm reviewed on 2025-04-25 09:39_

---

_Review comment by @jtfmumm on `crates/uv-client/src/registry_client.rs`:521 on 2025-04-25 09:39_

I've also added a regression test for this case.

---

_Review comment by @charliermarsh on `crates/uv-auth/src/cache.rs`:19 on 2025-04-29 13:24_

Rustdoc please for the variants.

---

_Review comment by @charliermarsh on `crates/uv-auth/src/cache.rs`:28 on 2025-04-29 13:24_

Nit: `Self` instead of `FetchUrl`

---

_Review comment by @charliermarsh on `crates/uv-auth/src/index.rs`:64 on 2025-04-29 13:25_

Nit: Rustdoc please with an example of when this returns `true` vs. `false`.

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:521 on 2025-04-29 13:36_

I'm still struggling a bit. Why do we break when (e.g.) we hit a 401, but 401 isn't included in the list of ignored errors? Why is it not something like this?

```diff
diff --git a/crates/uv-client/src/registry_client.rs b/crates/uv-client/src/registry_client.rs
index 8bdf2c065..3cb6b4978 100644
--- a/crates/uv-client/src/registry_client.rs
+++ b/crates/uv-client/src/registry_client.rs
@@ -348,12 +348,6 @@ impl RegistryClient {
                                 }
                                 // Package not found, so we will continue on to the next index (if there is one)
                                 SimpleMetadataSearchOutcome::NotFound => {}
-                                // The search failed because of an HTTP status code that we don't ignore for
-                                // this index. We end our search here.
-                                SimpleMetadataSearchOutcome::StatusCodeFailure(status_code) => {
-                                    debug!("Indexes search failed because of status code failure: {status_code}");
-                                    break;
-                                }
                             }
                         }
                         IndexFormat::Flat => {
@@ -522,17 +516,15 @@ impl RegistryClient {
                     let Some(status_code) = err.status() else {
                         return Err(ErrorKind::WrappedReqwestError(url, err).into());
                     };
-                    let decision =
-                        status_code_strategy.handle_status_code(status_code, index, capabilities);
-                    if let IndexStatusCodeDecision::Fail(status_code) = decision {
-                        if !matches!(
-                            status_code,
-                            StatusCode::UNAUTHORIZED | StatusCode::FORBIDDEN
-                        ) {
-                            return Err(ErrorKind::WrappedReqwestError(url, err).into());
+                    match status_code_strategy.handle_status_code(status_code, index, capabilities)
+                    {
+                        IndexStatusCodeDecision::Ignore => {
+                            Ok(SimpleMetadataSearchOutcome::NotFound)
+                        }
+                        IndexStatusCodeDecision::Fail(..) => {
+                            Err(ErrorKind::WrappedReqwestError(url, err).into())
                         }
                     }
-                    Ok(SimpleMetadataSearchOutcome::from(decision))
                 }
 
                 // The package is unavailable due to a lack of connectivity.
@@ -998,18 +990,6 @@ pub(crate) enum SimpleMetadataSearchOutcome {
     Found(OwnedArchive<SimpleMetadata>),
     /// Simple metadata was not found
     NotFound,
-    /// A status code failure was encountered when searching for
-    /// simple metadata and our strategy did not ignore it
-    StatusCodeFailure(StatusCode),
-}
-
-impl From<IndexStatusCodeDecision> for SimpleMetadataSearchOutcome {
-    fn from(item: IndexStatusCodeDecision) -> Self {
-        match item {
-            IndexStatusCodeDecision::Ignore => Self::NotFound,
-            IndexStatusCodeDecision::Fail(status_code) => Self::StatusCodeFailure(status_code),
-        }
-    }
 }
 
 /// A map from [`IndexUrl`] to [`FlatIndexEntry`] entries found at the given URL, indexed by
```

---

_@charliermarsh reviewed on 2025-04-29 13:37_

---

_Review comment by @jtfmumm on `crates/uv-client/src/registry_client.rs`:521 on 2025-04-29 13:50_

Because if we do it that way, we lose the richer error messages from the resolver. I actually originally did it the way you suggest, but @zanieb pointed out that there were regressions in the amount of information the user was getting. 

---

_@jtfmumm reviewed on 2025-04-29 13:50_

---

_@jtfmumm reviewed on 2025-04-29 13:51_

---

_Review comment by @jtfmumm on `crates/uv-client/src/registry_client.rs`:521 on 2025-04-29 13:51_

This is sort of like the original behavior of returning `Ok(None)` except that it also stops the search from going to the next index.

---

_@jtfmumm reviewed on 2025-04-29 13:54_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/index.rs`:64 on 2025-04-29 13:54_

Because of the timing of the release, can we defer this to a PR afterwards? I just moved this logic from a function to a method in this PR.

---

_@jtfmumm reviewed on 2025-04-29 13:57_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/cache.rs`:19 on 2025-04-29 13:57_

Added

---

_@jtfmumm reviewed on 2025-04-29 13:58_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/cache.rs`:28 on 2025-04-29 13:58_

Updated

---

_@charliermarsh approved on 2025-04-29 15:39_

---

_@charliermarsh reviewed on 2025-04-29 15:40_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:352 on 2025-04-29 15:40_

I think this should mention the assumption that the status code failure will be tracked by `IndexCapabilities` and surfaced via hints.

---

_@charliermarsh reviewed on 2025-04-29 15:41_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:530 on 2025-04-29 15:41_

I think this should mention that these specific codes are tracked via `IndexCapabilities` and surfaced via hints; otherwise, it's not really clear why these _specific_ codes are ignored.

---

_@charliermarsh reviewed on 2025-04-29 15:42_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:1003 on 2025-04-29 15:42_

I think I would find it clearer if we actually just made individual enum variants for Unauthorized and Forbidden, rather than allowing an arbitrary status code here. That way the expectations are encoded in the type system. We should make the invalid state (e.g., `StatusCodeFailure(400)`) unrepresentable.

---

_Comment by @zanieb on 2025-04-29 17:48_

I'm going to merge and kick the tires on the release.

---

_Merged by @zanieb on 2025-04-29 17:48_

---

_Closed by @zanieb on 2025-04-29 17:48_

---

_Branch deleted on 2025-04-29 17:48_

---

_Comment by @juhaszp95 on 2025-04-30 08:22_

Hi There,

Just a quick question on the above - I have an internal package index which also returns 403 for missing packages. Is it possible to make the `ignore-error-codes` a user-wide setting by somehow incorporating it in `uv.toml`? I.e. to declare custom exceptions user-wide.

Furthermore, it seems to me that the workaround in `pyproject.toml` only works if I include the authentication token as well in the url, which is obviously undesirable.

---

_Comment by @jtfmumm on 2025-04-30 08:46_

> Hi There,
> 
> Just a quick question on the above - I have an internal package index which also returns 403 for missing packages. Is it possible to make the `ignore-error-codes` a user-wide setting by somehow incorporating it in `uv.toml`? I.e. to declare custom exceptions user-wide.
> 
> Furthermore, it seems to me that the workaround in `pyproject.toml` only works if I include the authentication token as well in the url, which is obviously undesirable.

You shouldn't need to include a token in the URL (and I agree you shouldn't include one in `pyproject.toml`). Did you run into an issue when excluding it?

---

_Comment by @juhaszp95 on 2025-04-30 09:12_

Yes - interestingly, I get the error `An index URL (https://myorg.jfrog.io/artifactory/api/pypi/pypi/simple) could not be queried due to a lack of valid authentication credentials (403 Forbidden).`, even though I set `ignore-error-codes = [401, 403]`. This index is set as an `extra-index-url` in `uv.toml`. This doesn't seem to be an issue for the other internal index I use, which is set as the `index-url` in `uv.toml`.

---

_Comment by @jtfmumm on 2025-04-30 09:23_

Ok, would you mind opening an issue and including a reproduction and trace (`-vv`) logs? I can investigate this.

---

_Comment by @juhaszp95 on 2025-04-30 09:25_

Should I put it [here](https://github.com/astral-sh/uv/issues/13216) as it seems closely related, or would you rather a separate issue?

---

_Comment by @jtfmumm on 2025-04-30 09:28_

I think a separate issue for now would be helpful. It's not clear to me these are the same problem.

---

_Comment by @juhaszp95 on 2025-04-30 10:14_

I've realised what the problem is just now (and it's sort of OK): as I was debugging, I left the `url` in the format `USER@ORG@repository-url` which works for `extra-index-url` but strangely does not for `index-url`. When I removed the `USER@ORG` part it worked again, so that's all solved I believe! I have no idea why it behaves differently for `index-url` and `extra-index-url`, but at least it works for both when there aren't really any credentials there (i.e. not just the token deleted, but everything). Also, interestingly, I need to put the `index-url` in `pyproject.toml` (without any `ignore-error-codes`) for it to work - strange. It's like as if the `extra-index-url` is treated differently than the `index-url`, or perhaps if indexes are declared in `pyproject.toml`, the contents of `uv.toml` are ignored, hence the need to declare the index in `pyproject.toml` as well.

This still leaves the outstanding question whether all this config could be saved to a `uv.toml` for user-level configuration so I don't need to do this at every single project.

---

_Comment by @jtfmumm on 2025-04-30 11:16_

Is this problem unique to 0.7.0 or do you also see it on 0.6.17? The `ignore-error-codes` feature doesn't exist on 0.6.17, but I'm trying to understand if something broke with 0.7.0 or if this is just an enhancement you'd like to see to the new error code behavior. Or if the main concern is the change that we are no longer ignoring 403s by default (and you can't find a good way to configure uv to ignore them for your situation).

---

_Comment by @juhaszp95 on 2025-04-30 11:56_

The problem and enhancement request is both unique to 0.7.0. Before, I had all index and authentication info saved in `uv.toml` with no issues. Now, due to how 0.7.0 handles error codes, I have to copy ignores to all projects as well. It'd be good if the ignores could be configured centrally rather than at a project level (for my use-case). I've checked 0.7.1 doesn't solve this (as expected).

---
