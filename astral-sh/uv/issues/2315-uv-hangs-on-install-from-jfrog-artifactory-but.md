```yaml
number: 2315
title: uv hangs on install from JFrog Artifactory, but only when building source distributions
type: issue
state: closed
author: skellys
labels:
  - registry
  - needs-mre
assignees: []
created_at: 2024-03-09T03:34:34Z
updated_at: 2024-04-22T20:27:42Z
url: https://github.com/astral-sh/uv/issues/2315
synced_at: 2026-01-12T15:58:37Z
```

# uv hangs on install from JFrog Artifactory, but only when building source distributions

---

_@skellys_

`uv` version 0.1.16 hangs indefinitely on certain installs for me when using `--index-url` to install from JFrog Artifactory. I am not able to get this to happen when installing from the usual Internet/pypi.org. Fortunately, I am able to reliably reproduce my issue with a particular mix of 3 old, simple packages together, or when they're a part of a larger `requirements.txt`:

## Repro

to repro, run in the official `python:3.11-slim` docker image:

```shell
python3 -m venv testenv
. testenv/bin/activate

# install uv from Internet
pip install uv

# install these 3 packages from an Artifactory cache endpoint
uv pip install --index-url "https://$USERNAME:$TOKEN@subdomainnamegoeshere.jfrog.io/artifactory/api/pypi/pypi/simple" binpacking==1.5.2 expbackoff==0.1.1 frozendict==2.4.0 --no-deps --no-cache --verbose
```

the end of the verbose logs before it hangs:

```
...
<similar for expbackoff==0.1.1>
...
<similar for binpacking==1.5.2>
...
DEBUG uv_distribution::source::download_source_dist filename="frozendict-2.4.0.tar.gz", source_dist=frozendict==2.4.0
uv_distribution::source::build_source_dist
DEBUG uv_distribution::source Building: frozendict==2.4.0
uv_dispatch::setup_build package_id="frozendict==2.4.0", subdirectory=None
```

## Workaround
Pretty simple - just do regular `pip install` for those upfront, then `uv pip install` for the rest!

Can anyone else using JFrog Artifactory reproduce this issue? I ran this on an EC2 instance in AWS region us-east-1.


---

_Comment by @charliermarsh on 2024-03-09 13:05_

Thanks, I think I'll need to setup an Artifactory repo to reproduce.

---

_Label `registry` added by @charliermarsh on 2024-03-09 13:09_

---

_Label `needs-mre` added by @charliermarsh on 2024-03-09 13:09_

---

_Comment by @cmwetherell on 2024-03-09 15:57_

Tangential, is there a way to specify the index-url for a project? Without needing to specify it for each new package install.

---

_Comment by @zanieb on 2024-03-09 16:00_

You can use an environment variable e.g. `UV_INDEX_URL` — we'll add support for reading a persistent configuration file in the future too.

---

_Comment by @skellys on 2024-04-22 20:27_

@charliermarsh Following up - I think we can close this issue. I'm no longer able to repro this as of version 0.1.18. There was another (separate) issue where I got HTTP 401 (unauthorized) errors for versions 0.1.22, 0.1.23, but I didn't observe that behavior again as of 0.1.24. Everything seems to work as of the latest version (at time of writing: 0.1.35).

---

_Comment by @charliermarsh on 2024-04-22 20:27_

Oh great! Thank you for following up.

---

_Closed by @charliermarsh on 2024-04-22 20:27_

---
