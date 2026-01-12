```yaml
number: 16619
title: Warn on managed prerelease interpreters when a stable build is available
type: pull_request
state: merged
author: terror
labels:
  - enhancement
assignees: []
merged: true
base: main
head: run-warn-stable-python-version
created_at: 2025-11-06T18:55:29Z
updated_at: 2025-11-12T19:54:49Z
url: https://github.com/astral-sh/uv/pull/16619
synced_at: 2026-01-12T16:12:21Z
```

# Warn on managed prerelease interpreters when a stable build is available

---

_@terror_

Resolves https://github.com/astral-sh/uv/issues/16616

This PR detects managed prerelease interpreters during discovery and warns when a matching stable build is available, wiring the new helper into `PythonInstallation::find`, `find_best`, and `find_or_download`.

---

_Marked ready for review by @terror on 2025-11-06 19:19_

---

_@zanieb reviewed on 2025-11-06 19:50_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:945 on 2025-11-06 19:50_

I think we'd want to put this somewhere in Python discovery instead? Like, when we choose to use an interpreter?

---

_@terror reviewed on 2025-11-06 20:04_

---

_Review comment by @terror on `crates/uv/src/commands/project/run.rs`:945 on 2025-11-06 20:04_

Ah interesting, I initially thought this would be scoped to `uv run` only, but if we want it for other commands this makes sense.

---

_Review comment by @konstin on `crates/uv/src/commands/project/run.rs`:2084 on 2025-11-06 20:05_

I don't think that function is complex enough that it needs a unit test, if possible I'd test a bigger function. If we don't find a good scope what to unit test, an integration test is sufficient.

---

_Review comment by @konstin on `crates/uv/src/commands/project/run.rs`:1862 on 2025-11-06 20:10_

Can the are-we-on-an-outdated-prerelease logic go into `uv-python`? This should also make unit testing easier, cause the unit test can access `pub(crate)` in that crate. The `uv` is already heavy, so I'd move specific tasks into dedicated crates.

---

_@konstin reviewed on 2025-11-06 20:11_

---

_@zanieb reviewed on 2025-11-06 20:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1897 on 2025-11-06 20:13_

I think I'd expect this to be like... `PythonDownloadRequest::try_from(interpreter.key())`?

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1930 on 2025-11-06 20:14_

A dedicated function for this seems kind of overkill. What was your motivation?

---

_@zanieb reviewed on 2025-11-06 20:14_

---

_@zanieb reviewed on 2025-11-06 20:15_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1904 on 2025-11-06 20:15_

I think we need to avoid this warning if they've explicitly requested a pre-release version already?

---

_Review comment by @terror on `crates/uv/src/commands/project/run.rs`:1930 on 2025-11-06 20:15_

Introducing a few unit tests, now realizing that was was a bit overkill (https://github.com/astral-sh/uv/pull/16619#discussion_r2500649392) 😅

---

_@terror reviewed on 2025-11-06 20:15_

---

_Review comment by @terror on `crates/uv/src/commands/project/run.rs`:1904 on 2025-11-06 20:57_

Changed this up in the new approach and added a test for it, we'll now early return if the request allows pre-releases.

---

_@terror reviewed on 2025-11-06 20:57_

---

_@terror reviewed on 2025-11-06 20:58_

---

_Review comment by @terror on `crates/uv/src/commands/project/run.rs`:1897 on 2025-11-06 20:58_

Yeah this simplifies the call site a ton, I've added the plumbing for this!

---

_Renamed from "Add managed-prerelease upgrade hint to `uv run`" to "Warn on managed prerelease interpreters when a stable build is available" by @terror on 2025-11-06 21:14_

---

_Label `enhancement` added by @konstin on 2025-11-10 11:20_

---

_@zanieb reviewed on 2025-11-10 20:35_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:381 on 2025-11-10 20:35_

Why only CPython?

---

_@zanieb reviewed on 2025-11-10 20:36_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:384 on 2025-11-10 20:36_

I'm a little confused that we're not just doing `request.iter_downloads()` and looking for a stable download?

---

_@zanieb reviewed on 2025-11-10 20:37_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:349 on 2025-11-10 20:37_

We should add documentation here

I'd probably rename to `warn_if_outdated_prerelease`

---

_@terror reviewed on 2025-11-10 21:27_

---

_Review comment by @terror on `crates/uv-python/src/installation.rs`:381 on 2025-11-10 21:27_

I looked at the upgrade flow and noticed that we [gatekeep transparent upgrades](https://github.com/astral-sh/uv/blob/9a21897f3d5a36413c1d1305a5cd5a3da05b1391/crates/uv-python/src/managed.rs#L754) for CPython only, so this ensures we don't warn users where the upgrade would effectively be a no-op. I could be missing something though since this part is new to me.

---

_@terror reviewed on 2025-11-10 21:28_

---

_Review comment by @terror on `crates/uv-python/src/installation.rs`:384 on 2025-11-10 21:28_

Yeah I can inline this, `has_stable_download_at_least` is doing exactly that.

---

_@zanieb reviewed on 2025-11-10 21:36_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:381 on 2025-11-10 21:36_

I see. Let's just open an issue to figure that out later. Please add a comment here.

---

_@zanieb reviewed on 2025-11-10 21:37_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:384 on 2025-11-10 21:37_

Part of my question is why we're not just using `request` instead of constructing a new `download_request`.

---

_Review comment by @terror on `crates/uv-python/src/installation.rs`:384 on 2025-11-10 22:01_

Ah, my thinking here is that we want to construct a download request for the specific interpreter we've actually resolved, not necessarily what the user requested (which could be broader, and unable to be converted into a download request).

---

_@terror reviewed on 2025-11-10 22:01_

---

_@zanieb reviewed on 2025-11-10 22:13_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:384 on 2025-11-10 22:13_

mm I see, that makes sense. Thanks for explaining!

---

_@zanieb reviewed on 2025-11-10 22:14_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:404 on 2025-11-10 22:14_

Note this suggestion will not work in some edge cases, e.g., if the user request has an alternative architecture in it.

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:384 on 2025-11-10 22:15_

I think I'd prefer if it was inlined for now anyway.

---

_@zanieb reviewed on 2025-11-10 22:15_

---

_Review comment by @terror on `crates/uv-python/src/installation.rs`:404 on 2025-11-10 22:30_

Hmm interesting, we can detect when the interpreter's key matches the host platform defaults (in this case its safe to suggest) and then fall back to suggesting `uv python upgrade`? I don't particularly like this, but this is what I'm thinking.

---

_@terror reviewed on 2025-11-10 22:30_

---

_@zanieb reviewed on 2025-11-10 22:51_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:404 on 2025-11-10 22:51_

I think the transformation we're looking for is...

1. We have a `PythonDownloadRequest`
2. We want to unset any values that match the defaults (restore to `None`)
3. We want to drop the patch version from the `VersionRequest` component
4. We want to display it without `any` in the `None` positions

This would get us the request we're looking to pass to `uv python upgrade`. Maybe like `download_request.unset_defaults().without_patch().simplified_display()`? Is that ridiculous? :)

---

_@terror reviewed on 2025-11-10 23:29_

---

_Review comment by @terror on `crates/uv-python/src/installation.rs`:404 on 2025-11-10 23:29_

This doesn't sound unreasonable to me. It would be nice to have `uv python upgrade` accept fully-qualified installation keys, but I understand why there are specific constraints for this in place.

---

_@zanieb reviewed on 2025-11-10 23:32_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:404 on 2025-11-10 23:32_

Oh it could accept fully qualified keys, I just think it'd be ugly to tell a user to use it :D

---

_@zanieb reviewed on 2025-11-11 20:40_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:403 on 2025-11-11 20:40_

Why do we need to clone it? Isn't this the last use of the variable?

---

_@zanieb reviewed on 2025-11-11 20:41_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:458 on 2025-11-11 20:41_

This should explain when it would return `None`, e.g., "Returns [`None`] when no segments are explicitly set"

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:465 on 2025-11-11 20:45_

`any` vs `default` have different semantics, should we omit it in those cases?

---

_@zanieb reviewed on 2025-11-11 20:45_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:425 on 2025-11-11 20:47_

It seems weird to allow `Option` here?

I think I'd have `unset_defaults` include the implementation filtering, then have `unset_platform_defaults` include the platform specific part and do the `let Some(host) ...` matching in `unset_defaults`. Does that make sense?

---

_@zanieb reviewed on 2025-11-11 20:47_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:428 on 2025-11-11 20:48_

Can we use `ImplementationName::default()` here?

---

_@zanieb reviewed on 2025-11-11 20:48_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:443 on 2025-11-11 20:48_

This feels like it deserves a comment

---

_@zanieb reviewed on 2025-11-11 20:48_

---

_@terror reviewed on 2025-11-11 20:55_

---

_Review comment by @terror on `crates/uv-python/src/downloads.rs`:443 on 2025-11-11 20:55_

Agreed!

---

_@terror reviewed on 2025-11-11 21:07_

---

_Review comment by @terror on `crates/uv-python/src/installation.rs`:403 on 2025-11-11 21:07_

Ah it was the iterator from the outer `download_request.iter_downloads(...)` call that held onto a reference. I've put this into the `has_stable_download` block instead to fix it.

---

_@terror reviewed on 2025-11-11 21:13_

---

_Review comment by @terror on `crates/uv-python/src/downloads.rs`:425 on 2025-11-11 21:13_

Yeah this makes sense. I do agree its a bit awkward with `None`, I guess part of my thinking was that `unset_defaults_with_host` wouldn't be user-facing so it didn't matter too much.

---

_Review comment by @terror on `crates/uv-python/src/downloads.rs`:465 on 2025-11-11 21:19_

Hmm, we are omitting them here right (i.e. !matches)? My understanding is we'd like to surface only the segments a user explicitly set, and these both represent (afaik) no explicit version constraint.

---

_@terror reviewed on 2025-11-11 21:19_

---

_@zanieb reviewed on 2025-11-11 22:52_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:465 on 2025-11-11 22:52_

Sorry, I meant "should we be omitting it"

That's fair. I think I'd expect those to be filtered in the `unset_defaults` method instead though?

It's a possible foot gun, in the sense that they have different semantics so omitting them could have a different result, but I can't really think of a concrete situation where this would occur. Emitting `any` would probably mess up the key parser anyway? :/ I'm fine omitting them until there's a problem.

---

_@terror reviewed on 2025-11-11 23:42_

---

_Review comment by @terror on `crates/uv-python/src/downloads.rs`:465 on 2025-11-11 23:42_

> That's fair. I think I'd expect those to be filtered in the `unset_defaults` method instead though?

Yeah agreed, I think `simplified_display` should be simple in that it just filters out `None` values.

> It's a possible foot gun, in the sense that they have different semantics so omitting them could have a different result, but I can't really think of a concrete situation where this would occur. Emitting any would probably mess up the key parser anyway? :/ I'm fine omitting them until there's a problem.

Looks like omitting anything will screw up the key parser (it expects 5 components), but since what we pass into `uv python upgrade` doesn't go through the key parsing flow, I think it's fine? 🤔 Especially sine omitting `any`/`default` here doesn't have any semantic loss, but I understand how if we use this for some other purpose there could be issues.


---

_@zanieb reviewed on 2025-11-12 13:44_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:465 on 2025-11-12 13:44_

Just as a note

> Looks like omitting anything will screw up the key parser (it expects 5 components),

I was referring to `PythonDownloadRequest`, which is how a request for a "key" is formed and does support missing components since https://github.com/astral-sh/uv/pull/14399

>  since what we pass into uv python upgrade doesn't go through the key parsing flow

It could, e.g., we use the generic parser

https://github.com/astral-sh/uv/blob/caf49f845f1112a242f93f6860438e8ada324d6c/crates/uv/src/commands/python/install.rs#L262

https://github.com/astral-sh/uv/blob/60a811e715d1d1919afd73b0e4dec4c4be8aa3f9/crates/uv-python/src/discovery.rs#L1822-L1824

so if you request `uv python upgrade 3.14-x86_64` or something that goes through the key parsing flow



---

_@zanieb approved on 2025-11-12 13:45_

---

_Merged by @zanieb on 2025-11-12 13:45_

---

_Closed by @zanieb on 2025-11-12 13:45_

---

_@MeitarR reviewed on 2025-11-12 19:51_

---

_Review comment by @MeitarR on `crates/uv-python/src/installation.rs`:82 on 2025-11-12 19:51_

Hey @terror and @zanieb, it's problematic to not always pass the `python_downloads_json_url` as it may warn users about versions that are not really available to them, as it does not look at the actual json that they use (if it's configured)

I think it should be passed, or not try to warn at all if `python_downloads_json_url` is configured

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:82 on 2025-11-12 19:54_

Thanks, I missed that

---

_@zanieb reviewed on 2025-11-12 19:54_

---

_@zanieb reviewed on 2025-11-12 19:54_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:82 on 2025-11-12 19:54_

https://github.com/astral-sh/uv/issues/16711

---
