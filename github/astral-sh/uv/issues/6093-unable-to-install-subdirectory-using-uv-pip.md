---
number: 6093
title: "Unable to install subdirectory using `uv pip install` with a git tag"
type: issue
state: closed
author: thomasjpfan
labels: []
assignees: []
created_at: 2024-08-14T20:29:47Z
updated_at: 2024-08-14T22:51:10Z
url: https://github.com/astral-sh/uv/issues/6093
synced_at: 2026-01-07T13:12:17-06:00
---

# Unable to install subdirectory using `uv pip install` with a git tag

---

_Issue opened by @thomasjpfan on 2024-08-14 20:29_

Similar to #5942, but with a git tag:

```
uv pip install git+https://github.com/flyteorg/flytekit@9be1060a7a353cbeb566a10913356dee8a07717a#subdirectory=plugins/flytekit-flyteinteractive
```

This returns an error:

```
error: Failed to parse entry for: `flytekit`
  Caused by: Package is not included as workspace package in `tool.uv.workspace`
```

I am using uv 0.2.36 on macOS 14.5.

---

_Comment by @charliermarsh on 2024-08-14 20:32_

I just ran this without error on my machine. Are you certain you're on the latest version?

```
❯ uv pip install git+https://github.com/flyteorg/flytekit@9be1060a7a353cbeb566a10913356dee8a07717a#subdirectory=plugins/flytekit-flyteinteractive

 Updated https://github.com/flyteorg/flytekit (9be1060a)
Resolved 176 packages in 2.75s
   Built flytekitplugins-flyteinteractive @ git+https://github.com/flyteorg/flytekit@9be1060a7a353cbeb566a10913356dee8a07717a#subdirectory=plugins/flytekit-flyteinteractive
   Built google-crc32c==1.5.0
Prepared 175 packages in 2.25s
Installed 176 packages in 352ms
 + adlfs==2024.7.0
 + aiobotocore==2.13.2
 + aiohappyeyeballs==2.3.5
 + aiohttp==3.10.3
 + aioitertools==0.11.0
 + aiosignal==1.3.1
 + anyio==4.4.0
 + appnope==0.1.4
 + argon2-cffi==23.1.0
 + argon2-cffi-bindings==21.2.0
 + arrow==1.3.0
 + asttokens==2.4.1
 + async-lru==2.0.4
 + attrs==24.2.0
 + azure-core==1.30.2
 + azure-datalake-store==0.0.53
 + azure-identity==1.17.1
 + azure-storage-blob==12.22.0
 + babel==2.16.0
 + beautifulsoup4==4.12.3
 + bleach==6.1.0
 + botocore==1.34.131
 + cachetools==5.4.0
 + certifi==2024.7.4
 + cffi==1.17.0
 + charset-normalizer==3.3.2
 + click==8.1.7
 + cloudpickle==3.0.0
 + comm==0.2.2
 + croniter==3.0.3
 + cryptography==43.0.0
 + dataclasses-json==0.5.9
 + debugpy==1.8.5
 + decorator==5.1.1
 + defusedxml==0.7.1
 + diskcache==5.6.3
 + docker==7.1.0
 + docstring-parser==0.16
 + executing==2.0.1
 + fastjsonschema==2.20.0
 + flyteidl==1.13.2
 + flytekit==1.13.3
 + flytekitplugins-flyteinteractive==0.0.0+develop (from git+https://github.com/flyteorg/flytekit@9be1060a7a353cbeb566a10913356dee8a07717a#subdirectory=plugins/flytekit-flyteinteractive)
 + fqdn==1.5.1
 + frozenlist==1.4.1
 + fsspec==2024.6.1
 + gcsfs==2024.6.1
 + google-api-core==2.19.1
 + google-auth==2.33.0
 + google-auth-oauthlib==1.2.1
 + google-cloud-core==2.4.1
 + google-cloud-storage==2.18.2
 + google-crc32c==1.5.0
 + google-resumable-media==2.7.2
 + googleapis-common-protos==1.63.2
 + grpcio==1.65.4
 + grpcio-status==1.65.4
 + h11==0.14.0
 + httpcore==1.0.5
 + httpx==0.27.0
 + idna==3.7
 + importlib-metadata==8.2.0
 + ipykernel==6.29.5
 + ipython==8.26.0
 + ipywidgets==8.1.3
 + isodate==0.6.1
 + isoduration==20.11.0
 + jaraco-classes==3.4.0
 + jaraco-context==5.3.0
 + jaraco-functools==4.0.2
 + jedi==0.19.1
 + jinja2==3.1.4
 + jmespath==1.0.1
 + joblib==1.4.2
 + json5==0.9.25
 + jsonlines==4.0.0
 + jsonpickle==3.2.2
 + jsonpointer==3.0.0
 + jsonschema==4.23.0
 + jsonschema-specifications==2023.12.1
 + jupyter==1.0.0
 + jupyter-client==8.6.2
 + jupyter-console==6.6.3
 + jupyter-core==5.7.2
 + jupyter-events==0.10.0
 + jupyter-lsp==2.2.5
 + jupyter-server==2.14.2
 + jupyter-server-terminals==0.5.3
 + jupyterlab==4.2.4
 + jupyterlab-pygments==0.3.0
 + jupyterlab-server==2.27.3
 + jupyterlab-widgets==3.0.11
 + keyring==25.3.0
 + markdown-it-py==3.0.0
 + markupsafe==2.1.5
 + marshmallow==3.21.3
 + marshmallow-enum==1.5.1
 + marshmallow-jsonschema==0.13.0
 + mashumaro==3.13.1
 + matplotlib-inline==0.1.7
 + mdurl==0.1.2
 + mistune==3.0.2
 + more-itertools==10.4.0
 + msal==1.30.0
 + msal-extensions==1.2.0
 + multidict==6.0.5
 + mypy-extensions==1.0.0
 + nbclient==0.10.0
 + nbconvert==7.16.4
 + nbformat==5.10.4
 + nest-asyncio==1.6.0
 + notebook==7.2.1
 + notebook-shim==0.2.4
 + oauthlib==3.2.2
 + overrides==7.7.0
 + packaging==24.1
 + pandocfilters==1.5.1
 + parso==0.8.4
 + pexpect==4.9.0
 + platformdirs==4.2.2
 + portalocker==2.10.1
 + prometheus-client==0.20.0
 + prompt-toolkit==3.0.47
 + proto-plus==1.24.0
 + protobuf==5.27.3
 + protoc-gen-openapiv2==0.0.1
 + psutil==6.0.0
 + ptyprocess==0.7.0
 + pure-eval==0.2.3
 + pyasn1==0.6.0
 + pyasn1-modules==0.4.0
 + pycparser==2.22
 + pygments==2.18.0
 + pyjwt==2.9.0
 + python-dateutil==2.9.0.post0
 + python-json-logger==2.0.7
 + pytimeparse==1.1.8
 + pytz==2024.1
 + pyyaml==6.0.2
 + pyzmq==26.1.0
 + qtconsole==5.5.2
 + qtpy==2.4.1
 + referencing==0.35.1
 + requests==2.32.3
 + requests-oauthlib==2.0.0
 + rfc3339-validator==0.1.4
 + rfc3986-validator==0.1.1
 + rich==13.7.1
 + rich-click==1.8.3
 + rpds-py==0.20.0
 + rsa==4.9
 + s3fs==2024.6.1
 + send2trash==1.8.3
 + setuptools==72.2.0
 + six==1.16.0
 + sniffio==1.3.1
 + soupsieve==2.6
 + stack-data==0.6.3
 + statsd==4.0.1
 + terminado==0.18.1
 + tinycss2==1.3.0
 + tornado==6.4.1
 + traitlets==5.14.3
 + types-python-dateutil==2.9.0.20240316
 + typing-extensions==4.12.2
 + typing-inspect==0.9.0
 + uri-template==1.3.0
 + urllib3==2.2.2
 + wcwidth==0.2.13
 + webcolors==24.8.0
 + webencodings==0.5.1
 + websocket-client==1.8.0
 + widgetsnbextension==4.0.11
 + wrapt==1.16.0
 + yarl==1.9.4
 + zipp==3.20.0
```

---

_Comment by @zanieb on 2024-08-14 20:41_

I produced a similar error to this in https://github.com/astral-sh/uv/pull/6073/files#diff-273076013b4f5a8139defd5dcd24f5d1eb91c0266dceb4448fdeddceb79f7738R1377-R1379, but that's in a workspace context.

---

_Comment by @thomasjpfan on 2024-08-14 20:44_

I think something is going on with the cache. Here is a reproducer:

```bash
uv env first
source first/bin/activate
uv pip install --cache-dir uv_cache git+https://github.com/flyteorg/flytekit@9be1060a7a353cbeb566a10913356dee8a07717a#subdirectory=plugins/flytekit-flyteinteractive
deactivate

uv env second
source second/bin/activate
uv pip install --cache-dir uv_cache git+https://github.com/flyteorg/flytekit@9be1060a7a353cbeb566a10913356dee8a07717a#subdirectory=plugins/flytekit-flyteinteractive
```



---

_Comment by @zanieb on 2024-08-14 20:49_

Interesting, I reproduced that.

Here's the verbose output

```
❯ uv pip install --cache-dir uv_cache git+https://github.com/flyteorg/flytekit@9be1060a7a353cbeb566a10913356dee8a07717a#subdirectory=plugins/flytekit-flyteinteractive -v
DEBUG uv 0.2.36
DEBUG Searching for Python interpreter in system path
DEBUG Found `cpython-3.12.4-macos-aarch64-none` at `/Users/zb/workspace/example/second/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.4 environment at second/bin/python3
DEBUG Acquired lock for `second`
DEBUG At least one requirement is not satisfied: git+https://github.com/flyteorg/flytekit@9be1060a7a353cbeb566a10913356dee8a07717a#subdirectory=plugins/flytekit-flyteinteractive
DEBUG Using request timeout of 30s
DEBUG Fetching source distribution from Git: https://github.com/flyteorg/flytekit
DEBUG Acquired lock for `https://github.com/flyteorg/flytekit`
DEBUG Using existing git source `Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/flyteorg/flytekit", query: None, fragment: None }`
DEBUG Acquired lock for `/Users/zb/workspace/example/uv_cache/built-wheels-v3/git/abda65965bb197ea/9be1060a7a353cbe`
DEBUG Using cached metadata for: git+https://github.com/flyteorg/flytekit@9be1060a7a353cbeb566a10913356dee8a07717a#subdirectory=plugins/flytekit-flyteinteractive
DEBUG No workspace root found, using project root
error: Failed to parse entry for: `flytekit`
  Caused by: Package is not included as workspace package in `tool.uv.workspace`
```

---

_Comment by @zanieb on 2024-08-14 21:06_

Doing some debugging, the problem appears to be that we're inferring the requirement on `flytekit` as being a requirement on a workspace member because we detect an implicit workspace at the repository root. Kind of confusing. I'm not super familiar with implicit workspaces, but I think with installing from Git repository subdirectories we should avoid searching above the target directory?

---

_Comment by @charliermarsh on 2024-08-14 21:08_

Be sure to review https://github.com/astral-sh/uv/pull/5944 if you haven't already and the comments in there.

---

_Referenced in [astral-sh/uv#6094](../../astral-sh/uv/pulls/6094.md) on 2024-08-14 21:14_

---

_Comment by @zanieb on 2024-08-14 21:17_

I've got a fix up at #6094. Thanks!

---

_Closed by @zanieb on 2024-08-14 21:19_

---

_Comment by @thomasjpfan on 2024-08-14 22:48_

I build `uv` from source and confirm that https://github.com/astral-sh/uv/commit/dc67023677726006d0548c340cc62876eb972bba fixes the issue.

Thank you @zanieb ! 🎉

---

_Comment by @zanieb on 2024-08-14 22:51_

Thanks for confirming!

---

_Referenced in [astral-sh/uv#10728](../../astral-sh/uv/issues/10728.md) on 2025-09-25 10:37_

---
