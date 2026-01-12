```yaml
number: 2570
title: Use URL-encoded username for keyring if no URL-encoded password
type: pull_request
state: closed
author: BakerNet
labels: []
assignees: []
base: main
head: fix/passthrough-username-to-keyring
created_at: 2024-03-20T18:55:29Z
updated_at: 2024-04-16T16:50:02Z
url: https://github.com/astral-sh/uv/pull/2570
synced_at: 2026-01-12T16:05:06Z
```

# Use URL-encoded username for keyring if no URL-encoded password

---

_@BakerNet_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Currently, the middleware will not check keyring or netrc if there is any URL-encoded credentials.  `pip` will check keyring and netrc if there is no URL-encoded password, and use the URL-encoded username for keyring.  This updates the middleware to match `pip` functionality in this way.

See issue:
- #2563 

## Test Plan

<!-- How was it tested? -->

See included unit tests

Note:  Introduced `cfg`-branching functionality to `get_keyring_subprocess_auth` in order to bypass subprocess calls in unit tests.

---

_Assigned to @zanieb by @zanieb on 2024-03-20 19:15_

---

_Comment by @BakerNet on 2024-03-20 19:50_

Flaky tests? Only change in my branch was a comment and failing tests are in unrelated parts of the codebase.
Unable to rerun failed checks on my end, btw.

---

_Comment by @charliermarsh on 2024-03-20 19:50_

I think GitHub has been having issues today.

---

_Comment by @zanieb on 2024-03-25 17:58_

This is on my to-do list, it's been slow because of all the cfg changes for testing — we've generally avoided doing something like this so far. Without an in-depth review, I'd say some sort of pluggable `Provider` interface is preferable.

---

_Comment by @jenshnielsen on 2024-03-26 11:58_

@BakerNet Thanks for the PR. I can confirm that this resolved the direct issue. If I try to install a package it will now call keyring with the correct username allowing me to get a token as expected. However, it seems to hit a code path where this still fails when downloading transitive dependencies. 

E.g. installing numpy works but installing scipy (that depends on numpy) fails like this:
```
DEBUG Adding transitive dependency: numpy>=1.22.4, <1.29.0
DEBUG No cache entry for: https://pkgs.dev.azure.com/someorg/_packaging/somefeed/pypi/simple/numpy/
DEBUG Request already has an authorization header with URL-encoded password: https://pkgs.dev.azure.com/someorg/_packaging/somefeed/pypi/simple/numpy/
error: HTTP status client error (401 Unauthorized) for url (https://pkgs.dev.azure.com/someorg/_packaging/somefeed/pypi/simple/numpy/)
```



---

_Comment by @BakerNet on 2024-03-26 15:25_

> @BakerNet Thanks for the PR. I can confirm that this resolved the direct issue. If I try to install a package it will now call keyring with the correct username allowing me to get a token as expected. However, it seems to hit a code path where this still fails when downloading transitive dependencies. 
> 
> E.g. installing numpy works but installing scipy (that depends on numpy) fails like this:
> ```
> DEBUG Adding transitive dependency: numpy>=1.22.4, <1.29.0
> DEBUG No cache entry for: https://pkgs.dev.azure.com/someorg/_packaging/somefeed/pypi/simple/numpy/
> DEBUG Request already has an authorization header with URL-encoded password: https://pkgs.dev.azure.com/someorg/_packaging/somefeed/pypi/simple/numpy/
> error: HTTP status client error (401 Unauthorized) for url (https://pkgs.dev.azure.com/someorg/_packaging/somefeed/pypi/simple/numpy/)
> ```
> 
> 

Thanks for testing and sharing!  I'm pretty sure I know what's going on here and will get a fix in later today.

---

_Comment by @BakerNet on 2024-03-26 22:03_

> This is on my to-do list, it's been slow because of all the cfg changes for testing — we've generally avoided doing something like this so far. Without an in-depth review, I'd say some sort of pluggable `Provider` interface is preferable.

I have an idea to propose:
We could make the keyring check function a method on the `KeyringProvider` enum, and add a `clap`-skipped enum variant for test suite.
So instead of using `cfg(test)` we would `match self`
```rust
#[derive(Debug, Default, Clone, Copy, PartialEq, Eq)]
#[cfg_attr(feature = "clap", derive(clap::ValueEnum))]
pub enum KeyringProvider {
    /// Will not use keyring for authentication
    #[default]
    Disabled,
    /// Will use keyring CLI command for authentication
    Subprocess,
    // /// Not yet implemented
    // Auto,
    // /// Not implemented yet.  Maybe use <https://docs.rs/keyring/latest/keyring/> for this?
    // Import,
    #[cfg_attr(feature = "clap", clap(skip))]
    TestStub,
}
```
in method:
```rust
let output = match self {
    Self::Subprocess => match Command::new("keyring")
        ... ,
    Self::Disabled => Ok(None),
    Self::TestStub => Ok(Some("mypassword".to_string())),
}
```

---

_Comment by @BakerNet on 2024-03-26 22:04_

@jenshnielsen would you mind testing this again to check if my fix works on your end when you have time?

---

_Comment by @jenshnielsen on 2024-03-27 06:08_

@BakerNet Thanks this does seem to resolve the issue. I can install packages including their dependencies now. I am still seeing intermittent 401 errors that go away if I rerun the same command. I have not been able to figure out exactly which code path triggers it or if its even related

---

_Comment by @zanieb on 2024-04-15 19:56_

I'm tackling this in a more comprehensive overhaul over in https://github.com/astral-sh/uv/pull/2976 if you're interested in looking at it.

---

_Comment by @zanieb on 2024-04-16 16:50_

Thanks for taking the time to put up a pull request, this was really helpful for understanding the desired behavior. I'm closing in favor of https://github.com/astral-sh/uv/pull/2976 which makes some other changes.

---

_Closed by @zanieb on 2024-04-16 16:50_

---
