```yaml
number: 8729
title: "Build a wheel using pinned/resolved dependencies from `uv.lock`?"
type: issue
state: open
author: rahuliyer95
labels:
  - question
assignees: []
created_at: 2024-10-31T16:52:45Z
updated_at: 2025-07-02T10:45:27Z
url: https://github.com/astral-sh/uv/issues/8729
synced_at: 2026-01-12T15:59:33Z
```

# Build a wheel using pinned/resolved dependencies from `uv.lock`?

---

_@rahuliyer95_

_(I believe this question has probably been asked before, but for some reason I am not able to find the previous issue so please feel free to direct me that issue if you are able to find it)_

I am trying to build a wheel for my project, for which I simply ran

```sh
$ uv build --wheel
```

Inspecting the wheel I noticed that it took the dependencies from `pyproject.toml`. Is there a way to use the pinned dependencies from the `uv.lock` file itself?

```sh
$ unzip -p dist/playground-1.0.0-py3-none-any.whl playground-1.0.0.dist-info/METADATA
...
Requires-Dist: aiofiles ~=24.1
Requires-Dist: fsspec ~=2024.0
Requires-Dist: matplotlib ~=3.7
Requires-Dist: mpire[dill] ~=2.8
Requires-Dist: numpy ~=1.26
Requires-Dist: pandas ==1.5.3
Requires-Dist: pendulum ~=3.0
Requires-Dist: pyarrow ~=16.0
Requires-Dist: pyyaml ~=6.0
Requires-Dist: s3fs ~=2024.0
Requires-Dist: tqdm ~=4.0
Requires-Dist: universal-pathlib ~=0.2
Requires-Dist: uvloop ~=0.19
Requires-Dist: yarl ~=1.8
```

<details>

<summary><code>pyproject.toml</code></summary>

```toml
[project]
name = "playground"
version = "1.0.0"
description = "Playground"
authors = [{ name = "Rahul Iyer", email = "me@rahuliyer.me" }]
requires-python = ">=3.10"
readme = "README.md"
dependencies = [
  "aiofiles~=24.1",
  "fsspec~=2024.0",
  "matplotlib~=3.7",
  "mpire[dill]~=2.8",
  "numpy~=1.26",
  "pandas==1.5.3",
  "pendulum~=3.0",
  "pyarrow~=16.0",
  "pyyaml~=6.0",
  "s3fs~=2024.0",
  "tqdm~=4.0",
  "universal-pathlib~=0.2",
  "uvloop~=0.19",
  "yarl~=1.8",
]

[tool.uv]
dev-dependencies = [
  "mypy~=1.11",
  "pandas-stubs~=1.5.3",
  "ptpython~=3.0",
  "pytest~=7.2",
  "ruff~=0.4",
  "types-aiofiles~=23.2",
  "types-certifi~=2021.10",
  "types-pyyaml~=6.0",
  "types-tqdm~=4.0",
]

[tool.ruff]
exclude = [".venv"]
line-length = 100
target-version = "py310"

[tool.ruff.format]
docstring-code-format = true
```

</details>



---

_Comment by @charliermarsh on 2024-11-01 13:34_

I think we're somewhat unlikely to support this... The built wheel needs to use the declared project metadata, not the resolved application versions. It might also violate the spec in some sense.

What problem are you trying to solve? What are you looking to do with the wheel?


---

_Label `question` added by @charliermarsh on 2024-11-01 13:34_

---

_Comment by @rahuliyer95 on 2024-11-01 16:33_

> What problem are you trying to solve?

I'll try to explain without sharing too much internal details. Our setup for ETL jobs require us to install wheels from our internal PyPI installations (because of various limitations on support for alternatives like Docker images). When the ETL job starts the first thing it does is 

```sh
pip install --index <internal-pypi-index-url> <package-name>==<package-version>
# with above example
# pip install --index <internal-pypi-index-url> playground==1.0.0
```

With the above wheel it would end up resolving versions for dependencies when it's installing the wheel and it might install a different patch version than the one we tested with. To avoid this problem I was hoping that we can build the wheel from the pinned versions. Our existing setup uses [poetry](https://python-poetry.org/) and we use the [poetry-freeze-wheel](https://github.com/cloud-custodian/poetry-plugin-freeze) plugin to solve this problem.

> I think we're somewhat unlikely to support this... The built wheel needs to use the declared project metadata, not the resolved application versions. It might also violate the spec in some sense.

I can totally understand the complexity of this very non-standard use-case. Unfortunately, I am not sure how many others want a behavior like this (through some CLI option maybe). I was trying to migrate from `poetry` to `uv` and this was the last blocker on the list. 

@charliermarsh Please let me know if this explains the use-case and if any other details are needed from me. Thanks for all the amazing work you do!

---

_Comment by @zanieb on 2024-11-01 23:23_

I'd love to try to get some sort of `--locked` install concept into the standards, perhaps after we manage to standardize on a lock format.

---

_Comment by @EternityForest on 2024-11-11 04:10_

This issue is also the blocker for me switching from Poetry.

Locked dependencies in wheels allow leaving Docker and the like out completely and making apps users can install with a single pipx command.

As I understand Poetry plugins do it by building the wheel, then modifying it after, so it doesn't seem like it's too hard to implement since it's basically just copying the data you already have in uv.lock, right?

---

_Comment by @idan-rahamim-lendbuzz on 2024-11-20 10:04_

Let me share an experience to illustrate why using pinned dependencies during the build process is crucial.

I maintain a package (CLI tool) called My Package, which lists aiodocker as a dependency in its pyproject.toml. The aiodocker package, in turn, specifies aiohttp with a version constraint of ^3.8 in its pyproject.toml. Since my package doesn’t directly use aiohttp, it’s not listed as a direct dependency.

In our GitHub Actions (GHA) workflows, my package is installed frequently from a private artifact. Each installation pulls the latest compatible version of aiohttp because it’s a transitive dependency of aiodocker and satisfies the ^3.8 version constraint.

This week, a new version of aiohttp (3.11) introduced a breaking change that caused aiodocker to break. As a result, my package—despite no changes on my end—broke because of this upstream issue.

Here’s the related issue for reference: [aio-libs/aiodocker#918](https://github.com/aio-libs/aiodocker/issues/918).

Currently i use poetry and they don't have a solution for that:
https://github.com/python-poetry/poetry/issues/2778

---

_Comment by @EternityForest on 2024-11-20 10:07_

@idan-rahamim-lendbuzz poetry does have third party plugins like https://github.com/cloud-custodian/poetry-plugin-freeze, but I'm not aware of a UV equivalent yet

---

_Comment by @idan-rahamim-lendbuzz on 2024-11-20 10:12_

> @idan-rahamim-lendbuzz poetry does have third party plugins like https://github.com/cloud-custodian/poetry-plugin-freeze, but I'm not aware of a UV equivalent yet

I can't use such plugins due to security reasons.

---

_Comment by @sadaisystems on 2024-11-29 00:23_

I believe that this option would be interesting to ensure that the users get the same exact set of project dependencies that was used during the development process. Just to make sure that the experience is smooth. 

As of now, my team uses `setup.py` in our old workflow with `install_requires` a frozen requirements.txt. 

But the idea behind it is that we force our package to be installed with the set of dependencies that we now for sure are working.

Example of a bad scenario was illustrated by @rahuliyer95, and it is exactly what I'm talking about. Sometimes things just break with newer versions and it is hard to track those sometimes.

Though I believe that this not a best practice in general, but in corporate setting with some flows this is gold.

---

_Comment by @snizovtsev on 2024-12-20 15:32_

I found a way to work around this issue by using custom hatch plugin. The recipe has 3 ingredients:

* A requirement that refers to an environment variable that is expanded by Hatch during build:
```toml
[project]
dependencies = [
  "hello-world == {env:UV_HELLO_WORLD_VERSION}",
]
```

* A build hook that adds the `uv.lock' file to the source distribution for use in the wheel builder:

 ```py
@cache
def find_workspace_root(cwd: Path | str) -> Path | None:
    cwd = Path(cwd)
    if cwd == Path(cwd.root) or cwd == cwd.parent:
        return None
    elif (cwd / "uv.lock").exists():
        return cwd
    return find_workspace_root(cwd.parent)


class MyBuildHook(BuildHookInterface[BuilderConfig]):
    PLUGIN_NAME = "my-hook"

    @override
    def initialize(self, version: str, build_data: dict[str, Any]) -> None:
        if self.target_name == "sdist":
            workspace_root = find_workspace_root(self.root) or self.root
            uv_lock_path = os.path.join(workspace_root, "uv.lock")
            build_data["force_include"][uv_lock_path] = "uv.lock"
```

* And finally a metadata hook that would set environment variables from the contents of `uv.lock` before interpolation happens:
```python
def load_uv_lock(root: Path | str):
    uv_lock_path = Path(root) / "uv.lock"
    try:
        with uv_lock_path.open("rb") as fp:
            return tomllib.load(fp)
    except:
        log.warning("Cannot load uv.lock", exc_info=True)
        return {}

class MyMetadataHook(MetadataHookInterface):
    PLUGIN_NAME = "my-hook"

    def update(self, metadata: dict[str, Any]) -> None:
        uv_workspace_root = find_workspace_root(self.root) or self.root
        uv_lock = load_uv_lock(uv_workspace_root)
        uv_members = uv_lock.get("manifest", {}).get("members", [])
        uv_packages = uv_lock.get("package", [])
        uv_dist_packages = dict({
            p["name"]: p for p in uv_packages if p["name"] in uv_members
        })

        # Export environment variables with local package versions
        for name, package in uv_dist_packages.items():
            norm_name = re.sub(r"[-_.]+", "_", name).upper()
            version = package["version"]
            var = f"UV_{norm_name}_VERSION"
            print(f"{var}={version}")
            os.environ[var] = version
```
* To force the metadata hook to run, you may also need to add an additional dynamic field:
```toml
[project]
dynamic = ["version", "x-run-hook"]
```

---

_Comment by @edgarrmondragon on 2025-01-18 01:43_

Perhaps something like this Poetry PoC PR could work:

- https://github.com/python-poetry/poetry/pull/9428

---

_Comment by @kwaegel on 2025-02-12 19:10_

@edgarrmondragon I think I figured out a bash equivalent for that Poetry PoC, which adds a `pinned` dependency group, builds the package, then reverts the change. There's probably some way to write this as a `hatchling` plugin.

```
#!/bin/bash
set -euo pipefail

# For safety, only run this with a clean working directory.
uv lock --check
if [ -n "$(git status --porcelain)" ]; then
    echo "Working directory not clean."
    exit 1
fi

cp pyproject.toml pyproject.toml.bak
cp uv.lock uv.lock.bak

# Export all non-development dependency versions from uv.lock.
uv export --locked --no-dev --no-extra pinned --no-hashes --no-emit-workspace \
    --output-file pinned_requirements.txt > /dev/null

# Add an optional 'pinned' group that contains the pinned dependency list.
uv add --optional pinned -r pinned_requirements.txt

# Build the package.
uv build

# Restore the original project state.
mv pyproject.toml.bak pyproject.toml
mv uv.lock.bak uv.lock
rm -f pinned_requirements.txt
```

---

_Comment by @edgarrmondragon on 2025-02-12 20:03_

> There's probably some way to write this as a `hatchling` plugin.

@kwaegel let me know what you think of https://github.com/edgarrmondragon/hatch-pinned-extra 🙂 


---

_Comment by @kwaegel on 2025-02-13 00:21_

Cool! That mostly seems to work, but it's giving me very slightly different results from the script-based version. I'll try to file a ticket once I figure out what's going on, but in general it seems like a better solution.

---

_Comment by @timvink on 2025-02-15 13:04_

+1 for this.

My use case for something like `uv build --wheel --pinned` is databricks asset bundles, where you can include a wheel to go along with a job definition. I need exact, reproducible environments to train ML models. Of course there are workarounds, but this feature would be cleanest.

For reference, I would use it like this:

```yaml
# databricks.yml
artifacts:
  python_package:
    type: wheel
    build: uv build --wheel --pinned
    path: .
```

---

_Comment by @EternityForest on 2025-02-27 02:06_

@timvink I think I'd prefer it be a setting in pyproject.toml, I can't imagine any time I would ever want to build a non-frozen wheel for a completed application, and by adding it to the metadata, unmodified build and publish commands could do the right thing.

---

_Comment by @konstin on 2025-02-27 09:27_

Since it hasn't been mentioned before, I want to share this workaround: You can `uv export` the lockfile and use it as constraint on installation of the wheel, e.g.

```
uv export --no-emit-workspace --no-hashes -o requirements-frozen.txt
uv build
pip install -c requirements-frozen.txt dist/foo-0.1.0-py3-none-any.whl 
```

---

_Comment by @Dobatymo on 2025-04-09 03:45_

I would also like to replace a `poetry-plugin-freeze` based workflow with `uv`. I would like to export a wheel file which uses exactly the same versions for the dependencies as the tests.

---

_Comment by @gsemet on 2025-07-02 10:45_

So for my little story today.

At 11am30 this morning, `uv tool install mysoftware` worked great

At 11am31 it did not anymore, because someone has published on pypi a new version of its software that lifted a restriction on a another dependency, leading to an incompatibility. semver not carefully followed by anyone.

What i want is to be able to create 2 wheels.

```hash
$ uv build mysoftware --pinned
this is my main CLI, mainly usable by `uv tool install`

$ uv build mysoftware[api]
the API version of the cli, without any frozen dependencies, used with `uv add`
```

Basically the reverse of hatch-pinned-extra

---
