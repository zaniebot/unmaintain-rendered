```yaml
number: 2976
title: "Rewrite `uv-auth`"
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: main
head: zb/refactor-auth-store
created_at: 2024-04-10T19:53:00Z
updated_at: 2024-04-18T14:29:53Z
url: https://github.com/astral-sh/uv/pull/2976
synced_at: 2026-01-12T16:05:20Z
```

# Rewrite `uv-auth`

---

_@zanieb_

Closes 

- #2822 
- https://github.com/astral-sh/uv/issues/2563 (via #2984)

Partially address:

- https://github.com/astral-sh/uv/issues/2465
- https://github.com/astral-sh/uv/issues/2464

Supersedes:

- https://github.com/astral-sh/uv/pull/2947
- https://github.com/astral-sh/uv/pull/2570 (via #2984)

Some significant refactors to the whole `uv-auth` crate:

- Improving the API
- Adding test coverage
- Fixing handling of URL-encoded passwords
- Fixing keyring authentication
- Updated middleware (see #2984 for more)

---

_Renamed from "zb/refactor auth store" to "Refactor `uv-auth`" by @zanieb on 2024-04-10 19:53_

---

_Comment by @zanieb on 2024-04-10 19:59_

Lookee this demonstrates the fix to passwords with percent-encoded characters

```diff
diff --git a/crates/uv-auth/src/credentials.rs b/crates/uv-auth/src/credentials.rs
index 9c222cff..dd1c8af8 100644
--- a/crates/uv-auth/src/credentials.rs
+++ b/crates/uv-auth/src/credentials.rs
@@ -37,11 +37,7 @@ impl Credentials {
             username: urlencoding::decode(url.username())
                 .expect("An encoded username should always decode")
                 .into_owned(),
-            password: url.password().map(|password| {
-                urlencoding::decode(password)
-                    .expect("An encoded password should always decode")
-                    .into_owned()
-            }),
+            password: url.password().map(|password| password.to_string()),
         })
     }
 }
@@ -191,7 +187,7 @@ mod test {
             .clone();
         header.set_sensitive(false);
 
-        assert_debug_snapshot!(header, @r###""Basic dXNlcjpwYXNzd29yZD09""###);
-        assert_snapshot!(decode_basic_auth(header), @"Basic: user:password==");
+        assert_debug_snapshot!(header, @r###""Basic dXNlcjpwYXNzd29yZCUzRCUzRA==""###);
+        assert_snapshot!(decode_basic_auth(header), @"Basic: user:password%3D%3D");
     }
 }
```

---

_Review requested from @charliermarsh by @zanieb on 2024-04-10 20:00_

---

_@zanieb reviewed on 2024-04-10 20:00_

---

_Review comment by @zanieb on `crates/uv-auth/src/store.rs`:13 on 2024-04-10 20:00_

Moved to `credentials.rs`, no longer an enum.

---

_@zanieb reviewed on 2024-04-10 20:01_

---

_Review comment by @zanieb on `crates/uv-auth/src/store.rs`:39 on 2024-04-10 20:01_

Instead of using `set` for indicating attempted fetches we introduce a separate API for that.

---

_@zanieb reviewed on 2024-04-10 20:01_

---

_Review comment by @zanieb on `crates/uv-auth/src/store.rs`:28 on 2024-04-10 20:01_

Adds borrowing of credentials instead of cloning.

Avoids the nested `Option<Option<...>>` use `should_attempt_fetch` instead.

---

_@zanieb reviewed on 2024-04-10 20:02_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:148 on 2024-04-10 20:02_

Bothered me that this crept out of the crate

---

_Comment by @zanieb on 2024-04-10 20:06_

Intentionally leaving this unlabeled because we need a changelog entry for the user-facing fix

---

_Comment by @zanieb on 2024-04-10 20:14_

```
❯ packse index build scenarios/example.json --no-hash
❯ npx http-server --username user --password foo== ./index
❯ cargo run -q -- pip install --extra-index-url http://user:foo==@127.0.0.1:8080/simple-html example-a --reinstall --no-cache
Resolved 2 packages in 60ms
Downloaded 2 packages in 4ms
Installed 2 packages in 3ms
 - example-a==1.0.0
 + example-a==1.0.0
 - example-b==2.0.0
 + example-b==2.0.0
❯ gcm
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
❯ cargo run -q -- pip install --extra-index-url http://user:foo==@127.0.0.1:8080/simple-html example-a --reinstall --no-cache
error: Failed to download: example-a==1.0.0
  Caused by: HTTP status client error (401 Unauthorized) for url (http://127.0.0.1:8080/files/example_a-1.0.0-py3-none-any.whl#sha256=05f52f5cb7b5a677b4810004ec487f6420fbee6a368038cf8cf8384de5be1939)
```

---

_Comment by @charliermarsh on 2024-04-10 21:55_

Sweet, I will review tonight.

---

_@charliermarsh reviewed on 2024-04-10 21:59_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:159 on 2024-04-10 21:59_

I thought that request headers was a multi-map, but it looks like the docs say this _will_ override (which we _need_).

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:159 on 2024-04-10 22:00_

Ah okay, `append` would append, but `insert` replaces, good.

---

_@charliermarsh reviewed on 2024-04-10 22:00_

---

_@charliermarsh reviewed on 2024-04-10 22:02_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/store.rs`:31 on 2024-04-10 22:02_

Can you use `let fetched = store.get(&netloc)?` here instead of manual `if`-`else`?

---

_Review comment by @charliermarsh on `crates/uv-auth/src/store.rs`:76 on 2024-04-10 22:03_

I'm having trouble following this. If `get` returns `None`, we insert `None`?

---

_@charliermarsh reviewed on 2024-04-10 22:03_

---

_@charliermarsh reviewed on 2024-04-10 22:04_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/store.rs`:76 on 2024-04-10 22:04_

Is that because missing credentials would return `Some(None)`?

I'm wondering if we should use a custom enum here for missing credentials, instead of `Option<Credentials>`.

---

_@charliermarsh reviewed on 2024-04-10 22:06_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:66 on 2024-04-10 22:06_

This logging is gone, though perhaps that's ok.

---

_@charliermarsh reviewed on 2024-04-10 22:06_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:56 on 2024-04-10 22:06_

Kind of wonder if these should all be `trace` at some point.

---

_@charliermarsh reviewed on 2024-04-10 22:08_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:58 on 2024-04-10 22:08_

I think one minor downside here is we now do two hash lookups per unauthenticated request. I wonder if returning an enum from `.get` would make this simpler? Like three cases: we have credentials, the credentials are unset, there are no credetials.

---

_@zanieb reviewed on 2024-04-10 23:49_

---

_Review comment by @zanieb on `crates/uv-auth/src/credentials.rs`:159 on 2024-04-10 23:49_

We actually skip if there's an AUTHORIZATION header (in the middleware) so this isn't critical, but yeah.

---

_@zanieb reviewed on 2024-04-10 23:50_

---

_Review comment by @zanieb on `crates/uv-auth/src/store.rs`:31 on 2024-04-10 23:50_

Yeah TIL you can `?` on `Option` lmao

---

_@zanieb reviewed on 2024-04-10 23:51_

---

_Review comment by @zanieb on `crates/uv-auth/src/store.rs`:76 on 2024-04-10 23:51_

Yes the latter `Some(None)`. An enum sounds nice although I might want to tackle that separately. I've already moved all the nested option logic _inside_ this struct in this pull request rather than having it be a part of its public API.

---

_@zanieb reviewed on 2024-04-10 23:52_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:66 on 2024-04-10 23:52_

Intentional, I found it too noisy. I'll be improving logging further in subsequent pull requests.

---

_@zanieb reviewed on 2024-04-10 23:52_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:56 on 2024-04-10 23:52_

Yeah that's a fair point.

---

_@zanieb reviewed on 2024-04-10 23:53_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:58 on 2024-04-10 23:53_

Yeah I considered this — an enum does seem helpful for that. Are we really concerned about that being a performance bottleneck though? I guess the mutex makes it even slower?

---

_@charliermarsh reviewed on 2024-04-10 23:57_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/store.rs`:31 on 2024-04-10 23:57_

Welcome to the `Option` **monad**

---

_@zanieb reviewed on 2024-04-10 23:57_

---

_Review comment by @zanieb on `crates/uv-auth/src/store.rs`:76 on 2024-04-10 23:57_

I can stack it on top of this before merging though.

---

_@charliermarsh reviewed on 2024-04-11 00:04_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:58 on 2024-04-11 00:04_

I don't know if the perf matters, probably not, but I do find myself thinking that if we're querying the data source twice for the same information, something is not quite right in the data model.

---

_@zanieb reviewed on 2024-04-11 00:25_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:58 on 2024-04-11 00:25_

I'll switch to an enum it makes sense to me

---

_@charliermarsh approved on 2024-04-11 00:28_

The overall ideas make sense to me, I defer to you on which feedback to address specifically prior to merging.

---

_Review comment by @charliermarsh on `crates/uv-auth/src/keyring.rs`:49 on 2024-04-13 22:58_

Nit: `URL` for consistency (sorry, I know annoying, but this could be user-facing)

---

_Review comment by @charliermarsh on `crates/uv-auth/src/keyring.rs`:59 on 2024-04-13 22:59_

This can be `debug!("Checking keyring for credentials for `{url}`");` (no need for `.to_string()` since `Url` implements `Display`; can inline directly into the `debug!` format).

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:58 on 2024-04-13 23:15_

I still think the double fetch here represents a deficiency in the data model but perhaps it's addressed in the next PR.

---

_@charliermarsh approved on 2024-04-13 23:15_

---

_@zanieb reviewed on 2024-04-14 15:47_

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:59 on 2024-04-14 15:47_

The display for `Url` is not in the same format as the string, the string is better for a user-facing message.

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:49 on 2024-04-14 15:48_

Np that's fine will fix

---

_@zanieb reviewed on 2024-04-14 15:48_

---

_@charliermarsh reviewed on 2024-04-14 15:51_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/keyring.rs`:59 on 2024-04-14 15:51_

Sorry, what’s the difference?

---

_@charliermarsh reviewed on 2024-04-14 15:54_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/keyring.rs`:59 on 2024-04-14 15:54_

`to_string` is just a wrapper around `Display`:

```rust
impl<T: fmt::Display + ?Sized> ToString for T {
    // A common guideline is to not inline generic functions. However,
    // removing `#[inline]` from this method causes non-negligible regressions.
    // See <https://github.com/rust-lang/rust/pull/74852>, the last attempt
    // to try to remove it.
    #[inline]
    default fn to_string(&self) -> String {
        let mut buf = String::new();
        let mut formatter = core::fmt::Formatter::new(&mut buf);
        // Bypass format_args!() to avoid write_str with zero-length strs
        fmt::Display::fmt(self, &mut formatter)
            .expect("a Display implementation returned an error unexpectedly");
        buf
    }
}
```

Could you be confusing `Display` and `Debug`?

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:59 on 2024-04-14 16:09_

Um... weird. I swear I was getting different output. Let me check back on that.

---

_@zanieb reviewed on 2024-04-14 16:09_

---

_@charliermarsh reviewed on 2024-04-14 16:15_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/keyring.rs`:59 on 2024-04-14 16:15_

(It's possible that types can override this behavior and maybe it _is_ different for URL, I just assumed it wasn't.)

---

_@konstin reviewed on 2024-04-15 08:49_

---

_Review comment by @konstin on `crates/uv-auth/src/keyring.rs`:74 on 2024-04-15 08:49_

```suggestion
    #[instrument]
    fn fetch_subprocess(&self, url: &Url) -> Result<Option<String>, Error> {
```

To track where we're losing 80ms

```
$ hyperfine -i "keyring get https://pypi.org konstin"
Benchmark 1: keyring get https://pypi.org konstin
  Time (mean ± σ):      80.8 ms ±   5.7 ms    [User: 66.2 ms, System: 11.7 ms]
  Range (min … max):    73.8 ms …  96.2 ms    30 runs
 
  Warning: Ignoring non-zero exit code.
```

---

_@vlad-ivanov-name reviewed on 2024-04-15 10:56_

---

_Review comment by @vlad-ivanov-name on `crates/uv-auth/src/keyring.rs`:75 on 2024-04-15 10:56_

since this is being refactored now anyway -- would it be possible to make this adjustable?

```suggestion
        let keyring_command = std::env::var("UV_KEYRING_COMMAND").unwrap_or("keyring");
        let output = match Command::new(keyring_command)
```

---

_@zanieb reviewed on 2024-04-15 16:18_

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:59 on 2024-04-15 16:18_

I think I just got confused by some of the reqwest internal logs which look like `("http", 127.0.0.1:53514)`

---

_@zanieb reviewed on 2024-04-15 21:11_

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:75 on 2024-04-15 21:11_

I'd rather consider this separately, sorry. This pull request is already huge.

---

_@BakerNet reviewed on 2024-04-15 22:58_

---

_Review comment by @BakerNet on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-15 22:58_

This is technically a breaking change - since before, it would attempt keyring with a placeholder username for keyring backends which don't need a username (notably [Google Artifact Registry](https://pypi.org/project/keyrings.google-artifactregistry-auth/) which my company uses).

In the initial/current implementation, it would use the same placeholder username that Google Artifact Registry uses internally (`oauth2accesstoken` see [here](https://github.com/GoogleCloudPlatform/artifact-registry-python-tools/blob/1589e75c0879fb63d376f566971b82635717fc33/keyrings/gauth.py#L69)).  Annoyingly, the keyring CLI command requires a username is provided, even though some backends don't even check the username at all (in linked Google code, you can see username is never read).

[Here](https://github.com/pypa/pip/blob/ae5fff36b0aad6e5e0037884927eaa29163c0611/src/pip/_internal/network/auth.py#L86 ), you can see pip attempting keyring even when username is `None` when trying import strategy - which is first attempt by default for pip.

This change would be an annoyance for our use case, as we'd have to add usernames to all  of our index urls in all of our projects.  We could deal with it, of course, but would be nice not to

---

_@zanieb reviewed on 2024-04-15 23:26_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-15 23:26_

Thanks for giving the pull request a look!

I'm pretty uncomfortable using a hard-coded default username for all queries. It doesn't make sense to me to default to a specific username. I'd be willing to consider adding a workaround like passing `null` to keyring but not using it in the credentials or perhaps providing the Google placeholder if we detect a Google Artifact Registry (via [this logic](https://github.com/GoogleCloudPlatform/artifact-registry-python-tools/blob/1589e75c0879fb63d376f566971b82635717fc33/keyrings/gauth.py#L39-L40)).

pip reading the keyring with a `None` username there is valid (and seems like a separate use case) because the `get_credentials` API is designed to return both a username and password. I don't know why this API is not available via the CLI. cc @jaraco ?

---

_@zanieb reviewed on 2024-04-16 00:07_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-16 00:07_

Here's a change where we send an arbitrary username but don't populate the URL with it... https://github.com/astral-sh/uv/pull/3048 — this seems more correct than `main` but does not match pip's subprocess keyring provider afaict

---

_Review comment by @BakerNet on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-16 01:16_

> I'm pretty uncomfortable using a hard-coded default username for all queries.

Yeah, totally fair

> perhaps providing the Google placeholder if we detect a Google Artifact Registry (via [this logic](https://github.com/GoogleCloudPlatform/artifact-registry-python-tools/blob/1589e75c0879fb63d376f566971b82635717fc33/keyrings/gauth.py#L39-L40)).

Selfishly, I'd like this but also understand if not

---

_@BakerNet reviewed on 2024-04-16 01:16_

---

_@zanieb reviewed on 2024-04-16 02:24_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-16 02:24_

Does #3048 work for you though? Or do they require that username to be in the request?

---

_Comment by @jenshnielsen on 2024-04-16 12:07_

@zanieb Did a quick test and can confirm that this works against Azure artifacts

---

_Renamed from "Refactor `uv-auth`" to "Rewrite `uv-auth`" by @zanieb on 2024-04-16 13:29_

---

_@jaraco reviewed on 2024-04-16 13:59_

---

_Review comment by @jaraco on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-16 13:59_

> pip reading the keyring with a `None` username there is valid (and seems like a separate use case) because the `get_credentials` API is designed to return both a username and password. I don't know why this API is not available via the CLI. cc @jaraco ?

The use case that demanded `get_credential` (https://github.com/jaraco/keyring/issues/350) was API-only and it wasn't obvious if there should be a CLI interface to it, especially because `get_credential` was returning something requiring more structure than the other calls, something for which there was no obvious right answer. It quite possible I decided to defer the possibility of a CLI option until someone requested it. Reading up on that issue, it's sparse on details, in part because we had a lot of the discussion in person and didn't capture it, so my recollection is probably the best you'll get. We're always open to suggestions and pull requests.

---

_@zanieb reviewed on 2024-04-16 14:02_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-16 14:02_

Thanks for the link and context!

---

_@BakerNet reviewed on 2024-04-16 16:32_

---

_Review comment by @BakerNet on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-16 16:32_

> Does #3048 work for you though? Or do they require that username to be in the request?

It does not - it appears that Google Artifact Registry does in fact expect to receive `oauth2accesstoken` as the username in the basic auth (not documented anywhere, unfortunately)

---

_@zanieb reviewed on 2024-04-16 16:39_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-16 16:39_

I think you will need to update your URLs then, unfortunately. We can suggest exposing this API in the keyring CLI though so we can support retrieval of credentials without an explicit username.

---

_Label `breaking` added by @zanieb on 2024-04-16 16:39_

---

_@zanieb reviewed on 2024-04-16 16:43_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-16 16:43_

Or we can peek at the URL but I'm hesitant to add logic for specific providers.

---

_Merged by @zanieb on 2024-04-16 16:48_

---

_Closed by @zanieb on 2024-04-16 16:48_

---

_Branch deleted on 2024-04-16 16:48_

---

_@BakerNet reviewed on 2024-04-16 17:46_

---

_Review comment by @BakerNet on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-16 17:46_

@jaraco Made a quick and dirty PoC PR for exposing `get_credential` via cli:  https://github.com/jaraco/keyring/pull/678

> The use case that demanded get_credential (jaraco/keyring#350) was API-only and it wasn't obvious if there should be a CLI interface to it, especially because get_credential was returning something requiring more structure than the other calls, something for which there was no obvious right answer. 

Agreed with this - it would be nice to have it exposed via optional argument to `keyring get` but then the output is not consistent when providing username vs not.  Which is why in my PoC PR it's a different operation

---

_@BakerNet reviewed on 2024-04-16 17:56_

---

_Review comment by @BakerNet on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-16 17:56_

@zanieb would be good to mention in release notes that anyone using keyring subprocess with Google Artifact Registry should update index-urls with `oauth2accesstoken@`

---

_@zanieb reviewed on 2024-04-16 18:58_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:147 on 2024-04-16 18:58_

Yep I'll note this as a breaking change.

---

_Comment by @alelavml3 on 2024-04-18 12:13_

Hi, I'm writing this comment because with this new release we are facing an authentication problems. We use a private `Sonatype Nexus` repository server which uses a private PyPi Repo to store our company private Python packages.
The command for compiling and install are the following:
```bash
export PYPI_INDEX_URL=https://$(NEXUS_USER):$(NEXUS_PWD)@$(NEXUS_SERVER_URL)
uv pip compile pyproject.toml -o requirements.txt --index-url $(PYPI_INDEX_URL)
uv pip sync requirements.txt --index-url $(PYPI_INDEX_URL)
```

The error we get is during `uv pip sync`:
```
error: Failed to download distributions
  Caused by: Failed to fetch wheel: google-cloud-pubsub==2.21.0
  Caused by: HTTP status client error (401 Unauthorized) for url (<NEXUS_SERVER_URL>/packages/google-cloud-pubsub/2.21.0/google_cloud_pubsub-2.21.0-py2.py3-none-any.whl#sha256=fabd19e08faa1f70081b0e5ea003a3c031a1d2c1b798cf1be5ded2dbf1d1dbef)
```
here the package is `google-cloud-pubsub` but it happens with any random package.

How can we fix this issue with this new version? For now, we are using v0.1.32 and everything works.

Thank you in advance for your answers!

@zanieb @jenshnielsen 

---

_Comment by @jenshnielsen on 2024-04-18 13:33_

@alelavml3 I think you should probably create a new issue to track this. That being said I have observed when fetching from Azure  Artifacts that I get a 401 on the first attempt to install a package but retrying to install the same package works as expected. Perhaps your issue is the same? I have not had a chance to create an issue for this just yet

---

_Comment by @zanieb on 2024-04-18 14:29_

@alelavml3 Thanks for the report, a new issue would be greatly appreciated with `RUST_LOG=trace uv pip sync -v ...` logs. Be sure to redact your password.

---
