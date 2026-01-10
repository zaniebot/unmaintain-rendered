```yaml
number: 7279
title: "feat(cli): add `--token` option to `self update` command"
type: pull_request
state: merged
author: frostming
labels:
  - enhancement
assignees: []
merged: true
base: main
head: fix/self-update-take-token-option
created_at: 2024-09-11T03:25:36Z
updated_at: 2024-09-12T23:04:34Z
url: https://github.com/astral-sh/uv/pull/7279
synced_at: 2026-01-10T12:53:43Z
```

# feat(cli): add `--token` option to `self update` command

---

_Pull request opened by @frostming on 2024-09-11 03:25_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

It often reaches the GitHub API rate limit and shows error like `error: HTTP status client error (403 Forbidden) for url (https://api.github.com/repos/astral-sh/uv/releases)` when running `uv self update`.

To bypass this rate limit issue, allow user to pass a GitHub token via `--token` or `UV_GITHUB_TOKEN` env.

## Test Plan

<!-- How was it tested? -->


---

_@zanieb reviewed on 2024-09-11 11:43_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:433 on 2024-09-11 11:43_

I think we could probably just respect `GITHUB_TOKEN` here since it's standardized

---

_@zanieb reviewed on 2024-09-11 11:45_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:431 on 2024-09-11 11:45_

Maybe something like
```suggestion
    /// A GitHub token for authentication.
    ///
    /// A token is not required but can be used to reduce the chance of encountering rate limits.
```

---

_@zanieb reviewed on 2024-09-11 11:45_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:431 on 2024-09-11 11:45_

You'll need to run `cargo dev generate-all` to update the reference docs.

---

_Comment by @zanieb on 2024-09-11 11:45_

Thanks for contributing and welcome to the project! Nice to see you over here.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:433 on 2024-09-11 11:46_

We'll also need to update `docs/configuration/environment.md`

---

_@zanieb reviewed on 2024-09-11 11:46_

---

_@mattmess1221 reviewed on 2024-09-11 18:46_

---

_Review comment by @mattmess1221 on `crates/uv-cli/src/lib.rs`:433 on 2024-09-11 18:46_

axoupdater [recommends against that](https://github.com/axodotdev/axoupdater?tab=readme-ov-file#github-actions-and-rate-limits-in-ci).

> We also recommend picking an environment variable name that's specific to your application; it's not uncommon for users to have stale or expired `GITHUB_TOKEN` tokens in their environment, and using that name may cause your app to behave unexpectedly.

---

_@zanieb reviewed on 2024-09-11 19:31_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:433 on 2024-09-11 19:31_

Hm, interesting. Thanks for the reference. Let me see if anyone else has an opinion, but it's always easier to start with the scoped variable as you have here.

cc @charliermarsh 

---

_Review comment by @frostming on `crates/uv-cli/src/lib.rs`:431 on 2024-09-12 01:08_

> You'll need to run `cargo dev generate-all` to update the reference docs.

Run but nothing is updated.

---

_@frostming reviewed on 2024-09-12 01:08_

---

_@frostming reviewed on 2024-09-12 01:10_

---

_Review comment by @frostming on `crates/uv-cli/src/lib.rs`:433 on 2024-09-12 01:10_

An extra bonus is should we show friendly error message to remind people of setting github token when rate limit is reached?

---

_@zanieb reviewed on 2024-09-12 01:10_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:431 on 2024-09-12 01:10_

Oh sorry, we don't have this command in the CLI reference — which is a separate problem, I'll fix that. #7317 

---

_@frostming reviewed on 2024-09-12 01:13_

---

_Review comment by @frostming on `crates/uv-cli/src/lib.rs`:433 on 2024-09-12 01:13_

> An extra bonus is should we show friendly error message to remind people of setting github token when rate limit is reached?

* * *

However, from the HTTP status code 403, we can only conclude that it might be rate limiting or an invalid token.


---

_@zanieb reviewed on 2024-09-12 02:28_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:433 on 2024-09-12 02:28_

I think if we encounter a 403 and weren't given a token we can show a rate limit hint, that seems really nice.

---

_Review requested from @zanieb by @frostming on 2024-09-12 03:30_

---

_@zanieb approved on 2024-09-12 19:27_

Thanks!

---

_Merged by @zanieb on 2024-09-12 19:27_

---

_Closed by @zanieb on 2024-09-12 19:27_

---

_Label `enhancement` added by @zanieb on 2024-09-12 19:27_

---

_Branch deleted on 2024-09-12 23:04_

---
