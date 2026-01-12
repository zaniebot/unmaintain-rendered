```yaml
number: 14080
title: Clear the env vars for tests
type: pull_request
state: open
author: konstin
labels:
  - internal
assignees: []
draft: true
base: main
head: konsti/clear-env-for-tests
created_at: 2025-06-16T16:28:29Z
updated_at: 2026-01-12T13:53:34Z
url: https://github.com/astral-sh/uv/pull/14080
synced_at: 2026-01-12T14:04:51Z
```

# Clear the env vars for tests

---

_Pull request opened by @konstin on 2025-06-16 16:28_

Currently, it's possible to break our test suite by having an env var set that influences uv, either a `UV_*` var, or something more generic such as the XDG env vars. We previously fixed them env-var-by-env-var as we discovered. By clearing the env var for subcommands entirely, we can invert this to only include the env vars we care about.

Notable limitations are that this only affects tests that use `TestContext::add_shared_env` (default, can be opted-out) and we still pass a (modified) `PATH`.

Fixed #9873

---

_Label `internal` added by @konstin on 2025-06-16 16:28_

---

_Comment by @konstin on 2025-06-16 16:29_

@mgorny Would this work for you too or would this break the tests distros run for uv?

---

_Comment by @mgorny on 2025-06-16 16:54_

Well, it would definitely break some stuff.

https://gitweb.gentoo.org/repo/gentoo.git/tree/dev-python/uv/uv-0.7.13.ebuild#n132

It's possible that some of them are no longer necessary, and others could be replaced by better fixes on uv's end (for example, by not looking at system configuration files when testing). However, my general feeling is that clearing the environment is a very bad idea, because you can't know what internal variables are needed for programs in that environment to run correctly.

---

_Comment by @konstin on 2025-06-16 16:59_

For the code you linked, it would actually fix those cases: We don't read `COLUMNS` anymore (we're already overriding it in tests) and we fix all XDG problems including `XDG_CONFIG_DIRS`; `PATH` we'll continue to pass in. Re `PYTHONDONTWRITEBYTECODE`, do you know why it's in there? Bytecode compilation is something we've put some effort in cause it's a big performance factor.

---

_Comment by @mgorny on 2025-06-16 17:13_

Could you ping me once the CI passes, so I could test it?

---

_Converted to draft by @konstin on 2025-06-16 17:26_

---

_Comment by @zanieb on 2025-10-09 13:50_

I'm still in favor of this approach, I think.

We can always add a `UV_TEST__NO_CLEAR_ENV=....` option if we need to.

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:1575 on 2025-10-09 13:52_

This is being used for non-Windows too?

---

_@zanieb reviewed on 2025-10-09 13:52_

---

_Comment by @konstin on 2025-10-09 13:53_

It works for Unix, it's currently stuck on figuring out what env vars we need to pass through for Windows, CI should show what fails with the current generated list. Last time I tried a long list of env vars we pass through on Windows but got some unclear failures that I didn't get to investigate yet.

---

_@konstin reviewed on 2025-10-09 13:55_

---

_Review comment by @konstin on `crates/uv/tests/it/common/mod.rs`:1575 on 2025-10-09 13:55_

I'm not sure yet. I thought about passing the same env vars through on Windows and on Unix, to minimize the cases where something works locally but then fails in CI for the opposite platform.

---

_Comment by @samypr100 on 2025-10-17 13:09_

Purely offering an alternative approach as I'm maybe misunderstanding the goal of this PR. Would leveraging the EnvVars struct to label test environment variables to keep be sufficient? (at least that was on my mind originally back then).

This way you have a list of all the env vars you should remove, and all the ones you should keep supporting rather than clearing them all and spend significant amount of time figuring out which one to proxy / add back to satisfy all build environments.

e.g. `#[test_allow_override]` similar to attr_hidden for testing, with an additional exposed helper method like `EnvVars::list` to get all the test and non-test enablement ones. It also from my perspective would make it easier for contributions as there's less sources of truth.

---

_Renamed from "Clear the env vars for tests" to "Clear known env vars for tests" by @konstin on 2026-01-12 13:53_

---
