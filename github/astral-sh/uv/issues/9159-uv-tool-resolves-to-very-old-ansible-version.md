---
number: 9159
title: "`uv tool` resolves to very old Ansible version unless `--no-build` is set"
type: issue
state: closed
author: ketozhang
labels:
  - bug
assignees: []
created_at: 2024-11-16T01:55:25Z
updated_at: 2024-11-16T03:24:49Z
url: https://github.com/astral-sh/uv/issues/9159
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv tool` resolves to very old Ansible version unless `--no-build` is set

---

_Issue opened by @ketozhang on 2024-11-16 01:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Ansbile is currently at version >=10. To install Ansible such that `ansible-playbook` CLI is available, the entrypoint exists in `ansible-core`. However, most of the features and build-in plugins are from the `ansible` package. Both needs to be installed via `uv tool install --with ansible  ansible-core`.

Oddly, uv is selecting a very old source distribution version of Ansbile unless I pass in the `--no-build` flag.


```Dockerfile
FROM debian:bookworm AS base

COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/local/bin/uv

RUN --mount=type=cache,target=/root/.cache \
    uv tool install --python=3.12 --verbose --with ansible  ansible-core
```

```console
#8 6.636 DEBUG Selecting: ansible==5.10.0 [compatible] (ansible-5.10.0.tar.gz)
```

<details><summary>Full build log</summary>
<p>

```console
❯ docker build -f Dockerfile --progress=plain .
#0 building with "default" instance using docker-container driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 251B 0.0s done
#1 DONE 0.0s

#2 [internal] load metadata for docker.io/library/debian:bookworm
#2 ...

#3 [internal] load metadata for ghcr.io/astral-sh/uv:latest
#3 DONE 0.2s

#2 [internal] load metadata for docker.io/library/debian:bookworm
#2 DONE 0.4s

#4 [internal] load .dockerignore
#4 transferring context: 2B done
#4 DONE 0.0s

#5 FROM ghcr.io/astral-sh/uv:latest@sha256:ab5cd8c7946ae6a359a9aea9073b5effd311d40a65310380caae938a1abf55da
#5 resolve ghcr.io/astral-sh/uv:latest@sha256:ab5cd8c7946ae6a359a9aea9073b5effd311d40a65310380caae938a1abf55da 0.0s done
#5 DONE 0.0s

#6 [base 1/3] FROM docker.io/library/debian:bookworm@sha256:10901ccd8d249047f9761845b4594f121edef079cfd8224edebd9ea726f0a7f6
#6 resolve docker.io/library/debian:bookworm@sha256:10901ccd8d249047f9761845b4594f121edef079cfd8224edebd9ea726f0a7f6 0.0s done
#6 DONE 0.0s

#7 [base 2/3] COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/local/bin/uv
#7 CACHED

#8 [base 3/3] RUN --mount=type=cache,target=/root/.cache     uv tool install --python=3.12 --verbose --with ansible  ansible-core
#8 0.094 DEBUG uv 0.5.2
#8 0.094 DEBUG Searching for Python 3.12 in managed installations or search path
#8 0.094 DEBUG Searching for managed installations at `/root/.local/share/uv/python`
#8 0.100 DEBUG Requested Python not found, checking for available download...
#8 0.103 DEBUG Acquired lock for `/root/.local/share/uv/python`
#8 0.103 DEBUG Using request timeout of 30s
#8 0.104 INFO Fetching requested Python...
#8 0.640 DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.12.7%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /root/.local/share/uv/python/.cache/.tmpseZBkQ
#8 0.640 DEBUG Extracting cpython-3.12.7%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
#8 6.286 DEBUG Moving /root/.local/share/uv/python/.cache/.tmpseZBkQ/python to /root/.local/share/uv/python/cpython-3.12.7-linux-x86_64-gnu
#8 6.528 DEBUG Released lock at `/root/.local/share/uv/python/.lock`
#8 6.530 DEBUG Acquired lock for `/root/.local/share/uv/tools`
#8 6.534 DEBUG Using request timeout of 30s
#8 6.536 DEBUG Solving with installed Python version: 3.12.7
#8 6.536 DEBUG Solving with target Python version: >=3.12.7
#8 6.536 DEBUG Adding direct dependency: ansible-core*
#8 6.536 DEBUG Adding direct dependency: ansible*
#8 6.548 DEBUG Found fresh response for: https://pypi.org/simple/ansible/
#8 6.550 DEBUG Found fresh response for: https://pypi.org/simple/ansible-core/
#8 6.550 DEBUG Searching for a compatible version of ansible-core (*)
#8 6.550 DEBUG Selecting: ansible-core==2.18.0 [compatible] (ansible_core-2.18.0-py3-none-any.whl)
#8 6.551 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ee/6b/08f9e535de750a2a2f367934bc04a6757c3c9106b1a85e9b15ae50b47945/ansible-10.6.0-py3-none-any.whl.metadata
#8 6.553 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/73/14/c70341980b7636dfdbc03b0762102d2a69739bc5d7687a0476739007aad4/ansible_core-2.18.0-py3-none-any.whl.metadata
#8 6.553 DEBUG Adding transitive dependency for ansible-core==2.18.0: jinja2>=3.0.0
#8 6.553 DEBUG Adding transitive dependency for ansible-core==2.18.0: pyyaml>=5.1
#8 6.553 DEBUG Adding transitive dependency for ansible-core==2.18.0: cryptography*
#8 6.553 DEBUG Adding transitive dependency for ansible-core==2.18.0: packaging*
#8 6.553 DEBUG Adding transitive dependency for ansible-core==2.18.0: resolvelib>=0.5.3, <1.1.0
#8 6.553 DEBUG Searching for a compatible version of ansible (*)
#8 6.553 DEBUG Selecting: ansible==10.6.0 [compatible] (ansible-10.6.0-py3-none-any.whl)
#8 6.553 DEBUG Adding transitive dependency for ansible==10.6.0: ansible-core>=2.17.6, <2.18.dev0
#8 6.555 DEBUG Searching for a compatible version of ansible (<10.6.0 | >10.6.0)
#8 6.555 DEBUG Selecting: ansible==10.5.0 [compatible] (ansible-10.5.0-py3-none-any.whl)
#8 6.559 DEBUG Found fresh response for: https://pypi.org/simple/jinja2/
#8 6.563 DEBUG Found fresh response for: https://pypi.org/simple/resolvelib/
#8 6.563 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2e/33/4cb64286f44cd36753cd15ef636be6c9e40be331e14e97caca74cb7a3242/ansible-10.5.0-py3-none-any.whl.metadata
#8 6.564 DEBUG Found fresh response for: https://pypi.org/simple/pyyaml/
#8 6.567 DEBUG Found fresh response for: https://pypi.org/simple/packaging/
#8 6.568 DEBUG Adding transitive dependency for ansible==10.5.0: ansible-core>=2.17.5, <2.18.dev0
#8 6.568 DEBUG Searching for a compatible version of ansible (<10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.569 DEBUG Selecting: ansible==10.4.0 [compatible] (ansible-10.4.0-py3-none-any.whl)
#8 6.569 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/31/80/3a54838c3fb461f6fec263ebf3a3a41771bd05190238de3486aae8540c36/jinja2-3.1.4-py3-none-any.whl.metadata
#8 6.571 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d2/fc/e9ccf0521607bcd244aa0b3fbd574f71b65e9ce6a112c83af988bbbe2e23/resolvelib-1.0.1-py2.py3-none-any.whl.metadata
#8 6.572 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl.metadata
#8 6.573 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b9/2b/614b4752f2e127db5cc206abc23a8c19678e92b23c3db30fc86ab731d3bd/PyYAML-6.0.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
#8 6.574 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7f/8d/b3db0a928de1a15cb5374e745e5ee089b2b2e5f8459bd7fb9bd1b1754808/ansible-10.4.0-py3-none-any.whl.metadata
#8 6.575 DEBUG Adding transitive dependency for ansible==10.4.0: ansible-core>=2.17.4, <2.18.dev0
#8 6.575 DEBUG Searching for a compatible version of ansible (<10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.575 DEBUG Selecting: ansible==10.3.0 [compatible] (ansible-10.3.0-py3-none-any.whl)
#8 6.576 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/05/f6/3f0f055b1bda33adb64b0af0686b0360b1144835fbc7cfde915430cc41d1/ansible-10.3.0-py3-none-any.whl.metadata
#8 6.576 DEBUG Adding transitive dependency for ansible==10.3.0: ansible-core>=2.17.3, <2.18.dev0
#8 6.577 DEBUG Searching for a compatible version of ansible (<10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.577 DEBUG Selecting: ansible==10.2.0 [compatible] (ansible-10.2.0-py3-none-any.whl)
#8 6.577 DEBUG Found fresh response for: https://pypi.org/simple/cryptography/
#8 6.579 DEBUG Prefetching 5 ansible versions
#8 6.580 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/27/f4/e1d6465313ea3d30be5dfebad910c59e09df9457cf3a57c121a00741157a/ansible-10.2.0-py3-none-any.whl.metadata
#8 6.580 DEBUG Adding transitive dependency for ansible==10.2.0: ansible-core>=2.17.2, <2.18.dev0
#8 6.580 DEBUG Searching for a compatible version of ansible (<10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.580 DEBUG Selecting: ansible==10.1.0 [compatible] (ansible-10.1.0-py3-none-any.whl)
#8 6.583 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ac/25/e715fa0bc24ac2114ed69da33adf451a38abb6f3f24ec207908112e9ba53/cryptography-43.0.3-cp39-abi3-manylinux_2_28_x86_64.whl.metadata
#8 6.584 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/78/be/45a0b5456711fe792a9a9a7e7cc06dfb40edd46713aaf0609f7500afc806/ansible-10.1.0-py3-none-any.whl.metadata
#8 6.585 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/dc/96/830575b7e33d3f4f98530a5bea11f2534e7389a03a8723cbd7cbbbca6d50/ansible-9.12.0-py3-none-any.whl.metadata
#8 6.585 DEBUG Adding transitive dependency for ansible==10.1.0: ansible-core>=2.17.1, <2.18.dev0
#8 6.585 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/28/7c/a5f708b7b033f068a8ef40db5c993bee4cfafadd985d48dfe44db8566fc6/ansible-10.0.1-py3-none-any.whl.metadata
#8 6.585 DEBUG Searching for a compatible version of ansible (<10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.585 DEBUG Selecting: ansible==10.0.1 [compatible] (ansible-10.0.1-py3-none-any.whl)
#8 6.587 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/56/d6/f879ce85aa2c9e1ea8f8aa14c3e129accf21b317ab408ec310e2f7fe1bc6/ansible-9.11.0-py3-none-any.whl.metadata
#8 6.587 DEBUG Adding transitive dependency for ansible==10.0.1: ansible-core>=2.17.0, <2.18.dev0
#8 6.587 DEBUG Searching for a compatible version of ansible (<10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.587 DEBUG Selecting: ansible==9.12.0 [compatible] (ansible-9.12.0-py3-none-any.whl)
#8 6.587 DEBUG Adding transitive dependency for ansible==9.12.0: ansible-core>=2.16.13, <2.17.dev0
#8 6.588 DEBUG Searching for a compatible version of ansible (<9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.588 DEBUG Selecting: ansible==9.11.0 [compatible] (ansible-9.11.0-py3-none-any.whl)
#8 6.588 DEBUG Adding transitive dependency for ansible==9.11.0: ansible-core>=2.16.12, <2.17.dev0
#8 6.588 DEBUG Searching for a compatible version of ansible (<9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.588 DEBUG Selecting: ansible==9.10.0 [compatible] (ansible-9.10.0-py3-none-any.whl)
#8 6.591 DEBUG Prefetching 10 ansible versions
#8 6.592 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/96/1c/3715ae56d9ff839a72a6d246f32b4061ee497834ed76386dd32e688c49ad/ansible-9.8.0-py3-none-any.whl.metadata
#8 6.592 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1b/06/76fb08a99cdedbfaacf878a5f01ab8ba20ed97bff0e757dcf5db45f63130/ansible-9.6.1-py3-none-any.whl.metadata
#8 6.592 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/54/31/ca3c339eb792489f352d4e274bc22099005b4ca5551cff1bb2d761934f3e/ansible-9.7.0-py3-none-any.whl.metadata
#8 6.593 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b3/7e/7f614931d25e1e4cac4bf4d959441768c952eeada91ff2b21e563b1f5f79/ansible-9.10.0-py3-none-any.whl.metadata
#8 6.593 DEBUG Adding transitive dependency for ansible==9.10.0: ansible-core>=2.16.11, <2.17.dev0
#8 6.593 DEBUG Searching for a compatible version of ansible (<9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.593 DEBUG Selecting: ansible==9.9.0 [compatible] (ansible-9.9.0-py3-none-any.whl)
#8 6.594 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ac/40/a2c6f948559339e15f6c6fe4b793fb741a50f5ca6c9ba63b5d961029e269/ansible-9.5.1-py3-none-any.whl.metadata
#8 6.594 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cb/c4/b65ba6af9f609570773dd7d73d27a4c1aeba62a5bb14e512e44a8de663c0/ansible-9.9.0-py3-none-any.whl.metadata
#8 6.594 DEBUG Adding transitive dependency for ansible==9.9.0: ansible-core>=2.16.10, <2.17.dev0
#8 6.594 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/22/2a/3d251fb613e0f6a6bac5d6795bf0a34f4397b1908813bc216e483a6e3b4c/ansible-9.4.0-py3-none-any.whl.metadata
#8 6.595 DEBUG Searching for a compatible version of ansible (<9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.595 DEBUG Selecting: ansible==9.8.0 [compatible] (ansible-9.8.0-py3-none-any.whl)
#8 6.595 DEBUG Adding transitive dependency for ansible==9.8.0: ansible-core>=2.16.9, <2.17.dev0
#8 6.596 DEBUG Searching for a compatible version of ansible (<9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.596 DEBUG Selecting: ansible==9.7.0 [compatible] (ansible-9.7.0-py3-none-any.whl)
#8 6.596 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/32/f0/f7a66e975b9edf511e7223fe8d5642b16f1147087d4acc3d1be788267b41/ansible-9.1.0-py3-none-any.whl.metadata
#8 6.596 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b9/39/7a6f7698f2af3393600653200543ba41e67edaa10b4efe729815031cc850/ansible-9.3.0-py3-none-any.whl.metadata
#8 6.596 DEBUG Adding transitive dependency for ansible==9.7.0: ansible-core>=2.16.8, <2.17.dev0
#8 6.596 DEBUG Searching for a compatible version of ansible (<9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.596 DEBUG Selecting: ansible==9.6.1 [compatible] (ansible-9.6.1-py3-none-any.whl)
#8 6.596 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0d/7e/3e4967b4881e8eb7bbbb430de328f95969db235adff2d3c32581e4e2f800/ansible-9.2.0-py3-none-any.whl.metadata
#8 6.596 DEBUG Adding transitive dependency for ansible==9.6.1: ansible-core>=2.16.7, <2.17.dev0
#8 6.596 DEBUG Searching for a compatible version of ansible (<9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.596 DEBUG Selecting: ansible==9.5.1 [compatible] (ansible-9.5.1-py3-none-any.whl)
#8 6.596 DEBUG Adding transitive dependency for ansible==9.5.1: ansible-core>=2.16.6, <2.17.dev0
#8 6.596 DEBUG Searching for a compatible version of ansible (<9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.596 DEBUG Selecting: ansible==9.4.0 [compatible] (ansible-9.4.0-py3-none-any.whl)
#8 6.596 DEBUG Adding transitive dependency for ansible==9.4.0: ansible-core>=2.16.5, <2.17.dev0
#8 6.597 DEBUG Searching for a compatible version of ansible (<9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.597 DEBUG Selecting: ansible==9.3.0 [compatible] (ansible-9.3.0-py3-none-any.whl)
#8 6.597 DEBUG Adding transitive dependency for ansible==9.3.0: ansible-core>=2.16.4, <2.17.dev0
#8 6.597 DEBUG Searching for a compatible version of ansible (<9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.597 DEBUG Selecting: ansible==9.2.0 [compatible] (ansible-9.2.0-py3-none-any.whl)
#8 6.597 DEBUG Adding transitive dependency for ansible==9.2.0: ansible-core>=2.16.3, <2.17.dev0
#8 6.597 DEBUG Searching for a compatible version of ansible (<9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.597 DEBUG Selecting: ansible==9.1.0 [compatible] (ansible-9.1.0-py3-none-any.whl)
#8 6.597 DEBUG Adding transitive dependency for ansible==9.1.0: ansible-core>=2.16.1, <2.17.dev0
#8 6.597 DEBUG Searching for a compatible version of ansible (<9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.597 DEBUG Selecting: ansible==9.0.1 [compatible] (ansible-9.0.1-py3-none-any.whl)
#8 6.602 DEBUG Prefetching 20 ansible versions
#8 6.604 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e5/9a/d8e83a8f64f208eeaf84f45358d85302efc14ff7c60c4c9d792c3d275155/ansible-9.0.1-py3-none-any.whl.metadata
#8 6.604 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b4/1a/c88115997ce826c402d63da78bcbf5183a1ffe23308898eafedc5db9a8aa/ansible-8.7.0-py3-none-any.whl.metadata
#8 6.604 DEBUG Adding transitive dependency for ansible==9.0.1: ansible-core>=2.16.0, <2.17.dev0
#8 6.604 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b3/b1/9b9f975f491fedb5562e5eba17a1b6adab86f408e1168b59f5633bd36801/ansible-8.6.1-py3-none-any.whl.metadata
#8 6.605 DEBUG Searching for a compatible version of ansible (<9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.605 DEBUG Selecting: ansible==8.7.0 [compatible] (ansible-8.7.0-py3-none-any.whl)
#8 6.605 DEBUG Adding transitive dependency for ansible==8.7.0: ansible-core>=2.15.7, <2.16.dev0
#8 6.607 DEBUG Searching for a compatible version of ansible (<8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.607 DEBUG Selecting: ansible==8.6.1 [compatible] (ansible-8.6.1-py3-none-any.whl)
#8 6.610 DEBUG Adding transitive dependency for ansible==8.6.1: ansible-core>=2.15.6, <2.16.dev0
#8 6.612 DEBUG Searching for a compatible version of ansible (<8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.612 DEBUG Selecting: ansible==8.6.0 [compatible] (ansible-8.6.0-py3-none-any.whl)
#8 6.612 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/74/ef/54a04b6abbeeb88689bbb13eb66304733e98dc433a1ec6106d01b1492f67/ansible-8.6.0-py3-none-any.whl.metadata
#8 6.612 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4f/7d/de88451cb4b7cda10eebf94b448fdae6a6feb8719e735c135994ef312f17/ansible-8.4.0-py3-none-any.whl.metadata
#8 6.612 DEBUG Adding transitive dependency for ansible==8.6.0: ansible-core>=2.15.6, <2.16.dev0
#8 6.613 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/a2/35c6110299306db0495b61c57d24d042f57858923fbb1bee85347c1ed8bc/ansible-8.5.0-py3-none-any.whl.metadata
#8 6.613 DEBUG Searching for a compatible version of ansible (<8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.613 DEBUG Selecting: ansible==8.5.0 [compatible] (ansible-8.5.0-py3-none-any.whl)
#8 6.613 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/37/12/77be5a84116e25cb0d6e0fb7e62fa02b8578319486466cb14035a13c115a/ansible-8.2.0-py3-none-any.whl.metadata
#8 6.613 DEBUG Adding transitive dependency for ansible==8.5.0: ansible-core>=2.15.5, <2.16.dev0
#8 6.613 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/68/a7/101e0ae83e4932a0e9c6e5db2d3088e3273198a78677eccd4d432f06dfa3/ansible-8.3.0-py3-none-any.whl.metadata
#8 6.614 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f5/0f/fd3546905e2fae6fa8fc646d6698887c8e31f625d1016936f984223d2d95/ansible-7.7.0-py3-none-any.whl.metadata
#8 6.614 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/01/43/b5620e57e14a84d9bf257ba4797c2abfa13f7c3be04bc8cd35f86d2e6a2d/ansible-8.1.0-py3-none-any.whl.metadata
#8 6.614 DEBUG Searching for a compatible version of ansible (<8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.614 DEBUG Selecting: ansible==8.4.0 [compatible] (ansible-8.4.0-py3-none-any.whl)
#8 6.614 DEBUG Adding transitive dependency for ansible==8.4.0: ansible-core>=2.15.4, <2.16.dev0
#8 6.614 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/41/21/48fa41c5de7985672cd332a9e91fd21bb6b76e51c72bd30beffff1e63829/ansible-7.5.0-py3-none-any.whl.metadata
#8 6.614 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d8/47/46e0517e62e17fde4f29fac3f29876df914eba3012c7cd260748a740d7a1/ansible-7.3.0-py3-none-any.whl.metadata
#8 6.614 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c5/27/3cb0c589345f29aa26ab0c12006dbd16365bb8b137e854736243b3e1a61a/ansible-7.2.0-py3-none-any.whl.metadata
#8 6.614 DEBUG Searching for a compatible version of ansible (<8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.614 DEBUG Selecting: ansible==8.3.0 [compatible] (ansible-8.3.0-py3-none-any.whl)
#8 6.614 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0d/34/1b50f134f3136eeddf87f1b50253c1dece059407f5de57044963c82d07c0/ansible-7.4.0-py3-none-any.whl.metadata
#8 6.614 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/57/33/0c6024d1114e267a362f1c5a0593c2c1a41f5b556da59b48ae47b35acc56/ansible-7.1.0-py3-none-any.whl.metadata
#8 6.614 DEBUG Adding transitive dependency for ansible==8.3.0: ansible-core>=2.15.3, <2.16.dev0
#8 6.615 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9a/54/ceba345e4f42ea9d4b8c6f24a51c5edd382ead93acd8f170ce5150e4885d/ansible-7.6.0-py3-none-any.whl.metadata
#8 6.615 DEBUG Searching for a compatible version of ansible (<8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.615 DEBUG Selecting: ansible==8.2.0 [compatible] (ansible-8.2.0-py3-none-any.whl)
#8 6.615 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8f/78/ccf8d6b0cd4d71761be99fb8486f4a8ac3ca2f86887ff6802dcfa384a8ac/ansible-8.0.0-py3-none-any.whl.metadata
#8 6.615 DEBUG Adding transitive dependency for ansible==8.2.0: ansible-core>=2.15.2, <2.16.dev0
#8 6.615 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f7/62/4b3ee9140dd95b49ae43a0321e2a517fd6101797e620bbed7095a3ecd46e/ansible-7.0.0-py3-none-any.whl.metadata
#8 6.616 DEBUG Searching for a compatible version of ansible (<8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.616 DEBUG Selecting: ansible==8.1.0 [compatible] (ansible-8.1.0-py3-none-any.whl)
#8 6.616 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d2/ba/99b47f2c618eef5044e2d8cbb0c12d80ba9586492f3e85fe1367fc38523b/ansible-6.7.0-py3-none-any.whl.metadata
#8 6.616 DEBUG Adding transitive dependency for ansible==8.1.0: ansible-core>=2.15.1, <2.16.dev0
#8 6.617 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8c/95/e5c31232b6f9d8fcdd2c9dfc33f09438895fd3d0eea912a1183784040f46/ansible-6.6.0-py3-none-any.whl.metadata
#8 6.617 DEBUG Searching for a compatible version of ansible (<8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.617 DEBUG Selecting: ansible==8.0.0 [compatible] (ansible-8.0.0-py3-none-any.whl)
#8 6.618 DEBUG Adding transitive dependency for ansible==8.0.0: ansible-core>=2.15.0, <2.16.dev0
#8 6.618 DEBUG Searching for a compatible version of ansible (<8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.618 DEBUG Selecting: ansible==7.7.0 [compatible] (ansible-7.7.0-py3-none-any.whl)
#8 6.619 DEBUG Adding transitive dependency for ansible==7.7.0: ansible-core>=2.14.7, <2.15.dev0
#8 6.619 DEBUG Searching for a compatible version of ansible (<7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.619 DEBUG Selecting: ansible==7.6.0 [compatible] (ansible-7.6.0-py3-none-any.whl)
#8 6.619 DEBUG Adding transitive dependency for ansible==7.6.0: ansible-core>=2.14.6, <2.15.dev0
#8 6.619 DEBUG Searching for a compatible version of ansible (<7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.619 DEBUG Selecting: ansible==7.5.0 [compatible] (ansible-7.5.0-py3-none-any.whl)
#8 6.619 DEBUG Adding transitive dependency for ansible==7.5.0: ansible-core>=2.14.5, <2.15.dev0
#8 6.619 DEBUG Searching for a compatible version of ansible (<7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.619 DEBUG Selecting: ansible==7.4.0 [compatible] (ansible-7.4.0-py3-none-any.whl)
#8 6.619 DEBUG Adding transitive dependency for ansible==7.4.0: ansible-core>=2.14.4, <2.15.dev0
#8 6.620 DEBUG Searching for a compatible version of ansible (<7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.620 DEBUG Selecting: ansible==7.3.0 [compatible] (ansible-7.3.0-py3-none-any.whl)
#8 6.620 DEBUG Adding transitive dependency for ansible==7.3.0: ansible-core>=2.14.3, <2.15.dev0
#8 6.620 DEBUG Searching for a compatible version of ansible (<7.3.0 | >7.3.0, <7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.620 DEBUG Selecting: ansible==7.2.0 [compatible] (ansible-7.2.0-py3-none-any.whl)
#8 6.620 DEBUG Adding transitive dependency for ansible==7.2.0: ansible-core>=2.14.2, <2.15.dev0
#8 6.621 DEBUG Searching for a compatible version of ansible (<7.2.0 | >7.2.0, <7.3.0 | >7.3.0, <7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.621 DEBUG Selecting: ansible==7.1.0 [compatible] (ansible-7.1.0-py3-none-any.whl)
#8 6.621 DEBUG Adding transitive dependency for ansible==7.1.0: ansible-core>=2.14.1, <2.15.dev0
#8 6.621 DEBUG Searching for a compatible version of ansible (<7.1.0 | >7.1.0, <7.2.0 | >7.2.0, <7.3.0 | >7.3.0, <7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.621 DEBUG Selecting: ansible==7.0.0 [compatible] (ansible-7.0.0-py3-none-any.whl)
#8 6.621 DEBUG Adding transitive dependency for ansible==7.0.0: ansible-core>=2.14.0, <2.15.dev0
#8 6.622 DEBUG Searching for a compatible version of ansible (<7.0.0 | >7.0.0, <7.1.0 | >7.1.0, <7.2.0 | >7.2.0, <7.3.0 | >7.3.0, <7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.622 DEBUG Selecting: ansible==6.7.0 [compatible] (ansible-6.7.0-py3-none-any.whl)
#8 6.622 DEBUG Adding transitive dependency for ansible==6.7.0: ansible-core>=2.13.7, <2.14.dev0
#8 6.623 DEBUG Searching for a compatible version of ansible (<6.7.0 | >6.7.0, <7.0.0 | >7.0.0, <7.1.0 | >7.1.0, <7.2.0 | >7.2.0, <7.3.0 | >7.3.0, <7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.623 DEBUG Selecting: ansible==6.6.0 [compatible] (ansible-6.6.0-py3-none-any.whl)
#8 6.623 DEBUG Adding transitive dependency for ansible==6.6.0: ansible-core>=2.13.6, <2.14.dev0
#8 6.623 DEBUG Searching for a compatible version of ansible (<6.6.0 | >6.6.0, <6.7.0 | >6.7.0, <7.0.0 | >7.0.0, <7.1.0 | >7.1.0, <7.2.0 | >7.2.0, <7.3.0 | >7.3.0, <7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.623 DEBUG Selecting: ansible==6.5.0 [compatible] (ansible-6.5.0-py3-none-any.whl)
#8 6.627 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/70/32/9998469eef19b04278f4695d8c633cdd8bdb985c25536de0b32134c927b7/ansible-6.5.0-py3-none-any.whl.metadata
#8 6.628 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1a/96/7fe46c417a1587318c740bd952a37cde6ae29e620f37392d8204a7ec7e95/ansible-6.4.0-py3-none-any.whl.metadata
#8 6.629 DEBUG Prefetching 6 ansible versions
#8 6.629 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/49/1c/5514bc4ff1a9ca3edc42aeedeb2aafd66a28617559be8c01268f22894e37/ansible-6.3.0-py3-none-any.whl.metadata
#8 6.630 DEBUG Adding transitive dependency for ansible==6.5.0: ansible-core>=2.13.5, <2.14.dev0
#8 6.630 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/10/84/f91f381b068d11e9553d57a02f3b116cb1949e023d6f61c72213fb3e6529/ansible-6.0.0-py3-none-any.whl.metadata
#8 6.630 DEBUG Searching for a compatible version of ansible (<6.5.0 | >6.5.0, <6.6.0 | >6.6.0, <6.7.0 | >6.7.0, <7.0.0 | >7.0.0, <7.1.0 | >7.1.0, <7.2.0 | >7.2.0, <7.3.0 | >7.3.0, <7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.630 DEBUG Selecting: ansible==6.4.0 [compatible] (ansible-6.4.0-py3-none-any.whl)
#8 6.630 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7d/e6/e303be0d0256816f9308ff901c983682d3618b5d007349c39e7559c1c92b/ansible-6.2.0-py3-none-any.whl.metadata
#8 6.630 DEBUG Adding transitive dependency for ansible==6.4.0: ansible-core>=2.13.4, <2.14.dev0
#8 6.631 DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3a/84/51264fe488339cfed6954703b2f7c335c3542cc468309737a3280a900a3f/ansible-6.1.0-py3-none-any.whl.metadata
#8 6.631 DEBUG Searching for a compatible version of ansible (<6.4.0 | >6.4.0, <6.5.0 | >6.5.0, <6.6.0 | >6.6.0, <6.7.0 | >6.7.0, <7.0.0 | >7.0.0, <7.1.0 | >7.1.0, <7.2.0 | >7.2.0, <7.3.0 | >7.3.0, <7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.631 DEBUG Selecting: ansible==6.3.0 [compatible] (ansible-6.3.0-py3-none-any.whl)
#8 6.631 DEBUG Adding transitive dependency for ansible==6.3.0: ansible-core>=2.13.3, <2.14.dev0
#8 6.632 DEBUG Searching for a compatible version of ansible (<6.3.0 | >6.3.0, <6.4.0 | >6.4.0, <6.5.0 | >6.5.0, <6.6.0 | >6.6.0, <6.7.0 | >6.7.0, <7.0.0 | >7.0.0, <7.1.0 | >7.1.0, <7.2.0 | >7.2.0, <7.3.0 | >7.3.0, <7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.632 DEBUG Selecting: ansible==6.2.0 [compatible] (ansible-6.2.0-py3-none-any.whl)
#8 6.632 DEBUG Adding transitive dependency for ansible==6.2.0: ansible-core>=2.13.2, <2.14.dev0
#8 6.632 DEBUG Searching for a compatible version of ansible (<6.2.0 | >6.2.0, <6.3.0 | >6.3.0, <6.4.0 | >6.4.0, <6.5.0 | >6.5.0, <6.6.0 | >6.6.0, <6.7.0 | >6.7.0, <7.0.0 | >7.0.0, <7.1.0 | >7.1.0, <7.2.0 | >7.2.0, <7.3.0 | >7.3.0, <7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.632 DEBUG Selecting: ansible==6.1.0 [compatible] (ansible-6.1.0-py3-none-any.whl)
#8 6.633 DEBUG Adding transitive dependency for ansible==6.1.0: ansible-core>=2.13.1, <2.14.dev0
#8 6.634 DEBUG Searching for a compatible version of ansible (<6.1.0 | >6.1.0, <6.2.0 | >6.2.0, <6.3.0 | >6.3.0, <6.4.0 | >6.4.0, <6.5.0 | >6.5.0, <6.6.0 | >6.6.0, <6.7.0 | >6.7.0, <7.0.0 | >7.0.0, <7.1.0 | >7.1.0, <7.2.0 | >7.2.0, <7.3.0 | >7.3.0, <7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.634 DEBUG Selecting: ansible==6.0.0 [compatible] (ansible-6.0.0-py3-none-any.whl)
#8 6.634 DEBUG Adding transitive dependency for ansible==6.0.0: ansible-core>=2.13.0, <2.14.dev0
#8 6.635 DEBUG Acquired lock for `/root/.cache/uv/sdists-v6/pypi/ansible/5.10.0`
#8 6.636 DEBUG Searching for a compatible version of ansible (<6.0.0 | >6.0.0, <6.1.0 | >6.1.0, <6.2.0 | >6.2.0, <6.3.0 | >6.3.0, <6.4.0 | >6.4.0, <6.5.0 | >6.5.0, <6.6.0 | >6.6.0, <6.7.0 | >6.7.0, <7.0.0 | >7.0.0, <7.1.0 | >7.1.0, <7.2.0 | >7.2.0, <7.3.0 | >7.3.0, <7.4.0 | >7.4.0, <7.5.0 | >7.5.0, <7.6.0 | >7.6.0, <7.7.0 | >7.7.0, <8.0.0 | >8.0.0, <8.1.0 | >8.1.0, <8.2.0 | >8.2.0, <8.3.0 | >8.3.0, <8.4.0 | >8.4.0, <8.5.0 | >8.5.0, <8.6.0 | >8.6.0, <8.6.1 | >8.6.1, <8.7.0 | >8.7.0, <9.0.1 | >9.0.1, <9.1.0 | >9.1.0, <9.2.0 | >9.2.0, <9.3.0 | >9.3.0, <9.4.0 | >9.4.0, <9.5.1 | >9.5.1, <9.6.1 | >9.6.1, <9.7.0 | >9.7.0, <9.8.0 | >9.8.0, <9.9.0 | >9.9.0, <9.10.0 | >9.10.0, <9.11.0 | >9.11.0, <9.12.0 | >9.12.0, <10.0.1 | >10.0.1, <10.1.0 | >10.1.0, <10.2.0 | >10.2.0, <10.3.0 | >10.3.0, <10.4.0 | >10.4.0, <10.5.0 | >10.5.0, <10.6.0 | >10.6.0)
#8 6.636 DEBUG Selecting: ansible==5.10.0 [compatible] (ansible-5.10.0.tar.gz)
#8 6.636 DEBUG No cache entry for: https://files.pythonhosted.org/packages/e9/f9/8c96f93dcb3441048520d39e09790edb5af082305fb93cef461f2d443a0c/ansible-5.10.0.tar.gz
#8 6.711 DEBUG Downloading source distribution: ansible==5.10.0
^C#8 CANCELED
WARNING: No output specified with docker-container driver. Build result will only remain in the build cache. To push result image into registry use --push or to load image into docker use --load
ERROR: failed to solve: Canceled: context canceled
```

</p>
</details> 

```console
❯ uv --version
uv 0.5.1 (Homebrew 2024-11-08)
```

---

_Comment by @charliermarsh on 2024-11-16 03:24_

Ah interesting. This is the same as https://github.com/astral-sh/uv/issues/8157.

The basic idea...

- We try to pick the most recent version of `ansible-core`. We pick `2.18.0`.

- We then try to pick the most recent version of `ansible`. We pick `10.6.0`.

- That version of `ansible` declares a dependency on `ansible-core ~=2.17.6`.

- `2.18.0` isn't compatible with `ansible-core ~=2.17.6`. So we then have to backtrack.

- We backtrack on `ansible` rather than `ansible-core`, because we saw `ansbile` first.
- We eventually start checking very old versions of `ansible` (like `5.10.0`) to see if any of them are compatible with the latest `ansible-core`.

This will be solved by the approach outlined in https://github.com/astral-sh/uv/issues/8157.

---

_Label `bug` added by @charliermarsh on 2024-11-16 03:24_

---

_Comment by @charliermarsh on 2024-11-16 03:24_

In the meantime, you should probably do `--with ansible>=10` or similar.

---

_Closed by @charliermarsh on 2024-11-16 03:24_

---

_Comment by @charliermarsh on 2024-11-16 03:24_

(Combining with the linked issue.)

---
