```yaml
number: 2493
title: "feat: add linehaul info to uv-client"
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
assignees: []
merged: true
base: main
head: linehaul
created_at: 2024-03-17T03:29:44Z
updated_at: 2024-03-20T17:41:44Z
url: https://github.com/astral-sh/uv/pull/2493
synced_at: 2026-01-10T14:49:08Z
```

# feat: add linehaul info to uv-client

---

_Pull request opened by @samypr100 on 2024-03-17 03:29_

## Summary

Closes #1958

This adds linehaul metadata to uv's user-agent when pep 508 markers are provided to the RegistryClientBuilder. Thanks to #2381, we were able to leverage most information from markers and avoid inconsistency.

Linehaul is meant to be accompanying metadata pip sends in it's user agent when talking to registries. You can see this output by running something like `python -c 'from pip._internal.network.session import user_agent; print(user_agent())'`. 
In PyPI, this metadata processed by the [linehaul-cloud-function](https://github.com/pypi/linehaul-cloud-function). More info about linehaul can be found in #1958.

Below are some examples from pip:

* Linux GHA: `pip/24.0 {"ci":true,"cpu":"x86_64","distro":{"id":"jammy","libc":{"lib":"glibc","version":"2.35"},"name":"Ubuntu","version":"22.04"},"implementation":{"name":"CPython","version":"3.12.2"},"installer":{"name":"pip","version":"24.0"},"openssl_version":"OpenSSL 3.0.2 15 Mar 2022","python":"3.12.2","rustc_version":"1.76.0","system":{"name":"Linux","release":"6.5.0-1016-azure"}}`
* Windows GHA: `pip/24.0 {"ci":true,"cpu":"AMD64","implementation":{"name":"CPython","version":"3.12.2"},"installer":{"name":"pip","version":"24.0"},"openssl_version":"OpenSSL 3.0.13 30 Jan 2024","python":"3.12.2","rustc_version":"1.76.0","system":{"name":"Windows","release":"2022Server"}}`
* OSX GHA: `pip/24.0 {"ci":true,"cpu":"arm64","distro":{"name":"macOS","version":"14.2.1"},"implementation":{"name":"CPython","version":"3.12.2"},"installer":{"name":"pip","version":"24.0"},"openssl_version":"OpenSSL 3.0.13 30 Jan 2024","python":"3.12.2","rustc_version":"1.76.0","system":{"name":"Darwin","release":"23.2.0"}}`



Here's how uv results look like (sorry for the keys not having the same order):

* Linux GHA: `uv/0.1.21 {"installer":{"name":"uv","version":"0.1.21"},"python":"3.12.2","implementation":{"name":"CPython","version":"3.12.2"},"distro":{"name":"Ubuntu","version":"22.04","id":"jammy","libc":null},"system":{"name":"Linux","release":"6.5.0-1016-azure"},"cpu":"x86_64","openssl_version":null,"setuptools_version":null,"rustc_version":null,"ci":true}`
* Windows GHA: `uv/0.1.21 {"installer":{"name":"uv","version":"0.1.21"},"python":"3.12.2","implementation":{"name":"CPython","version":"3.12.2"},"distro":null,"system":{"name":"Windows","release":"2022Server"},"cpu":"AMD64","openssl_version":null,"setuptools_version":null,"rustc_version":null,"ci":true}`
* OSX GHA: `uv/0.1.21 {"installer":{"name":"uv","version":"0.1.21"},"python":"3.12.2","implementation":{"name":"CPython","version":"3.12.2"},"distro":{"name":"macOS","version":"14.2.1","id":null,"libc":null},"system":{"name":"Darwin","release":"23.2.0"},"cpu":"arm64","openssl_version":null,"setuptools_version":null,"rustc_version":null,"ci":true}`

Distro information (such as the one pip uses `from pip._vendor import distro` to retrieve instead of `platform` module) was not retrieved from markers. Instead, the linux release codename/name/version uses `sys-info` crate, adding about 50us of extra overhead on linux. The distro osx version re-used the [mac_os version implementation](https://github.com/astral-sh/uv/blob/99c992e38b220fbcda09b0b43602b3db2321480b/crates/platform-host/src/mac_os.rs) from #2381 which adds about 20us of overhead on osx. I tried to use other crates to avoid re-introducing `mac_os.rs` but most of them didn't yield satisfactory performance (40ms-60ms~) or had the wrong values needed (e.g. darwin version vs osx version).

I also didn't add rustc retrieval as those seem to add substantial overhead due to querying `rustc`. PyPy version detection was also not added to avoid adding extra overhead to [support PyPy for linehaul](https://github.com/pypa/pip/blob/24.0/src/pip/_internal/network/session.py#L123). All other behavior was kept 1-1 to match what pip's linehaul implementation does (as of 24.0). This also aligns with what was discussed in #1958.

## Test Plan

Added new integration test to uv-client.

---

_Marked ready for review by @samypr100 on 2024-03-17 03:40_

---

_Comment by @konstin on 2024-03-18 10:36_

> I also didn't add libc retrieval or rustc retrieval as those seem to add substantial overhead due to querying ldd or rustc.

We now have this information for the python interpreter itself, i added it.

> Distro information (such as the one pip uses from pip._vendor import distro to retrieve instead of platform module) was not retrieved from markers. Instead, the linux release codename/name/version uses sys-info crate, adding about 50us of extra overhead on linux. The distro osx version re-used the [mac_os version implementation](https://github.com/astral-sh/uv/blob/99c992e38b220fbcda09b0b43602b3db2321480b/crates/platform-host/src/mac_os.rs) from https://github.com/astral-sh/uv/pull/2381 which adds about 20us of overhead on osx. I tried to use other crates to avoid re-introducing mac_os.rs but most of them didn't yield satisfactory performance (40ms-60ms~) or had the wrong values needed (e.g. darwin version vs osx version).

Great performance work! On my machine (ubuntu) we're at 0.000s and even relative to the 2ms warm cache black resolve the linehaul instantiation is fast.

**Warm cache resolve black**
![Warm cache resolve black](https://github.com/astral-sh/uv/assets/6826232/7c626b78-a7ba-4bf5-a453-6b243ec2c6ce)


---

_Merged by @konstin on 2024-03-18 10:46_

---

_Closed by @konstin on 2024-03-18 10:46_

---

_@samypr100 reviewed on 2024-03-18 12:09_

---

_Review comment by @samypr100 on `crates/uv/src/commands/pip_install.rs`:195 on 2024-03-18 12:09_

Thanks for the edits @konstin

I think this is missing adding `.platform(venv.interpreter().platform())`

---

_@samypr100 reviewed on 2024-03-18 12:09_

---

_Review comment by @samypr100 on `crates/uv/src/commands/pip_sync.rs`:129 on 2024-03-18 12:09_

This is also missing `.platform(venv.interpreter().platform())`

---

_@samypr100 reviewed on 2024-03-18 12:09_

---

_Review comment by @samypr100 on `crates/uv/src/commands/venv.rs`:154 on 2024-03-18 12:09_

This also is missing `.platform(interpreter.platform())`

---

_@konstin reviewed on 2024-03-18 12:17_

---

_Review comment by @konstin on `crates/uv/src/commands/pip_install.rs`:195 on 2024-03-18 12:17_

Thank you! #2507

---

_Label `enhancement` added by @konstin on 2024-03-18 12:17_

---

_Branch deleted on 2024-03-18 12:45_

---

_@charliermarsh reviewed on 2024-03-18 13:01_

---

_Review comment by @charliermarsh on `crates/uv-client/src/mac_version.rs`:5 on 2024-03-18 13:01_

Do we need this? I thought we got it from the interpreter now.

---

_@charliermarsh reviewed on 2024-03-18 13:01_

Thank you!

---

_Review comment by @konstin on `crates/uv-client/src/mac_version.rs`:5 on 2024-03-18 13:28_

Oh that's the same thing, thanks https://github.com/astral-sh/uv/pull/2509

---

_@konstin reviewed on 2024-03-18 13:28_

---

_@samypr100 reviewed on 2024-03-18 13:45_

---

_Review comment by @samypr100 on `crates/uv-client/src/mac_version.rs`:5 on 2024-03-18 13:45_

Note, it's not quite just major.minor, it should include the product version from the plist as-is based on my testing with pip.

---

_@konstin reviewed on 2024-03-18 13:59_

---

_Review comment by @konstin on `crates/uv-client/src/mac_version.rs`:5 on 2024-03-18 13:59_

Happy to hand this off to a mac user who can test this on an actual mac

---

_Comment by @MichaReiser on 2024-03-20 17:03_

I tried to query the big-data table for `uv` downloads but there are no uv records yet (but the latest ingested data is from today). I think the reason for it is that we need to add a `uv` parser in here https://github.com/pypi/linehaul-cloud-function/blob/main/linehaul/ua/parser.py

---

_Comment by @samypr100 on 2024-03-20 17:41_

Yes, a parser can now be added since the data is being sent as of 0.1.22 ❤️ 

---
