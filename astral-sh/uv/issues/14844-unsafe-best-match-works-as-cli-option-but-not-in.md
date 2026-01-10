---
number: 14844
title: unsafe-best-match works as CLI option but not in pyproject.toml
type: issue
state: closed
author: SebBanDev
labels:
  - needs-mre
assignees: []
created_at: 2025-07-23T12:34:39Z
updated_at: 2025-08-21T12:55:41Z
url: https://github.com/astral-sh/uv/issues/14844
synced_at: 2026-01-10T01:25:49Z
---

# unsafe-best-match works as CLI option but not in pyproject.toml

---

_Issue opened by @SebBanDev on 2025-07-23 12:34_

### Summary

We are facing an issue to resolve dependencies with uv in a case where they are coming from two different indexes.
The public index contains dash-design-kit 0.0.1 whereas the private index contains dash-design-kit==1.*
(Vice versa, the public index contains dash 2.* but the private index does not.)

using uv 0.8.2 we tried:
pyproject.toml
```
[...]
dependencies = [
    [...]
    "dash-design-kit==1.*",
    [...]
]
[...]

[[tool.uv.index]]
name = "de-index"
url = "https://{private-index-url}/packages"

[[tool.uv.index]]
url = "https://pypi.org/simple"
default = true

[tool.uv.sources]
dash-design-kit = { index = "de-index" }
```
with
`uv pip compile --output-file bla.txt pyproject.toml`

This will not work as uv will try to install transient dependencies of dash-design-kit from the publix index as well, where it will also not find the correct versions which are only in the private index

---

However, using:
requirements.txt
```
--extra-index-url=https://{private-index-url}/packages
dash-design-kit==1.*
[...]
dash==2.*
[...]
```
with
`uv pip compile --index-strategy unsafe-best-match --output-file bla.txt requirements.txt`

works fine and resolved all dependencies:
```
[...]
dash-design-kit==1.14.0
    # via
    #   -r requirements.txt
    #   dash-enterprise-libraries
[...]
```

---

So next we tried to reproduce this behaviour with pyproject.toml settings, to avoid changing the CLI command in our pipeline:
pyproject.toml
```
[...]
dependencies = [
    [...]
    "dash-design-kit==1.*",
    [...]
]
[...]

[tool.uv]
index-strategy = "unsafe-best-match"

[tool.uv.pip]
index-url = "https://pypi.org/simple"
extra-index-url = ["https://{private-index-url}/packages"]
```

But it does not work. In that case, it seems like the extra index url is ignored.
```
uv pip compile --verbose --output-file bla.txt pyproject.toml
DEBUG uv 0.8.2
DEBUG Starting Python discovery for a default Python
DEBUG Looking for exact match for request a default Python
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/tests/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.3 interpreter at /tests/.venv/bin/python3 for builds
DEBUG Using request timeout of 30s
WARN Fixing invalid requirement by removing star after comparison operator other than equal and not equal (before: `dash-enterprise-libraries>=1.*`; after: `dash-enterprise-libraries>=1`)
DEBUG Found static `requires-dist` for: /tests/controls-graph-header
DEBUG Found workspace root: `/tests`
DEBUG Adding root workspace member: `/tests`
DEBUG Adding discovered workspace member: `/tests/controls-graph-header`
DEBUG Solving with installed Python version: 3.12.3
DEBUG Solving with target Python version: >=3.12.3
DEBUG Adding direct dependency: dash>=2.dev0, <3.dev0
DEBUG Adding direct dependency: dash-design-kit>=1.dev0, <2.dev0
DEBUG Adding direct dependency: dash-enterprise-libraries>=1
DEBUG Adding direct dependency: gunicorn>=23.dev0, <24.dev0
DEBUG Adding direct dependency: pandas>=1.1.5
DEBUG Found stale response for: https://pypi.org/simple/dash-design-kit/
DEBUG Sending revalidation request for: https://pypi.org/simple/dash-design-kit/
DEBUG Found stale response for: https://pypi.org/simple/dash-enterprise-libraries/
DEBUG Sending revalidation request for: https://pypi.org/simple/dash-enterprise-libraries/
DEBUG Found stale response for: https://pypi.org/simple/gunicorn/
DEBUG Sending revalidation request for: https://pypi.org/simple/gunicorn/
DEBUG Found stale response for: https://pypi.org/simple/dash/
DEBUG Sending revalidation request for: https://pypi.org/simple/dash/
DEBUG Found stale response for: https://pypi.org/simple/pandas/
DEBUG Sending revalidation request for: https://pypi.org/simple/pandas/
DEBUG Found not-modified response for: https://pypi.org/simple/pandas/
DEBUG Found not-modified response for: https://pypi.org/simple/dash/
DEBUG Found not-modified response for: https://pypi.org/simple/gunicorn/
DEBUG Searching for a compatible version of dash (>=2.dev0, <3.dev0)
DEBUG Selecting: dash==2.18.2 [preference] (dash-2.18.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cb/7d/6dac2a6e1eba33ee43f318edbed4ff29151a49b5d37f080aad1e6469bca4/gunicorn-23.0.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/da/01/e383018feba0a1ead6cf5fe8728e5d767fee02f06a3d800e82c489e5daaf/pandas-2.3.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/72/ef/d46131f4817f18b329e4fb7c53ba1d31774239d91266a74bccdc932708cc/dash-2.18.2-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for dash==2.18.2: flask>=1.0.4, <3.1
DEBUG Adding transitive dependency for dash==2.18.2: werkzeug<3.1
DEBUG Adding transitive dependency for dash==2.18.2: plotly>=5.0.0
DEBUG Adding transitive dependency for dash==2.18.2: dash-html-components>=2.0.0, <2.0.0+
DEBUG Adding transitive dependency for dash==2.18.2: dash-core-components>=2.0.0, <2.0.0+
DEBUG Adding transitive dependency for dash==2.18.2: dash-table>=5.0.0, <5.0.0+
DEBUG Adding transitive dependency for dash==2.18.2: importlib-metadata*
DEBUG Adding transitive dependency for dash==2.18.2: typing-extensions>=4.1.1
DEBUG Adding transitive dependency for dash==2.18.2: requests*
DEBUG Adding transitive dependency for dash==2.18.2: retrying*
DEBUG Adding transitive dependency for dash==2.18.2: nest-asyncio*
DEBUG Adding transitive dependency for dash==2.18.2: setuptools*
DEBUG Found stale response for: https://pypi.org/simple/flask/
DEBUG Sending revalidation request for: https://pypi.org/simple/flask/
DEBUG Found stale response for: https://pypi.org/simple/dash-html-components/
DEBUG Sending revalidation request for: https://pypi.org/simple/dash-html-components/
DEBUG Found stale response for: https://pypi.org/simple/werkzeug/
DEBUG Sending revalidation request for: https://pypi.org/simple/werkzeug/
DEBUG Found stale response for: https://pypi.org/simple/dash-core-components/
DEBUG Sending revalidation request for: https://pypi.org/simple/dash-core-components/
DEBUG Found stale response for: https://pypi.org/simple/requests/
DEBUG Sending revalidation request for: https://pypi.org/simple/requests/
DEBUG Found stale response for: https://pypi.org/simple/plotly/
DEBUG Sending revalidation request for: https://pypi.org/simple/plotly/
DEBUG Found stale response for: https://pypi.org/simple/dash-table/
DEBUG Sending revalidation request for: https://pypi.org/simple/dash-table/
DEBUG Found stale response for: https://pypi.org/simple/typing-extensions/
DEBUG Sending revalidation request for: https://pypi.org/simple/typing-extensions/
DEBUG Found stale response for: https://pypi.org/simple/retrying/
DEBUG Sending revalidation request for: https://pypi.org/simple/retrying/
DEBUG Found stale response for: https://pypi.org/simple/nest-asyncio/
DEBUG Sending revalidation request for: https://pypi.org/simple/nest-asyncio/
DEBUG Found not-modified response for: https://pypi.org/simple/werkzeug/
DEBUG Found not-modified response for: https://pypi.org/simple/requests/
DEBUG Found stale response for: https://pypi.org/simple/importlib-metadata/
DEBUG Sending revalidation request for: https://pypi.org/simple/importlib-metadata/
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/plotly/
DEBUG Found not-modified response for: https://pypi.org/simple/flask/
DEBUG Found not-modified response for: https://pypi.org/simple/dash-core-components/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6c/69/05837f91dfe42109203ffa3e488214ff86a6d68b2ed6c167da6cdc42349b/werkzeug-3.0.6-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7c/e4/56027c4a6b4ae70ca9de302488c5ca95ad4a39e190093d6c1a8ace08341b/requests-2.32.4-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/dash-table/
DEBUG Found not-modified response for: https://pypi.org/simple/dash-html-components/
DEBUG Found not-modified response for: https://pypi.org/simple/typing-extensions/
DEBUG Found not-modified response for: https://pypi.org/simple/nest-asyncio/
DEBUG Searching for a compatible version of dash-html-components (>=2.0.0, <2.0.0+)
DEBUG Selecting: dash-html-components==2.0.0 [preference] (dash_html_components-2.0.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ed/20/f2b7ac96a91cc5f70d81320adad24cc41bf52013508d649b1481db225780/plotly-6.2.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/9e/a29f726e84e531a36d56cff187e61d8c96d2cc253c5bcef9a7695acb7e6a/dash_core_components-2.0.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/61/80/ffe1da13ad9300f87c93af113edd0638c75138c42a0994becfacac078c06/flask-3.0.3-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/importlib-metadata/
DEBUG Found not-modified response for: https://pypi.org/simple/retrying/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/da/ce/43f77dc8e7bbad02a9f88d07bf794eaf68359df756a28bb9f2f78e255bb1/dash_table-5.0.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/75/65/1b16b853844ef59b2742a7de74a598f376ac0ab581f0dcc34db294e5c90e/dash_html_components-2.0.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a0/c4/c2971a3ba4c6103a3d10c4b0f24f461ddc027f0f09763220cf35ca1401b3/nest_asyncio-1.6.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b5/00/d631e67a838026495268c2f6884f3711a15a9a2a96cd244fdaea53b823fb/typing_extensions-4.14.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of dash-core-components (>=2.0.0, <2.0.0+)
DEBUG Selecting: dash-core-components==2.0.0 [preference] (dash_core_components-2.0.0-py3-none-any.whl)
DEBUG Searching for a compatible version of dash-table (>=5.0.0, <5.0.0+)
DEBUG Selecting: dash-table==5.0.0 [preference] (dash_table-5.0.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7f/25/f3b628e123699139b959551ed922f35af97fa1505e195ae3e6537a14fbc3/retrying-1.4.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/b0/36bd937216ec521246249be3bf9855081de4c5e06a0c9b4219dbeda50373/importlib_metadata-8.7.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/dash-design-kit/
DEBUG Searching for a compatible version of dash-design-kit (>=1.dev0, <2.dev0)
DEBUG No compatible version found for: dash-design-kit
DEBUG Found not-modified response for: https://pypi.org/simple/dash-enterprise-libraries/
  × No solution found when resolving dependencies:
  ╰─▶ Because only dash-design-kit==0.0.1 is available and controls-graph-header depends on dash-design-kit==1.*, we can conclude that your requirements are
      unsatisfiable.
```

### Platform

Linux 6.1.141-165.249.amzn2023.x86_64 x86_64 GNU/Linux

### Version

uv 0.8.2

### Python version

Python 3.12

---

_Label `bug` added by @SebBanDev on 2025-07-23 12:34_

---

_Comment by @SebBanDev on 2025-07-23 12:42_

Also tried 
```
[tool.uv]
index-strategy = "unsafe-best-match"

[[tool.uv.index]]
name = "dash-enterprise"
url = "https://{private-index-url}/packages"
default = true

[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"
```
same result:
```
DEBUG uv 0.8.2
DEBUG Starting Python discovery for a default Python
DEBUG Looking for exact match for request a default Python
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/tests/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.3 interpreter at /tests/.venv/bin/python3 for builds
DEBUG Using request timeout of 30s
WARN Fixing invalid requirement by removing star after comparison operator other than equal and not equal (before: `dash-enterprise-libraries>=1.*`; after: `dash-enterprise-libraries>=1`)
DEBUG Found static `requires-dist` for: /tests/controls-graph-header
DEBUG Found workspace root: `/tests`
DEBUG Adding root workspace member: `/tests`
DEBUG Adding discovered workspace member: `/tests/controls-graph-header`
DEBUG Solving with installed Python version: 3.12.3
DEBUG Solving with target Python version: >=3.12.3
DEBUG Adding direct dependency: dash>=2.dev0, <3.dev0
DEBUG Adding direct dependency: dash-design-kit>=1.dev0, <2.dev0
DEBUG Adding direct dependency: dash-enterprise-libraries>=1
DEBUG Adding direct dependency: gunicorn>=23.dev0, <24.dev0
DEBUG Adding direct dependency: pandas>=1.1.5
DEBUG Found stale response for: https://pypi.org/simple/dash/
DEBUG Sending revalidation request for: https://pypi.org/simple/dash/
DEBUG Found stale response for: https://pypi.org/simple/gunicorn/
DEBUG Sending revalidation request for: https://pypi.org/simple/gunicorn/
DEBUG Found stale response for: https://pypi.org/simple/dash-enterprise-libraries/
DEBUG Sending revalidation request for: https://pypi.org/simple/dash-enterprise-libraries/
DEBUG Found stale response for: https://pypi.org/simple/dash-design-kit/
DEBUG Sending revalidation request for: https://pypi.org/simple/dash-design-kit/
DEBUG Found stale response for: https://pypi.org/simple/pandas/
DEBUG Sending revalidation request for: https://pypi.org/simple/pandas/
DEBUG Found not-modified response for: https://pypi.org/simple/pandas/
DEBUG Found not-modified response for: https://pypi.org/simple/dash/
DEBUG Found not-modified response for: https://pypi.org/simple/dash-design-kit/
DEBUG Found not-modified response for: https://pypi.org/simple/dash-enterprise-libraries/
DEBUG Found not-modified response for: https://pypi.org/simple/gunicorn/
DEBUG Searching for a compatible version of dash (>=2.dev0, <3.dev0)
DEBUG Selecting: dash==2.18.2 [preference] (dash-2.18.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/da/01/e383018feba0a1ead6cf5fe8728e5d767fee02f06a3d800e82c489e5daaf/pandas-2.3.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/72/ef/d46131f4817f18b329e4fb7c53ba1d31774239d91266a74bccdc932708cc/dash-2.18.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cb/7d/6dac2a6e1eba33ee43f318edbed4ff29151a49b5d37f080aad1e6469bca4/gunicorn-23.0.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for dash==2.18.2: flask>=1.0.4, <3.1
DEBUG Adding transitive dependency for dash==2.18.2: werkzeug<3.1
DEBUG Adding transitive dependency for dash==2.18.2: plotly>=5.0.0
DEBUG Adding transitive dependency for dash==2.18.2: dash-html-components>=2.0.0, <2.0.0+
DEBUG Adding transitive dependency for dash==2.18.2: dash-core-components>=2.0.0, <2.0.0+
DEBUG Adding transitive dependency for dash==2.18.2: dash-table>=5.0.0, <5.0.0+
DEBUG Adding transitive dependency for dash==2.18.2: importlib-metadata*
DEBUG Adding transitive dependency for dash==2.18.2: typing-extensions>=4.1.1
DEBUG Adding transitive dependency for dash==2.18.2: requests*
DEBUG Adding transitive dependency for dash==2.18.2: retrying*
DEBUG Adding transitive dependency for dash==2.18.2: nest-asyncio*
DEBUG Adding transitive dependency for dash==2.18.2: setuptools*
DEBUG Found stale response for: https://pypi.org/simple/werkzeug/
DEBUG Sending revalidation request for: https://pypi.org/simple/werkzeug/
DEBUG Found stale response for: https://pypi.org/simple/flask/
DEBUG Sending revalidation request for: https://pypi.org/simple/flask/
DEBUG Found stale response for: https://pypi.org/simple/plotly/
DEBUG Sending revalidation request for: https://pypi.org/simple/plotly/
DEBUG Found stale response for: https://pypi.org/simple/dash-table/
DEBUG Sending revalidation request for: https://pypi.org/simple/dash-table/
DEBUG Found stale response for: https://pypi.org/simple/typing-extensions/
DEBUG Sending revalidation request for: https://pypi.org/simple/typing-extensions/
DEBUG Found stale response for: https://pypi.org/simple/dash-html-components/
DEBUG Sending revalidation request for: https://pypi.org/simple/dash-html-components/
DEBUG Found stale response for: https://pypi.org/simple/retrying/
DEBUG Sending revalidation request for: https://pypi.org/simple/retrying/
DEBUG Found stale response for: https://pypi.org/simple/requests/
DEBUG Sending revalidation request for: https://pypi.org/simple/requests/
DEBUG Found stale response for: https://pypi.org/simple/dash-core-components/
DEBUG Sending revalidation request for: https://pypi.org/simple/dash-core-components/
DEBUG Found stale response for: https://pypi.org/simple/nest-asyncio/
DEBUG Sending revalidation request for: https://pypi.org/simple/nest-asyncio/
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG Found stale response for: https://pypi.org/simple/importlib-metadata/
DEBUG Sending revalidation request for: https://pypi.org/simple/importlib-metadata/
DEBUG Found not-modified response for: https://pypi.org/simple/nest-asyncio/
DEBUG Found not-modified response for: https://pypi.org/simple/flask/
DEBUG Found not-modified response for: https://pypi.org/simple/werkzeug/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/61/80/ffe1da13ad9300f87c93af113edd0638c75138c42a0994becfacac078c06/flask-3.0.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6c/69/05837f91dfe42109203ffa3e488214ff86a6d68b2ed6c167da6cdc42349b/werkzeug-3.0.6-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/retrying/
DEBUG Found not-modified response for: https://pypi.org/simple/dash-core-components/
DEBUG Found not-modified response for: https://pypi.org/simple/requests/
DEBUG Found not-modified response for: https://pypi.org/simple/dash-html-components/
DEBUG Found not-modified response for: https://pypi.org/simple/typing-extensions/
DEBUG Found not-modified response for: https://pypi.org/simple/importlib-metadata/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/dash-table/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a0/c4/c2971a3ba4c6103a3d10c4b0f24f461ddc027f0f09763220cf35ca1401b3/nest_asyncio-1.6.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of dash-html-components (>=2.0.0, <2.0.0+)
DEBUG Selecting: dash-html-components==2.0.0 [preference] (dash_html_components-2.0.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7f/25/f3b628e123699139b959551ed922f35af97fa1505e195ae3e6537a14fbc3/retrying-1.4.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/75/65/1b16b853844ef59b2742a7de74a598f376ac0ab581f0dcc34db294e5c90e/dash_html_components-2.0.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of dash-core-components (>=2.0.0, <2.0.0+)
DEBUG Selecting: dash-core-components==2.0.0 [preference] (dash_core_components-2.0.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7c/e4/56027c4a6b4ae70ca9de302488c5ca95ad4a39e190093d6c1a8ace08341b/requests-2.32.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b5/00/d631e67a838026495268c2f6884f3711a15a9a2a96cd244fdaea53b823fb/typing_extensions-4.14.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/9e/a29f726e84e531a36d56cff187e61d8c96d2cc253c5bcef9a7695acb7e6a/dash_core_components-2.0.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of dash-table (>=5.0.0, <5.0.0+)
DEBUG Selecting: dash-table==5.0.0 [preference] (dash_table-5.0.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/b0/36bd937216ec521246249be3bf9855081de4c5e06a0c9b4219dbeda50373/importlib_metadata-8.7.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/da/ce/43f77dc8e7bbad02a9f88d07bf794eaf68359df756a28bb9f2f78e255bb1/dash_table-5.0.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of dash-design-kit (>=1.dev0, <2.dev0)
DEBUG No compatible version found for: dash-design-kit
DEBUG Found not-modified response for: https://pypi.org/simple/plotly/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ed/20/f2b7ac96a91cc5f70d81320adad24cc41bf52013508d649b1481db225780/plotly-6.2.0-py3-none-any.whl.metadata
  × No solution found when resolving dependencies:
  ╰─▶ Because only dash-design-kit==0.0.1 is available and controls-graph-header depends on dash-design-kit==1.*, we can conclude that your requirements are
      unsatisfiable.
```

---

_Comment by @zanieb on 2025-07-23 12:46_

You should use `uv export` instead of `uv pip compile pyproject.toml`. Though I'm not entirely sure it'll resolve your use-case due to https://github.com/astral-sh/uv/issues/10008#issuecomment-2772723062.

---

_Renamed from "unsafe-best-match works as CLI ption but not in pyproject.toml" to "unsafe-best-match works as CLI option but not in pyproject.toml" by @SebBanDev on 2025-07-23 13:30_

---

_Comment by @charliermarsh on 2025-07-23 22:47_

Can you please include `--verbose` logs from the "failing" command? It's hard to help without more information.

---

_Comment by @SebBanDev on 2025-07-24 07:59_

> Can you please include `--verbose` logs from the "failing" command? It's hard to help without more information.

Added the `--verbose` logs in the original messages as **edit**

---

_Comment by @SebBanDev on 2025-07-24 12:10_

**update**
This works:

```
[tool.uv]
index-strategy = "unsafe-best-match"
 
[[tool.uv.index]]
name = "extra-index"
url = "https://{private-index-url}/packages"
 
[[tool.uv.index]]
url = "[https://pypi.org/simple"](https://pypi.org/simple%22)
default = true
 
[tool.uv.sources]
dash-design-kit = { index = "extra-index" }
dash-enterprise-libraries = { index = "extra-index" }
```
This way it seems also transient dependencies are taken from the `extra-index`
However, we did not find a neat way to determine the packages that need `{ index = "extra-index" }` automatically yet.
(Only a bash script that reads them from uv.lock after uv sync)

---

_Comment by @charliermarsh on 2025-07-24 13:00_

What you have there looks right. You should be able to omit the second `[[tool.uv.index]]` definition.

Otherwise, are you able to put together a minimal reproduction with a public index? You can use https://test.pypi.org/simple if you need a non-PyPI secondary index. But it's a little hard to help without otherwise. For example, this seems to correctly pull packages from Test PyPI:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.2"
dependencies = ["flask"]

[tool.uv.pip]
extra-index-url = ["https://test.pypi.org/simple"]
```

Followed by `uv pip compile pyproject.toml`.

---

_Label `bug` removed by @charliermarsh on 2025-07-24 13:00_

---

_Label `needs-mre` added by @charliermarsh on 2025-07-24 13:00_

---

_Comment by @charliermarsh on 2025-08-21 12:55_

(Closing for now, can always re-open with follow-up.)

---

_Closed by @charliermarsh on 2025-08-21 12:55_

---
