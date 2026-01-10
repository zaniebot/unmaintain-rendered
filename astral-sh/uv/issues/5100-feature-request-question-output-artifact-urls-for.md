---
number: 5100
title: "Feature Request / Question: Output artifact urls for pip compile"
type: issue
state: open
author: jbw-vtl
labels: []
assignees: []
created_at: 2024-07-16T12:17:35Z
updated_at: 2024-07-18T13:02:04Z
url: https://github.com/astral-sh/uv/issues/5100
synced_at: 2026-01-10T01:23:45Z
---

# Feature Request / Question: Output artifact urls for pip compile

---

_Issue opened by @jbw-vtl on 2024-07-16 12:17_

Hi All,

First of all, thank you very much for developing uv, it has already had a large impact on our CI / CD performance and is slowly growing in adoption across our developers.

To transform our last pip dependent internal process, we are looking to get access to either the raw artifacts (.whl / .tar.gz) or the url to the resources in the index, to then download them ourselves.
This would be solved by implementing `pip download`, I know there's an existing issue #3163  that tracks this request.

In the meantime, wondering whether we could somehow get access to the artifact urls to hack together our own script.
Noticed that when running `uv pip compile requirements.in -v`, it does output the resources used which could be parsed.
This seems to work pretty well, see a an example below for whats working as expected.

```
# Version & System used
uv==0.2.24
python==3.11.9
windows
```

### Working as expected
```text
# requirements.in
pandas==2.2.2
```
```bash
uv pip compile requirements.in --python-version 3.11 --python-platform windows -v
```
Output (path and repo details hidden)
```
DEBUG uv 0.2.24
DEBUG Starting Python discovery for Python 3.11
DEBUG Looking for exact match for request Python 3.11
DEBUG Searching for Python 3.11 in system path or `py` launcher
DEBUG Found cpython 3.11.9 at `...\.venv\Scripts\python.exe` (active virtual environment)
DEBUG Using Python 3.11.9 interpreter at ...\.venv\Scripts\python.exe for builds
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.9
DEBUG Solving with target Python version: 3.11.0
DEBUG Adding direct dependency: pandas==2.2.2
DEBUG Found fresh response for: https://<internal-nexus>/repository/pypi-group/simple/pandas/
DEBUG Searching for a compatible version of pandas (==2.2.2)
DEBUG Selecting: pandas==2.2.2 (pandas-2.2.2-cp311-cp311-win_amd64.whl)
DEBUG Found stale response for: https://<internal-nexus>/repository/pypi-group/packages/pandas/2.2.2/pandas-2.2.2-cp311-cp311-win_amd64.whl#sha256=873d13d177501a28b2756375d59816c365e42ed8417b41665f346289adc68d24
DEBUG Sending revalidation request for: https://<internal-nexus>/repository/pypi-group/packages/pandas/2.2.2/pandas-2.2.2-cp311-cp311-win_amd64.whl#sha256=873d13d177501a28b2756375d59816c365e42ed8417b41665f346289adc68d24
DEBUG Found not-modified response for: https://<internal-nexus>/repository/pypi-group/packages/pandas/2.2.2/pandas-2.2.2-cp311-cp311-win_amd64.whl#sha256=873d13d177501a28b2756375d59816c365e42ed8417b41665f346289adc68d24
DEBUG Adding transitive dependency for pandas==2.2.2: numpy{python_version == '3.11'}>=1.23.2
DEBUG Adding transitive dependency for pandas==2.2.2: python-dateutil>=2.8.2
DEBUG Adding transitive dependency for pandas==2.2.2: pytz>=2020.1
DEBUG Adding transitive dependency for pandas==2.2.2: tzdata>=2022.7
DEBUG Found fresh response for: https://<internal-nexus>/repository/pypi-group/simple/python-dateutil/
DEBUG Found fresh response for: https://<internal-nexus>/repository/pypi-group/simple/pytz/
DEBUG Found fresh response for: https://<internal-nexus>/repository/pypi-group/simple/tzdata/
DEBUG Found fresh response for: https://<internal-nexus>/repository/pypi-group/packages/python-dateutil/2.9.0.post0/python_dateutil-2.9.0.post0-py2.py3-none-any.whl#sha256=a8b2bc7bffae282281c8140a97d3aa9c14da0b136dfe83f850eea9a5f7470427
DEBUG Found fresh response for: https://<internal-nexus>/repository/pypi-group/packages/pytz/2024.1/pytz-2024.1-py2.py3-none-any.whl#sha256=328171f4e3623139da4983451950b28e95ac706e13f3f2630a879749e7a8b319
DEBUG Found fresh response for: https://<internal-nexus>/repository/pypi-group/packages/tzdata/2024.1/tzdata-2024.1-py2.py3-none-any.whl#sha256=9068bc196136463f5245e51efda838afa15aaeca9903f49050dfa2679db4d252
DEBUG Found fresh response for: https://<internal-nexus>/repository/pypi-group/simple/numpy/
DEBUG Searching for a compatible version of numpy{python_version == '3.11'} (>=1.23.2)
DEBUG Selecting: numpy==2.0.0 (numpy-2.0.0-cp311-cp311-win_amd64.whl)
DEBUG Adding transitive dependency for numpy==2.0.0: numpy==2.0.0
DEBUG Adding transitive dependency for numpy==2.0.0: numpy{python_version == '3.11'}==2.0.0
DEBUG Searching for a compatible version of numpy{python_version == '3.11'} (==2.0.0)
DEBUG Selecting: numpy==2.0.0 (numpy-2.0.0-cp311-cp311-win_amd64.whl)
DEBUG Found stale response for: https://<internal-nexus>/repository/pypi-group/packages/numpy/2.0.0/numpy-2.0.0-cp311-cp311-win_amd64.whl#sha256=fbd6acc766814ea6443628f4e6751d0da6593dae29c08c0b2606164db026970c
DEBUG Sending revalidation request for: https://<internal-nexus>/repository/pypi-group/packages/numpy/2.0.0/numpy-2.0.0-cp311-cp311-win_amd64.whl#sha256=fbd6acc766814ea6443628f4e6751d0da6593dae29c08c0b2606164db026970c
DEBUG Found not-modified response for: https://<internal-nexus>/repository/pypi-group/packages/numpy/2.0.0/numpy-2.0.0-cp311-cp311-win_amd64.whl#sha256=fbd6acc766814ea6443628f4e6751d0da6593dae29c08c0b2606164db026970c
DEBUG Searching for a compatible version of numpy (==2.0.0)
DEBUG Selecting: numpy==2.0.0 (numpy-2.0.0-cp311-cp311-win_amd64.whl)
DEBUG Searching for a compatible version of python-dateutil (>=2.8.2)
DEBUG Selecting: python-dateutil==2.9.0.post0 (python_dateutil-2.9.0.post0-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for python-dateutil==2.9.0.post0: six>=1.5
DEBUG Searching for a compatible version of pytz (>=2020.1)
DEBUG Selecting: pytz==2024.1 (pytz-2024.1-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of tzdata (>=2022.7)
DEBUG Selecting: tzdata==2024.1 (tzdata-2024.1-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://<internal-nexus>/repository/pypi-group/simple/six/
DEBUG Searching for a compatible version of six (>=1.5)
DEBUG Selecting: six==1.16.0 (six-1.16.0-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://<internal-nexus>/repository/pypi-group/packages/six/1.16.0/six-1.16.0-py2.py3-none-any.whl#sha256=8abb2f1d86890a2dfb989f9a77cfcfd3e47c2a354b01111771326f8aa26e0254
DEBUG Tried 6 versions: numpy 1, pandas 1, python-dateutil 1, pytz 1, six 1, tzdata 1
DEBUG Split  took 0.091s
Resolved 6 packages in 92ms
# This file was autogenerated by uv via the following command:
#    uv pip compile requirements.in --python-version 3.11 --python-platform windows
numpy==2.0.0
    # via pandas
pandas==2.2.2
    # via -r requirements.in
python-dateutil==2.9.0.post0
    # via pandas
pytz==2024.1
    # via pandas
six==1.16.0
    # via python-dateutil
tzdata==2024.1
    # via pandas
```

Using the above output, its fairly trivial to extract the links to all the .whl artifacts used.
So far so good.

### Not working as expected

When doing the same for package without a matching whl file (i.e. cx-oracle==8.3.0, which is deprecated and has no wheels for 3.11), the output seems to select the wrong artifact.
The compilation is valid as there exists a source distribution, however would expect to see the source distro in the `Found refresh response for: ...` log.
How come `uv` is even checking the manylinux wheel in the first place + the wheel is for 3.10?
I know this is debug output, however something seems to be going wrong here.
Is it possible the debug log is simply outputting the wrong url?


Any help is greatly appreciated.

```text
# requirements.in
cx-oracle==8.3.0
```
```bash
uv pip compile requirements.in --python-version 3.11 --python-platform windows -v
```
```
DEBUG uv 0.2.24
DEBUG Starting Python discovery for Python 3.11
DEBUG Looking for exact match for request Python 3.11
DEBUG Searching for Python 3.11 in system path or `py` launcher
DEBUG Found cpython 3.11.9 at `...\.venv\Scripts\python.exe` (active virtual environment)
DEBUG Using Python 3.11.9 interpreter at ...\.venv\Scripts\python.exe for builds
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.9
DEBUG Solving with target Python version: 3.11.0
DEBUG Adding direct dependency: cx-oracle==8.3.0
DEBUG Found fresh response for: https://<internal-nexus>/repository/pypi-group/simple/cx-oracle/
DEBUG Searching for a compatible version of cx-oracle (==8.3.0)
DEBUG Selecting: cx-oracle==8.3.0 (cx_Oracle-8.3.0.tar.gz)
DEBUG Found fresh response for: https://<internal-nexus>/repository/pypi-group/packages/cx-oracle/8.3.0/cx_Oracle-8.3.0-cp310-cp310-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_12_x86_64.manylinux2010_x86_64.whl#sha256=b6a23da225f03f50a81980c61dbd6a358c3575f212ca7f4c22bb65a9faf94f7f
DEBUG Tried 1 versions: cx-oracle 1
DEBUG Split  took 0.002s
Resolved 1 package in 4ms
# This file was autogenerated by uv via the following command:
#    uv pip compile requirements.in --python-version 3.11 --python-platform windows
cx-oracle==8.3.0
    # via -r requirements.in
```

cx-oracle 8.3.0 artifacts in our repo
![image](https://github.com/user-attachments/assets/51686246-e3b6-4ccd-914f-992e132d5d41)


---

_Comment by @charliermarsh on 2024-07-16 19:46_

Per your second question: if we don't have an available wheel for the current platform, but _do_ have a matching source distribution, we read the metadata from the wheel to avoid having to build the project from source.

---

_Comment by @jbw-vtl on 2024-07-16 20:28_

Okay understood, that makes sense.
Will stay tuned for the `pip download` support then for a more proper solution.

---

_Closed by @jbw-vtl on 2024-07-16 20:28_

---

_Comment by @charliermarsh on 2024-07-16 20:32_

Oh sorry, I was just answering your second question. We _do_ know the list of compatible URLs, so in theory we could write them to the output file.

---

_Comment by @jbw-vtl on 2024-07-16 20:36_

having the compatible artifact url as an optional annotation in the output would make my life a lot easier.

However shouldn't be prioritized too much, for now have built around the debug logs, replacing the logged artifact url with source distribution for packages matching a source distribution.

I.e. `DEBUG Selecting: cx-oracle==8.3.0 (cx_Oracle-8.3.0.tar.gz)`
Definitely not the best solution and building on top of debug logs feels a bit scary, but will do for now.

---

_Comment by @jbw-vtl on 2024-07-18 13:02_

Did actually run in an issue with the debug logs containing different versions for the same artifact, which means I can't really rely on the logs.

Quite keen on getting the option to return the compatible urls somehow

---

_Reopened by @jbw-vtl on 2024-07-18 13:02_

---
