```yaml
number: 8970
title: Enhance offline testing
type: issue
state: open
author: mgorny
labels:
  - testing
assignees: []
created_at: 2024-11-09T13:28:51Z
updated_at: 2024-12-04T04:44:27Z
url: https://github.com/astral-sh/uv/issues/8970
synced_at: 2026-01-12T15:59:39Z
```

# Enhance offline testing

---

_@mgorny_

I think uv's getting quite mature at this point. Unfortunately, the test suite is still largely dependent on accessing `pypi.org` online. FWICS, on top of 65e2f955f152304c1a4857a96d69d3507d548221 there are ~1250 tests skipped with it disabled, i.e. only ~10% of the test suite is run without it, and I suppose a lot of important features aren't tested.

Other points aside, the problem with the current approach to online testing is that it's very fragile to changes to PyPI packages. In the past, I've seen more than once that the tests were already failing within 24 hours from publishing a new version. While many of these failures can be trivially resolved, this effectively that means `uv` keeps being a rapidly moving target. As such, we can't really mark it stable in Gentoo — our rough guideline for the Python ecosystem is to mark stable after 2 weeks with no major issues, and by then the test suite must still be passing.

Could you please consider a long-term goal of making the tests less dependent on third-party services and more offline? Some Python packages run their own "PyPI server" (i.e. `pypiserver` PyPI package) for that purpose — I suppose a similar solution would be the cleanest approach to offline testing.

---

_Label `testing` added by @charliermarsh on 2024-11-09 13:43_

---

_Comment by @charliermarsh on 2024-11-09 13:47_

See also https://github.com/astral-sh/uv/pull/8153.

---

_Comment by @mgorny on 2024-11-09 13:59_

That looks kinda similar to what I've been trying to do (externally) in the past (with mitmproxy). That said, probably a well-fit pack of small test packages would still work better.

---

_Comment by @zanieb on 2024-11-09 16:24_

Regarding 

> In the past, I've seen more than once that the tests were already failing within 24 hours from publishing a new version

I consider any test that fails due to changes in PyPI a bug in our test suite — there are probably a couple cases of this left but the intention is for the test suite to be stable. Feel free to report the failures and we will fix them.

> Could you please consider a long-term goal of making the tests less dependent on third-party services and more offline?

This is absolutely a goal. It's just a ton of work. Unfortunately, there is not a clear specification for the behavior of Python package indexes and PyPI itself is non-trivial to run locally. I've explored using `devpi` and `pypiserver` but both have behavior that differs from PyPI and can't handle the concurrency of our test suite.

I've invested in improving them, but there are still blockers. For some context on `devpi`, see: 

- https://github.com/devpi/devpi/pull/1023
- https://github.com/devpi/devpi/issues/1021
- https://github.com/devpi/devpi/issues/1022
- https://github.com/devpi/devpi/issues/1020
- https://github.com/Pylons/waitress/issues/427

See also, some previous work on this topic:

- https://github.com/astral-sh/uv/pull/609
- https://github.com/astral-sh/uv/pull/5726

In the long run, I think we'll need to write our own minimal index. It's hard to find time to work on this, usage of uv is growing and we are prioritizing correctness and functionality for users over internal improvements like this. I'm sorry this makes redistribution of uv harder.


---

_Comment by @musicinmybrain on 2024-11-10 16:14_

To piggyback on this, I find that there are about 70 tests in the `-p uv --test it` target that still try to access the network even when I remove `pypi`, `git`, `crates-io`, and `python-managed` from the default features of the `uv` crate.

I’m currently dealing with this in Fedora by patching `crates/uv/tests/it/main.rs` to remove a number of groups of integration tests. I should probably offer one or more PR’s to more accurately conditionalize these tests.

However, what I’m seeing suggests that disabling these features is not necessarily routinely tested upstream, so tests with “unadvertised” dependencies on various online resources are likely to reappear in the future even if I manage to fix most of them now.

---

_Comment by @zanieb on 2024-12-04 04:44_

@musicinmybrain I'm supportive of adding Linux-only test jobs here that confirm the features are working as intended. We aren't likely to prioritize it, but if someone wants to help it would be appreciated.

---
