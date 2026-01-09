---
number: 14610
title: Unexpected upgrade with uv run
type: issue
state: closed
author: ElliottKasoar
labels:
  - question
assignees: []
created_at: 2025-07-14T16:42:56Z
updated_at: 2025-07-15T13:25:22Z
url: https://github.com/astral-sh/uv/issues/14610
synced_at: 2026-01-07T13:12:18-06:00
---

# Unexpected upgrade with uv run

---

_Issue opened by @ElliottKasoar on 2025-07-14 16:42_

### Summary

When calling `uv run`, dependencies seem to be upgraded, even if a valid version has already been installed. For example, with the following pyproject.toml file:

```
[project]
name = "test"
version = "0.0.1"
requires-python = ">=3.10, <3.13"

dependencies = [
    "numpy<3.0.0,>=1.26.4",
]
```

If I run 

```
uv venv
source .venv/bin/activate
uv pip install numpy==1.26.4
uv run python -c "import numpy; print(numpy.__version__)"
```

I find that it upgrades numpy:

```
Using CPython 3.12.8
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
Resolved 1 package in 54ms
Installed 1 package in 6ms
 + numpy==1.26.4
Uninstalled 1 package in 126ms
Installed 1 package in 10ms
2.3.1
```

The documentation suggests that `uv run` makes the `minimum necessary changes`, so this is unexpected, and  causes complications in more complex cases. For example, I have two conflicting optional dependencies, one of which depends on numpy<2, while the other depends on numpy>2, and this upgrade prevents me using `uv sync --extra dependency_1; uv run ...`, as this performs an upgrade inconsistent with `dependency_1`.

I'm aware that `uv run` can also take the `--extra` option, but the current behaviour seems contrary to the intended use, including the non-exact sync.

### Platform

macOS (Darwin 24.5.0 arm64)

### Version

uv 0.7.20 (251420396 2025-07-09)

### Python version

Python 3.12.8

---

_Label `bug` added by @ElliottKasoar on 2025-07-14 16:42_

---

_Comment by @zanieb on 2025-07-14 16:45_

Can you share verbose logs please?

---

_Comment by @zanieb on 2025-07-14 16:46_

With `uv run`, we'll be installing to a version resolved during a lock operation, so the version reflected in `uv.lock`. We'll respect that over existing versions in the environment.

---

_Comment by @charliermarsh on 2025-07-14 16:46_

(Starting typing this so I'll post it anyway.)

Hmm, this seems right to me. Dependency resolution in the project API (including `uv run`) is declarative: it intentionally doesn't take your current virtual environment state into account when performing resolution. So in this case, uv sees your requirements, creates a `uv.lock`, then modifies your mirror the versions in the `uv.lock`; the creation of the `uv.lock` is not affected by your existing `.venv`.

---

_Label `bug` removed by @charliermarsh on 2025-07-14 16:46_

---

_Label `question` added by @charliermarsh on 2025-07-14 16:46_

---

_Comment by @ElliottKasoar on 2025-07-14 16:50_

```
DEBUG uv 0.7.20 (251420396 2025-07-09)
DEBUG Found project root: `/Users/elliottkasoar/Documents/PSDI/test`
DEBUG No workspace root found, using project root
DEBUG Discovered project `test` at: /Users/elliottkasoar/Documents/PSDI/test
DEBUG Acquired lock for `/Users/elliottkasoar/Documents/PSDI/test`
DEBUG No Python version file found in workspace: /Users/elliottkasoar/Documents/PSDI/test
DEBUG Using Python request `>=3.10, <3.13` from `requires-python` metadata
DEBUG Checking for Python environment at `.venv`
DEBUG The project environment's Python version satisfies the request: `Python >=3.10, <3.13`
DEBUG Released lock at `/var/folders/p8/6555_ljx3zx2m306t4nzkfp00000gn/T/uv-d206aab47ecd0b03.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test @ file:///Users/elliottkasoar/Documents/PSDI/test
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 3 packages in 1ms
DEBUG Using request timeout of 30s
DEBUG Requirement installed, but mismatched:
  Installed: Registry(InstalledRegistryDist { name: PackageName("numpy"), version: "1.26.4", path: "/Users/elliottkasoar/Documents/PSDI/test/.venv/lib/python3.12/site-packages/numpy-1.26.4.dist-info", cache_info: None })
  Requested: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.3.1" }]), index: Some(IndexMetadata { url: Pypi(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }, given: None }), format: Simple }), conflict: None }
DEBUG Registry requirement already cached: numpy==2.3.1
DEBUG Uninstalled numpy (793 files, 86 directories)
Uninstalled 1 package in 168ms
Installed 1 package in 10ms
 - numpy==1.26.4
 + numpy==2.3.1
DEBUG Released lock at `/Users/elliottkasoar/Documents/PSDI/test/.venv/.lock`
DEBUG Using Python 3.12.8 interpreter at: /Users/elliottkasoar/Documents/PSDI/test/.venv/bin/python3
DEBUG Running `python -c import numpy; print(numpy.__version__)`
DEBUG Spawned child 79514 in process group 79513
2.3.1
DEBUG Command exited with code: 0
```

I used `uv pip install` for convenience in the minimal example, but this occurs having created a lock file with `uv sync --extra` too, and the installed dependencies shouldn't be inconsistent with the `uv.lock` file created by that.

---

_Comment by @zanieb on 2025-07-14 16:56_

Can you share the `uv sync` example with verbose logs?

---

_Comment by @ElliottKasoar on 2025-07-14 17:01_

Sorry it's a fairly heavy dependency, but this is an example:

```
[project]
name = "test"
version = "0.0.1"
requires-python = ">=3.10, <3.13"

dependencies = [
    "numpy<3.0.0,>=1.26.4",
]

[project.optional-dependencies]
fairchem_1 = [
    "fairchem-core == 1.10.0",
]

fairchem_2 = [
    "fairchem-core == 2.3.0",
]

[tool.uv]
conflicts = [
    [
      { extra = "fairchem_1" },
      { extra = "fairchem_2" },
    ],
]
```

```
uv sync -p 3.12 -U --extra fairchem_1
uv run -v python -c "import numpy; print(numpy.__version__)"
```

```
DEBUG uv 0.7.20 (251420396 2025-07-09)
DEBUG Found project root: `/Users/elliottkasoar/Documents/PSDI/test`
DEBUG No workspace root found, using project root
DEBUG Discovered project `test` at: /Users/elliottkasoar/Documents/PSDI/test
DEBUG Acquired lock for `/Users/elliottkasoar/Documents/PSDI/test`
DEBUG No Python version file found in workspace: /Users/elliottkasoar/Documents/PSDI/test
DEBUG Using Python request `>=3.10, <3.13` from `requires-python` metadata
DEBUG Checking for Python environment at `.venv`
DEBUG The project environment's Python version satisfies the request: `Python >=3.10, <3.13`
DEBUG Released lock at `/var/folders/p8/6555_ljx3zx2m306t4nzkfp00000gn/T/uv-d206aab47ecd0b03.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test @ file:///Users/elliottkasoar/Documents/PSDI/test
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 132 packages in 1ms
DEBUG Using request timeout of 30s
DEBUG Requirement installed, but mismatched:
  Installed: Registry(InstalledRegistryDist { name: PackageName("numpy"), version: "1.26.4", path: "/Users/elliottkasoar/Documents/PSDI/test/.venv/lib/python3.12/site-packages/numpy-1.26.4.dist-info", cache_info: None })
  Requested: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2.2.6" }]), index: Some(IndexMetadata { url: Pypi(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }, given: None }), format: Simple }), conflict: None }
DEBUG Registry requirement already cached: numpy==2.2.6
DEBUG Unnecessary package: gitpython==3.1.44
DEBUG Unnecessary package: markupsafe==3.0.2
DEBUG Unnecessary package: pyyaml==6.0.2
DEBUG Unnecessary package: absl-py==2.3.1
DEBUG Unnecessary package: aiohappyeyeballs==2.6.1
DEBUG Unnecessary package: aiohttp==3.12.14
DEBUG Unnecessary package: aiosignal==1.4.0
DEBUG Unnecessary package: annotated-types==0.7.0
DEBUG Unnecessary package: antlr4-python3-runtime==4.9.3
DEBUG Unnecessary package: ase==3.25.0
DEBUG Unnecessary package: attrs==25.3.0
DEBUG Unnecessary package: bibtexparser==1.4.3
DEBUG Unnecessary package: certifi==2025.7.14
DEBUG Unnecessary package: charset-normalizer==3.4.2
DEBUG Unnecessary package: click==8.2.1
DEBUG Unnecessary package: cloudpickle==3.1.1
DEBUG Unnecessary package: contourpy==1.3.2
DEBUG Unnecessary package: cycler==0.12.1
DEBUG Unnecessary package: e3nn==0.5.6
DEBUG Unnecessary package: fairchem-core==1.10.0
DEBUG Unnecessary package: filelock==3.18.0
DEBUG Unnecessary package: fonttools==4.58.5
DEBUG Unnecessary package: frozenlist==1.7.0
DEBUG Unnecessary package: fsspec==2025.5.1
DEBUG Unnecessary package: gitdb==4.0.12
DEBUG Unnecessary package: grpcio==1.73.1
DEBUG Unnecessary package: hf-xet==1.1.5
DEBUG Unnecessary package: huggingface-hub==0.33.4
DEBUG Unnecessary package: hydra-core==1.3.2
DEBUG Unnecessary package: idna==3.10
DEBUG Unnecessary package: jinja2==3.1.6
DEBUG Unnecessary package: joblib==1.5.1
DEBUG Unnecessary package: kiwisolver==1.4.8
DEBUG Unnecessary package: llvmlite==0.44.0
DEBUG Unnecessary package: lmdb==1.7.2
DEBUG Unnecessary package: markdown==3.8.2
DEBUG Unnecessary package: matplotlib==3.10.3
DEBUG Unnecessary package: monty==2025.3.3
DEBUG Unnecessary package: mpmath==1.3.0
DEBUG Unnecessary package: multidict==6.6.3
DEBUG Unnecessary package: mypy-extensions==1.1.0
DEBUG Unnecessary package: narwhals==1.47.0
DEBUG Unnecessary package: networkx==3.5
DEBUG Unnecessary package: numba==0.61.2
DEBUG Unnecessary package: omegaconf==2.3.0
DEBUG Unnecessary package: opt-einsum==3.4.0
DEBUG Unnecessary package: opt-einsum-fx==0.1.4
DEBUG Unnecessary package: orjson==3.10.18
DEBUG Unnecessary package: packaging==25.0
DEBUG Unnecessary package: palettable==3.3.3
DEBUG Unnecessary package: pandas==2.3.1
DEBUG Unnecessary package: pillow==11.3.0
DEBUG Unnecessary package: platformdirs==4.3.8
DEBUG Unnecessary package: plotly==6.2.0
DEBUG Unnecessary package: propcache==0.3.2
DEBUG Unnecessary package: protobuf==6.31.1
DEBUG Unnecessary package: psutil==7.0.0
DEBUG Unnecessary package: pydantic==2.11.7
DEBUG Unnecessary package: pydantic-core==2.33.2
DEBUG Unnecessary package: pymatgen==2025.6.14
DEBUG Unnecessary package: pyparsing==3.2.3
DEBUG Unnecessary package: pyre-extensions==0.0.32
DEBUG Unnecessary package: python-dateutil==2.9.0.post0
DEBUG Unnecessary package: pytz==2025.2
DEBUG Unnecessary package: requests==2.32.4
DEBUG Unnecessary package: ruamel-yaml==0.18.14
DEBUG Unnecessary package: ruamel-yaml-clib==0.2.12
DEBUG Unnecessary package: scipy==1.16.0
DEBUG Unnecessary package: sentry-sdk==2.32.0
DEBUG Unnecessary package: setuptools==80.9.0
DEBUG Unnecessary package: six==1.17.0
DEBUG Unnecessary package: smmap==5.0.2
DEBUG Unnecessary package: spglib==2.6.0
DEBUG Unnecessary package: submitit==1.5.3
DEBUG Unnecessary package: sympy==1.13.1
DEBUG Unnecessary package: tabulate==0.9.0
DEBUG Unnecessary package: tensorboard==2.19.0
DEBUG Unnecessary package: tensorboard-data-server==0.7.2
DEBUG Unnecessary package: torch==2.4.1
DEBUG Unnecessary package: torch-geometric==2.6.1
DEBUG Unnecessary package: torchtnt==0.2.4
DEBUG Unnecessary package: tqdm==4.67.1
DEBUG Unnecessary package: typing-extensions==4.14.1
DEBUG Unnecessary package: typing-inspect==0.9.0
DEBUG Unnecessary package: typing-inspection==0.4.1
DEBUG Unnecessary package: tzdata==2025.2
DEBUG Unnecessary package: uncertainties==3.2.3
DEBUG Unnecessary package: urllib3==2.5.0
DEBUG Unnecessary package: wandb==0.21.0
DEBUG Unnecessary package: werkzeug==3.1.3
DEBUG Unnecessary package: yarl==1.20.1
DEBUG Uninstalled numpy (793 files, 86 directories)
Uninstalled 1 package in 85ms
Installed 1 package in 10ms
 - numpy==1.26.4
 + numpy==2.2.6
DEBUG Released lock at `/Users/elliottkasoar/Documents/PSDI/test/.venv/.lock`
DEBUG Using Python 3.12.8 interpreter at: /Users/elliottkasoar/Documents/PSDI/test/.venv/bin/python3
DEBUG Running `python -c import numpy; print(numpy.__version__)`
DEBUG Spawned child 81719 in process group 81718
2.2.6
DEBUG Command exited with code: 0
```


---

_Comment by @zanieb on 2025-07-14 17:43_

Thanks for the additional info :)

Since you're not including the `--extra` in the `uv run` command, we are using a different subset of the lockfile which has resolved to a different version of numpy. I don't think there's anything we can do differently here while retaining reasonable behaviors for installing from a lockfile except perhaps resolve to a consistent version of numpy in the first place?

---

_Comment by @ElliottKasoar on 2025-07-15 13:10_

Thanks for all the quick replies!

I hadn't appreciated that this is really about the behaviour of `uv sync`, and more specifically I think it only becomes an issue when you have conflicting dependencies, otherwise the resolution without extras would (I believe) just be a subset of the resolution with extras.

If I remove `fairchem_2` from the above pyproject.toml, for example, `uv sync` would only install `numpy==1.26.4`, so no changes need to  occur between the same `uv sync --extra fairchem_1` and `uv run` (no `--extra`) commands.

Given the conflict, `uv sync` defaulting to the higher version of `numpy` with no extras is definitely preferable to me.

I think a solution might need multiple `uv sync` resolutions that were compatible with the different conflicting extras, but that would likely add a lot of complexity for what is quite a niche issue, and there's a simple alternative with `uv run --extra`, so it's probably not worth the trouble.

---

_Comment by @zanieb on 2025-07-15 13:22_

You can also use `uv run --no-sync` to just leave the environment as-is.

---

_Closed by @zanieb on 2025-07-15 13:25_

---

_Referenced in [stfc/janus-core#574](../../stfc/janus-core/pulls/574.md) on 2025-07-24 15:01_

---
