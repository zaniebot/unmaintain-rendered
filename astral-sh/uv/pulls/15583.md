```yaml
number: 15583
title: Support Gitlab CI/CD as a trusted publisher
type: pull_request
state: merged
author: harsh-ps-2003
labels:
  - enhancement
assignees: []
merged: true
base: main
head: gitlab
created_at: 2025-08-29T17:08:05Z
updated_at: 2025-09-12T16:41:47Z
url: https://github.com/astral-sh/uv/pull/15583
synced_at: 2026-01-10T06:36:15Z
```

# Support Gitlab CI/CD as a trusted publisher

---

_Pull request opened by @harsh-ps-2003 on 2025-08-29 17:08_

`uv publish` now supports GitLab CI as a Trusted Publishing provider.

Closes #12754 

---

_Review requested from @konstin by @konstin on 2025-08-29 17:11_

---

_Review requested from @woodruffw by @konstin on 2025-08-29 17:11_

---

_Label `enhancement` added by @konstin on 2025-08-29 17:11_

---

_Assigned to @konstin by @konstin on 2025-08-29 17:12_

---

_Review comment by @woodruffw on `crates/uv-static/src/env_vars.rs`:630 on 2025-08-29 18:02_

I don't believe this will work, unfortunately -- both `CI_JOB_JWT` and `CI_JOB_JWT_V2` are deprecated, and neither will work with Trusted Publishing on PyPI because they lack the appropriate `aud` claim (i.e. `pypi`).

Instead of using these, uv needs to check for `PYPI_ID_TOKEN`, which the user has to explicitly declare in their GitLab job definition under `id_tokens`.

There's an example of that here: https://docs.pypi.org/trusted-publishers/using-a-publisher/#gitlab-cicd

More details here: https://docs.gitlab.com/ci/yaml/#id_tokens

---

_Review comment by @woodruffw on `crates/uv-static/src/env_vars.rs`:637 on 2025-08-29 18:05_

0.02c: I think it'd be better to not add this variable, and instead access `PYPI_ID_TOKEN` directly when we infer that the environment is Gitlab CI/CD. 

(My rationale: incorrect or invalid OIDC creds can be really annoying to debug, so I think we should stick to a "discover" rather than "user-supplied" flow as much as we can.)

---

_@woodruffw reviewed on 2025-08-29 18:09_

Thanks @harsh-ps-2003! I think there's some stuff we could remove here, but the overall approach looks good to me.

One larger thought that this PR doesn't need to/shouldn't address: at some point it will probably make sense to abstract the OIDC token discovery, similarly to how the [id package](https://pypi.org/project/id/) does for PyPI, twine, etc. That'll give uv the ability to add support for new Trusted Publishing providers without needing to know the internal details of how each expects credentials to be discovered. But that's a larger project.

---

_@woodruffw reviewed on 2025-08-29 18:17_

---

_Review comment by @woodruffw on `crates/uv-static/src/env_vars.rs`:630 on 2025-08-29 18:17_

Specifically, my suggest here would be to add `PYPI_ID_TOKEN` and `TESTPYPI_ID_TOKEN`, since those are the conventional variables that `id` looks for. This technically generalizes to `{AUD}_ID_TOKEN`, but I don't think indices other than PyPI or TestPyPI support Trusted Publishing yet 🙂 

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:139 on 2025-08-29 18:20_

This will need to check `{AUD}_ID_TOKEN` instead.

---

_@woodruffw reviewed on 2025-08-29 18:20_

---

_@woodruffw reviewed on 2025-08-29 18:21_

---

_Review comment by @woodruffw on `crates/uv-static/src/env_vars.rs`:630 on 2025-08-29 18:21_

Actually, thinking about it more, we probably just want to remove these entirely and just do `{AUD}_ID_TOKEN`, to save ourselves some future work.

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:124 on 2025-08-29 19:01_

This probably needs to be normalized, i.e. so that `aud: foo-bar` doesn't become `FOO-BAR_ID_TOKEN`.

Here's the relevant transform from `id`:

https://github.com/di/id/blob/48252f62c5a3d7bf16432466ae6fa78d7923639f/id/_internal/oidc/ambient.py#L295

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:144 on 2025-08-29 19:02_

For generality, this should be formatted as `{AUD}_ID_TOKEN`, after obtaining and normalizing the `audience` per above.

---

_@woodruffw reviewed on 2025-08-29 19:02_

---

_@harsh-ps-2003 reviewed on 2025-08-29 19:17_

---

_Review comment by @harsh-ps-2003 on `crates/uv-publish/src/trusted_publishing.rs`:124 on 2025-08-29 19:17_

Should the normalisation method be in the `uv-normalize` crate, or should I keep it in `trusted_publishing` in the same `uv-publish`?

---

_@woodruffw reviewed on 2025-08-29 19:25_

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:124 on 2025-08-29 19:25_

Here's probably fine for now -- I believe `uv-normalize` is primarily for package metadata.

---

_@woodruffw reviewed on 2025-08-29 19:29_

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:159 on 2025-08-29 19:29_

Can we switch this back to HTTPS only? I can't think of a good reason why we'd want to negotiate the OIDC audience or mint-token exchange over HTTP (or allow a registry to control that).

---

_@woodruffw reviewed on 2025-08-29 19:31_

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:144 on 2025-08-29 19:31_

It feels a little weird to me to have these error checking cases in sequence to the bodies above, rather than directly adjacent to their respective cases. Is there some way we can reflow this function more generally to bring the error handling closer to the site where the error occurs?

---

_@harsh-ps-2003 reviewed on 2025-08-29 19:49_

---

_Review comment by @harsh-ps-2003 on `crates/uv-publish/src/trusted_publishing.rs`:159 on 2025-08-29 19:49_

I did that because tests use a local mock server over HTTP, while real registries (PyPI/TestPyPI) are HTTPS. Hardcoding HTTPS would break the integration tests; hardcoding HTTP would be insecure. Using `registry.scheme()` makes both work, so I went ahead with that. 

---

_@woodruffw reviewed on 2025-08-29 20:07_

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:159 on 2025-08-29 20:07_

Ah, hmm. One of the maintainers can override me, but my preference would be for us to not do something with HTTP if the only use case for it is tests. 

Two alternatives come to mind:

1. We could do a `cfg(test)` check here and allow HTTP in test-only.
2. We could maybe accept a fully online test here, against TestPyPI? I'm not sure if there's any precedent for that in the other uv tests.



---

_@harsh-ps-2003 reviewed on 2025-08-30 08:26_

---

_Review comment by @harsh-ps-2003 on `crates/uv-publish/src/trusted_publishing.rs`:159 on 2025-08-30 08:26_

I thought the `cfg(test)` would be useful in this situation, but it's only set for the crate that defines the tests; it’s not set for dependencies when running integration tests. Since `uv-publish` is a dependency of uv’s integration tests, `cfg!(test)` evaluates to false there, so my HTTPS-only path is taken, breaking the HTTP server.

Maybe I can gate HTTP for OIDC endpoints on debug assertions `cfg!(debug_assertions)` instead of test. That flag is enabled for dependencies in dev/test builds, but disabled in release, so the Dev/test (including integration tests) will use the registry’s scheme (HTTP for mock) and release for HTTPS only.

---

_Comment by @woodruffw on 2025-09-08 15:47_

Thanks for your hard work here @harsh-ps-2003! I've thought about this a bit more, and I think the approach we want to go with here is a larger architectural one: I'm going to build a "get an ambient OIDC credential" crate that'll abstract GitHub and GitLab OIDC retrieval, similar to what the [id](https://pypi.org/project/id/) package in Python does. That'll make testing and future extensions (for other Trusted Publishing providers) much easier.

If you're OK with it, I'll take over this PR to make those changes. But if not, I'll start from scratch (and attribute you as a co-committer in my new changes).

---

_Unassigned @konstin by @konstin on 2025-09-08 15:52_

---

_Assigned to @woodruffw by @konstin on 2025-09-08 15:52_

---

_Review request for @konstin removed by @konstin on 2025-09-08 15:52_

---

_Comment by @harsh-ps-2003 on 2025-09-08 18:26_

Thanks and please feel free to take over this PR!  @woodruffw 

---

_@woodruffw reviewed on 2025-09-09 20:56_

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:133 on 2025-09-09 20:56_

Flagging: this is my trick/hack for test behavior specialization -- this crate exposes a `test` feature that only the tests compile with. 

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:157 on 2025-09-09 20:56_

`ambient_id` now abstracts all of this 🙂 

---

_@woodruffw reviewed on 2025-09-09 20:56_

---

_@woodruffw reviewed on 2025-09-09 20:59_

---

_Review comment by @woodruffw on `crates/uv-publish/src/lib.rs`:338 on 2025-09-09 20:59_

This now distinguishes between the two cases mentioned in the previous TODO, although both currently still produce errors:

* `Ok(None)` means that we didn't discover any OIDC credentials, but also didn't encounter any internal failures
* `Err(_)` means that we encountered an internal failure during OIDC discovery or token exchange

---

_Marked ready for review by @woodruffw on 2025-09-09 21:07_

---

_Review requested from @konstin by @woodruffw on 2025-09-09 21:08_

---

_Comment by @woodruffw on 2025-09-09 21:08_

This is good for a look -- I've rewritten the TP flow to use `ambient_id` (our new crate) for the OIDC discovery phase, which is both a nice simplification _and_ makes adding support for other Trusted Publishing providers much easier in the future. 

---

_Review comment by @konstin on `crates/uv/tests/it/publish.rs`:115 on 2025-09-10 07:32_

We should retain the hint for `id-token: write`

---

_Review comment by @konstin on `crates/uv-publish/src/trusted_publishing.rs`:37 on 2025-09-10 07:41_

Can we add some specific suggestions about what's gone wrong? Does this only happen when running outside of CI or also when GitLab CI is misconfigured?

---

_Review comment by @konstin on `crates/uv-publish/src/trusted_publishing.rs`:102 on 2025-09-10 07:42_

nit: We can pass an id token as argument and only reveal it for building the json payload.

---

_Review comment by @konstin on `crates/uv-publish/src/trusted_publishing.rs`:133 on 2025-09-10 07:56_

We may need to use a feature flag, but feature flags are not the easiest to ensure that they are either turned off or turned on, they are globally additive not properly checkable e.g. in CI. So I want to go through two possible alternatives:

Can we know some way that real CI only passes us a https URL so we can read this from `registry`? Or can we control this through an env var?

Can we use https in tests through https://github.com/LukeMathWalker/wiremock-rs/pull/163 ?

---

_@konstin reviewed on 2025-09-10 07:59_

---

_@woodruffw reviewed on 2025-09-10 14:03_

---

_Review comment by @woodruffw on `crates/uv/tests/it/publish.rs`:115 on 2025-09-10 14:03_

Yep, I need to specialize the error handling a bit for that case (since it won't be applicable to all `ambient_id` errors, just GitHub ones). I'll do that now 🙂 

---

_@woodruffw reviewed on 2025-09-10 14:05_

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:37 on 2025-09-10 14:05_

Yeah, I'll add suggestions here. This specifically happens when `ambient_id`'s discovery returns `None`, which happens if none of the discovery methods match. That roughly corresponds to running outside of CI, since discovery will fail (instead of returning `None`) if the environment has `GITHUB_CI` or `GITLAB_CI` but none of the other required state.

---

_@woodruffw reviewed on 2025-09-10 14:13_

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:133 on 2025-09-10 14:13_

> Can we know some way that real CI only passes us a https URL so we can read this from `registry`? Or can we control this through an env var?

Hmm, I can't think of a straightforward test for this -- in the "real" (end-user) CI the environment will look a lot like our unit testing CI. I guess we could set a special environment variable in our own testing (`ASTRAL_UV_INTERNAL_PLEASE_DO_NOT_SET_THIS_EVER_UNLESS_UNIT_TESTING`?) but that doesn't seem super ideal/easy to ensure that end users don't misuse either.

> Can we use https in tests through [LukeMathWalker/wiremock-rs#163](https://github.com/LukeMathWalker/wiremock-rs/pull/163) ?

Yeah, I was looking at this but I don't think it's quite ready for use yet -- it's something I could look at finishing (and carrying on our own fork?) but I didn't want to sink a ton of time into it.

---

To take a step back -- the only reason we need this testing accommodation is because of the GitLab-specific tests added in this PR. I think we *could* arguably do without those, since the main thing they test that *isn't* covered by `ambient_id`'s own tests is the PyPI token exchange. So we'd lose coverage on the PyPI token exchange, but that isn't the main subject of this PR anyways (and is already demonstrably functioning).

(Or TL;DR: I think we could defer the problem of these tests this to a separate issue/PR, since the problem here stems from an area of code not actually being changed in this PR.)

---

_Review requested from @konstin by @woodruffw on 2025-09-10 15:46_

---

_Comment by @woodruffw on 2025-09-10 15:46_

(Current test failures look like flakes to me)

---

_@konstin approved on 2025-09-10 18:07_

Can you update the PR description for the merge commit?

---

_Merged by @woodruffw on 2025-09-11 14:35_

---

_Closed by @woodruffw on 2025-09-11 14:35_

---

_Comment by @woodruffw on 2025-09-11 14:43_

Thanks for your hard work here @harsh-ps-2003!

---

_Branch deleted on 2025-09-12 16:41_

---
