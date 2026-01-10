```yaml
number: 3456
title: uv pip install and uv pip compile fail while installing packages in container build process
type: issue
state: closed
author: benniekiss
labels:
  - bug
  - network
assignees: []
created_at: 2024-05-08T12:27:51Z
updated_at: 2024-11-26T11:43:52Z
url: https://github.com/astral-sh/uv/issues/3456
synced_at: 2026-01-10T04:36:19Z
```

# uv pip install and uv pip compile fail while installing packages in container build process

---

_Issue opened by @benniekiss on 2024-05-08 12:27_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
uv version = 0.1.40
uv platform = Fedora Atomic Linux (Bazzite) within a podman container

I am using `uv` to install python dependencies during a container build using podman.
I am using `uv pip compile` to generate a requirements.txt from my projects pyproject.toml, and I then install these dependencies using `uv pip install -r requirements.txt`.

The requirements.txt includes several ML and AI python libraries, such as transformers, openaiwhisper, and llama-cpp-python, some of which require wheels to be built.

I've experienced consistent issues in both steps of the install. Either `uv pip compile` or `uv pip install` will fail intermittently with the following error:
```
Caused by: Failed to fetch wheel: ctranslate2==4.2.1                                                                                                                         
Caused by: Failed to extract archive                                                                                                                                         
Caused by: error decoding response body                                                                                                                                      
Caused by: request or response body error                                                                                                                                    
Caused by: error reading a body from connection                                                                                                                              
Caused by: Connection reset by peer (os error 104)  
```
The specific package of the failure is inconsistent -- the install will fail on an almost random one. I've noticed it often fails on building llama-cpp-python

I've noticed the problem is exacerbated when using cached layers, such that re-running the build pipeline is more likely to trigger the failure, and simply running `podman image prune` after a failure and re-running the build increases the likelihood of the package install succeeding.

I've also noticed that `uv pip compile --no-cache` and `uv pip install --no-cache-dir` increase the likelihood of failure. Without the `--no-cache` and `--no-cache-dir` flags, the initial build succeeds, however, subsequent builds fail if the `uv pip compile` or the `uv pip install` layer has been invalidated

here is a sample pyproject.toml and Containerfile where multiple build attempts using the following command cause the error:

pyproject.toml:
```
[project]
name = "build_failure"
version = "0.1.0"
description = "a python program that causes a uv pip install failure"
authors = [
    { name = "benniekiss" }
]
dependencies = [
    "torch>=2.2.1",
    "torchaudio>=2.2.1",
    "torchvision>=0.17.1",
    "faster-whisper>=1.0.2",
    "llama-cpp-python>=0.2.70",
    "openai-whisper>=20231117",
    "openai>=1.16.2",
    "pyannote.audio>=3.1.2",
    "transformers>=4.40.2",
    "accelerate>=0.30.0",
]
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```
Containerfile:
```
FROM ghcr.io/mamba-org/micromamba:jammy

USER $MAMBA_USER

RUN micromamba create --yes \
    -n build_failure -c conda-forge \
    'python>3.12,<3.13' 'uv' 'cxx-compiler' 'cmake' \
    && micromamba clean --yes --all -f

ENV ENV_NAME=build_failure
ARG MAMBA_DOCKERFILE_ACTIVATE=1

COPY --chown=$MAMBA_USER:$MAMBA_USER pyproject.toml /tmp/

RUN uv pip compile pyproject.toml \
    --quiet --no-cache --no-annotate \
    -o runtime_requirements.txt

RUN uv pip install --no-cache-dir \
    -r runtime_requirements.txt
```
build command:
`podman build --platform linux --format docker -t test_failure .`

---

_Comment by @samypr100 on 2024-05-08 23:37_

@benniekiss can you add `RUST_LOG=uv=trace` to your `uv` commands and attach the uv log output when they fail? Ideally using the latest uv version (0.1.42) as of writting. Thank you!

```dockerfile
RUN RUST_LOG=uv=trace uv pip compile pyproject.toml \
    --quiet --no-cache --no-annotate \
    -o runtime_requirements.txt

RUN RUST_LOG=uv=trace uv pip install --no-cache-dir \
    -r runtime_requirements.txt
```

---

_Comment by @benniekiss on 2024-05-09 10:45_

I ran it with trace, but the console logs are immense, so I'll share some snippets from the end. I can share the full logs if a file upload is alright.

~~this is also for `uv==1.41`~~
~~as of writing, conda-forge has not updated to `uv==1.42`~~
EDIT: tested on `uv==0.1.42`, and the same issue occurs.

In one of my tests, I received this new error:
`Error: server probably quit: unexpected EOF`
<details><summary>Details</summary>
<p>

```
TRACE No cache entry exists for /tmp/.tmpN3tiGS/wheels-v1/pypi/fsspec/fsspec-2024.3.1-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl
TRACE cached request https://files.pythonhosted.org/packages/93/6d/66d48b03460768f523da62a57a7e14e5e95fdf339d79e996ce3cecda2cdb/fsspec-2024.3.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/7f/50/9fb3a5c80df6eb6516693270621676980acd6d5a9a7efdbfa273f8d616c7/alembic-1.13.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/65/58/f9c9e6be752e9fcb8b6a0ee9fb87e6e7a1f6bcab2cdc73f02bb7ba91ada0/tzdata-2024.1-py2.py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE No cache entry exists for /tmp/.tmpN3tiGS/wheels-v1/pypi/certifi/certifi-2024.2.2-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ba/06/a07f096c664aeb9f01624f858c3add0a4e913d6c96257acb4fce61e7de14/certifi-2024.2.2-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/ba/06/a07f096c664aeb9f01624f858c3add0a4e913d6c96257acb4fce61e7de14/certifi-2024.2.2-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/ba/06/a07f096c664aeb9f01624f858c3add0a4e913d6c96257acb4fce61e7de14/certifi-2024.2.2-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/ba/06/a07f096c664aeb9f01624f858c3add0a4e913d6c96257acb4fce61e7de14/certifi-2024.2.2-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/ba/06/a07f096c664aeb9f01624f858c3add0a4e913d6c96257acb4fce61e7de14/certifi-2024.2.2-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/ba/06/a07f096c664aeb9f01624f858c3add0a4e913d6c96257acb4fce61e7de14/certifi-2024.2.2-py3-none-any.whl
Error: server probably quit: unexpected EOF
```

</p>
</details> 

I also ran the build command a few times, and it failed on a different package each time -- av, llama-cpp-python, torchvision, and scikit-learn. I used `podman build --no-cache` each time.

Here is the console output from one of those builds:
<details><summary>Details</summary>
<p>

```
TRACE found candidate for package PackageName("wheel") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "0.43.0" version
TRACE selecting candidate for package setuptools with range Range { segments: [(Included("40.8.0"), Unbounded)] } with 557 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "69.5.1" version
DEBUG Installing in setuptools==69.5.1, wheel==0.43.0 in /tmp/.tmp6kkAee/.tmpEwiJ4V/.venv
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("setuptools"), version: "69.5.1", path: "/tmp/.tmp6kkAee/.tmpEwiJ4V/.venv/lib/python3.12/site-packages/setuptools-69.5.1.dist-info" }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "69.5.1" }]), index: None }
DEBUG Requirement already installed: setuptools==69.5.1
DEBUG Must revalidate requirement: wheel
DEBUG Downloading and building requirement for build: wheel==0.43.0
DEBUG Installing build requirement: wheel==0.43.0
DEBUG Calling `setuptools.build_meta:__legacy__.build_wheel(metadata_directory=None)`
DEBUG Finished building: julius==0.2.7
DEBUG Finished building: docopt==0.6.2
DEBUG Building: psutil==5.9.8
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: setuptools>=43
DEBUG Adding direct dependency: wheel*
DEBUG Searching for a compatible version of setuptools (>=43)
TRACE selecting candidate for package setuptools with range Range { segments: [(Included("43"), Unbounded)] } with 557 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("43"), Unbounded)] } after 1 steps: "69.5.1" version
DEBUG Selecting: setuptools==69.5.1 (setuptools-69.5.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wheel (*)
TRACE selecting candidate for package wheel with range Range { segments: [(Unbounded, Unbounded)] } with 75 remote versions
TRACE found candidate for package PackageName("wheel") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "0.43.0" version
DEBUG Selecting: wheel==0.43.0 (wheel-0.43.0-py3-none-any.whl)
DEBUG Tried 3 versions: root 1, setuptools 1, wheel 1
TRACE selecting candidate for package wheel with range Range { segments: [(Unbounded, Unbounded)] } with 75 remote versions
TRACE found candidate for package PackageName("wheel") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "0.43.0" version
TRACE selecting candidate for package setuptools with range Range { segments: [(Included("43"), Unbounded)] } with 557 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("43"), Unbounded)] } after 1 steps: "69.5.1" version
DEBUG Installing in setuptools==69.5.1, wheel==0.43.0 in /tmp/.tmp6kkAee/.tmpxAlLbm/.venv
DEBUG Must revalidate requirement: setuptools
DEBUG Must revalidate requirement: wheel
DEBUG Downloading and building requirements for build: setuptools==69.5.1, wheel==0.43.0
DEBUG Installing build requirements: setuptools==69.5.1, wheel==0.43.0
DEBUG Calling `setuptools.build_meta.get_requires_for_build_wheel()`
DEBUG Calling `setuptools.build_meta.build_wheel(metadata_directory=None)`
DEBUG Finished building: psutil==5.9.8
error: Failed to download distributions
  Caused by: Failed to fetch wheel: av==12.0.0
  Caused by: Failed to extract archive
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: Connection reset by peer (os error 104)
Error: building at STEP "RUN RUST_LOG=uv=trace uv pip install -v --no-cache-dir --refresh      -r runtime_requirements.txt": while running runtime: exit status 2
```

</p>
</details> 

Here's another output with a build failure, not just download failure:
<details><summary>Details</summary>
<p>

```
DEBUG Searching for a compatible version of julius (==0.2.7)
TRACE selecting candidate for package julius with range Range { segments: [(Included("0.2.7"), Included("0.2.7"))] } with 11 remote versions
TRACE found candidate for package PackageName("julius") with range Range { segments: [(Included("0.2.7"), Included("0.2.7"))] } after 1 steps: "0.2.7" version
DEBUG Selecting: julius==0.2.7 (julius-0.2.7.tar.gz)
TRACE Received source distribution metadata for: julius==0.2.7
DEBUG Adding transitive dependency for julius==0.2.7: torch>=1.7.0
DEBUG Searching for a compatible version of kiwisolver (==1.4.5)
TRACE selecting candidate for package kiwisolver with range Range { segments: [(Included("1.4.5"), Included("1.4.5"))] } with 17 remote versions
TRACE found candidate for package PackageName("kiwisolver") with range Range { segments: [(Included("1.4.5"), Included("1.4.5"))] } after 1 steps: "1.4.5" version
DEBUG Selecting: kiwisolver==1.4.5 (kiwisolver-1.4.5-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
DEBUG Searching for a compatible version of lazy-loader (==0.4)
TRACE selecting candidate for package lazy-loader with range Range { segments: [(Included("0.4"), Included("0.4"))] } with 9 remote versions
TRACE found candidate for package PackageName("lazy-loader") with range Range { segments: [(Included("0.4"), Included("0.4"))] } after 1 steps: "0.4" version
DEBUG Selecting: lazy-loader==0.4 (lazy_loader-0.4-py3-none-any.whl)
DEBUG Adding transitive dependency for lazy-loader==0.4: packaging*
DEBUG Searching for a compatible version of librosa (==0.10.2)
TRACE selecting candidate for package librosa with range Range { segments: [(Included("0.10.2"), Included("0.10.2"))] } with 40 remote versions
TRACE found candidate for package PackageName("librosa") with range Range { segments: [(Included("0.10.2"), Included("0.10.2"))] } after 1 steps: "0.10.2" version
DEBUG Selecting: librosa==0.10.2 (librosa-0.10.2-py3-none-any.whl)
DEBUG Adding transitive dependency for librosa==0.10.2: audioread>=2.1.9
DEBUG Adding transitive dependency for librosa==0.10.2: numpy>=1.20.3, <1.22.0 | >1.22.0, <1.22.1 | >1.22.1, <1.22.2 | >1.22.2
DEBUG Adding transitive dependency for librosa==0.10.2: scipy>=1.2.0
DEBUG Adding transitive dependency for librosa==0.10.2: scikit-learn>=0.20.0
DEBUG Adding transitive dependency for librosa==0.10.2: joblib>=0.14
DEBUG Adding transitive dependency for librosa==0.10.2: decorator>=4.3.0
DEBUG Adding transitive dependency for librosa==0.10.2: numba>=0.51.0
DEBUG Adding transitive dependency for librosa==0.10.2: soundfile>=0.12.1
DEBUG Adding transitive dependency for librosa==0.10.2: pooch>=1.1
DEBUG Adding transitive dependency for librosa==0.10.2: soxr>=0.3.2
DEBUG Adding transitive dependency for librosa==0.10.2: typing-extensions>=4.1.1
DEBUG Adding transitive dependency for librosa==0.10.2: lazy-loader>=0.1
DEBUG Adding transitive dependency for librosa==0.10.2: msgpack>=1.0
DEBUG Searching for a compatible version of lightning (==2.2.4)
TRACE selecting candidate for package lightning with range Range { segments: [(Included("2.2.4"), Included("2.2.4"))] } with 58 remote versions
TRACE found candidate for package PackageName("lightning") with range Range { segments: [(Included("2.2.4"), Included("2.2.4"))] } after 10 steps: "2.2.4" version
DEBUG Selecting: lightning==2.2.4 (lightning-2.2.4-py3-none-any.whl)
DEBUG Adding transitive dependency for lightning==2.2.4: pyyaml>=5.4, <8.0
DEBUG Adding transitive dependency for lightning==2.2.4: fsspec>=2022.5.0, <2025.0
DEBUG Adding transitive dependency for lightning==2.2.4: fsspec[http]>=2022.5.0, <2025.0
DEBUG Adding transitive dependency for lightning==2.2.4: lightning-utilities>=0.8.0, <2.0
DEBUG Adding transitive dependency for lightning==2.2.4: numpy>=1.17.2, <3.0
DEBUG Adding transitive dependency for lightning==2.2.4: packaging>=20.0, <25.0
DEBUG Adding transitive dependency for lightning==2.2.4: torch>=1.13.0, <4.0
DEBUG Adding transitive dependency for lightning==2.2.4: torchmetrics>=0.7.0, <3.0
DEBUG Adding transitive dependency for lightning==2.2.4: tqdm>=4.57.0, <6.0
DEBUG Adding transitive dependency for lightning==2.2.4: typing-extensions>=4.4.0, <6.0
DEBUG Adding transitive dependency for lightning==2.2.4: pytorch-lightning*
DEBUG Searching for a compatible version of fsspec[http] (>=2022.5.0, <2025.0)
TRACE selecting candidate for package fsspec with range Range { segments: [(Included("2022.5.0"), Excluded("2025.0"))] } with 79 remote versions
TRACE found candidate for package PackageName("fsspec") with range Range { segments: [(Included("2022.5.0"), Excluded("2025.0"))] } after 1 steps: "2024.3.1" version
DEBUG Selecting: fsspec[http]==2024.3.1 (fsspec-2024.3.1-py3-none-any.whl)
DEBUG Searching for a compatible version of fsspec[http] (==2024.3.1)
TRACE selecting candidate for package fsspec with range Range { segments: [(Included("2024.3.1"), Included("2024.3.1"))] } with 79 remote versions
TRACE found candidate for package PackageName("fsspec") with range Range { segments: [(Included("2024.3.1"), Included("2024.3.1"))] } after 1 steps: "2024.3.1" version
DEBUG Selecting: fsspec[http]==2024.3.1 (fsspec-2024.3.1-py3-none-any.whl)
DEBUG Adding transitive dependency for fsspec[http]==2024.3.1: aiohttp<4.0.0a0 | >4.0.0a0, <4.0.0a1 | >4.0.0a1
DEBUG Searching for a compatible version of lightning-utilities (==0.11.2)
TRACE selecting candidate for package lightning-utilities with range Range { segments: [(Included("0.11.2"), Included("0.11.2"))] } with 17 remote versions
TRACE found candidate for package PackageName("lightning-utilities") with range Range { segments: [(Included("0.11.2"), Included("0.11.2"))] } after 1 steps: "0.11.2" version
DEBUG Selecting: lightning-utilities==0.11.2 (lightning_utilities-0.11.2-py3-none-any.whl)
DEBUG Adding transitive dependency for lightning-utilities==0.11.2: packaging>=17.1
DEBUG Adding transitive dependency for lightning-utilities==0.11.2: setuptools*
DEBUG Adding transitive dependency for lightning-utilities==0.11.2: typing-extensions*
DEBUG Searching for a compatible version of llama-cpp-python (==0.2.71)
TRACE selecting candidate for package llama-cpp-python with range Range { segments: [(Included("0.2.71"), Included("0.2.71"))] } with 153 remote versions
TRACE found candidate for package PackageName("llama-cpp-python") with range Range { segments: [(Included("0.2.71"), Included("0.2.71"))] } after 1 steps: "0.2.71" version
DEBUG Selecting: llama-cpp-python==0.2.71 (llama_cpp_python-0.2.71.tar.gz)
DEBUG Prepared metadata for: openai-whisper==20231117
TRACE Received source distribution metadata for: openai-whisper==20231117
error: Failed to download and build `llama-cpp-python==0.2.71`
  Caused by: Failed to extract archive
  Caused by: failed to unpack `/tmp/.tmpNTDHWT/built-wheels-v3/.tmpy7UReQ/llama_cpp_python-0.2.71/.git/modules/vendor/llama.cpp/objects/pack/pack-95bb5b6ca6e6853a13794eaea7acaf6853907f95.pack`
  Caused by: failed to unpack `llama_cpp_python-0.2.71/.git/modules/vendor/llama.cpp/objects/pack/pack-95bb5b6ca6e6853a13794eaea7ac` into `/tmp/.tmpNTDHWT/built-wheels-v3/.tmpy7UReQ/llama_cpp_python-0.2.71/.git/modules/vendor/llama.cpp/objects/pack/pack-95bb5b6ca6e6853a13794eaea7acaf6853907f95.pack`
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: Connection reset by peer (os error 104)
Error: building at STEP "RUN RUST_LOG=uv=trace uv pip install -v --no-cache-dir --refresh      -r runtime_requirements.txt": while running runtime: exit status 2
```

</p>
</details> 

---

_Comment by @konstin on 2024-05-09 19:08_

Do you know if there's anything special about your network, like a proxy or a vpn?

---

_Comment by @MichaReiser on 2024-05-09 20:01_

I don't have a solution nor do I know what exactly is causing your issue. What I noticed when benchmarking `uv` is that I saw a lot of connection reset by peer errors. I suspect that it's my internet service provider who resets the connections because it thinks I'm doing something malicious because my computer issues a lot of requests to the same host in a very short period of time.

What helped in my case is to use a VPN (Proton to be precise) to bypass my ISPs throttling. Do you have a VPN that you could connect to?

We're planning to add an environment variable that would allow you to reduce the number of parallel requests https://github.com/astral-sh/uv/pull/3493. You could try to reduce the number of parallel requests once that PR is shipped.

---

_Comment by @benniekiss on 2024-05-09 20:13_

I have been able to reproduce the issue on two different machines in two different geographic locations. I've also tested locally with a VPN.

The Fedora Linux machine is a PC with AMD 7800x3D and the other machine is an Apple M2 Max laptop, where podman is running in a linux VM.

It's interesting because maybe 2/10 times it will succeed and install, otherwise, it fails--either with a build or download error, and always `os error 104`

---

_Label `bug` added by @konstin on 2024-05-09 20:14_

---

_Comment by @konstin on 2024-05-09 20:15_

Labelling as a bug because we should consider adding retries for `Connection reset by peer` errors.

---

_Comment by @charliermarsh on 2024-05-09 20:16_

In both cases you’re running with Podman? Are there any other commonalities? (Very strange, we’ve never seen reports of this.)

---

_Comment by @benniekiss on 2024-05-09 20:19_

> In both cases you’re running with Podman? Are there any other commonalities? (Very strange, we’ve never seen reports of this.)

Yep, both cases are with `podman v5.0.2`. I connect to the remote PC via tailscale, but I tested the build again with all of my vpns off, and I am getting the same errors locally.

I'm using the example pyproject.toml and Containerfile each time, on both machines.

---

_Comment by @samypr100 on 2024-05-09 22:46_

I was able to reproduce 1/20~ times, but using `docker` instead. I've been playing around with rayon, ulimits and throttling my bandwith to see there's some consistent way to reproduce. I did get an failed to unpack error on the one failure. Will keep experimenting.

```
error: Failed to download and build `llama-cpp-python==0.2.71`
  Caused by: Failed to extract archive
  Caused by: failed to unpack `/tmp/.tmppG2oG4/built-wheels-v3/.tmptTJnWx/llama_cpp_python-0.2.71/vendor/llama.cpp/kompute/docs/images/komputer-godot-4.gif`
  Caused by: failed to unpack `llama_cpp_python-0.2.71/vendor/llama.cpp/kompute/docs/images/komputer-godot-4.gif` into `/tmp/.tmppG2oG4/built-wheels-v3/.tmptTJnWx/llama_cpp_python-0.2.71/vendor/llama.cpp/kompute/docs/images/komputer-godot-4.gif`
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: Connection reset by peer (os error 104)
```

(edit: forgot to include trace logs - [uv_trace_logs.txt](https://github.com/astral-sh/uv/files/15269104/uv_trace_logs.txt))


---

_Comment by @polarathene on 2024-05-17 07:48_

> remote PC via tailscale

Does that affect DNS at all?

While I haven't encountered an issue myself with `uv`, I recently used some other tools which have encountered issues when run in Docker containers. Turned out to be due to the default DNS layer managed by Docker internally.

```console
# Get an IP address:
$ docker run --rm -it ghcr.io/natesales/q smtp.gmail.com
smtp.gmail.com. 2m22s A 64.233.170.108
smtp.gmail.com. 4m47s AAAA 2404:6800:4003:c1a::6c

# Attempt to do a reverse DNS lookup (silently fails, no response):
$ docker run --rm -it ghcr.io/natesales/q --verbose --reverse 64.233.170.108
DEBU[0000] Name: 108.170.233.64.in-addr.arpa.
DEBU[0000] RR types: [PTR A AAAA NS MX TXT CNAME]
DEBU[0000] No server specified or Q_DEFAULT_SERVER set, using /etc/resolv.conf
DEBU[0000] found server [192.168.65.7] from /etc/resolv.conf
DEBU[0000] Server(s): [192.168.65.7]
DEBU[0000] Using server 192.168.65.7:53 with transport plain
DEBU[0000] Using UDP with TCP fallback: 192.168.65.7:53

# Repeat, but use CloudFlare as the DNS service (success):
$ docker run --rm -it --dns 1.1.1.1 ghcr.io/natesales/q --verbose --reverse 64.233.170.108
DEBU[0000] Name: 108.170.233.64.in-addr.arpa.
DEBU[0000] RR types: [CNAME PTR A AAAA NS MX TXT]
DEBU[0000] No server specified or Q_DEFAULT_SERVER set, using /etc/resolv.conf
DEBU[0000] found server [1.1.1.1] from /etc/resolv.conf
DEBU[0000] Server(s): [1.1.1.1]
DEBU[0000] Using server 1.1.1.1:53 with transport plain
DEBU[0000] Using UDP with TCP fallback: 1.1.1.1:53

108.170.233.64.in-addr.arpa. 1h PTR sg-in-f108.1e100.net.
```

I can run `dig -x` for the equivalent on the Docker host and it resolves no problem (_goes through the DNS from the router IIRC_), so it appears to be an issue with the CRI networking layer. I'm not sure what Podman does, but it's probably similar to support related networking features?

I've also encountered other issues like a `TXT` record for `github.com` that is multi-part (exceeds the 255 limit) where this internal DNS cannot handle the reply and fails.

While performing larger queries (_resolve a variety of DNS records for a domain, and resolve the CNAME records to get any from those too_) can work, there were some stalls and failures. This would still occur when querying `1.1.1.1`, and I had not tried to reproduce outside of the container to check if it only occurs there.

---

Not sure if DNS issues like above could contribute to the failure here, just thought I'd draw attention to a known networking caveat within a container environment.

## Additional context

With Docker the daemon is by default also configured to use the `userland-proxy`, which IIRC halved the I/O rate vs host connections when I tested, but was minimal overhead when disabled. There's also host mode networking which should skip some other indirections which might help minimize the issue. Additionally the default network a container is assigned with Docker is the legacy bridge, it's slightly different than using custom networks (_`docker network create`, which Docker Compose uses by default implicitly, not the legacy bridge_).

```console
# NOTE: Doesn't use the host DNS (still the same one the Docker daemon is configured for):
$ docker run --rm -it --network host ghcr.io/natesales/q --verbose --reverse 64.233.170.108
DEBU[0000] Name: 108.170.233.64.in-addr.arpa.
DEBU[0000] RR types: [A AAAA NS MX TXT CNAME PTR]
DEBU[0000] No server specified or Q_DEFAULT_SERVER set, using /etc/resolv.conf
DEBU[0000] found server [192.168.65.7] from /etc/resolv.conf
DEBU[0000] Server(s): [192.168.65.7]
DEBU[0000] Using server 192.168.65.7:53 with transport plain
DEBU[0000] Using UDP with TCP fallback: 192.168.65.7:53
```

For added context, the snippets are run via WSL2 (Ubuntu) on Windows 11 with Docker Desktop (_which has `192.168.65.0/24` configured as it's managed network subnet for containers_), and runs Docker within a separate WSL2 distro than the default Ubuntu distro I run the commands from.

---

_Comment by @vikigenius on 2024-06-11 17:08_

I am also facing this issue. I don't use docker or any containers, I get this just on my local machine. Like mentioned above it's not consistent. I got it 1/5 times

---

_Comment by @charliermarsh on 2024-11-22 20:34_

We've added a lot more retrying, including for the cases described here, so I'm going to close for now.

---

_Closed by @charliermarsh on 2024-11-22 20:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-22 20:34_

---

_Label `network` added by @konstin on 2024-11-26 11:43_

---
