---
number: 9864
title: "`uv tool install` deploys different version of package when installing an extra"
type: issue
state: closed
author: hoechenberger
labels:
  - needs-mre
assignees: []
created_at: 2024-12-13T09:02:31Z
updated_at: 2024-12-16T20:58:36Z
url: https://github.com/astral-sh/uv/issues/9864
synced_at: 2026-01-10T01:24:47Z
---

# `uv tool install` deploys different version of package when installing an extra

---

_Issue opened by @hoechenberger on 2024-12-13 09:02_

Installing `mne` via `uv tool install` installs a different version depending on whether I request `mne` or `mne[full]`.

### `uv tool install mne` (installs `mne==1.8.0`)
```shell
❯ uv tool install mne
Resolved 25 packages in 8ms
Installed 25 packages in 100ms
 + certifi==2024.8.30
 + charset-normalizer==3.4.0
 + contourpy==1.3.1
 + cycler==0.12.1
 + decorator==5.1.1
 + fonttools==4.55.3
 + idna==3.10
 + jinja2==3.1.4
 + kiwisolver==1.4.7
 + lazy-loader==0.4
 + markupsafe==3.0.2
 + matplotlib==3.9.4
 + mne==1.8.0
 + numpy==2.2.0
 + packaging==24.2
 + pillow==11.0.0
 + platformdirs==4.3.6
 + pooch==1.8.2
 + pyparsing==3.2.0
 + python-dateutil==2.9.0.post0
 + requests==2.32.3
 + scipy==1.14.1
 + six==1.17.0
 + tqdm==4.67.1
 + urllib3==2.2.3
Installed 1 executable: mne
```

### `uv tool install "mne[full]"` (installs `mne==1.5.1`, which doesn't even have that extra yet)
```shell
❯ uv tool install "mne[full]"
Resolved 24 packages in 20ms
Installed 24 packages in 119ms
 + certifi==2024.8.30
 + charset-normalizer==3.4.0
 + contourpy==1.3.1
 + cycler==0.12.1
 + decorator==5.1.1
 + fonttools==4.55.3
 + idna==3.10
 + jinja2==3.1.4
 + kiwisolver==1.4.7
 + markupsafe==3.0.2
 + matplotlib==3.9.4
 + mne==1.5.1
 + numpy==2.2.0
 + packaging==24.2
 + pillow==11.0.0
 + platformdirs==4.3.6
 + pooch==1.8.2
 + pyparsing==3.2.0
 + python-dateutil==2.9.0.post0
 + requests==2.32.3
 + scipy==1.14.1
 + six==1.17.0
 + tqdm==4.67.1
 + urllib3==2.2.3
warning: The package `mne==1.5.1` does not have an extra named `full`
Installed 1 executable: mne
```

```shell
❯ uv -V
uv 0.5.8 (80d41671b 2024-12-11)
```
This is on macOS 15.2.

I couldn't post the `--verbose` output due to character limit in GH issues.

**Edit:**

It seems to be related to the Python version that is being selected. In the above examples, Python 3.13 would be used.

If I enforce Python 3.12, it works as expected:

```shell
❯ uv tool install --python 3.12 "mne[full]"
Resolved 184 packages in 35ms
Prepared 18 packages in 26.26s
Installed 184 packages in 883ms
 + aiohappyeyeballs==2.4.4
 + aiohttp==3.11.10
 + aiosignal==1.3.1
 + anyio==4.7.0
 + appnope==0.1.4
 + argon2-cffi==23.1.0
 + argon2-cffi-bindings==21.2.0
 + arrow==1.3.0
 + asttokens==3.0.0
 + async-lru==2.0.4
 + attrs==24.2.0
 + babel==2.16.0
 + beautifulsoup4==4.12.3
 + bleach==6.2.0
 + certifi==2024.8.30
 + cffi==1.17.1
 + charset-normalizer==3.4.0
 + colorama==0.4.6
 + comm==0.2.2
 + contourpy==1.3.1
 + cycler==0.12.1
 + darkdetect==0.8.0
 + debugpy==1.8.9
 + decorator==5.1.1
 + deepdiff==8.0.1
 + defusedxml==0.7.1
 + deprecated==1.2.15
 + dipy==1.10.0
 + edfio==0.4.5
 + eeglabio==0.0.3
 + executing==2.1.0
 + fastjsonschema==2.21.1
 + fonttools==4.55.3
 + fqdn==1.5.1
 + frozenlist==1.5.0
 + h11==0.14.0
 + h5io==0.2.4
 + h5py==3.12.1
 + httpcore==1.0.7
 + httpx==0.28.1
 + idna==3.10
 + imageio==2.36.1
 + imageio-ffmpeg==0.5.1
 + ipyevents==2.0.2
 + ipykernel==6.29.5
 + ipympl==0.9.4
 + ipython==8.30.0
 + ipython-genutils==0.2.0
 + ipywidgets==8.1.5
 + isoduration==20.11.0
 + jedi==0.19.2
 + jinja2==3.1.4
 + joblib==1.4.2
 + json5==0.10.0
 + jsonpointer==3.0.0
 + jsonschema==4.23.0
 + jsonschema-specifications==2024.10.1
 + jupyter==1.1.1
 + jupyter-client==8.6.3
 + jupyter-console==6.6.3
 + jupyter-core==5.7.2
 + jupyter-events==0.10.0
 + jupyter-lsp==2.2.5
 + jupyter-server==2.14.2
 + jupyter-server-terminals==0.5.3
 + jupyterlab==4.3.3
 + jupyterlab-pygments==0.3.0
 + jupyterlab-server==2.27.3
 + jupyterlab-widgets==3.0.13
 + kiwisolver==1.4.7
 + lazy-loader==0.4
 + llvmlite==0.43.0
 + lxml==5.3.0
 + markupsafe==3.0.2
 + matplotlib==3.9.4
 + matplotlib-inline==0.1.7
 + mffpy==0.10.0
 + mistune==3.0.2
 + mne==1.8.0
 + mne-qt-browser==0.6.3
 + more-itertools==10.5.0
 + msgpack==1.1.0
 + multidict==6.1.0
 + nbclient==0.10.1
 + nbconvert==7.16.4
 + nbformat==5.10.4
 + neo==0.13.4
 + nest-asyncio==1.6.0
 + nibabel==5.3.2
 + nilearn==0.11.0
 + notebook==7.3.1
 + notebook-shim==0.2.4
 + numba==0.60.0
 + numpy==1.26.4
 + openmeeg==2.5.12
 + orderly-set==5.2.2
 + overrides==7.7.0
 + packaging==24.2
 + pandas==2.2.3
 + pandocfilters==1.5.1
 + parso==0.8.4
 + patsy==1.0.1
 + pexpect==4.9.0
 + pillow==11.0.0
 + pip==24.3.1
 + platformdirs==4.3.6
 + pooch==1.8.2
 + prometheus-client==0.21.1
 + prompt-toolkit==3.0.48
 + propcache==0.2.1
 + psutil==6.1.0
 + ptyprocess==0.7.0
 + pure-eval==0.2.3
 + pyarrow==18.1.0
 + pybv==0.7.6
 + pycparser==2.22
 + pygments==2.18.0
 + pymatreader==1.0.0
 + pyobjc-core==10.3.2
 + pyobjc-framework-cocoa==10.3.2
 + pyopengl==3.1.7
 + pyparsing==3.2.0
 + pyqt6==6.8.0
 + pyqt6-qt6==6.8.1
 + pyqt6-sip==13.9.1
 + pyqtgraph==0.13.7
 + python-dateutil==2.9.0.post0
 + python-json-logger==3.2.0
 + python-picard==0.8
 + pytz==2024.2
 + pyvista==0.44.2
 + pyvistaqt==0.11.1
 + pyyaml==6.0.2
 + pyzmq==26.2.0
 + qdarkstyle==3.2.3
 + qtpy==2.4.2
 + quantities==0.16.1
 + referencing==0.35.1
 + requests==2.32.3
 + rfc3339-validator==0.1.4
 + rfc3986-validator==0.1.1
 + rpds-py==0.22.3
 + scikit-learn==1.6.0
 + scipy==1.14.1
 + scooby==0.10.0
 + send2trash==1.8.3
 + setuptools==75.6.0
 + setuptools-scm==8.1.0
 + sip==6.9.1
 + six==1.17.0
 + sniffio==1.3.1
 + snirf==0.8.0
 + soupsieve==2.6
 + stack-data==0.6.3
 + statsmodels==0.14.4
 + termcolor==2.5.0
 + terminado==0.18.1
 + threadpoolctl==3.5.0
 + tinycss2==1.4.0
 + tornado==6.4.2
 + tqdm==4.67.1
 + traitlets==5.14.3
 + trame==3.7.1
 + trame-client==3.5.0
 + trame-server==3.2.3
 + trame-vtk==2.8.12
 + trame-vuetify==2.7.2
 + trx-python==0.3
 + types-python-dateutil==2.9.0.20241206
 + typing-extensions==4.12.2
 + tzdata==2024.2
 + uri-template==1.3.0
 + urllib3==2.2.3
 + vtk==9.3.1
 + wcwidth==0.2.13
 + webcolors==24.11.1
 + webencodings==0.5.1
 + websocket-client==1.8.0
 + widgetsnbextension==4.0.13
 + wrapt==1.17.0
 + wslink==2.2.1
 + xlrd==2.0.1
 + xmltodict==0.14.2
 + yarl==1.18.3
Installed 1 executable: mne
```

I assume it's more of a packaging problem, then, instead of a `uv` issue per se?

---

_Referenced in [mne-tools/mne-python#13026](../../mne-tools/mne-python/issues/13026.md) on 2024-12-13 10:19_

---

_Comment by @hoechenberger on 2024-12-13 10:48_

I suppose I would at least like to get a warning if the latest PyPI-published version of the requested package cannot be installed due to dependency constraints. 🙂 I only realized I had gotten an outdated `mne` when I tried to actually run it.

---

_Comment by @charliermarsh on 2024-12-13 21:52_

Are you able to produce this consistently? Can you share the `--verbose` logs?

---

_Label `needs-mre` added by @charliermarsh on 2024-12-13 21:53_

---

_Comment by @hoechenberger on 2024-12-16 09:24_

Hello @charliermarsh,

sorry for the late response, but there were some changes to an upstream package (`openmeeg`) that changed the resolution, and I think it's now much more in line with what I'd expect.

```shell
❯ uv -V
uv 0.5.9 (0652800cb 2024-12-13)
```

### "Normal" installation
Output of `uv tool install --verbose  "mne[full]" 2>&1 |tee mne-full.log.txt`:
[mne-full.log.txt](https://github.com/user-attachments/files/18147533/mne-full.log.txt)

This will try to install `mne==1.8.0`, which depends on `dipy`, which in turn does not yet support for Python 3.13. Hence, no binary wheels will be found; `uv` downloads the sources and the build fails (as I don't have LLVM installed; besides, `dipy` doesn't support Python 3.13 yet anyway!)


### Binary-only installation
Output of `uv tool install --verbose  --no-build "mne[full]" 2>&1 |tee mne-full-no-build.log.txt`:
[mne-full-no-build.log.txt](https://github.com/user-attachments/files/18147545/mne-full-no-build.log.txt)

Now this will install `mne==1.5.1`, which apparently is the last version that doesn't have some dependencies for which binary wheels for Python 3.13 are still missing?

In this situation, think it would be nice to receive a warning that the installed version of the requested tool is older than what's currently on PyPI.

---

_Comment by @hoechenberger on 2024-12-16 20:58_

Closing for now as I believe this is more a packaging than a `uv` issue.

---

_Closed by @hoechenberger on 2024-12-16 20:58_

---
