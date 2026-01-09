---
number: 17243
title: UV_FROZEN=1 uvx should not ignore lockfile check
type: issue
state: closed
author: xiaoxiangmoe
labels:
  - bug
assignees: []
created_at: 2025-12-27T20:36:27Z
updated_at: 2026-01-05T14:36:39Z
url: https://github.com/astral-sh/uv/issues/17243
synced_at: 2026-01-07T13:12:19-06:00
---

# UV_FROZEN=1 uvx should not ignore lockfile check

---

_Issue opened by @xiaoxiangmoe on 2025-12-27 20:36_

### Summary

I want use uv single file script with ty, so I create a file `docker-client.py`

#### step 1:  env prepare
```sh
> cat docker-client.py
#!/usr/bin/env -S uv run --script
#
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "docker>=7.1.0",
#     "ty>=0.0.7",
# ]
# ///

import docker

print(docker.from_env())
```
<details>
You might be wondering why I added ty to dependencies:

my intention was to add it to dev dependencies.
But when I run `uv add --script docker-client.py --dev ty` , uv will throw error
```
error: the argument '--script <SCRIPT>' cannot be used with '--dev'
```
</details>

#### step 2:  run script and type check
Run it works well

```sh
> chmod a+x docker-client.py && uv lock --script ./docker-client.py && UV_FROZEN=1 ./docker-client.py
Resolved 8 packages in 0.28ms
<docker.client.DockerClient object at 0x7eff4e4457f0>
```

Run type check, it also works well.

```sh
> UV_FROZEN=1 uvx --with-requirements docker-client.py ty check docker-client.py
Installed 7 packages in 2ms
All checks passed!
```
See also: [Does ty support PEP 723 inline-metadata scripts?](https://docs.astral.sh/ty/reference/typing-faq/#does-ty-support-pep-723-inline-metadata-scripts)

#### step 3. break lockfile

Then I try to break lockfile

```sh
> sed -i 's/^requires-python = ">=3\.13"$/requires-python = ">=4.13"/' ./docker-client.py.lock
```

#### step 4. rerun script and type check

##### Successfully detected inconsistencies

Run script, break as intended

```sh
> grep 'requires-python' docker-client.py.lock && UV_FROZEN=1 ./docker-client.py
requires-python = ">=4.13"
error: The current Python version (3.13.3) is not compatible with the locked Python requirement: `>=4.13`
```

##### No inconsistency found
Run type check:

```sh
> grep 'requires-python' docker-client.py.lock && UV_FROZEN=1 uvx --with-requirements docker-client.py ty check docker-client.py
requires-python = ">=4.13"
All checks passed!
```
It runs well, without check `docker-client.py.lock`

### Platform

x86_64 GNU/Linux

### Version

uv 0.9.17

### Python version

Python 3.13.3

---

_Label `bug` added by @xiaoxiangmoe on 2025-12-27 20:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-04 19:22_

---

_Unassigned @charliermarsh by @charliermarsh on 2026-01-04 19:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-04 20:52_

---

_Referenced in [astral-sh/uv#17322](../../astral-sh/uv/pulls/17322.md) on 2026-01-05 00:26_

---

_Comment by @konstin on 2026-01-05 13:13_

I'm a bit confused: `--with-requirements` only read the requirements file, not the associated lockfile. Should uv really error when the lockfile is outdated even though it's not reading the lockfile? (The problem with reading the lockfile is that you can pass `--with-requirements` multiple times, and then all the lockfiles have to match)

---

_Comment by @charliermarsh on 2026-01-05 14:27_

I did consider it and I think we _should_ be updating the lockfiles here. It matches our semantics everywhere else.

---

_Comment by @charliermarsh on 2026-01-05 14:36_

> Can we document this, either in --with-requirements or in some locking docs? Otherwise users will be confused that the lockfile gets updated but not used.

Ahh, it didn't occur to me that (of course) we aren't using the lockfile here. In that case you might be right.


---

_Closed by @charliermarsh on 2026-01-05 14:36_

---
