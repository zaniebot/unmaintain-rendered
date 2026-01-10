```yaml
number: 8242
title: "Support https://astral.sh/uv/install.sh running as Nobody"
type: issue
state: closed
author: VirtualTim
labels:
  - external
  - releases
assignees: []
created_at: 2024-10-16T06:46:16Z
updated_at: 2024-10-21T13:35:25Z
url: https://github.com/astral-sh/uv/issues/8242
synced_at: 2026-01-10T04:45:10Z
```

# Support https://astral.sh/uv/install.sh running as Nobody

---

_Issue opened by @VirtualTim on 2024-10-16 06:46_

I would like to use uv as a jenkins action, similar to https://github.com/astral-sh/setup-uv.
When I'm running inside a docker container, Jenkins runs as `Nobody` (specifically `-u 1000:1000`).
`$HOME` evaluates to `/`, which I do not have permission to write to.

It seems like I can get things working by running:
```sh
export CARGO_HOME=$WORKSPACE/cargo
export HOME=$CARGO_HOME/uv
curl -LsSf https://astral.sh/uv/install.sh | sh -s -- -v --no-modify-path
export UV_CACHE_DIR=$CARGO_HOME/.cache/uv
```

I have concerns about how future-proof this is. Is the file written to `$HOME/.config/uv/uv-receipt.json` important?
Is this likely to cause other things to break?
It would be nice if I could install this without having to temporarily modify `$HOME`.

---

_Label `upstream` added by @charliermarsh on 2024-10-16 13:33_

---

_Label `releases` added by @charliermarsh on 2024-10-16 13:33_

---

_Comment by @charliermarsh on 2024-10-16 19:41_

In the next release, you should be able to set `UV_UNMANAGED_INSTALL` to the exact location you want to use, without a receipt: https://opensource.axo.dev/cargo-dist/book/installers/usage.html#receipt. It seems like a good fit for your use-case.

---

_Closed by @charliermarsh on 2024-10-16 20:25_

---

_Closed by @charliermarsh on 2024-10-16 20:25_

---

_Comment by @VirtualTim on 2024-10-17 04:43_

Awesome. Thanks for the quick response, and for updating the documentation.

---

_Comment by @VirtualTim on 2024-10-18 09:03_

@charliermarsh I just tried out uv 0.4.23 and 0.4.24, and I still need to modify `HOME`.
Installing like this works fine:
```sh
export CARGO_HOME=$WORKSPACE/cargo
export UV_UNMANAGED_INSTALL=$CARGO_HOME/uv
export UV_CACHE_DIR=$CARGO_HOME/.cache/uv
curl -LsSf https://astral.sh/uv/install.sh | sh -s -- -v --no-modify-path
```

However when I attempt to setup a venv I get the following error:
```sh
uv venv --python 3.13
  x failed to create directory `/.local/share/uv/python`
  `-> Permission denied (os error 13)
```
same command with a modified `$HOME`:
```sh
export HOME=$(pwd)
uv venv --python 3.13
Using CPython 3.13.0
Creating virtual environment at: .venv
```

Should I create a separate issue?

---

_Comment by @charliermarsh on 2024-10-18 18:01_

@VirtualTim -- That looks a bit different. You may need to set `UV_PYTHON_INSTALL_DIR` if you want the `python` installs to go elsewhere (see: https://docs.astral.sh/uv/configuration/environment/).

---

_Comment by @VirtualTim on 2024-10-21 03:29_

I'm now running with:
```sh
export UV_UNMANAGED_INSTALL=$WORKSPACE/cargo
export UV_PYTHON_INSTALL_DIR=$UV_UNMANAGED_INSTALL/python
export UV_CACHE_DIR=$CARGO_HOME/.cache/uv
export uv=$UV_UNMANAGED_INSTALL/uv
curl -LsSf https://astral.sh/uv/install.sh | sh -s -- -v

$uv venv --python 3.13
```

This all seems to work as expected.
I would have expected `UV_PYTHON_INSTALL_DIR` to default to somewhere under `UV_CACHE_DIR`.

It might be a good idea to add the defaults to the documentation (I also see `UV_UNMANAGED_INSTALL` is missing).

---

On an unrelated note, I really appreciate this tool. My use-case is that I want to run tests against a cross-platform SDK. I have ~10 different platforms to test on. These tests are written in Python. Managing Python (and dependency) versions would normally be a huge headache, but now I can just run uv in my build scripts, and my environment is set up. Now updating python versions across VMs/docker images/OSs is as simple as just changing one number in a Jenkinsfile.

---

_Comment by @VirtualTim on 2024-10-21 03:43_

Also, if you'd like, I could add a quick guide here https://docs.astral.sh/uv/guides/ on how to run as Nobody?
The guide would basically just be a copy/paste of the snippet I commented above.

---

_Comment by @zanieb on 2024-10-21 13:35_

I don't think is a common enough use-case to cover in the top-level guides. I'd definitely appreciate some content on it though. If you put up a pull request with the content I can try to figure out where it fits.

---
