```yaml
number: 9550
title: Different package names produced by uv pip freeze
type: issue
state: closed
author: LordAro
labels: []
assignees: []
created_at: 2024-12-01T10:19:39Z
updated_at: 2024-12-02T23:28:42Z
url: https://github.com/astral-sh/uv/issues/9550
synced_at: 2026-01-12T15:59:53Z
```

# Different package names produced by uv pip freeze

---

_@LordAro_

Seems to produce different outputs for packages containing `.` or `_` or `-`

Causes issues with lockfile comparisons.

Does seem a bit confusing - e.g. https://pypi.org/project/importlib-resources - the pypi package name is importlib-resources, but the package name that actually gets downloaded is `importlib_resources` (as is the actual package - https://github.com/python/importlib_resources/blob/d20000ee1c028588aa1eb8b60363539dca7bce8b/pyproject.toml#L6 )

`uv pip freeze`

```
aiohappyeyeballs==2.4.4
aiohttp==3.10.11
aiosignal==1.3.1
async-timeout==5.0.1
attrs==24.2.0
autocommand==2.2.2
backports-tarfile==1.2.0
certifi==2024.8.30
click==8.1.7
discord-py==2.4.0
frozenlist==1.5.0
idna==3.10
importlib-resources==6.4.5
irc==20.5.0
jaraco-collections==5.1.0
jaraco-context==6.0.1
jaraco-functools==4.1.0
jaraco-logging==3.3.0
jaraco-stream==3.0.4
jaraco-text==4.0.0
more-itertools==10.5.0
multidict==6.1.0
openttd-helpers==1.4.0
pip==23.0.1
propcache==0.2.0
python-dateutil==2.9.0.post0
pytz==2024.2
sentry-sdk==2.19.0
setuptools==56.0.0
six==1.16.0
tempora==5.7.0
typing-extensions==4.12.2
urllib3==2.2.3
yarl==1.15.2
zipp==3.20.2
```

vs `python -m pip freeze`

```
aiohappyeyeballs==2.4.4
aiohttp==3.10.11
aiosignal==1.3.1
async-timeout==5.0.1
attrs==24.2.0
autocommand==2.2.2
backports.tarfile==1.2.0
certifi==2024.8.30
click==8.1.7
discord.py==2.4.0
frozenlist==1.5.0
idna==3.10
importlib_resources==6.4.5
irc==20.5.0
jaraco.collections==5.1.0
jaraco.context==6.0.1
jaraco.functools==4.1.0
jaraco.logging==3.3.0
jaraco.stream==3.0.4
jaraco.text==4.0.0
more-itertools==10.5.0
multidict==6.1.0
openttd-helpers==1.4.0
propcache==0.2.0
python-dateutil==2.9.0.post0
pytz==2024.2
sentry-sdk==2.19.0
six==1.16.0
tempora==5.7.0
typing_extensions==4.12.2
urllib3==2.2.3
yarl==1.15.2
zipp==3.20.2
```

May be related to #3141 but I couldn't find anything specifically related to the mismatch

---

_Comment by @konstin on 2024-12-01 10:43_

In uv, we always use the [normalized name](https://packaging.python.org/en/latest/specifications/name-normalization/) with `-` over `_` and `.` when writing a lockfile for consistency. This avoids uv having different spellings from different version in universal resolution.

---

_Comment by @LordAro on 2024-12-01 11:25_

Definitely makes sense generally, but perhaps not for the pip interface?

---

_Comment by @charliermarsh on 2024-12-01 12:46_

Ah yeah, I consider this intentional.

---

_Closed by @charliermarsh on 2024-12-02 03:41_

---

_Comment by @zanieb on 2024-12-02 23:28_

We should add a note to https://docs.astral.sh/uv/pip/compatibility/

---
