```yaml
number: 1957
title: "uv does not correctly sync `project.scripts` when changed"
type: issue
state: closed
author: dsp
labels:
  - bug
assignees: []
created_at: 2024-02-24T22:37:48Z
updated_at: 2024-02-25T01:31:48Z
url: https://github.com/astral-sh/uv/issues/1957
synced_at: 2026-01-12T15:58:33Z
```

# uv does not correctly sync `project.scripts` when changed

---

_@dsp_

`uv` does not correctly pick up new scripts in `project.scripts` when they changed. `project.scripts` are correctly synced when the venv is initially created and the initial sync is done. A change to `pyproject.toml` afterwards followed by a `uv pip sync` does not correctly pick up new scripts. This currently affects `rye` with uv enabled (changing project.scripts, running `rye sync` and then `rye run` will not show the new script)

Steps to reproduce:
```sh
$ mkdir uv-test
$ cd uv-test
$ uv venv --verbose
 uv_interpreter::python_query::find_python selector=Default
      0.000850s   0ms DEBUG uv_interpreter::interpreter Detecting markers for: /Users/dsp/.rye/shims/python3
Using Python 3.12.2 interpreter at /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
$ #... create pyproject.toml
$ cat pyproject.toml
[project]
name = "uv-test"
version = "0.1.0"
description = "Add your description here"
authors = [
]
dependencies = []
requires-python = ">= 3.8"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.metadata]
allow-direct-references = true

[tool.hatch.build.targets.wheel]
packages = ["src/uv-test"]
$ cat requirements.txt
-e file:.

$ uv pip sync pyproject.toml requirements.txt
 uv::requirements::from_source source=pyproject.toml
 uv::requirements::from_source source=requirements.txt
    0.000405s DEBUG uv_interpreter::virtual_env Found a virtualenv named .venv at: /Users/dsp/src/uv-test/.venv
    0.000503s DEBUG uv_interpreter::interpreter Using cached markers for: /Users/dsp/src/uv-test/.venv/bin/python
    0.000508s DEBUG uv::commands::pip_sync Using Python 3.12.2 environment at /Users/dsp/src/uv-test/.venv/bin/python
    0.000733s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_installer::downloader::build_editables
      0.127057s   0ms DEBUG uv_distribution::source Building (editable) file:///Users/dsp/src/uv-test
   uv_dispatch::setup_build package_id="file:///Users/dsp/src/uv-test", subdirectory=None
     uv_resolver::resolver::solve
          0.128589s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.2
       uv_resolver::resolver::choose_version package=root
       uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
            0.128636s   0ms DEBUG uv_resolver::resolver Adding direct dependency: hatchling*
       uv_resolver::resolver::choose_version package=hatchling
         uv_resolver::resolver::package_wait package_name=hatchling
     uv_resolver::resolver::process_request request=Versions hatchling
       uv_client::registry_client::simple_api package=hatchling
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/dsp/Library/Caches/uv/simple-v2/pypi/hatchling.rkyv
     uv_resolver::resolver::process_request request=Prefetch hatchling *
              0.129081s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/hatchling/
 uv_resolver::version_map::from_metadata
       uv_distribution::distribution_database::get_or_build_wheel_metadata dist=hatchling==1.21.1
         uv_client::registry_client::wheel_metadata built_dist=hatchling==1.21.1
           uv_client::cached_client::get_serde
             uv_client::cached_client::get_cacheable
               uv_client::cached_client::read_and_parse_cache file=/Users/dsp/Library/Caches/uv/wheels-v0/pypi/hatchling/hatchling-1.21.1-py3-none-any.msgpack
            0.129462s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of hatchling (*)
            0.129472s   0ms DEBUG uv_resolver::resolver Selecting: hatchling==1.21.1 (hatchling-1.21.1-py3-none-any.whl)
       uv_resolver::resolver::get_dependencies package=hatchling, version=1.21.1
         uv_resolver::resolver::distributions_wait package_id=hatchling-1.21.1
                  0.129503s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/3a/bb/40528a09a33845bd7fd75c33b3be7faec3b5c8f15f68a58931da67420fb9/hatchling-1.21.1-py3-none-any.whl
            0.129655s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: editables>=0.3
            0.129659s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: packaging>=21.3
            0.129663s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: pathspec>=0.10.1
            0.129666s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: pluggy>=1.0.0
            0.129670s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: trove-classifiers*
       uv_resolver::resolver::choose_version package=editables
         uv_resolver::resolver::package_wait package_name=editables
     uv_resolver::resolver::process_request request=Versions editables
       uv_client::registry_client::simple_api package=editables
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/dsp/Library/Caches/uv/simple-v2/pypi/editables.rkyv
     uv_resolver::resolver::process_request request=Versions packaging
       uv_client::registry_client::simple_api package=packaging
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/dsp/Library/Caches/uv/simple-v2/pypi/packaging.rkyv
     uv_resolver::resolver::process_request request=Versions pathspec
       uv_client::registry_client::simple_api package=pathspec
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/dsp/Library/Caches/uv/simple-v2/pypi/pathspec.rkyv
     uv_resolver::resolver::process_request request=Versions pluggy
       uv_client::registry_client::simple_api package=pluggy
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/dsp/Library/Caches/uv/simple-v2/pypi/pluggy.rkyv
     uv_resolver::resolver::process_request request=Versions trove-classifiers
       uv_client::registry_client::simple_api package=trove-classifiers
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/dsp/Library/Caches/uv/simple-v2/pypi/trove-classifiers.rkyv
     uv_resolver::resolver::process_request request=Prefetch trove-classifiers *
     uv_resolver::resolver::process_request request=Prefetch pluggy >=1.0.0
     uv_resolver::resolver::process_request request=Prefetch pathspec >=0.10.1
     uv_resolver::resolver::process_request request=Prefetch packaging >=21.3
     uv_resolver::resolver::process_request request=Prefetch editables >=0.3
              0.129843s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/editables/
              0.129873s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/packaging/
 uv_resolver::version_map::from_metadata
              0.129904s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/pathspec/
 uv_resolver::version_map::from_metadata
 uv_resolver::version_map::from_metadata
              0.130023s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/pluggy/
              0.130064s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/trove-classifiers/
 uv_resolver::version_map::from_metadata
 uv_resolver::version_map::from_metadata
       uv_distribution::distribution_database::get_or_build_wheel_metadata dist=editables==0.5
         uv_client::registry_client::wheel_metadata built_dist=editables==0.5
           uv_client::cached_client::get_serde
             uv_client::cached_client::get_cacheable
               uv_client::cached_client::read_and_parse_cache file=/Users/dsp/Library/Caches/uv/wheels-v0/pypi/editables/editables-0.5-py3-none-any.msgpack
       uv_distribution::distribution_database::get_or_build_wheel_metadata dist=packaging==23.2
         uv_client::registry_client::wheel_metadata built_dist=packaging==23.2
           uv_client::cached_client::get_serde
             uv_client::cached_client::get_cacheable
               uv_client::cached_client::read_and_parse_cache file=/Users/dsp/Library/Caches/uv/wheels-v0/pypi/packaging/packaging-23.2-py3-none-any.msgpack
       uv_distribution::distribution_database::get_or_build_wheel_metadata dist=pathspec==0.12.1
         uv_client::registry_client::wheel_metadata built_dist=pathspec==0.12.1
           uv_client::cached_client::get_serde
             uv_client::cached_client::get_cacheable
               uv_client::cached_client::read_and_parse_cache file=/Users/dsp/Library/Caches/uv/wheels-v0/pypi/pathspec/pathspec-0.12.1-py3-none-any.msgpack
                  0.130198s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/6b/be/0f2f4a5e8adc114a02b63d92bf8edbfa24db6fc602fca83c885af2479e0e/editables-0.5-py3-none-any.whl
                  0.130211s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/ec/1a/610693ac4ee14fcdf2d9bf3c493370e4f2ef7ae2e19217d7a237ff42367d/packaging-23.2-py3-none-any.whl
       uv_distribution::distribution_database::get_or_build_wheel_metadata dist=pluggy==1.4.0
         uv_client::registry_client::wheel_metadata built_dist=pluggy==1.4.0
           uv_client::cached_client::get_serde
             uv_client::cached_client::get_cacheable
               uv_client::cached_client::read_and_parse_cache file=/Users/dsp/Library/Caches/uv/wheels-v0/pypi/pluggy/pluggy-1.4.0-py3-none-any.msgpack
       uv_distribution::distribution_database::get_or_build_wheel_metadata dist=trove-classifiers==2024.2.23
         uv_client::registry_client::wheel_metadata built_dist=trove-classifiers==2024.2.23
           uv_client::cached_client::get_serde
             uv_client::cached_client::get_cacheable
               uv_client::cached_client::read_and_parse_cache file=/Users/dsp/Library/Caches/uv/wheels-v0/pypi/trove-classifiers/trove_classifiers-2024.2.23-py3-none-any.msgpack
                  0.130265s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl
                  0.130273s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/a5/5b/0cc789b59e8cc1bf288b38111d002d8c5917123194d45b29dcdac64723cc/pluggy-1.4.0-py3-none-any.whl
            0.130283s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of editables (>=0.3)
            0.130287s   0ms DEBUG uv_resolver::resolver Selecting: editables==0.5 (editables-0.5-py3-none-any.whl)
       uv_resolver::resolver::get_dependencies package=editables, version=0.5
         uv_resolver::resolver::distributions_wait package_id=editables-0.5
       uv_resolver::resolver::choose_version package=packaging
         uv_resolver::resolver::package_wait package_name=packaging
            0.130306s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of packaging (>=21.3)
            0.130310s   0ms DEBUG uv_resolver::resolver Selecting: packaging==23.2 (packaging-23.2-py3-none-any.whl)
       uv_resolver::resolver::get_dependencies package=packaging, version=23.2
         uv_resolver::resolver::distributions_wait package_id=packaging-23.2
       uv_resolver::resolver::choose_version package=pathspec
         uv_resolver::resolver::package_wait package_name=pathspec
            0.130322s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of pathspec (>=0.10.1)
            0.130324s   0ms DEBUG uv_resolver::resolver Selecting: pathspec==0.12.1 (pathspec-0.12.1-py3-none-any.whl)
       uv_resolver::resolver::get_dependencies package=pathspec, version=0.12.1
         uv_resolver::resolver::distributions_wait package_id=pathspec-0.12.1
       uv_resolver::resolver::choose_version package=pluggy
         uv_resolver::resolver::package_wait package_name=pluggy
            0.130338s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of pluggy (>=1.0.0)
            0.130341s   0ms DEBUG uv_resolver::resolver Selecting: pluggy==1.4.0 (pluggy-1.4.0-py3-none-any.whl)
       uv_resolver::resolver::get_dependencies package=pluggy, version=1.4.0
         uv_resolver::resolver::distributions_wait package_id=pluggy-1.4.0
       uv_resolver::resolver::choose_version package=trove-classifiers
         uv_resolver::resolver::package_wait package_name=trove-classifiers
            0.130354s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of trove-classifiers (*)
            0.130356s   0ms DEBUG uv_resolver::resolver Selecting: trove-classifiers==2024.2.23 (trove_classifiers-2024.2.23-py3-none-any.whl)
       uv_resolver::resolver::get_dependencies package=trove-classifiers, version=2024.2.23
         uv_resolver::resolver::distributions_wait package_id=trove-classifiers-2024.2.23
                  0.130367s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/56/45/6887993f3d1fd941fe129c8dc3aa648d5bd20e0e02c08f99df961fd1e908/trove_classifiers-2024.2.23-py3-none-any.whl
     uv_dispatch::install resolution="editables==0.5, trove-classifiers==2024.2.23, pathspec==0.12.1, pluggy==1.4.0, packaging==23.2, hatchling==1.21.1", venv="/Users/dsp/Library/Caches/uv/.tmpbcvP0e/.venv"
          0.130415s   0ms DEBUG uv_dispatch Installing in editables==0.5, trove-classifiers==2024.2.23, pathspec==0.12.1, pluggy==1.4.0, packaging==23.2, hatchling==1.21.1 in /Users/dsp/Library/Caches/uv/.tmpbcvP0e/.venv
          0.130525s   0ms DEBUG uv_installer::plan Requirement already cached: editables==0.5
          0.130583s   0ms DEBUG uv_installer::plan Requirement already cached: hatchling==1.21.1
          0.130663s   0ms DEBUG uv_installer::plan Requirement already cached: packaging==23.2
          0.130714s   0ms DEBUG uv_installer::plan Requirement already cached: pathspec==0.12.1
          0.130766s   0ms DEBUG uv_installer::plan Requirement already cached: pluggy==1.4.0
          0.130820s   0ms DEBUG uv_installer::plan Requirement already cached: trove-classifiers==2024.2.23
          0.130830s   0ms DEBUG uv_dispatch Installing build requirements: editables==0.5, hatchling==1.21.1, packaging==23.2, pathspec==0.12.1, pluggy==1.4.0, trove-classifiers==2024.2.23
       uv_installer::installer::install num_wheels=6
        0.134233s   7ms DEBUG uv_build Calling `hatchling.build.get_requires_for_build_editable()`
     uv_build::run_python_script script="get_requires_for_build_editable", python_version=3.12.2
   uv_build::build package_id="file:///Users/dsp/src/uv-test"
        0.208199s   0ms DEBUG uv_build Calling `hatchling.build.build_editable(metadata_directory=None)`
     uv_build::run_python_script script="build_editable", python_version=3.12.2
      0.295955s 169ms DEBUG uv_distribution::source Finished building (editable): uv-test @ file:///Users/dsp/src/uv-test
Built 1 editable in 170ms
    0.296847s DEBUG uv_installer::plan Treating editable requirement as mutable: uv-test==0.1.0 (from file:///Users/dsp/src/uv-test)
 uv_installer::installer::install num_wheels=1
Installed 1 package in 0ms
 + uv-test==0.1.0 (from file:///Users/dsp/src/uv-test)

$ .... change pyproject.toml add project.scripts entry
$ cat pyproject.toml
[project]
name = "uv-test"
version = "0.1.0"
description = "Add your description here"
authors = [
]
dependencies = []
requires-python = ">= 3.8"

[project.scripts]
hello = "uv_test:hello"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.metadata]
allow-direct-references = true

[tool.hatch.build.targets.wheel]
packages = ["src/uv-test"]
$ uv pip sync --verbose pyproject.toml requirements.txt                                                                                                                                                      
 uv::requirements::from_source source=pyproject.toml
 uv::requirements::from_source source=requirements.txt
    0.000427s DEBUG uv_interpreter::virtual_env Found a virtualenv named .venv at: /Users/dsp/src/uv-test/.venv
    0.000520s DEBUG uv_interpreter::interpreter Using cached markers for: /Users/dsp/src/uv-test/.venv/bin/python
    0.000526s DEBUG uv::commands::pip_sync Using Python 3.12.2 environment at /Users/dsp/src/uv-test/.venv/bin/python
    0.000739s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
    0.125662s DEBUG uv_installer::plan Treating editable requirement as immutable: uv-test==0.1.0 (from file:///Users/dsp/src/uv-test)
Audited 1 package in 125ms
$ ls -al .venv/bin/hello
ls: .venv/bin/hello: No such file or directory
```

Notable, if I create an empty .venv, have a pyproject.toml that already includes the script and then run `uv pip sync` the `.venv/bin/` entry is there. The problem just appears if the pyproject.toml changed after the initial change

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-24 22:40_

---

_Label `bug` added by @charliermarsh on 2024-02-24 22:40_

---

_Comment by @charliermarsh on 2024-02-24 22:40_

Thanks! I'll make this caching a little smarter.

---

_Closed by @charliermarsh on 2024-02-25 01:31_

---
