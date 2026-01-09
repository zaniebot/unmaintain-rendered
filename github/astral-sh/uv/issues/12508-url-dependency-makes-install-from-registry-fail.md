---
number: 12508
title: Url dependency makes install from registry fail
type: issue
state: closed
author: ivan94fi
labels:
  - question
assignees: []
created_at: 2025-03-27T09:20:02Z
updated_at: 2025-03-27T14:14:07Z
url: https://github.com/astral-sh/uv/issues/12508
synced_at: 2026-01-07T13:12:18-06:00
---

# Url dependency makes install from registry fail

---

_Issue opened by @ivan94fi on 2025-03-27 09:20_

### Summary

I have a project with a URL dependency on `mvsfunc`:
```
[project]
name = "uv-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"

dependencies = [
    "mvsfunc @ https://github.com/HomeOfVapourSynthEvolution/mvsfunc/archive/865c7486ca860d323754ec4774bc4cca540a7076.zip",
    "vapoursynth==66",
]

[tool.uv.sources]
mvsfunc = { url = "https://github.com/HomeOfVapourSynthEvolution/mvsfunc/archive/865c7486ca860d323754ec4774bc4cca540a7076.zip" }
```

If I build the wheel with `uv build` and install the wheel locally, the installation works.

If I upload the wheel to a local pypi server and try to install the project from pypi, the installation fails with this error:
```
$ uv pip install uv-test
error: Package `mvsfunc` attempted to resolve via URL: https://github.com/HomeOfVapourSynthEvolution/mvsfunc/archive/865c7486ca860d323754ec4774bc4cca540a7076.zip. URL dependencies must be expressed as direct requirements or constraints. Consider adding `mvsfunc @ https://github.com/HomeOfVapourSynthEvolution/mvsfunc/archive/865c7486ca860d323754ec4774bc4cca540a7076.zip` to your dependencies or constraints file.
```

Is this the expected behavior? I am aware of https://docs.astral.sh/uv/pip/compatibility/#transitive-url-dependencies, but I am not sure if this is the case.

## How to reproduce
Use the attached Dockerfile and the following commands: `docker build -t uv_bug_test . && docker run --rm -it uv_bug_test`
```
FROM ubuntu:24.04

SHELL ["/bin/bash", "-c"]

# Install vapoursynth package (need to build vapoursynth itself before)
RUN <<EOF
apt update
apt install -y libzimg-dev git python3-pip python3-dev ffmpeg libass9 imagemagick autoconf pkg-config libtool curl
git clone https://github.com/vapoursynth/vapoursynth.git

pip install cython pypiserver --break-system-packages

cd vapoursynth/
./autogen.sh 
./configure
make -j 10
make install

pip install Vapoursynth==66 --break-system-packages

# Check that everything works
LD_LIBRARY_PATH=/usr/local/lib PYTHONPATH=/usr/local/lib/python3.12/site-packages vspipe --version

EOF

# Install uv, init project
RUN <<EOF
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.local/bin/env

mkdir ~/uv_test
cd ~/uv_test

uv init

EOF

# Create pyproject
COPY <<EOPYPROJECT /root/uv_test/pyproject.toml
[project]
name = "uv-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"

dependencies = [
    "mvsfunc @ https://github.com/HomeOfVapourSynthEvolution/mvsfunc/archive/865c7486ca860d323754ec4774bc4cca540a7076.zip",
    "vapoursynth==66",
]

# [tool.hatch.build.targets.wheel]
# packages = ["hello.py"]
#
# [tool.hatch.metadata]
# allow-direct-references = true

[tool.uv.sources]
mvsfunc = { url = "https://github.com/HomeOfVapourSynthEvolution/mvsfunc/archive/865c7486ca860d323754ec4774bc4cca540a7076.zip" }
EOPYPROJECT

# Build project
RUN <<EOF
cd ~/uv_test
source ~/.local/bin/env
uv venv
uv build
EOF

# Create test script
COPY --chmod=774 <<EOF /test_script.sh
source ~/.local/bin/env
cd ~/uv_test

# 1. Install from local wheel: ok
uv pip install ./dist/uv_test-0.1.0-py3-none-any.whl
uv pip list

# Cleanup
uv pip uninstall mvsfunc uv-test
uv cache clean
uv pip list

# Start pypi server in background subshell
( pypi-server run -p 8080 ~/uv_test/dist/ > /dev/null 2>&1 & )

sleep 1

# 2. Install from pypi: fails
uv pip install --extra-index-url http://localhost:8080 uv-test
# error: Package `mvsfunc` attempted to resolve via URL: https://github.com/HomeOfVapourSynthEvolution/mvsfunc/archive/865c7486ca860d323754ec4774bc4cca540a7076.zip. URL dependencies must be expressed as direct requirements or constraints. Consider adding `mvsfunc @ https://github.com/HomeOfVapourSynthEvolution/mvsfunc/archive/865c7486ca860d323754ec4774bc4cca540a7076.zip` to your dependencies or constraints file.

EOF

ENTRYPOINT ["bash", "/test_script.sh"]
```


### Platform

Linux 6.8.0-52-generic x86_64 GNU/Linux

### Version

0.6.10

### Python version

3.12.3

---

_Label `bug` added by @ivan94fi on 2025-03-27 09:20_

---

_Comment by @charliermarsh on 2025-03-27 12:54_

Yeah, this would qualify as a transitive URL dependency so the behavior is expected as of now.

---

_Label `bug` removed by @charliermarsh on 2025-03-27 12:54_

---

_Label `question` added by @charliermarsh on 2025-03-27 12:54_

---

_Comment by @ivan94fi on 2025-03-27 13:00_

Oh I see, thank you for the clarification. I actually suspected that it was a "non transitive" or direct url dependency as I listed the dependency in pyproject.toml.

What is the situation in which the url dependency is actually direct?

---

_Comment by @charliermarsh on 2025-03-27 13:11_

If you did something like `uv sync` or `uv pip install .`, and the `pyproject.toml` in the current directory referenced a URL, that'd be considered a direct URL dependency.

---

_Comment by @charliermarsh on 2025-03-27 13:11_

In the above case, it's like if you did `uv pip install requests`. We wouldn't consider requests's dependencies to be direct.

---

_Comment by @ivan94fi on 2025-03-27 14:14_

Ok got it now. Thank you very much, closing the issue.

---

_Closed by @ivan94fi on 2025-03-27 14:14_

---
