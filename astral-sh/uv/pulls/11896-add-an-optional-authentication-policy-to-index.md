```yaml
number: 11896
title: "Add an optional authentication policy to [index] configuration"
type: pull_request
state: merged
author: jtfmumm
labels: []
assignees: []
merged: true
base: main
head: jtfm/auth-mode
created_at: 2025-03-02T18:55:35Z
updated_at: 2025-03-10T17:24:27Z
url: https://github.com/astral-sh/uv/pull/11896
synced_at: 2026-01-12T16:10:02Z
```

# Add an optional authentication policy to [index] configuration

---

_@jtfmumm_

Adds a new optional key `auth-policy` to `[tool.uv.index]` that sets the authentication policy for the index URL. 

The default is `"auto"`, which attempts to authenticate when necessary. `"always"` always attempts to authenticate and fails if the endpoint is unauthenticated. `"never"` never attempts to authenticate.

These policy address two kinds of cases:
* Some indexes don’t fail on unauthenticated requests; instead they just forward to the public PyPI. This can leave the user confused as to why their package is missing. The "always" policy prevents this.
* "never" allows users to ensure their credentials couldn't be leaked to an unexpected index, though it will only allow for successful requests on an index that doesn't require credentials.

Closes #11600 


---

_Comment by @jtfmumm on 2025-03-03 13:59_

@zanieb One question @konstin brought up: should we be popping the "/simple" fragment of the URL (if found) as you currently do with `Index::root_url`?

---

_Review requested from @zanieb by @jtfmumm on 2025-03-03 14:08_

---

_Review comment by @charliermarsh on `crates/uv/src/settings.rs`:2476 on 2025-03-03 17:37_

In general (not a hard rule) I have a mild preference for `From` because it makes the types more obvious. For example, as a reader here, I actually don't know what type this is being converted into? Whereas `UrlAuthPolicies::from` tells you that immediately.

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:81 on 2025-03-03 17:37_

Nit: "an an"

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build_frontend.rs`:525 on 2025-03-03 17:39_

Nit: unnecessary parens here.

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index_url.rs`:399 on 2025-03-03 17:42_

There are a few unnecessary allocations here. You're cloning all the data here, and then collecting it into a `Vec`, but then in `UrlAuthPolicies::from_tuples`, you're again cloning all the data because it takes a slice.

Compare to this:

```diff
diff --git a/crates/uv-auth/src/middleware.rs b/crates/uv-auth/src/middleware.rs
index c59e20a86..762fde4a8 100644
--- a/crates/uv-auth/src/middleware.rs
+++ b/crates/uv-auth/src/middleware.rs
@@ -70,10 +70,10 @@ impl UrlAuthPolicies {
 
     /// Create a new `UrlAuthPolicies` from a list of URL and `AuthPolicy`
     /// tuples.
-    pub fn from_tuples(tuples: &[(Url, AuthPolicy)]) -> Self {
-        let mut auth_policies = UrlAuthPolicies::new();
+    pub fn from_tuples(tuples: impl IntoIterator<Item = (Url, AuthPolicy)>) -> Self {
+        let mut auth_policies = Self::new();
         for (url, auth_policy) in tuples {
-            auth_policies.add_policy(url.clone(), *auth_policy);
+            auth_policies.add_policy(url, auth_policy);
         }
         auth_policies
     }
diff --git a/crates/uv-distribution-types/src/index_url.rs b/crates/uv-distribution-types/src/index_url.rs
index 639dc84c0..ddf46a921 100644
--- a/crates/uv-distribution-types/src/index_url.rs
+++ b/crates/uv-distribution-types/src/index_url.rs
@@ -393,10 +393,9 @@ impl<'a> IndexLocations {
 impl From<&IndexLocations> for UrlAuthPolicies {
     fn from(index_locations: &IndexLocations) -> UrlAuthPolicies {
         UrlAuthPolicies::from_tuples(
-            &(index_locations
+            index_locations
                 .indexes()
                 .map(|index| (index.url().url().clone(), index.auth_policy.into()))
-                .collect::<Vec<_>>()),
         )
     }
 }
```

Now you're only cloning the URL once (rather than twice), and you can avoid allocating into a `Vec` altogether.

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:76 on 2025-03-03 17:43_

As a rule of thumb, if you're taking unowned data but cloning all of it, I would often suggest taking an owned value. Otherwise, you're kind of hiding the allocations from the caller.

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:86 on 2025-03-03 17:44_

Nit: wrap `AuthPolicy` in brackets? Same for all the other references. That way, editors will enable go-to definition for those references.

![Screenshot 2025-03-03 at 12 44 12 PM](https://github.com/user-attachments/assets/b72b308a-4536-4b45-9661-dc15107404e7)
![Screenshot 2025-03-03 at 12 44 22 PM](https://github.com/user-attachments/assets/a311237c-b4ef-4135-8849-f871b0dc0fc9)


---

_Review comment by @charliermarsh on `crates/uv/src/settings.rs`:2486 on 2025-03-03 17:45_

Why store this on the settings type at all, rather than create it from `index_locations` wherever we initialize the network client?

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:88 on 2025-03-03 17:46_

We tend to use the `TODO(name)` style in uv, where `name` indicates the author (i.e., the individual with context on the TODO -- not necessarily the individual response for resolving it).

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index.rs`:87 on 2025-03-03 17:46_

Nit: "URL" rather than "url". (This is also user-facing documentation.)

---

_@charliermarsh reviewed on 2025-03-03 17:46_

---

_Comment by @charliermarsh on 2025-03-03 17:47_

(My review only considered the implementation. I defer to @zanieb on the user-facing API.)

---

_@jtfmumm reviewed on 2025-03-03 18:39_

---

_Review comment by @jtfmumm on `crates/uv/src/settings.rs`:2486 on 2025-03-03 18:39_

This was just leftover from different approaches to threading this through. I've updated it.

---

_@zanieb reviewed on 2025-03-03 19:53_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:54 on 2025-03-03 19:53_

We should probably be more detailed here. Like, we try an unauthenticated request and if that fails we'll try an authenticated request?

---

_@zanieb reviewed on 2025-03-03 19:55_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:64 on 2025-03-03 19:55_

Can we remove this entirely in favor of `url_auth_policies`? (could be a follow-up refactor)

---

_@zanieb reviewed on 2025-03-03 19:58_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:66 on 2025-03-03 19:58_

Nit: I'd put this and related in a `uv-auth::policy` module instead of adding it to the `middleware` module (mostly to match the existing level of granularity). Kind of weird the NetrcMode is in here already.



---

_@zanieb reviewed on 2025-03-03 19:58_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:1 on 2025-03-03 19:58_

Can you add cases for this new behavior to the unit tests in this module?

---

_@zanieb reviewed on 2025-03-03 20:01_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:277 on 2025-03-03 20:01_

Above... should we be doing anything in particular when the policy is "Never" but there are credentials attached to the URL already? Right now, it looks like we'll still look for credentials in the cache if there's a username? which seems quite surprising.

---

_@zanieb reviewed on 2025-03-03 20:04_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:281 on 2025-03-03 20:04_

I would prefer if this remained immutable for reasoning purposes. Could we just assign a value conditionally?

---

_@zanieb reviewed on 2025-03-03 20:09_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:327 on 2025-03-03 20:09_

Perhaps I'm missing something, but I'm confused by this signature change. Shouldn't this read `self.url_auth_policies` directly like all the other configuration is handled here?

---

_@zanieb reviewed on 2025-03-03 20:12_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:88 on 2025-03-03 20:12_

I think we'll need to expand on this quite a bit for it to be user-facing. It's not clear when or how to use this.

---

_@zanieb reviewed on 2025-03-03 20:14_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:107 on 2025-03-03 20:14_

We may want to avoid repeating all of this if they're exactly equivalent. I've been hesitant to add `schemars` etc. as a dependency to the `uv-auth` crate, but I think we do that all over the place anyway.

If we _do_ retain these duplicates, the internal `uv_auth` type should refer to this type in the rustdoc so we don't need to maintain documentation in two places.

Also seems like this type might go in `uv-configuration::authentication` if we were going to expose a separate configuration-specific type.

---

_Comment by @zanieb on 2025-03-03 20:15_

> ... should we be popping the "/simple" fragment of the URL (if found) as you currently do with Index::root_url?

I think you'd need to do this, yeah. You can just use `root_url`, right?

---

_Comment by @zanieb on 2025-03-03 20:25_

We should also talk about this in

- https://docs.astral.sh/uv/configuration/authentication/
- and/or https://docs.astral.sh/uv/configuration/indexes/


---

_Comment by @zanieb on 2025-03-03 20:26_

I wonder if we can do something smart if a user has `keyring-provider = subprocess` and we have an `always` authentication mode without a username — this will not successfully use keyring for authentication because we require a username to query its CLI. Perhaps this would just be useful in the "Failed to query an index" error hints.

---

_@zanieb reviewed on 2025-03-03 20:28_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:10335 on 2025-03-03 20:28_

We should probably warn if these are set and `never` is used, right?

---

_@zanieb reviewed on 2025-03-03 20:29_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:10327 on 2025-03-03 20:29_

What about `never` when there are credentials hard-coded in the URL?

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:64 on 2025-03-04 09:02_

`only_authenticated` is at the level of the client as a whole. The auth policies are per URL. I think the whole picture here could be cleaner, but perhaps better as a follow-up.

---

_@jtfmumm reviewed on 2025-03-04 09:02_

---

_@jtfmumm reviewed on 2025-03-04 09:43_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:277 on 2025-03-04 09:43_

I've refactored this method so that we no longer look at the credentials when policy is "Never".

---

_@jtfmumm reviewed on 2025-03-04 09:44_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:327 on 2025-03-04 09:44_

Yes, that was an oversight. Fixed.

---

_Comment by @jtfmumm on 2025-03-04 09:51_

> > ... should we be popping the "/simple" fragment of the URL (if found) as you currently do with Index::root_url?
> 
> I think you'd need to do this, yeah. You can just use `root_url`, right?

We often only have an `IndexUrl` at the point of use. I've moved the `Index::root_url` logic to a new method on `IndexUrl` and called through to that from `Index::root_url`.

---

_@jtfmumm reviewed on 2025-03-04 11:11_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:107 on 2025-03-04 11:11_

I had originally tried that but following the pattern I saw elsewhere, I ended up getting a `the trait `JsonSchema` is not implemented for AuthPolicy` compiler error despite deriving it. However, I ended up abandoning it when investigating further because I thought we might want to keep these `JsonSchema` structs in `uv-configuration`.

However, if you'd prefer, we can look into why that wasn't compiling so we can update.

---

_@jtfmumm reviewed on 2025-03-04 12:01_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:1 on 2025-03-04 12:01_

Added

---

_@zanieb reviewed on 2025-03-04 14:12_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:64 on 2025-03-04 14:12_

Yeah but I don't think we need `only_authenticated` long-term, we can just set an auth policy for the publish URL (which is the one place it's used)

---

_@zanieb reviewed on 2025-03-04 14:17_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:107 on 2025-03-04 14:17_

Oh, weird. Given the types are identical, it sounds like it's worth understanding why the derive wasn't working. I don't feel particularly strongly otherwise.

---

_@charliermarsh reviewed on 2025-03-04 14:38_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index.rs`:107 on 2025-03-04 14:38_

Is it because `schemars` is an optional feature? You may need to enable it wherever you pull in the relevant `uv-*` crate.

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:10214 on 2025-03-04 14:46_

What motivates this user-facing name? I think it's rare for us to have "policy" be user-facing, even if we're using it internally.

My only concern for something short like `auth = "always"` is that we may want to use that key to select a specific authentication method in the future, e.g., `auth = "keyring"` /  `auth = "netrc"` / `auth = "env"`? I guess that'd be equivalent to "always" with a specific source? Do we need separate options for those?

---

_@zanieb reviewed on 2025-03-04 14:46_

---

_@jtfmumm reviewed on 2025-03-06 15:10_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:107 on 2025-03-06 15:10_

Yeah that was it. Updated

---

_@jtfmumm reviewed on 2025-03-07 10:54_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/edit.rs`:10214 on 2025-03-07 10:54_

We've decided to go with `authentication`.

---

_@zanieb reviewed on 2025-03-07 14:20_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:10214 on 2025-03-07 14:20_

vs `authenticate`? (which is what I sent in Discord)

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-07 16:42_

Could we alias `auth` here, or did we explicitly decide against that?

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index.rs`:144 on 2025-03-07 16:42_

These could feasibly use `AuthPolicy::default()`.

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index_url.rs`:422 on 2025-03-07 16:42_

I wonder if this should be done via `CanonicalUrl`...? That would handle other forms of normalization, but then we'd probably also need to go through `CanonicalUrl` when matching against the policies later on.

---

_@charliermarsh approved on 2025-03-07 16:44_

Looks good to me! Defer to @zanieb on the docs.

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-07 16:47_

We only decided on using `authentication` as the field. I'm ok with aliasing `auth` if @zanieb is.

---

_@jtfmumm reviewed on 2025-03-07 16:47_

---

_@jtfmumm reviewed on 2025-03-07 16:49_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:144 on 2025-03-07 16:49_

Yeah I like that better. Updated

---

_@zanieb reviewed on 2025-03-07 16:52_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-07 16:52_

I had suggested `authenticate = always | never | auto` leaving room for another option in the future that would specify a specific authentication provider, e.g., `keyring | netrc | env`. 

It looks like https://github.com/astral-sh/uv/pull/11896#discussion_r1985138948 got resolved but I'm not sure we're entirely settled on a name.

`authentication = always | never | auto` doesn't read clearly to me. I think it's ~equivalent to `auth` though (and it seems good to allow an alias if we support it).

On that note, we can do `auth = always | never | auto` if we think `keyring | netrc | env` would be compatible options in the future? Which I think would be equivalent to `always` but with a specific provider. We could also split into `auth-policy` and `auth-provider`. I just don't love `auth-policy` as a user-facing name.

---

_Comment by @zanieb on 2025-03-07 16:57_

@konstin we actually have an interesting opportunity here to fix some of the keyring behavior! If the authentication mode is `always` and there's not a username, we might as well _try_ the new keyring interface.

---

_@zanieb reviewed on 2025-03-07 16:59_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-07 16:59_

(that John and I got mixed up on `authenticate` might be a sign it's not a good option)

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index_url.rs`:422 on 2025-03-07 17:16_

Just as a note, in the `UrlTrie` we use for credentials, we just iterate on `path_segments`.

---

_@zanieb reviewed on 2025-03-07 17:16_

---

_@zanieb reviewed on 2025-03-07 17:18_

---

_Review comment by @zanieb on `crates/uv-auth/src/policy.rs`:47 on 2025-03-07 17:18_

In case you didn't see it, we do have a `UrlTrie` implementation — it'd just be making the inner type generic? kind of interesting but not worried about the performance.

---

_@zanieb reviewed on 2025-03-07 18:34_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-07 18:34_

Okay from our extensive chat in Discord... my take-away is:

- An `auth-` prefix is the most forward-looking, so we can group authentication related settings
- `auth-required = true | false | auto` is the most user-friendly option I can think of

---

_@jtfmumm reviewed on 2025-03-08 10:17_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-08 10:17_

I think `auth-required` won't work for the `never` case since you can be permitted to do something that's not required.

What about `auth-condition = always | never | fallback` (fallback is default)?

---

_@zanieb reviewed on 2025-03-08 14:28_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-08 14:28_

I think `condition` suffers from the same problem as many of the other flags, in that the name does not make it clear what it does.

 > since you can be permitted to do something that's not required.
 
 I think it'd be fine to say if `auth-required = false` and there are credentials then we can error and say "You tried to provide credentials to an index that does not require authentication".

I agree this is the most awkward part of `auth-required`, but I don't think `false` is the critical or common use-case.

---

_@charliermarsh reviewed on 2025-03-08 14:37_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-08 14:37_

Personally I prefer `auth-policy` over `auth-condition` or `auth-required`.

---

_@jtfmumm reviewed on 2025-03-08 18:13_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/policy.rs`:47 on 2025-03-08 18:13_

Yeah, I noticed that. Could be worth looking at but for now I think the simpler approach is fine.

---

_@jtfmumm reviewed on 2025-03-08 18:14_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-08 18:14_

That's what I had originally. I'm happy to revert to that. @zanieb thoughts?

---

_@jtfmumm reviewed on 2025-03-08 18:22_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-08 18:22_

I made the change, but it's straightforward to change to something else if need be.

---

_@zanieb reviewed on 2025-03-08 18:48_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-08 18:48_

I'm opposed to `auth-policy` as it is not self-explanatory. I'm left wondering what the policy refers to. I don't think it's clearly related to requiring credentials upfront / searching for credentials / or never searching for credentials. I also think we don't currently use "policy" elsewhere in user-facing configuration.

@charliermarsh can you say why you think it's preferable to `auth-required`?

---

_@jtfmumm reviewed on 2025-03-08 18:54_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-08 18:54_

I thought we wanted to provide a way to ensure credentials weren't leaked (the "never" case). I think we're dropping that case if we go with `auth-required`. We can delay merging this if there's a larger conversation about what functionality we really want to support.

---

_@charliermarsh reviewed on 2025-03-08 19:26_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-08 19:26_

I'm not really worried about that because the values are so self-explanatory? Like I find `auth-policy = "always"` and `auth-policy = "never"` to be very clear, whereas `auth-required = false` (assuming that's equivalent to `auth-policy = "never"`) is not obvious. To me, that just means "auth isn't required", not "we should never attempt to authenticate the index".

I'm also fine with `authentication` or `authenticate`. In fact that might be my preference? I _do_ think we can make `keyring` etc. compatible with that.

---

_@zanieb reviewed on 2025-03-08 20:28_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-08 20:28_

> I thought we wanted to provide a way to ensure credentials weren't leaked (the "never" case). I think we're dropping that case if we go with auth-required. 

I don't see why we wouldn't be able to error if someone tries to provide to credentials to an index that "does not require authentication". I'll admit, that's a weakness of the proposal though. I think optimizing for the "true" case is more important to me.

> I find auth-policy = "always" and auth-policy = "never" to be very clear

I'm not sure where the mismatch is, but I don't find it clear at all. "always" what? Perhaps you read that as "always authenticate" but I think "auth" is more intuitively short for "authentication" and "always authentication" reads weird to me? 

My only qualm with `authenticate` is I'm not sure how it fits with other options in the future?

```
authenticate = always | never | auto
authentication-providers = [keyring, netrc, ...]
authentication-method = basic | ...
```

But `authentication = always | never | auto` feels awkward to me there too. That's why I'm reaching for something else that fits in the group.

Something like `authentication-required` makes sense to me in a context where if we don't find it any of the providers, we should fail.

I think we're reaching a point where we're going back and forth on this too much. It doesn't feel like we're moving towards consensus. (which is not to say that we should just go with what I'm saying, but that we're reaching the point where we should make a decision without full consensus)

---

_@zanieb reviewed on 2025-03-08 21:14_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-08 21:14_

If nobody is into the `-required` variant and Charlie prefers `authenticate`, my inclination is to just go with that (as was my original intuition) and we can deal with the future interface if/when that becomes a problem — there's just too much unknown there.

---

_@jtfmumm reviewed on 2025-03-09 10:05_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:106 on 2025-03-09 10:05_

I'm happy with that. I've made the change.

---

_@zanieb approved on 2025-03-10 17:19_

Thanks for your patience on all the back and forth here!

---

_Merged by @zanieb on 2025-03-10 17:24_

---

_Closed by @zanieb on 2025-03-10 17:24_

---

_Branch deleted on 2025-03-10 17:24_

---
