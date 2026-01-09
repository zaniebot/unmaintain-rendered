---
number: 9506
title: Enable configuration of additional paths when detecting system pythons
type: issue
state: open
author: Stealthii
labels:
  - wish
  - configuration
assignees: []
created_at: 2024-11-28T16:57:22Z
updated_at: 2025-04-22T17:26:37Z
url: https://github.com/astral-sh/uv/issues/9506
synced_at: 2026-01-07T13:12:18-06:00
---

# Enable configuration of additional paths when detecting system pythons

---

_Issue opened by @Stealthii on 2024-11-28 16:57_

The high level ask here is to provide a **setting** configuration option to provide additional paths to search when resolving system Pythons.

---

On Enterprise Linux systems, it's common for additional packages to support products to be installed on non-system paths (such as `/opt/`). To use a public example, on CentOS 7 (and other RHEL7 derivatives) the Software Collections List ([CentOS](https://wiki.centos.org/SpecialInterestGroup(2f)SCLo.html), [RHEL](https://docs.redhat.com/en/documentation/red_hat_software_collections/3/html/3.8_release_notes/chap-usage#sect-Usage-Use-Executable)) provides a build of Python 3.8 (RPM: `rh-python38`) that has an interpreter path of `/opt/rh/rh-python38/root/bin/python` among its supporting files.

`uv` can be packaged and installed as an RPM with supporting config for the environment (such as internal PyPI mirrors or `python-preference = "only-system"`) at `/etc/uv/uv.toml`, however there are no provisions to provide additional paths to discover "system" Python installations. To enable interpreter auto-discovery without overriding `PATH` for an environment, a shim can be used (to replace `uv` and `uvx`):
```sh
#!/bin/sh
# uv shim script

# Add known internal Python versions to PATH if they exist
[ -d /opt/company/python311/bin ] && export PATH="/opt/company/python311/bin:${PATH}"
[ -d /opt/company/python39/bin ] && export PATH="/opt/company/python39/bin:${PATH}"
[ -d /opt/rh/rh-python38/root/bin ] && export PATH="/opt/rh/rh-python38/root/bin:${PATH}"

# Determine the actual binary to execute based on the name this script was called with
if [ "$(basename "$0")" = "uvx" ]; then
	exec /usr/lib/company-uv/bin/uvx "$@"
else
	exec /usr/lib/company-uv/bin/uv "$@"
fi
```
Rather than shim, and to avoid PATH overrides for autodiscovery, it would be nice to be able to provide additional (or override) paths when resolving available system Python installations.

---

_Comment by @Stealthii on 2024-11-28 17:04_

Please consider this low priority from a user-standpoint, as PATH overrides in a build environment are common and a supported method of doing this.

To provide a contextual example, we may be producing an RPM targeting Python 3.11, containing a virtualenv built with uv and supporting scriptlets. If we decide to target el7 through el9:
* We should inform uv of `-p 3.11` as our desired target
* uv would resolve `/opt/company/python311/bin/python3.11` on an el7 build container, and `/usr/bin/python3.11` on el9
* RPM dependency resolution will require "company-python311" on el7, and "python3.11" on el9

---

_Label `wish` added by @charliermarsh on 2024-11-29 16:38_

---

_Label `configuration` added by @charliermarsh on 2024-11-29 16:38_

---

_Comment by @lgarrison on 2025-02-11 22:35_

To add different use-case where a `PATH` shim wouldn't work quite as well: on our cluster we have multiple Python modules (installations), each with some pre-installed packages. I'd like uv to discover these installations without the user having to load a given module first. The reason is mostly convenience, but also to eliminate surprises when you run `uv venv -p 3.11 --system-site-packages` but forget to run `module load python/3.11` first. This will result in a venv without the site packages that you expected, because the OS python was found.  We don't want to totally disallow managed Python, so I don't think `UV_PYTHON_PREFERENCE=only-system` is exactly what we want either.

And I'd rather not inject these Pythons into the `PATH` via a uv/uvx shim because each Python has quite a few pre-installed packages that place scripts in the `bin` dir (over 200 scripts each). Having all these scripts on the path simultaneously, corresponding to different Python versions, seems like a recipe for tricky bugs.

For us, I think the ideal solution is similar to the initial request: a setting that accepts a list of additional Python interpreters to discover.

In terms of what the setting could look like, a `UV_SYSTEM_PYTHONS` environment variable that accepted a list of Python interpreters would be one option. Then we could keep a versioned `uv` module that would set that variable. We might be able to make a system-level configuration file work instead, but we'd probably need a way to select this file via an environment variable since we have multiple module sets.

---

_Referenced in [astral-sh/uv#11308](../../astral-sh/uv/issues/11308.md) on 2025-02-11 22:42_

---

_Comment by @mneumei on 2025-03-06 12:36_

And: more and more applications ship some own python, which is a reason PATH can be cluttered with python versions you really don´t want to be used for your execution

---

_Comment by @Stealthii on 2025-03-06 16:14_

> And: more and more applications ship some own python, which is a reason PATH can be cluttered with python versions you really don´t want to be used for your execution

Arguably, that's a failing in the application for cluttering global PATH, rather than using its own PATH extensions when executing. The request here is sort of the opposite - providing additional paths to search for Python distributions that are **not** part of the executed PATH env.

---

_Comment by @MayeulC on 2025-04-22 17:26_

Ah, I was just creating an issue on this topic, and finally found this one when looking for other relevant issues to link.

I will instead add the content of my issue text below, it may be relevant.

----
Title: Make system python search path configurable

<details><summary>Summary</summary>

In short: I would like the Python system installation discovery mechanism to examine a second list, independent of PATH.

Changing `PATH` may not be appropriate due to the side effects, therefore providing an alternate way tell `uv` where to look for Python versions is desirable.

My current use-case is: we already have multiple Python versions installed on our machines, in separate prefixes, and would like `uv` to leverage them.
</details>

Example:
<details><summary>I would suggest introducing a new setting and environment variable.</summary>

* `UV_PYTHON_SEARCH_PATH=/some/location/python3.13/bin/`
* `python-search-path` in `uv.conf` or `pyproject.toml`
  * Could be either a list
    ```toml
    python-search-path = [
        "/some/location/python3.13/bin",
        "/some/location/python3.14/bin",
    ]
    ```
  * or key/value pairs (may be less desirable? More complex to implement, anyway):
     ```toml
     [python-search-path]
     "3.11-clang" = "/some/location/python3.11-clang/bin"
     "3.11-gcc" = "/some/location/python3.11-gcc/bin"
    ```

An alternate name could be `PYTHON_DISCOVERY_PATH`.
</details>

<details><summary>Code that would be impacted</summary>

* New source at https://github.com/astral-sh/uv/blob/45910eb6d1152903d54b9bc3a607630898e2d862/crates/uv-python/src/discovery.rs#L187
* Repeat search path search on the additional path:
  * https://github.com/astral-sh/uv/blob/473d7c75a4e8a99a14bbbca459c493acb717fae2/crates/uv-python/src/discovery.rs#L338
* I would likely parametrize this function with the path: https://github.com/astral-sh/uv/blob/473d7c75a4e8a99a14bbbca459c493acb717fae2/crates/uv-python/src/discovery.rs#L477
* And then duplicate its call or iterate on *NEW_VAR*, *PATH*: https://github.com/astral-sh/uv/blob/473d7c75a4e8a99a14bbbca459c493acb717fae2/crates/uv-python/src/discovery.rs#L338
* Needs a discussion on whether this would just prepend to `from_search_path`, or impact search preference in some other way (I could see some wanting to disable `PATH` scanning while allowing this new setting): https://github.com/astral-sh/uv/blob/473d7c75a4e8a99a14bbbca459c493acb717fae2/crates/uv-python/src/discovery.rs#L399
* Config parsing

I can work towards a PR if wanted.
</details>

Incidentally, I see in the code that `UV_TEST_PYTHON_PATH` overrides `PATH`, which could be a suitable temporary workaround (`TEST`, it being intended for unit testing, and no documentation probably mean that this should not be relied upon).


---

_Referenced in [tox-dev/tox-uv#170](../../tox-dev/tox-uv/issues/170.md) on 2025-06-20 10:40_

---
