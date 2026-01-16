```yaml
number: 17456
title: Isolate test environment from user configuration and remove redundant env_remove calls
type: pull_request
state: open
author: gaborbernat
labels: []
assignees: []
base: main
head: stable-ci
created_at: 2026-01-14T00:42:38Z
updated_at: 2026-01-16T15:08:21Z
url: https://github.com/astral-sh/uv/pull/17456
synced_at: 2026-01-16T15:58:35Z
```

# Isolate test environment from user configuration and remove redundant env_remove calls

---

_@gaborbernat_

Isolates the test environment from user-specific configuration that can cause test failures:

## User config isolation in `with_real_home()`

The `with_real_home()` method sets `HOME` to the real home directory, which is required for tests that use the macOS keychain for authentication. However, this causes a problem: when `HOME` is set but `XDG_CONFIG_HOME` is not, uv resolves the config directory to `$HOME/.config/uv` per the XDG specification. This means tests would read the user's real configuration files (like `.python-version` or `uv.toml`), which can override test-specified settings and cause failures.

This PR sets `XDG_CONFIG_HOME` to the test's isolated config directory in `with_real_home()`, ensuring that even when the real HOME is needed for keychain access, configuration files are read from an isolated location.

## Removed redundant `env_remove` calls

With #14080 merged, `TestContext::new_command_with()` now automatically clears all environment variables defined in `EnvVars::all_names()`. This makes the following explicit `env_remove` calls redundant:

- `UV_CACHE_DIR` - test cache directory isolation
- `UV_TOOL_BIN_DIR` - tool binary directory isolation
- `XDG_CONFIG_HOME` - config directory isolation
- `UV_INDEX_URL` - package index override
- `UV_EXTRA_INDEX_URL` - extra package index override
- `UV_PYTHON_INSTALL_MIRROR` - Python installation mirror override
- `UV_INSTALLER_GHE_BASE_URL` - GitHub Enterprise installer URL override

These were previously added to ensure tests use public PyPI and isolated directories, but are now handled automatically by #14080.

## Documentation for network-dependent tests

Adds guidance to CONTRIBUTING.md for developers running tests in offline environments, behind corporate firewalls, or on VPNs that block access to external test services like `pypi-proxy.fly.dev`. Documents how to skip network-dependent tests locally using nextest filtersets or by disabling features like `native-auth`.

---

These were blockers for me to work on https://github.com/astral-sh/uv/pull/14728

---

_Comment by @zanieb on 2026-01-14 01:33_

Overlaps with #14080 

---

_Review comment by @EliteTK on `CONTRIBUTING.md`:102 on 2026-01-14 11:05_

There are many other tests than just `native-auth` which can fail if there's no clear internet connection but this (along with the example directly following it) reads a bit like an implication that only `native-auth` tests are affected.

I would suggest:

Mention that many tests need network access, in some cases access is needed to services which are beyond pypi etc, e.g. `pypi-proxy.fly.dev` etc. And explain that if you have a network configuration which causes some of tests to fail, and you wish to skip those tests locally, you can run tests without certain features (e.g. `native-auth`), or use nextest [filtersets](https://nexte.st/docs/filtersets/) to filter out specific tests.

I think that would be sufficient for this section.

---

_Review comment by @EliteTK on `CONTRIBUTING.md`:122 on 2026-01-14 11:10_

I appreciate the effort to be helpful but it seems a little beyond the scope of this document to explain (in a potentially fragile - e.g. if the firewall/proxy is configured weird - way) how to check for firewall/proxy restrictions.

---

_Review comment by @EliteTK on `CONTRIBUTING.md`:96 on 2026-01-14 11:14_

I would just rename this to "Tests which require the network" (and ditto any referneces specifically to the scenario of a corporate firewall) as the problem is broader than just corporate firewalls and proxies. Some people may just want to run the tests in an offline environment.

---

_Review comment by @EliteTK on `crates/uv/tests/it/common/mod.rs`:1044 on 2026-01-14 11:26_

I think for now let's drop this since, as mentioned already by @zanieb, #14080 will cover this.

---

_Review comment by @EliteTK on `crates/uv/tests/it/common/mod.rs`:1530 on 2026-01-14 11:28_

Would this be necessary with #14080? I think if `$HOME` is set and `$XDG_CONFIG_HOME` is unset then the whole problem will be avoided?

---

_@EliteTK reviewed on 2026-01-14 11:28_

---

_@gaborbernat reviewed on 2026-01-14 13:03_

---

_Review comment by @gaborbernat on `CONTRIBUTING.md`:102 on 2026-01-14 13:03_

Fwiw fly.dev is especially sensitive as it allows hosting any code, so likely other corporate firewalls also ban it as opposed to other network requests (seems an easy place to host viruses and such 🤔 so in practice I think worth calling it out specifically). 

---

_@gaborbernat reviewed on 2026-01-14 13:03_

---

_Review comment by @gaborbernat on `CONTRIBUTING.md`:102 on 2026-01-14 13:03_

Fwiw fly.dev is especially sensitive as it allows hosting any code, so likely other corporate firewalls also ban it as opposed to other network requests (seems an easy place to host viruses and such 🤔 so in practice I think worth calling it out specifically). 

---

_@EliteTK reviewed on 2026-01-14 13:06_

---

_Review comment by @EliteTK on `CONTRIBUTING.md`:102 on 2026-01-14 13:06_

Yes I don't object to mentioning it, I just think the section shouldn't be entirely oriented around it.

---

_@gaborbernat reviewed on 2026-01-14 13:10_

---

_Review comment by @gaborbernat on `crates/uv/tests/it/common/mod.rs`:1530 on 2026-01-14 13:10_

Didn't test it, so not sure as that pr is not merged yet. 

---

_@zanieb reviewed on 2026-01-14 13:27_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:102 on 2026-01-14 13:27_

We also hit fly.dev outside of native authentication tests.

---

_Review comment by @zanieb on `CONTRIBUTING.md`:102 on 2026-01-14 13:28_

We could add a `fly-proxy` feature like we have for other tests which interact with a remote service

---

_@zanieb reviewed on 2026-01-14 13:28_

---

_@konstin reviewed on 2026-01-16 10:00_

---

_Review comment by @konstin on `crates/uv/tests/it/common/mod.rs`:1044 on 2026-01-16 10:00_

https://github.com/astral-sh/uv/pull/14080 is now merged.

---

_Renamed from "Isolate test environment from user configuration" to "Isolate test environment from user configuration and remove redundant env_remove calls" by @gaborbernat on 2026-01-16 15:07_

---

_@gaborbernat reviewed on 2026-01-16 15:07_

---

_Review comment by @gaborbernat on `CONTRIBUTING.md`:102 on 2026-01-16 15:07_

Addressed 😊 

---

_@gaborbernat reviewed on 2026-01-16 15:07_

---

_Review comment by @gaborbernat on `CONTRIBUTING.md`:122 on 2026-01-16 15:07_

Addressed 😊 

---

_@gaborbernat reviewed on 2026-01-16 15:07_

---

_Review comment by @gaborbernat on `CONTRIBUTING.md`:96 on 2026-01-16 15:07_

Addressed 😊 

---

_@gaborbernat reviewed on 2026-01-16 15:07_

---

_Review comment by @gaborbernat on `crates/uv/tests/it/common/mod.rs`:1044 on 2026-01-16 15:07_

Addressed 😊 

---

_@gaborbernat reviewed on 2026-01-16 15:07_

---

_Review comment by @gaborbernat on `crates/uv/tests/it/common/mod.rs`:1530 on 2026-01-16 15:07_

Addressed 😊 

---

_Review requested from @zanieb by @gaborbernat on 2026-01-16 15:08_

---

_Review requested from @konstin by @gaborbernat on 2026-01-16 15:08_

---

_Review requested from @EliteTK by @gaborbernat on 2026-01-16 15:08_

---
