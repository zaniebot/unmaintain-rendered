---
number: 12434
title: "Invalid dash-separated key 'description-file' on aiosonic, pytest-watch appeared in last hour"
type: issue
state: closed
author: thundergolfer
labels:
  - external
assignees: []
created_at: 2025-03-24T15:47:16Z
updated_at: 2025-03-25T01:50:31Z
url: https://github.com/astral-sh/uv/issues/12434
synced_at: 2026-01-07T13:12:18-06:00
---

# Invalid dash-separated key 'description-file' on aiosonic, pytest-watch appeared in last hour

---

_Issue opened by @thundergolfer on 2025-03-24 15:47_

### Summary

Can minimally reproduce with:

`uv pip install datadog-api-client[async]~=2.33.1`

```
Using Python 3.11.11 environment at: venv
  × Failed to build `aiosonic==0.15.1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stderr]
      <string>:5: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpDeYjkr/lib/python3.11/site-packages/setuptools/build_meta.py", line 334, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpDeYjkr/lib/python3.11/site-packages/setuptools/build_meta.py", line 304, in _get_build_requires
          self.run_setup()
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpDeYjkr/lib/python3.11/site-packages/setuptools/build_meta.py", line 522, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpDeYjkr/lib/python3.11/site-packages/setuptools/build_meta.py", line 320, in run_setup
          exec(code, locals())
        File "<string>", line 50, in <module>
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpDeYjkr/lib/python3.11/site-packages/setuptools/__init__.py", line 116, in setup
          _install_setup_requires(attrs)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpDeYjkr/lib/python3.11/site-packages/setuptools/__init__.py", line 87, in _install_setup_requires
          dist.parse_config_files(ignore_option_errors=True)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpDeYjkr/lib/python3.11/site-packages/_virtualenv.py", line 20, in parse_config_files
          result = old_parse_config_files(self, *args, **kwargs)
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpDeYjkr/lib/python3.11/site-packages/setuptools/dist.py", line 730, in parse_config_files
          self._parse_config_files(filenames=inifiles)
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpDeYjkr/lib/python3.11/site-packages/setuptools/dist.py", line 599, in _parse_config_files
          opt = self._enforce_underscore(opt, section)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/ubuntu/.cache/uv/builds-v0/.tmpDeYjkr/lib/python3.11/site-packages/setuptools/dist.py", line 629, in _enforce_underscore
          raise InvalidConfigError(
      setuptools.errors.InvalidConfigError: Invalid dash-separated key 'description-file' in 'metadata' (setup.cfg), please use the underscore name 'description_file'
      instead.

      hint: This usually indicates a problem with the package or the build environment.
  help: `aiosonic` (v0.15.1) was included because `datadog-api-client[async]` (v2.33.1) depends on `aiosonic==0.15.1`
```

Also happens with `pytest-marker`. Our Github Actions based CI/CD system just started throwing these errors, so it seems that something inside `uv`, or `setuptools`, or PyPi has changed very recently. 

### Platform

Linux 5.15.0-1077-aws x86_64 GNU/Linux

### Version

uv 0.6.9

### Python version

Python 3.11.11

---

_Label `bug` added by @thundergolfer on 2025-03-24 15:47_

---

_Comment by @charliermarsh on 2025-03-24 15:48_

This is a change in the latest `setuptools` release: https://setuptools.pypa.io/en/stable/history.html#v78-0-0

You can pin your `setuptools` version via [build constraints](https://docs.astral.sh/uv/reference/settings/#build-constraint-dependencies).


---

_Comment by @charliermarsh on 2025-03-24 15:50_

Let me see if I can add a dedicated hint for this.

---

_Comment by @thundergolfer on 2025-03-24 15:53_

Cheers, figured it wasn't a `uv` thing but also figured others would come here and search the string "Invalid dash-separated key". 

Thanks as always for the super fast responses! ❤ 

---

_Comment by @charliermarsh on 2025-03-24 15:54_

No worries. Someone brought this up on Discord ~30 minutes ago so I was expecting to get a few issues along these lines. Unfortunately, I think this will affect a large number of projects.

---

_Comment by @thundergolfer on 2025-03-24 15:59_

The build constraint suggestion worked, so happy for you to close this if that's best 👍 

```
[tool.uv]
# Ensure that the setuptools is pinned whenever a package has a build dependency
# on setuptools. https://github.com/astral-sh/uv/issues/12434
build-constraint-dependencies = ["setuptools~=77.0.3"]
```

---

_Comment by @jacob-bush-shopify on 2025-03-24 16:01_

:wave: Just for search visibility, I also noticed this issue when trying to install `apache-airflow-providers-google` due to its dependency on on `json-merge-patch==0.2`

As above, constraining `setuptools` works well:
```toml
[tool.uv]
build-constraint-dependencies = ["setuptools<78"]
```

Edit: I have also noticed some flakiness. Will comment below.

---

_Comment by @thundergolfer on 2025-03-24 16:06_

Also looks like this fix requires a relatively new version of `uv`  (>=0.6.2)

https://github.com/astral-sh/uv/pull/11585

---

_Referenced in [astral-sh/uv#12438](../../astral-sh/uv/pulls/12438.md) on 2025-03-24 16:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-24 16:10_

---

_Label `bug` removed by @charliermarsh on 2025-03-24 16:10_

---

_Label `external` added by @charliermarsh on 2025-03-24 16:10_

---

_Comment by @donielix on 2025-03-24 16:25_

For me, the workaround setting build-constraint-dependencies doesn't work

---

_Comment by @zanieb on 2025-03-24 16:28_

@donielix Can you share more details please?

---

_Comment by @zanieb on 2025-03-24 16:28_

Cross-linking to https://github.com/pypa/setuptools/pull/4870

---

_Comment by @darkwind-noveo on 2025-03-24 16:29_

@zanieb having same issues. proposed solutions do not help. tried both versions of pinning setuptools and tried uv 0.6.9 and lesser version
```
[tool.uv]
# Ensure that the setuptools is pinned whenever a package has a build dependency
# on setuptools. https://github.com/astral-sh/uv/issues/12434
build-constraint-dependencies = ["setuptools<78"]
```

Error swith
```
        File "/root/.cache/uv/builds-v0/.tmpt66fDB/lib/python3.11/site-packages/setuptools/dist.py", line 629, in _enforce_underscore
          raise InvalidConfigError(
      setuptools.errors.InvalidConfigError: Invalid dash-separated key 'description-file' in 'metadata' (setup.cfg), please use the underscore name 'description_file' instead.

      hint: This usually indicates a problem with the package or the build environment.
  help: `direct-sdk-python3` (v1.10.0) was included because `app` (v0.1.0) depends on `direct-sdk-python3`
```
(    "direct-sdk-python3==1.10.0", is needing to be installed, it has similar issues to pytest-watch which i had too )
Using python3.11 `FROM python:3.11-buster`

---

_Comment by @charliermarsh on 2025-03-24 16:31_

The most likely explanation is that uv is not being configured correctly. Is this a `pyproject.toml` in the current working directory?


---

_Comment by @darkwind-noveo on 2025-03-24 16:34_

@charliermarsh yes, in current working directory, i am in the same folder is it is
content of pyproject.toml
```toml
[tool.black]
target-version = ['py310']
exclude = '''
/(
  | migrations
)/
'''

# [[tool.uv.index]]
# name = "our_pypi"
# url = "blabla"
# default = true

[tool.uv]
# Ensure that the setuptools is pinned whenever a package has a build dependency
# on setuptools. https://github.com/astral-sh/uv/issues/12434
build-constraint-dependencies = ["setuptools<78"]

[project]
name = "lyria"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
    "direct-sdk-python3==1.10.0",
    # other stuff
]
```

Installed in docker as
```docker
FROM python:3.11-buster AS builder

# Install uwsgi
RUN pip install uv==0.6.9 setuptools~=77.0.3

# Setup working directory
RUN mkdir /code
WORKDIR /code

# Install requirements (doing this before copying code improves caching)
ENV UV_PYTHON_PREFERENCE=only-system
ENV UV_PROJECT_ENVIRONMENT=/usr/local
COPY pyproject.toml uv.lock /code/
RUN uv sync --frozen
```

---

_Comment by @donielix on 2025-03-24 16:35_

I have the pyproject.toml file in the root of the repo, and all the configurations seems to be ok. The only difference is that we need to use a custom artifactory (JFrog).


---

_Comment by @charliermarsh on 2025-03-24 16:37_

We must not be propagating these in `uv sync` somewhere. I'll take a look.

---

_Comment by @jacob-bush-shopify on 2025-03-24 16:38_

I think there may be an issue with `uv sync`

Here is a `pyproject.toml` (`.python-version` is 3.12)
```toml
[project]
name = "test-uv-sync-with-setuptools"
version = "0.1.0"
dependencies = [
    "apache-airflow-providers-google>=14.0.0",
]

[tool.uv]
build-constraint-dependencies = ["setuptools<78"]
```

```
> uv cache clean
> rm -r .venv
> uv sync
[...]
  × Failed to build `json-merge-patch==0.2`
[...]
```

However, I can use `uv pip install` to install this without issue
```
❯ uv pip install json-merge-patch
Resolved 1 package in 769ms
      Built json-merge-patch==0.2
Prepared 1 package in 290ms
Installed 1 package in 1ms
 + json-merge-patch==0.2
```

After which I can `uv sync`
```
❯ uv sync
Resolved 238 packages in 0.62ms
      Built python-nvd3==0.16.0
      Built lazy-object-proxy==1.10.0
Prepared 228 packages in 5.08s
Installed 235 packages in 370ms
 + aiofiles==24.1.0
 + aiohappyeyeballs==2.6.1
 + aiohttp==3.11.14
[...]
 + wtforms==3.2.1
 + yarl==1.18.3
 + zipp==3.21.0
```

---

_Comment by @vient on 2025-03-24 16:39_

Manually installed `setuptools==77.0.3` in empty ubuntu:24.04 docker, `python -m uv pip install gputil` still does not work. `python -m pip install gputil` works with any setuptools version. I'm using python 3.11.11, uv 0.6.9, pip 25.0.1.

---

_Comment by @charliermarsh on 2025-03-24 16:41_

Yeah we're missing one spot in `uv sync`. I will fix it.

---

_Referenced in [astral-sh/uv#12437](../../astral-sh/uv/issues/12437.md) on 2025-03-24 16:43_

---

_Referenced in [olirice/alembic_utils#153](../../olirice/alembic_utils/issues/153.md) on 2025-03-24 16:48_

---

_Referenced in [astral-sh/uv#12440](../../astral-sh/uv/issues/12440.md) on 2025-03-24 16:48_

---

_Comment by @darkwind-noveo on 2025-03-24 16:49_

in the mean time, thanks for workaround adviced
```docker
FROM python:3.11-buster AS builder

# Install uwsgi
RUN pip install uv setuptools==77.0.3

# Install requirements (doing this before copying code improves caching)
ENV UV_PYTHON_PREFERENCE=only-system
ENV UV_PROJECT_ENVIRONMENT=/usr/local
COPY pyproject.toml uv.lock /code/
RUN pip install direct-sdk-python3==1.10.0
RUN uv sync --frozen
```
Just preinstalled problematic package separately

---

_Comment by @charliermarsh on 2025-03-24 16:50_

Ah yeah. Alternatively, if you build it in advance with `uv pip`, then we should be able to read it from the cache in `uv sync`.


---

_Comment by @zanieb on 2025-03-24 16:52_

I've posted another workaround in https://github.com/astral-sh/uv/issues/12440

---

_Comment by @jacob-bush-shopify on 2025-03-24 16:53_

> Manually installed `setuptools==77.0.3` in empty ubuntu:24.04 docker, `python -m uv pip install gputil` still does not work. `python -m pip install gputil` works with any setuptools version. I'm using python 3.11.11, uv 0.6.9, pip 25.0.1.

@vient you still need to set the build constraint. If you don't want to set up a file you can pipe it in
```
echo "setuptools<78" | uv pip install -b - gputil
```

---

_Referenced in [crytic/solc-select#227](../../crytic/solc-select/pulls/227.md) on 2025-03-24 17:23_

---

_Referenced in [ethereum/execution-spec-tests#1345](../../ethereum/execution-spec-tests/pulls/1345.md) on 2025-03-24 17:30_

---

_Referenced in [pypa/setuptools#4910](../../pypa/setuptools/issues/4910.md) on 2025-03-24 17:46_

---

_Referenced in [astral-sh/uv#12446](../../astral-sh/uv/issues/12446.md) on 2025-03-24 19:06_

---

_Comment by @jacob-bush-shopify on 2025-03-24 20:10_

Setuptools rolled back the change

https://github.com/pypa/setuptools/blob/main/NEWS.rst

> Postponed removals of deprecated dash-separated and uppercase fields in setup.cfg. All packages with deprecated configurations are advised to move before 2026. ([#4911](https://github.com/pypa/setuptools/pull/4911))

I can no longer reproduce this bug myself.
I expect that the workarounds discussed above are no longer needed.


---

_Comment by @zanieb on 2025-03-24 20:13_

Thank you!

---

_Closed by @zanieb on 2025-03-25 01:50_

---
