```yaml
number: 4434
title: uv pip list returns incomplete list of installed packages
type: issue
state: closed
author: lcheylus
labels:
  - bug
  - duplicate
  - compatibility
assignees: []
created_at: 2024-06-21T15:05:03Z
updated_at: 2024-06-23T17:23:59Z
url: https://github.com/astral-sh/uv/issues/4434
synced_at: 2026-01-12T15:58:50Z
```

# uv pip list returns incomplete list of installed packages

---

_@lcheylus_

uv 0.2.14 built from sources with Rust v1.79.0 on Debian testing (amd64)

I have a mix of Python packages installed 
- via Debian packages (`apt install python-xxx`) => `/usr/lib/python3/dist-packages/`
- via `pip` (`pip install xxx`) => `/usr/local/lib/python3.11/dist-packages/`

`uv pip list` command returns only packages installed in `/usr/local/lib/python3.11/dist-packages/` via `pip`, not those installed via Debian system packages.

```shell
$ uv pip list -vv
    0.000049s DEBUG uv uv 0.2.14 (e0ad649c7 2024-06-20)
    0.000342s DEBUG uv_toolchain::discovery Searching for Python interpreter in system toolchains
    0.000528s DEBUG uv_toolchain::discovery Found cpython 3.11.9 at `/usr/bin/python3` (search path)
    0.000559s DEBUG uv::commands::pip::list Using Python 3.11.9 environment at /usr/bin/python3
Package           Version
----------------- -------
blaeu             1.1.10
epr-reader        2.4.15
imap-tools        1.0.0
oauthlib          3.2.2
pysocks           1.7.1
requests-oauthlib 1.3.1
shtab             1.5.8
termcolor         2.2.0
tldr              3.1.0
tweepy            3.10.0

$ pip show pip
Name: pip
Version: 24.0
Summary: The PyPA recommended tool for installing Python packages.
Home-page:
Author:
Author-email: The pip developers <distutils-sig@python.org>
License: MIT
Location: /usr/lib/python3/dist-packages
Requires:
Required-by:

$ pip list --verbose
Package                     Version                   Location                                Installer
--------------------------- ------------------------- --------------------------------------- ---------
aiodns                      3.2.0                     /usr/lib/python3/dist-packages
aiohttp                     3.9.5                     /usr/lib/python3/dist-packages          debian
aiosignal                   1.3.1                     /usr/lib/python3/dist-packages
alabaster                   0.7.16                    /usr/lib/python3/dist-packages          debian
ansible-core                2.16.6                    /usr/lib/python3/dist-packages          debian
anyio                       4.3.0                     /usr/lib/python3/dist-packages          debian
apache-libcloud             3.8.0                     /usr/lib/python3/dist-packages
appdirs                     1.4.4                     /usr/lib/python3/dist-packages
archey4                     4.13.4                    /usr/lib/python3/dist-packages
argcomplete                 3.4.0                     /usr/lib/python3/dist-packages          debian
asgiref                     3.8.1                     /usr/lib/python3/dist-packages          debian
astroid                     3.2.2                     /usr/lib/python3/dist-packages          debian
asttokens                   2.4.1                     /usr/lib/python3/dist-packages          debian
async-timeout               4.0.3                     /usr/lib/python3/dist-packages
attrs                       23.2.0                    /usr/lib/python3/dist-packages          debian
autopep8                    2.1.0                     /usr/lib/python3/dist-packages          debian
Babel                       2.14.0                    /usr/lib/python3/dist-packages
beautifulsoup4              4.12.3                    /usr/lib/python3/dist-packages          debian
binwalk                     2.3.3                     /usr/lib/python3/dist-packages
black                       24.4.2                    /usr/lib/python3/dist-packages          debian
blaeu                       1.1.10                    /usr/local/lib/python3.11/dist-packages pip
blinker                     1.8.2                     /usr/lib/python3/dist-packages          debian
Bottleneck                  1.3.8                     /usr/lib/python3/dist-packages
Brotli                      1.1.0                     /usr/lib/python3/dist-packages
certifi                     2024.6.2                  /usr/lib/python3/dist-packages
cffi                        1.16.0                    /usr/lib/python3/dist-packages          debian
chardet                     5.2.0                     /usr/lib/python3/dist-packages          debian
charset-normalizer          3.3.2                     /usr/lib/python3/dist-packages          debian
click                       8.1.7                     /usr/lib/python3/dist-packages
colorama                    0.4.6                     /usr/lib/python3/dist-packages          debian
configobj                   5.0.8                     /usr/lib/python3/dist-packages          debian
contourpy                   1.0.7                     /usr/lib/python3/dist-packages          debian
cryptography                42.0.5                    /usr/lib/python3/dist-packages
cssselect                   1.2.0                     /usr/lib/python3/dist-packages
cupshelpers                 1.0                       /usr/lib/python3/dist-packages
cycler                      0.12.1                    /usr/lib/python3/dist-packages          debian
cymruwhois                  1.6                       /usr/lib/python3/dist-packages
dbus-python                 1.3.2                     /usr/lib/python3/dist-packages
decorator                   5.1.1                     /usr/lib/python3/dist-packages
defusedxml                  0.7.1                     /usr/lib/python3/dist-packages
Deprecated                  1.2.14                    /usr/lib/python3/dist-packages
deprecation                 2.0.7                     /usr/lib/python3/dist-packages
devscripts                  2.23.7                    /usr/lib/python3/dist-packages
dill                        0.3.8                     /usr/lib/python3/dist-packages          debian
distlib                     0.3.8                     /usr/lib/python3/dist-packages
distro                      1.9.0                     /usr/lib/python3/dist-packages          debian
dnspython                   2.6.1                     /usr/lib/python3/dist-packages          debian
docopt                      0.6.2                     /usr/lib/python3/dist-packages
docstring-to-markdown       0.15                      /usr/lib/python3/dist-packages
docutils                    0.20.1                    /usr/lib/python3/dist-packages
epr-reader                  2.4.15                    /usr/local/lib/python3.11/dist-packages pip
et-xmlfile                  1.0.1                     /usr/lib/python3/dist-packages
(...)
```



---

_Label `bug` added by @zanieb on 2024-06-21 22:04_

---

_Label `compatibility` added by @zanieb on 2024-06-21 22:04_

---

_Comment by @zanieb on 2024-06-21 22:04_

Thanks for the report!

---

_Comment by @ChannyClaus on 2024-06-21 22:14_

sounds vaguely similar to https://github.com/astral-sh/uv/issues/2500...

---

_Comment by @zanieb on 2024-06-21 22:19_

Ah thanks I was looking for that one but couldn't find it.

---

_Comment by @lcheylus on 2024-06-22 08:04_

> sounds vaguely similar to #2500...

I agree. I read #2500 before creating this issue but I preferred to create a specific issue for my case.

---

_Comment by @charliermarsh on 2024-06-23 15:54_

Ah yeah, this is the same as https://github.com/astral-sh/uv/issues/2500 -- you have system packages in multiple places. Merging with that issue.

---

_Closed by @charliermarsh on 2024-06-23 15:54_

---

_Label `duplicate` added by @zanieb on 2024-06-23 17:23_

---
