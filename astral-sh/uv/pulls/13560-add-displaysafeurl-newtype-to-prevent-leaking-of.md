```yaml
number: 13560
title: "Add `DisplaySafeUrl` newtype to prevent leaking of credentials by default"
type: pull_request
state: merged
author: jtfmumm
labels:
  - security
assignees: []
merged: true
base: main
head: jtfm/log-safe-url
created_at: 2025-05-20T19:09:43Z
updated_at: 2025-06-02T22:13:35Z
url: https://github.com/astral-sh/uv/pull/13560
synced_at: 2026-01-12T16:10:44Z
```

# Add `DisplaySafeUrl` newtype to prevent leaking of credentials by default

---

_@jtfmumm_

Prior to this PR, there were numerous places where uv would leak credentials in logs. We had a way to mask credentials by calling methods or a recently-added `redact_url` function, but this was not secure by default. There were a number of other types (like `GitUrl`) that would leak credentials on display. 

This PR adds a `DisplaySafeUrl` newtype to prevent leaking credentials when logging by default. It takes a maximalist approach, replacing the use of `Url` almost everywhere. This includes when first parsing config files, when storing URLs in types like `GitUrl`, and also when storing URLs in types that in practice will never contain credentials (like `DirectorySourceUrl`). The idea is to make it easy for developers to do the right thing and for the compiler to support this (and to minimize ever having to manually convert back and forth). Displaying credentials now requires an active step. Note that despite this maximalist approach, the use of the newtype should be zero cost.

One conspicuous place this PR does not use `DisplaySafeUrl` is in the `uv-auth` crate. That would require new clones since there are calls to `request.url()` that return a `&Url`. One option would have been to make `DisplaySafeUrl` wrap a `Cow`, but this would lead to lifetime annotations all over the codebase. I've created a separate PR based on this one (#13576) that updates `uv-auth` to use `DisplaySafeUrl` with one  new clone. We can discuss the tradeoffs there.

Most of this PR just replaces `Url` with `DisplaySafeUrl`. The core is `uv_redacted/lib.rs`, where the newtype is implemented. To make it easier to review the rest, here are some points of note:

* `DisplaySafeUrl` has a `Display` implementation that masks credentials. Currently, it will still display the username when there is both a username and password. If we think is the wrong choice, it can now be changed in one place.
* `DisplaySafeUrl` has a `remove_credentials()` method and also a `.to_string_with_credentials()` method. This allows us to use it in a variety of scenarios.
* `IndexUrl::redacted()` was renamed to `IndexUrl::removed_credentials()` to make it clearer that we are not masking.
* We convert from a `DisplaySafeUrl` to a `Url` when calling `reqwest` methods like `.get()` and `.head()`.
* We convert from a `DisplaySafeUrl` to a `Url` when creating a `uv_auth::Index`. That is because, as mentioned above, I will be updating the `uv_auth` crate to use this newtype in a separate PR.
* A number of tests (e.g., in `pip_install.rs`) that formerly used filters to mask tokens in the test output no longer need those filters since tokens in URLs are now masked automatically.
* The one place we are still knowingly writing credentials to `pyproject.toml` is when a URL with credentials is passed to `uv add` with `--raw`. Since displaying credentials is no longer automatic, I have added a `to_string_with_credentials()` method to the `Pep508Url` trait. This is used when `--raw` is passed. Adding it to that trait is a bit weird, but it's the simplest way to achieve the goal. I'm open to suggestions on how to improve this, but note that because of the way we're using generic bounds, it's not as simple as just creating a separate trait for that method.




---

_Renamed from "Add `LogSafeUrl` newtype to prevent leaking of credentials" to "Add `LogSafeUrl` newtype to prevent leaking of credentials by default" by @jtfmumm on 2025-05-20 19:09_

---

_Label `security` added by @jtfmumm on 2025-05-20 19:25_

---

_Comment by @jtfmumm on 2025-05-20 19:26_

I continued to use the new `uv-redacted` crate for the newtype, replacing the `redacted_url` method there. I'm not sure that's the best name for the crate anymore. 

---

_@konstin reviewed on 2025-05-26 14:24_

---

_Review comment by @konstin on `crates/uv-redacted/src/lib.rs`:41 on 2025-05-26 14:24_

nit: Maybe some more generic name, such as `RedactingUrl`?

---

_@konstin reviewed on 2025-05-26 14:25_

---

_Review comment by @konstin on `crates/uv-redacted/src/lib.rs`:57 on 2025-05-26 14:25_

This docstring is wrong

---

_@konstin reviewed on 2025-05-26 14:27_

---

_Review comment by @konstin on `crates/uv-redacted/src/lib.rs`:6 on 2025-05-26 14:27_

I would make this more generic, since we're also displaying URLs in error messages and serializing it to files, e.g.:

```suggestion
/// A [`Url`] wrapper that redacts credentials when displaying the URL.
```

---

_@konstin reviewed on 2025-05-26 14:28_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/index_url.rs`:131 on 2025-05-26 14:28_

nit: For consistency: `without_credentials`

---

_@konstin reviewed on 2025-05-26 14:38_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/index_url.rs`:111 on 2025-05-26 14:38_

I find it confusing that `LogSafeUrl` here is both working a URL that has credentials, but redacts them, and as a URL that had its credentials removed.

---

_@jtfmumm reviewed on 2025-05-26 14:41_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:111 on 2025-05-26 14:41_

The idea is that it's a version of `Url` that you can log safely. It's not meant to imply that the credentials are in any other state. I wanted it to effectively be uv's replacement for using `Url` in general.

---

_@jtfmumm reviewed on 2025-05-26 14:46_

---

_Review comment by @jtfmumm on `crates/uv-redacted/src/lib.rs`:41 on 2025-05-26 14:46_

I'm not really sure what the best name is. I want to be careful not to imply that the credentials are in an obfuscated/redacted state internally, which is why I didn't go with `ObfuscatedUrl` initially. 

Your suggestion is better than that because it implies something active. But my worry would still be that it might mislead developers to think that the credentials can't be accessed at all (i.e., via `.username()` or `.password()`). 

---

_@jtfmumm reviewed on 2025-05-26 14:50_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:111 on 2025-05-26 14:50_

But maybe I'm missing part of your point. The `without_credentials()` case is for when you want to display or log the URL without the masked credentials. There are some cases where we want to indicate the credentials were part of the raw URL by showing the masked version (e.g., for debugging requests) and some cases where we just want the URL itself (like in the lockfile).

---

_Review comment by @konstin on `crates/uv/src/commands/project/add.rs`:653 on 2025-05-26 14:54_

Do we still need this?

This question applies to all non-test `remove_credentials` calls: Do we still need explicit redaction, or does automatic redaction handle everything we need now?

---

_Review comment by @konstin on `crates/uv/src/commands/project/run.rs`:1499 on 2025-05-26 15:00_

Is there a specific reason this didn't become a redacted URL?

---

_Review comment by @konstin on `crates/uv/src/commands/publish.rs`:349 on 2025-05-26 15:03_

re publishing, `TrustedPublishingError` should also use redacted URLs

---

_Review comment by @konstin on `crates/uv-git/src/source.rs`:86 on 2025-05-26 15:04_

`credentials.apply` should operate on a redacting URL by default

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:8407 on 2025-05-26 15:08_

The new redactions in action!

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:488 on 2025-05-26 15:10_

```suggestions
        trace!("Fetching metadata for {package_name} from {url}");
```

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:631 on 2025-05-26 15:11_

This method should take a `LogSafeUrl`

---

_Review comment by @konstin on `crates/uv-distribution/src/source/mod.rs`:1472 on 2025-05-26 15:13_

Errors are displayed and should use the redacted URL

---

_Review comment by @konstin on `crates/uv-distribution/src/error.rs`:6 on 2025-05-26 15:13_

nit: import sorting

---

_Review comment by @konstin on `crates/uv-distribution-types/src/file.rs`:193 on 2025-05-26 15:14_

Do we need to apply redactions when displaying `UrlString`s?

---

_Review comment by @konstin on `crates/uv-distribution-types/src/index_url.rs`:111 on 2025-05-26 15:18_

This is something that's already in the existing code: Sometimes `Url` refers to a URL with credentials, and sometimes to the same underlying "data" without credentials. What about returning a `impl Display` instead of a real type from `without_credentials` to make it clearer that this is an output-only method?

---

_Review comment by @konstin on `crates/uv-git/src/credentials.rs`:34 on 2025-05-26 15:20_

```suggestion
        trace!("Caching credentials for {url}");
```

for `println!` clippy catches those, but apparently it can't through tracing's macros. 

---

_Review comment by @konstin on `crates/uv-pep508/src/lib.rs`:149 on 2025-05-26 15:25_

We can give this a more native-feeling API with a proxy type (eliding generics):
```rust
struct RequirementDisplay<'a> {
    display_credentials: bool,
    requirement: &'a Requirement,
}

impl Display for RequirementDisplay<'_> {
    fn fmt(&self, f: &mut Formatter<'_>) -> std::fmt::Result {
        // inlining fmt_requirement
    }
}
```

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/report.rs`:1533 on 2025-05-26 15:36_

I'm a bit torn here, on one side seeing `***` here could be confusing cause those credentials are evidently the wrong ones, otoh I'd really want to know that there were credentials on the URL here.

---

_Review comment by @konstin on `uv.schema.json`:417 on 2025-05-26 15:38_

The JSON schema here should be just URL (or `uri`), the `LogSafeUrl` description should not be part of the IDE completion help.

---

_@konstin approved on 2025-05-26 15:38_

---

_@jtfmumm reviewed on 2025-05-26 15:48_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/add.rs`:653 on 2025-05-26 15:48_

I tried to preserve the existing logic as much as I could. This old `redact_credentials` function removed the credentials from the `Url`. So this is just doing the same thing the new way.

The `LogSafeUrl` only automatically masks credentials (if they exist) when displaying. That doesn't handle cases where you want to display a URL without credentials or for some other reason want to remove them. In those cases, you still have to handle it.

---

_@jtfmumm reviewed on 2025-05-26 15:50_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/add.rs`:653 on 2025-05-26 15:50_

But since the codebase does not currently have a single consistent way of handling this, it could make sense to do another pass to see where we really need to remove them and where we don't. I'd rather do that as part of a separate PR.

---

_@jtfmumm reviewed on 2025-05-26 15:54_

---

_Review comment by @jtfmumm on `crates/uv-resolver/src/pubgrub/report.rs`:1533 on 2025-05-26 15:54_

Yeah that's a good point. Note that this is not changing the existing behavior.

---

_@jtfmumm reviewed on 2025-05-26 18:09_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:111 on 2025-05-26 18:09_

Are there cases where we might want the URL but not the credentials? That's thinking ahead. For now, we'd add some extra allocations and lose the type information (i.e., that this is safe to display). So I'm hesitant to change it, but not a strong opinion.

---

_@jtfmumm reviewed on 2025-05-26 18:12_

---

_Review comment by @jtfmumm on `crates/uv-git/src/source.rs`:86 on 2025-05-26 18:12_

The reason is that I've intentionally handled the `uv_auth` part of the work in a separate PR off of this one (#13576). `Credentials` does operate on the new type there.

---

_@jtfmumm reviewed on 2025-05-26 18:27_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/file.rs`:193 on 2025-05-26 18:27_

This is a good question. We probably should. I'm using `LogSafeUrl`s to create `UrlString`s in these changes wherever we were using `Url`'s, but you can also create one directly from a `&str`. 

---

_@jtfmumm reviewed on 2025-05-26 18:27_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/file.rs`:193 on 2025-05-26 18:27_

I'll create an issue for that and can follow up.

---

_@jtfmumm reviewed on 2025-05-26 18:29_

---

_Review comment by @jtfmumm on `crates/uv-resolver/src/pubgrub/report.rs`:1533 on 2025-05-26 18:29_

Reflecting more, I think it's helpful this way since it indicates the invalid credentials were on the URL.

---

_@konstin reviewed on 2025-05-26 20:16_

---

_Review comment by @konstin on `crates/uv-pep508/src/lib.rs`:154 on 2025-05-26 20:16_

You can return an `-> impl Display` here, no need to evaluate eagerly

---

_@jtfmumm reviewed on 2025-05-26 21:37_

---

_Review comment by @jtfmumm on `crates/uv-redacted/src/lib.rs`:41 on 2025-05-26 21:37_

I've gone with `DisplaySafeUrl` after some discussion. Updating PR name.

---

_Renamed from "Add `LogSafeUrl` newtype to prevent leaking of credentials by default" to "Add `DisplaySafeUrl` newtype to prevent leaking of credentials by default" by @jtfmumm on 2025-05-26 21:37_

---

_@jtfmumm reviewed on 2025-05-26 22:04_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/file.rs`:193 on 2025-05-26 22:04_

Created #13668

---

_Merged by @jtfmumm on 2025-05-26 22:05_

---

_Closed by @jtfmumm on 2025-05-26 22:05_

---

_Branch deleted on 2025-05-26 22:05_

---

_@charliermarsh reviewed on 2025-05-27 11:37_

---

_Review comment by @charliermarsh on `crates/uv-redacted/src/lib.rs`:201 on 2025-05-27 11:37_

This type can / should be `Copy`. It doesn't really make sense for users to use `&DisplaySafeUrlRef` since it's already a reference.

---

_@charliermarsh reviewed on 2025-05-27 11:39_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/show_settings.rs`:5365 on 2025-05-27 11:39_

Is this the intended debug representation?

---

_@charliermarsh reviewed on 2025-05-27 11:40_

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/verbatim_url.rs`:80 on 2025-05-27 11:40_

Can these use `DisplaySafeUrl::from_file_path` directly?

---

_@charliermarsh reviewed on 2025-05-27 11:41_

---

_Review comment by @charliermarsh on `crates/uv-redacted/src/lib.rs`:96 on 2025-05-27 11:41_

Should this instead return a type that implements `Display`? It seems more flexible?

---

_@jtfmumm reviewed on 2025-05-27 15:00_

---

_Review comment by @jtfmumm on `crates/uv-redacted/src/lib.rs`:201 on 2025-05-27 15:00_

Implemented this and your other suggestions in a follow-up PR: #13683.

---

_Comment by @rivershah on 2025-06-02 15:50_

`0.7.9` fails to propagate credentials for authenticated wheel URLs. When those wheels depend on other authenticated wheels, uv fetches the transitive dependencies without authentication and raises errors

For example I now seeing strange behaviour such as this:

```
$ uv pip install --compile-bytecode --system -r pyproject.toml --all-extras --no-cache
Using Python 3.12.10 environment at: /usr/local
Resolved 159 packages in 3.85s
Bytecode compiled 28515 files in 1.02s

$ uv pip install --compile-bytecode --system -r pyproject.toml --all-extras
Using Python 3.12.10 environment at: /usr/local
  × Failed to download `xxx @ https://<token>@gitlab.com/api/v4/projects/<project-id>/packages/pypi/files/<file-id>/xxx.whl`
  ├─▶ Failed to fetch: `https://gitlab.com/api/v4/projects/<project-id>/packages/pypi/files/<file-id>/xxx.whl`
  ╰─▶ HTTP status client error (401 Unauthorized) for url (https://gitlab.com/api/v4/projects/<project-id>/packages/pypi/files/<file-id>/xxxx.whl)
  ```
The wheel installation that is rejected is not a direct dependency


---

_Comment by @charliermarsh on 2025-06-02 15:51_

Can you please create a separate issue, ideally with a minimal reproduction that we can execute ourselves?

---

_Comment by @jlconnor on 2025-06-02 18:57_

My team and I are also running into an issue where `uv export` is not un-obfuscating the credentials and causing pip to fail.

``` Running command git clone --filter=blob:none --quiet 'ssh://****@github.com/REDACTED' /tmp/pip-install-_9jitr12/REDACTED
  ****@github.com: Permission denied (publickey).
  fatal: Could not read from remote repository.

---

_Comment by @zanieb on 2025-06-02 18:58_

@jlconnor please please open a new issue with all the details described in #9452 

---

_Comment by @jlconnor on 2025-06-02 22:13_

@zanieb Here you go: https://github.com/astral-sh/uv/issues/13791

---
