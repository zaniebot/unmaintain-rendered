```yaml
number: 796
title: "`ty` not detecting `pixi` environments if installed by `pixi global`"
type: issue
state: closed
author: JaRoSchm
labels:
  - imports
assignees: []
created_at: 2025-07-10T13:28:48Z
updated_at: 2025-10-07T10:29:35Z
url: https://github.com/astral-sh/ty/issues/796
synced_at: 2026-01-12T15:54:24Z
```

# `ty` not detecting `pixi` environments if installed by `pixi global`

---

_@JaRoSchm_

### Summary

Hi, although https://github.com/astral-sh/ruff/pull/18267 has been merged a while back, there is still a problem if `ty` is installed by `pixi global install -c conda-forge/label/ty_alpha -c conda-forge ty` (the following does not occur if `ty` is installed by `uv tool install ty`). I have the most recent versions (`ty 0.0.1-alpha.14` and `pixi 0.49.0`) of both tools installed.

First, I create a pixi project resulting in this `pixi.toml`:
```toml
[workspace]
authors = ["..."]
channels = ["conda-forge"]
name = "test_pixi_ty"
platforms = ["linux-64"]
version = "0.1.0"

[tasks]

[dependencies]
python = ">=3.13.5,<3.14"
numpy = ">=2.3.1,<3"
```

Then I run the command `pixi shell` which correctly sets `$CONDA_PREFIX` to `/home/username/Devel/test_pixi_ty/.pixi/envs/default`. The variable `$VIRTUAL_ENV` is not set. If I create a file which just imports numpy, this runs without problem. Running `ty check` on the file gives
```sh
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-import]: Cannot resolve imported module `numpy`
 --> test.py:1:8
  |
1 | import numpy as np
  |        ^^^^^^^^^^^
  |
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
```

Although I don't know anything about the internals of `pixi`, I suspect that the reason for this is, that `pixi` installs its global tools into their own "conda" environment. So even if the correct `$CONDA_PREFIX` is set in my shell, `ty` somehow seems to pick up the wrong one from its own environment. An indication for this is that if I add numpy to the global environment of `ty` (`pixi global install -c conda-forge/label/ty_alpha -c conda-forge ty --with numpy`), everything works as expected.

As this architecture is fundamental to `pixi` and probably will not be changed, is there something `ty` can do to pick up the right `$CONDA_PREFIX` variable?

Thank your for your great work!

### Version

ty 0.0.1-alpha.14

---

_Comment by @zanieb on 2025-07-10 13:34_

cc @wolfv maybe you have some insights here.

Can you share verbose output for the ty invocation? Is Pixi setting `CONDA_PREFIX` to the tool environment on invocation?

---

_Comment by @JaRoSchm on 2025-07-10 14:17_

Here is the verbose output:

<details>

````sh
2025-07-10 16:11:18.927157761 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
2025-07-10 16:11:18.927991267 DEBUG Version: 0.0.1-alpha.14
2025-07-10 16:11:18.928013409 DEBUG Architecture: x86_64, OS: linux, case-sensitive: case-sensitive
2025-07-10 16:11:18.928116703 DEBUG Searching for a project in '/home/jschmidt/Devel/test_pixi_ty'
2025-07-10 16:11:18.928143584 DEBUG The ancestor directories contain no `pyproject.toml`. Falling back to a virtual project.
2025-07-10 16:11:18.928162048 DEBUG Searching for a user-level configuration at `/home/jschmidt/.config/ty/ty.toml`
2025-07-10 16:11:18.953596231 INFO Defaulting to python-platform `linux`
2025-07-10 16:11:18.953641907 DEBUG Resolving `CONDA_PREFIX` environment variable: /home/jschmidt/.pixi/envs/ty
2025-07-10 16:11:18.953679929 DEBUG Attempting to parse virtual environment metadata at '/home/jschmidt/.pixi/envs/ty/pyvenv.cfg'
2025-07-10 16:11:18.953690679 DEBUG Searching for site-packages directory in sys.prefix /home/jschmidt/.pixi/envs/ty
2025-07-10 16:11:18.953882369 DEBUG Resolved site-packages directories for this environment are: SitePackagesPaths({"/home/jschmidt/.pixi/envs/ty/lib/python3.13/site-packages"})
2025-07-10 16:11:18.953911073 DEBUG Including `.` in `environment.root`
2025-07-10 16:11:18.953936301 DEBUG Adding first-party search path '/home/jschmidt/Devel/test_pixi_ty'
2025-07-10 16:11:18.953952261 DEBUG Using vendored stdlib
2025-07-10 16:11:18.954306306 DEBUG Adding site-packages search path '/home/jschmidt/.pixi/envs/ty/lib/python3.13/site-packages'
2025-07-10 16:11:18.954332485 INFO Python version: Python 3.13, platform: linux
2025-07-10 16:11:18.954770248 DEBUG Setting included paths: 1
2025-07-10 16:11:18.954787901 DEBUG Reloading files for project `test_pixi_ty`
2025-07-10 16:11:18.954918797 DEBUG Starting main loop
2025-07-10 16:11:18.954933545 DEBUG Waiting for next main loop message.
2025-07-10 16:11:18.954993467 DEBUG Checking project 'test_pixi_ty'
2025-07-10 16:11:18.957628961 INFO Indexed 1 file(s) in 0.003s
Checking ------------------------------------------------------------ 0/1 files                                                                                                                         2025-07-10 16:11:18.957833716 DEBUG Checking file '/home/jschmidt/Devel/test_pixi_ty/test.py'
2025-07-10 16:11:18.958153667 DEBUG Resolving dynamic module resolution paths
2025-07-10 16:11:18.958198692 DEBUG Skipping editable installation: /home/jschmidt/.pixi/envs/ty/lib/python3.1/site-packages does not point to a directory
2025-07-10 16:11:18.958216285 DEBUG Skipping editable installation: /home/jschmidt/.pixi/envs/ty/lib/python/site-packages does not point to a directory
2025-07-10 16:11:18.958251491 DEBUG Module `numpy` not found in search paths
Checking ------------------------------------------------------------ 1/1 files                                                                                                                         2025-07-10 16:11:18.958356769 DEBUG Checking all files took 0.001s
error[unresolved-import]: Cannot resolve imported module `numpy`
 --> test.py:1:8
  |
1 | import numpy as np
  |        ^^^^^^^^^^^
  |
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
2025-07-10 16:11:18.958483457 DEBUG Exiting main loop
(test_pixi_ty) (CPG_stable) jschmidt-laptop:~/Devel/test_pixi_ty % ty check -vvv test.py
  2025-07-10T14:11:28.500682Z  WARN ty: ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
    at crates/ty/src/lib.rs:64 on ThreadId(1)

  2025-07-10T14:11:28.500703Z DEBUG ty: Version: 0.0.1-alpha.14
    at crates/ty/src/lib.rs:69 on ThreadId(1)

  2025-07-10T14:11:28.500726Z DEBUG ruff_db::system::os: Architecture: x86_64, OS: linux, case-sensitive: case-sensitive
    at crates/ruff_db/src/system/os.rs:45 on ThreadId(1)

  2025-07-10T14:11:28.500823Z DEBUG ty_project::metadata: Searching for a project in '/home/jschmidt/Devel/test_pixi_ty'
    at crates/ty_project/src/metadata.rs:134 on ThreadId(1)

  2025-07-10T14:11:28.500852Z DEBUG ty_project::metadata: The ancestor directories contain no `pyproject.toml`. Falling back to a virtual project.
    at crates/ty_project/src/metadata.rs:240 on ThreadId(1)

  2025-07-10T14:11:28.500939Z DEBUG ty_project::metadata::configuration_file: Searching for a user-level configuration at `/home/jschmidt/.config/ty/ty.toml`
    at crates/ty_project/src/metadata/configuration_file.rs:47 on ThreadId(1)

  2025-07-10T14:11:28.513582Z  INFO ty_project::metadata::options: Defaulting to python-platform `linux`
    at crates/ty_project/src/metadata/options.rs:133 on ThreadId(1)

  2025-07-10T14:11:28.513620Z DEBUG ty_python_semantic::site_packages: Resolving `CONDA_PREFIX` environment variable: /home/jschmidt/.pixi/envs/ty
    at crates/ty_python_semantic/src/site_packages.rs:148 on ThreadId(1)

  2025-07-10T14:11:28.513646Z DEBUG ty_python_semantic::site_packages: Attempting to parse virtual environment metadata at '/home/jschmidt/.pixi/envs/ty/pyvenv.cfg'
    at crates/ty_python_semantic/src/site_packages.rs:300 on ThreadId(1)

  2025-07-10T14:11:28.513655Z DEBUG ty_python_semantic::site_packages: Searching for site-packages directory in sys.prefix /home/jschmidt/.pixi/envs/ty
    at crates/ty_python_semantic/src/site_packages.rs:832 on ThreadId(1)

  2025-07-10T14:11:28.513792Z DEBUG ty_python_semantic::site_packages: Resolved site-packages directories for this environment are: SitePackagesPaths({"/home/jschmidt/.pixi/envs/ty/lib/python3.13/site-packages"})
    at crates/ty_python_semantic/src/site_packages.rs:619 on ThreadId(1)

  2025-07-10T14:11:28.513827Z DEBUG ty_project::metadata::options: Including `.` in `environment.root`
    at crates/ty_project/src/metadata/options.rs:236 on ThreadId(1)

  2025-07-10T14:11:28.513853Z DEBUG ty_python_semantic::module_resolver::resolver: Adding first-party search path '/home/jschmidt/Devel/test_pixi_ty'
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:193 on ThreadId(1)

  2025-07-10T14:11:28.513877Z DEBUG ty_python_semantic::module_resolver::resolver: Using vendored stdlib
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:216 on ThreadId(1)

  2025-07-10T14:11:28.514166Z DEBUG ty_python_semantic::module_resolver::resolver: Adding site-packages search path '/home/jschmidt/.pixi/envs/ty/lib/python3.13/site-packages'
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:228 on ThreadId(1)

  2025-07-10T14:11:28.514183Z  INFO ty_project::metadata::options: Python version: Python 3.13, platform: linux
    at crates/ty_project/src/metadata/options.rs:183 on ThreadId(1)

  2025-07-10T14:11:28.514557Z DEBUG ty_project: Setting included paths: 1
    at crates/ty_project/src/lib.rs:326 on ThreadId(1)

  2025-07-10T14:11:28.514568Z DEBUG ty_project: Reloading files for project `test_pixi_ty`
    at crates/ty_project/src/lib.rs:471 on ThreadId(1)

  2025-07-10T14:11:28.514685Z DEBUG ty: Starting main loop
    at crates/ty/src/lib.rs:255 on ThreadId(1)

  2025-07-10T14:11:28.514698Z DEBUG ty: Waiting for next main loop message.
    at crates/ty/src/lib.rs:377 on ThreadId(1)

  2025-07-10T14:11:28.514762Z DEBUG ty_project: Checking project 'test_pixi_ty'
    at crates/ty_project/src/lib.rs:220 on ThreadId(2)
    in ty_project::Project::check

  2025-07-10T14:11:28.517341Z  INFO ty_project: Indexed 1 file(s) in 0.003s
    at crates/ty_project/src/lib.rs:459 on ThreadId(2)
    in ty_project::Project::index_files with project=test_pixi_ty
    in ty_project::Project::check

Checking ------------------------------------------------------------ 0/1 files                                                                                                                           2025-07-10T14:11:28.517546Z DEBUG ty_python_semantic::types: Checking file '/home/jschmidt/Devel/test_pixi_ty/test.py'
    at crates/ty_python_semantic/src/types.rs:95 on ThreadId(2)
    in ty_project::check_file with file=file(Id(c00))
    in ty_project::Project::check

  2025-07-10T14:11:28.517888Z DEBUG ty_python_semantic::module_resolver::resolver: Resolving dynamic module resolution paths
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:302 on ThreadId(2)
    in ty_project::check_file with file=file(Id(c00))
    in ty_project::Project::check

  2025-07-10T14:11:28.517963Z DEBUG ty_python_semantic::module_resolver::resolver: Skipping editable installation: /home/jschmidt/.pixi/envs/ty/lib/python3.1/site-packages does not point to a directory
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:387 on ThreadId(2)
    in ty_project::check_file with file=file(Id(c00))
    in ty_project::Project::check

  2025-07-10T14:11:28.517982Z DEBUG ty_python_semantic::module_resolver::resolver: Skipping editable installation: /home/jschmidt/.pixi/envs/ty/lib/python/site-packages does not point to a directory
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:387 on ThreadId(2)
    in ty_project::check_file with file=file(Id(c00))
    in ty_project::Project::check

  2025-07-10T14:11:28.518026Z DEBUG ty_python_semantic::module_resolver::resolver: Module `numpy` not found in search paths
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:42 on ThreadId(2)
    in ty_project::check_file with file=file(Id(c00))
    in ty_project::Project::check

Checking ------------------------------------------------------------ 1/1 files                                                                                                                           2025-07-10T14:11:28.518133Z DEBUG ty_project: Checking all files took 0.001s
    at crates/ty_project/src/lib.rs:269 on ThreadId(2)
    in ty_project::Project::check

error[unresolved-import]: Cannot resolve imported module `numpy`
 --> test.py:1:8
  |
1 | import numpy as np
  |        ^^^^^^^^^^^
  |
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
  2025-07-10T14:11:28.518253Z DEBUG ty: Exiting main loop
    at crates/ty/src/lib.rs:245 on ThreadId(1)
````
</details>

Indeed, `pixi` sets `$CONDA_ENV` to the tool environment. So probably this is a question to the `pixi` team, how `ty` could get the local `$CONDA_ENV`.

---

_Comment by @MichaReiser on 2025-07-10 14:20_

Maybe related to https://github.com/astral-sh/ty/issues/611

---

_Comment by @JaRoSchm on 2025-07-10 14:29_

> Maybe related to [#611](https://github.com/astral-sh/ty/issues/611)

I found this issue too, but I don't think it is related because pixi doesn't have a base environment anymore. If we translate the problem here to the old conda workflow, `ty` is just in a different conda environment compared to the dependencies of the project.

---

_Label `imports` added by @MichaReiser on 2025-07-10 14:35_

---

_Comment by @zanieb on 2025-07-10 14:37_

> Indeed, pixi sets $CONDA_ENV to the tool environment. 

I don't know if it should do that, e.g., `uv tool install ty` just installs a `ty` binary — there's no `VIRTUAL_ENV` injection.

---

_Comment by @Hofer-Julian on 2025-07-15 12:52_

Yeah, that's a bit annoying. We discussed it within the team, and we can't just stop setting `CONDA_PREFIX` since packages might rely on it.

For now, the best way is to add `ty` to your Pixi environment instead of installing it globally.

We'd be open to add a way for packages to communicate to `pixi global` that they don't want `CONDA_PREFIX` to be set

---

_Comment by @Hofer-Julian on 2025-09-23 08:39_

I've added a feature to Pixi which allows packages to opt-out of activating `CONDA_PREFIX` when installed via `pixi global`: https://github.com/prefix-dev/pixi/pull/4619. I also opened a PR to the [ty-feedstock](https://github.com/conda-forge/ty-feedstock/pull/36).

As soon as there's another Pixi release and the feedstock PR has been merged, this issue can be closed in my opinion

---

_Comment by @JaRoSchm on 2025-10-07 10:29_

Fixed in pixi 0.56.0. Thank you for working on this!

---

_Closed by @JaRoSchm on 2025-10-07 10:29_

---
