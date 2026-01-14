```yaml
number: 17456
title: Isolate test environment from user configuration
type: pull_request
state: open
author: gaborbernat
labels: []
assignees: []
base: main
head: stable-ci
created_at: 2026-01-14T00:42:38Z
updated_at: 2026-01-14T13:28:06Z
url: https://github.com/astral-sh/uv/pull/17456
synced_at: 2026-01-14T13:42:35Z
```

# Isolate test environment from user configuration

---

_@gaborbernat_

Isolates the test environment from user-specific configuration that can cause test failures:

- **Internal package mirrors**: Remove `UV_INDEX_URL`, `UV_EXTRA_INDEX_URL`, `UV_PYTHON_INSTALL_MIRROR`, and `UV_INSTALLER_GHE_BASE_URL` from test environment to ensure tests use public PyPI instead of internal mirrors that may lack upload date metadata

- **User config files**: Set `XDG_CONFIG_HOME` in `with_real_home()` to prevent tests from reading user configuration files (like `.python-version`) that could override test-specified Python versions

- **Documentation**: Add guidance to CONTRIBUTING.md for developers behind corporate firewalls that block access to external test services like `pypi-proxy.fly.dev`


These were the one that I noticed and were a blocker for me to work on https://github.com/astral-sh/uv/pull/14728

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

_Review comment by @EliteTK on `crates/uv/tests/it/common/mod.rs`:1536 on 2026-01-14 11:28_

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

_Review comment by @gaborbernat on `crates/uv/tests/it/common/mod.rs`:1536 on 2026-01-14 13:10_

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
