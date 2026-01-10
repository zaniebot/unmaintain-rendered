```yaml
number: 7548
title: Implement trusted publishing
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/publish4
created_at: 2024-09-19T13:26:07Z
updated_at: 2024-09-24T16:07:21Z
url: https://github.com/astral-sh/uv/pull/7548
synced_at: 2026-01-10T12:53:49Z
```

# Implement trusted publishing

---

_Pull request opened by @konstin on 2024-09-19 13:26_

Trusted publishing allows uploading to PyPI from GitHub actions without setting a (long-lived) secret token. Instead, you configure a GitHub Actions workflow as trusted publisher. With the `id-token: write` permission, GitHub Actions then allows us to obtain an OpenID Connect (OIDC) token, with which we can ask PyPI for a short lived upload token just for this session. The user experiences this as credentials-free upload. See https://docs.pypi.org/trusted-publishers/ for details and https://github.com/pypa/gh-action-pypi-publish for the reference implementation.

When we are in GitHub Actions and there are no explicit credentials, we try to obtain trusted publishing credentials. This can be controlled with `--trusted-publishing` (`automatic`, `always`, `never`)

The auth middleware gained a new option that allows us to selectively turn off request-cloning only (for uploads) or the entire middleware (for OIDC, which determines credentials through network requests), while still sharing the client once initialized.

Since we can't do testing offline, we upload `astral-test-trusted-publishing` in the test github action.

---

_Review comment by @lucab on `.github/workflows/ci.yml`:446 on 2024-09-20 06:55_

I guess this is going to disappear in the final form of the PR, but I'm curious why this was needed. Was this only a paid-plan restriction, or is there something more subtle behind?

---

_Review comment by @lucab on `docs/reference/settings.md`:1288 on 2024-09-20 07:23_

UX feedback: this is slightly confusing because here the default "use trusted-publishing: no" in config corresponds to the default "use trusted-publishing: yes, automatically" in the code logic (if I understood correctly).
The fact that there is also a negative config-key which can be doubly-negated (`no-trusted-publishing = false`) does not help either.

I wonder if this can be re-arranged into some kind of enum `trusted-publishing-mode: auto | none | only-github | during-blue-moons`.
That would also leave some room for other future providers as listed in https://docs.pypi.org/trusted-publishers/security-model/#provider-specific-considerations.

---

_Review comment by @lucab on `crates/uv-publish/src/trusted_publishing.rs`:60 on 2024-09-20 07:39_

I suspect you'll actually encounter network failures to both GH and PyPI, action runners are very noisy-neighbor environments at time.
I think each individual HTTP call here would benefit from having its own timeout and retries.

---

_Review comment by @lucab on `crates/uv-publish/src/trusted_publishing.rs`:76 on 2024-09-20 07:41_

This is probably fine now but may become a future hazard if more providers get added. It should be cheap to re-verify the sentinel flag in the env, or directly let the caller pass this information to here.

---

_Review comment by @lucab on `crates/uv-cli/src/lib.rs`:4352 on 2024-09-20 08:02_

Self-note: this sets the default flow of the publishing logic in a way that doesn't require the user to opt-in into it, and I was wondering whether this a security surprise/hazard.
I convinced myself that this should be fine because, although this starts automatically using ambient capabilities that are provided (by default?) by GH and that the users may not be fully aware of, completing a proper authentication loop requires a setup with an explicit opt-in step by the user on the [PyPi side](https://docs.pypi.org/trusted-publishers/adding-a-publisher/).

---

_@lucab reviewed on 2024-09-20 08:05_

---

_Review comment by @lucab on `crates/uv-publish/src/trusted_publishing.rs`:60 on 2024-09-20 08:08_

As a datapoint, the last CI run on `main` is failing due to a network hang on a GH-to-GH fetch, during the setup of job environment itself.
Their client implements the following timeout-and-retry approach:
```
Download action repository 'actions/checkout@v4' (SHA:692973e3d937129bcbf40652eb9f2f61becf3332)
Download action repository 'actions/download-artifact@v4' (SHA:fa0a91b85d4f404e444e00e005971372dc801d16)
Warning: Failed to download action 'https://api.github.com/repos/actions/download-artifact/tarball/fa0a91b85d4f404e444e00e005971372dc801d16'. Error: The request was canceled due to the configured HttpClient.Timeout of 100 seconds elapsing.
Warning: Back off 12.296 seconds before retry.
Warning: Failed to download action 'https://api.github.com/repos/actions/download-artifact/tarball/fa0a91b85d4f404e444e00e005971372dc801d16'. Error: The request was canceled due to the configured HttpClient.Timeout of 100 seconds elapsing.
Warning: Back off 11.758 seconds before retry.
Error: Action 'https://api.github.com/repos/actions/download-artifact/tarball/fa0a91b85d4f404e444e00e005971372dc801d16' download has timed out. Error: The request was canceled due to the configured HttpClient.Timeout of 100 seconds elapsing.
```

---

_@lucab reviewed on 2024-09-20 08:08_

---

_@konstin reviewed on 2024-09-20 08:33_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:446 on 2024-09-20 08:33_

This was just because i was testing in my personal fork to avoid messing with permissions on astral-sh/uv while i figure it out so i blanket removed everything custom

---

_@konstin reviewed on 2024-09-20 09:12_

---

_Review comment by @konstin on `crates/uv-publish/src/trusted_publishing.rs`:60 on 2024-09-20 09:12_

That's good to know

---

_Marked ready for review by @konstin on 2024-09-21 14:27_

---

_Review requested from @zanieb by @konstin on 2024-09-21 14:27_

---

_Review requested from @charliermarsh by @konstin on 2024-09-21 14:27_

---

_Label `enhancement` added by @konstin on 2024-09-21 14:31_

---

_@charliermarsh reviewed on 2024-09-23 19:12_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yml`:984 on 2024-09-23 19:12_

This is dubious, no?

---

_@charliermarsh reviewed on 2024-09-23 19:14_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/publish.rs`:39 on 2024-09-23 19:14_

```suggestion
    //   retries.
```

---

_@charliermarsh reviewed on 2024-09-23 19:14_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/publish.rs`:45 on 2024-09-23 19:14_

Yeah painful.

---

_@charliermarsh reviewed on 2024-09-23 19:16_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/trusted_publishing.rs`:58 on 2024-09-23 19:16_

Can we create a wrapper type for the token?

---

_@charliermarsh reviewed on 2024-09-23 19:17_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/trusted_publishing.rs`:93 on 2024-09-23 19:17_

Should we check the env var again to confirm that we're in GHA?

---

_@charliermarsh reviewed on 2024-09-23 19:17_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/trusted_publishing.rs`:105 on 2024-09-23 19:17_

Nit: you could DRY up this URL builder which is also used below.

---

_Review comment by @charliermarsh on `crates/uv-publish/src/trusted_publishing.rs`:118 on 2024-09-23 19:17_

Probably worth a dedicated error message if the env var is missing?

---

_@charliermarsh reviewed on 2024-09-23 19:17_

---

_@charliermarsh reviewed on 2024-09-23 19:18_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/settings.rs`:1651 on 2024-09-23 19:18_

```suggestion
    /// Configure trusted publishing via GitHub Actions.
```

---

_@charliermarsh approved on 2024-09-23 19:18_

---

_Comment by @charliermarsh on 2024-09-23 19:18_

How was this tested? Can you include a test plan in the summary?

---

_@konstin reviewed on 2024-09-24 10:02_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:984 on 2024-09-24 10:02_

It's from the docs:

![image](https://github.com/user-attachments/assets/ffbc48c7-5a17-4fcc-a14f-ec8e220b2abc)

https://docs.pypi.org/trusted-publishers/using-a-publisher/


---

_Review comment by @konstin on `crates/uv-publish/src/trusted_publishing.rs`:93 on 2024-09-24 10:06_

We can only enter the function if we're in github actions and the process can only succeed when we're in github actions (i.e. we can't obtain the token and execute this line without running in github actions), so it's save to assume we are in github actions.

---

_@konstin reviewed on 2024-09-24 10:06_

---

_@konstin reviewed on 2024-09-24 10:08_

---

_Review comment by @konstin on `crates/uv-publish/src/trusted_publishing.rs`:105 on 2024-09-24 10:08_

I had looked for this, but i couldn't find anything in the url crate that would allow me to build an url from an authority.

---

_@konstin reviewed on 2024-09-24 10:09_

---

_Review comment by @konstin on `crates/uv-publish/src/trusted_publishing.rs`:118 on 2024-09-24 10:09_

Where would that error message be displayed?

---

_@konstin reviewed on 2024-09-24 10:27_

---

_Review comment by @konstin on `crates/uv-publish/src/trusted_publishing.rs`:58 on 2024-09-24 10:27_

The token we get gets used as the password later. We currently store the password as `Option<String>`, both in publish and the index api. Should we introduce a new `Redacted` type and use it for all password? (By using a newtype just for the token, we just have to turn it back to a string two functions down)

---

_@charliermarsh reviewed on 2024-09-24 12:13_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/trusted_publishing.rs`:118 on 2024-09-24 12:13_

Like, here, instead of `env::var("ACTIONS_ID_TOKEN_REQUEST_URL")?`, was my thinking.

---

_@charliermarsh reviewed on 2024-09-24 12:15_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/trusted_publishing.rs`:93 on 2024-09-24 12:15_

Why is it a problem to check the env var again? Otherwise we risk printing the unmasked token in other environments.

---

_@charliermarsh reviewed on 2024-09-24 12:15_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/trusted_publishing.rs`:58 on 2024-09-24 12:15_

Still worth it to have a dedicated `Token` type from my perspective. Less `String` is always better.

---

_@zanieb reviewed on 2024-09-24 13:31_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:984 on 2024-09-24 13:31_

It seems like it still may be dubious to run in our public CI, right?

---

_@konstin reviewed on 2024-09-24 14:59_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:984 on 2024-09-24 14:59_

We of course need to review all changes to the code called here, but the risk is otherwise the same as if someone makes a PR that adds `id-token: write`. The permission in testpypi itself is clearly scoped to allow only this package and only this environment.

---

_Merged by @konstin on 2024-09-24 16:07_

---

_Closed by @konstin on 2024-09-24 16:07_

---

_Branch deleted on 2024-09-24 16:07_

---
