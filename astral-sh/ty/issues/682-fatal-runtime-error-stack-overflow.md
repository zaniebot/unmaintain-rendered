```yaml
number: 682
title: "fatal runtime error: stack overflow"
type: issue
state: closed
author: nzedler
labels:
  - needs-mre
  - fatal
assignees: []
created_at: 2025-06-18T18:46:52Z
updated_at: 2025-08-15T01:24:14Z
url: https://github.com/astral-sh/ty/issues/682
synced_at: 2026-01-10T02:06:24Z
```

# fatal runtime error: stack overflow

---

_Issue opened by @nzedler on 2025-06-18 18:46_

### Summary

When running `uvx ty check` inside my project I get the following error message:

```sh
uvx ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/22 files                                                                                                                                                                                                                                                                 
thread '<unknown>' has overflowed its stack
fatal runtime error: stack overflow
```

Here the output with `-vvv`:
```
uvx ty check -vvv
  2025-06-18T18:40:20.500602Z  WARN ty: ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
    at crates/ty/src/lib.rs:64 on ThreadId(1)

  2025-06-18T18:40:20.500822Z DEBUG ty: Version: 0.0.1-alpha.11 (1ae703836 2025-06-17)
    at crates/ty/src/lib.rs:69 on ThreadId(1)

  2025-06-18T18:40:20.500916Z DEBUG ruff_db::system::os: Architecture: aarch64, OS: macos, case-sensitive: case-insensitive
    at crates/ruff_db/src/system/os.rs:45 on ThreadId(1)

  2025-06-18T18:40:20.500931Z DEBUG ty_project::metadata: Searching for a project in '/Users/johndoe/Projects/XYZ/someproject'
    at crates/ty_project/src/metadata.rs:133 on ThreadId(1)

  2025-06-18T18:40:20.501050Z DEBUG ty_project::metadata::pyproject: Resolving requires-python constraint: `>=3.11`
    at crates/ty_project/src/metadata/pyproject.rs:67 on ThreadId(1)

  2025-06-18T18:40:20.501062Z DEBUG ty_project::metadata::pyproject: Resolved requires-python constraint to: 3.11
    at crates/ty_project/src/metadata/pyproject.rs:101 on ThreadId(1)

  2025-06-18T18:40:20.501084Z DEBUG ty_project::metadata: Project without `tool.ty` section: '/Users/johndoe/Projects/XYZ/someproject'
    at crates/ty_project/src/metadata.rs:232 on ThreadId(1)

  2025-06-18T18:40:20.501093Z DEBUG ty_project::metadata::configuration_file: Searching for a user-level configuration at `/Users/johndoe/.config/ty/ty.toml`
    at crates/ty_project/src/metadata/configuration_file.rs:47 on ThreadId(1)

  2025-06-18T18:40:20.501105Z  INFO ty_project::metadata::options: Defaulting to python-platform `darwin`
    at crates/ty_project/src/metadata/options.rs:128 on ThreadId(1)

  2025-06-18T18:40:20.501114Z DEBUG ty_project::metadata::options: Including `./src` in `src.root` because a `./src` directory exists
    at crates/ty_project/src/metadata/options.rs:153 on ThreadId(1)

  2025-06-18T18:40:20.501132Z DEBUG ty_python_semantic::module_resolver::resolver: Adding first-party search path '/Users/johndoe/Projects/XYZ/someproject'
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:207 on ThreadId(1)

  2025-06-18T18:40:20.501144Z DEBUG ty_python_semantic::module_resolver::resolver: Adding first-party search path '/Users/johndoe/Projects/XYZ/someproject/src'
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:207 on ThreadId(1)

  2025-06-18T18:40:20.501149Z DEBUG ty_python_semantic::module_resolver::resolver: Using vendored stdlib
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:232 on ThreadId(1)

  2025-06-18T18:40:20.501500Z DEBUG ty_python_semantic::module_resolver::resolver: Discovering virtual environment in `/Users/johndoe/Projects/XYZ/someproject`
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:244 on ThreadId(1)

  2025-06-18T18:40:20.501548Z DEBUG ty_python_semantic::site_packages: Attempting to parse virtual environment metadata at '/Users/johndoe/Projects/XYZ/someproject/.venv/pyvenv.cfg'
    at crates/ty_python_semantic/src/site_packages.rs:207 on ThreadId(1)

  2025-06-18T18:40:20.501607Z DEBUG ty_python_semantic::site_packages: Searching for site-packages directory in sys.prefix /Users/johndoe/Projects/XYZ/someproject/.venv
    at crates/ty_python_semantic/src/site_packages.rs:739 on ThreadId(1)

  2025-06-18T18:40:20.501624Z DEBUG ty_python_semantic::site_packages: Resolved site-packages directories for this virtual environment are: SitePackagesPaths({"/Users/johndoe/Projects/XYZ/someproject/.venv/lib/python3.11/site-packages"})
    at crates/ty_python_semantic/src/site_packages.rs:370 on ThreadId(1)

  2025-06-18T18:40:20.501629Z DEBUG ty_python_semantic::module_resolver::resolver: Adding site-packages search path '/Users/johndoe/Projects/XYZ/someproject/.venv/lib/python3.11/site-packages'
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:284 on ThreadId(1)

  2025-06-18T18:40:20.501657Z  INFO ty_python_semantic::program: Python version: Python 3.11, platform: darwin
    at crates/ty_python_semantic/src/program.rs:44 on ThreadId(1)

  2025-06-18T18:40:20.501928Z DEBUG ty: Starting main loop
    at crates/ty/src/lib.rs:247 on ThreadId(1)

  2025-06-18T18:40:20.501938Z DEBUG ty: Waiting for next main loop message.
    at crates/ty/src/lib.rs:368 on ThreadId(1)

  2025-06-18T18:40:20.502016Z DEBUG ty_project: Checking project 'graylog2thehive'
    at crates/ty_project/src/lib.rs:221 on ThreadId(2)
    in ty_project::Project::check

  2025-06-18T18:40:20.502935Z DEBUG ty_project::walk: Skipping directory '/Users/johndoe/Projects/XYZ/someproject/.git' because it is excluded by a default or `src.exclude` pattern
    at crates/ty_project/src/walk.rs:180 on ThreadId(13)

  2025-06-18T18:40:20.506607Z  INFO ty_project: Indexed 22 file(s)
    at crates/ty_project/src/lib.rs:449 on ThreadId(2)
    in ty_project::Project::index_files with project=graylog2thehive
    in ty_project::Project::check

Checking ------------------------------------------------------------ 0/22 files                                                                                                                                                                                                                                                                       2025-06-18T18:40:20.507047Z DEBUG ty_python_semantic::types: Checking file '/Users/johndoe/Projects/XYZ/someproject/src/types.py'
    at crates/ty_python_semantic/src/types.rs:92 on ThreadId(5)
    in ty_project::check_file with file=file(Id(c0f))
    in ty_project::Project::check

  2025-06-18T18:40:20.507060Z DEBUG ty_python_semantic::types: Checking file '/Users/johndoe/Projects/XYZ/someproject/main.py'
    at crates/ty_python_semantic/src/types.rs:92 on ThreadId(6)
    in ty_project::check_file with file=file(Id(c11))
    in ty_project::Project::check

  2025-06-18T18:40:20.507123Z DEBUG ty_python_semantic::types: Checking file '/Users/johndoe/Projects/XYZ/someproject/src/extractors/artifacts_hostname.py'
    at crates/ty_python_semantic/src/types.rs:92 on ThreadId(4)
    in ty_project::check_file with file=file(Id(c07))
    in ty_project::Project::check

  2025-06-18T18:40:20.507136Z DEBUG ty_python_semantic::types: Checking file '/Users/johndoe/Projects/XYZ/someproject/src/extractors/artifacts_ip.py'
    at crates/ty_python_semantic/src/types.rs:92 on ThreadId(7)
    in ty_project::check_file with file=file(Id(c05))
    in ty_project::Project::check

  2025-06-18T18:40:20.507141Z DEBUG ty_python_semantic::types: Checking file '/Users/johndoe/Projects/XYZ/someproject/src/extractors/logsources.py'
    at crates/ty_python_semantic/src/types.rs:92 on ThreadId(9)
    in ty_project::check_file with file=file(Id(c0a))
    in ty_project::Project::check

  2025-06-18T18:40:20.507167Z DEBUG ty_python_semantic::types: Checking file '/Users/johndoe/Projects/XYZ/someproject/src/settings.py'
    at crates/ty_python_semantic/src/types.rs:92 on ThreadId(2)
    in ty_project::check_file with file=file(Id(c0c))
    in ty_project::Project::check

  2025-06-18T18:40:20.507180Z DEBUG ty_python_semantic::types: Checking file '/Users/johndoe/Projects/XYZ/someproject/src/routes/heartbeat.py'
    at crates/ty_python_semantic/src/types.rs:92 on ThreadId(8)
    in ty_project::check_file with file=file(Id(c00))
    in ty_project::Project::check

  2025-06-18T18:40:20.507166Z DEBUG ty_python_semantic::types: Checking file '/Users/johndoe/Projects/XYZ/someproject/tests/__init__.py'
    at crates/ty_python_semantic/src/types.rs:92 on ThreadId(10)
    in ty_project::check_file with file=file(Id(c14))
    in ty_project::Project::check

  2025-06-18T18:40:20.507518Z DEBUG ty_python_semantic::module_resolver::resolver: Resolving dynamic module resolution paths
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:395 on ThreadId(7)
    in ty_project::check_file with file=file(Id(c05))
    in ty_project::Project::check

Checking ------------------------------------------------------------ 1/22 files                                                                                                                                                                                                                                                                       2025-06-18T18:40:20.507249Z DEBUG ty_python_semantic::types: Checking file '/Users/johndoe/Projects/XYZ/someproject/src/routes/webhook.py'
    at crates/ty_python_semantic/src/types.rs:92 on ThreadId(3)
    in ty_project::check_file with file=file(Id(c02))
    in ty_project::Project::check

  2025-06-18T18:40:20.507602Z DEBUG ty_python_semantic::types: Checking file '/Users/johndoe/Projects/XYZ/someproject/src/extractors/users.py'
    at crates/ty_python_semantic/src/types.rs:92 on ThreadId(10)
    in ty_project::check_file with file=file(Id(c08))
    in ty_project::Project::check

  2025-06-18T18:40:20.507263Z DEBUG ty_python_semantic::types: Checking file '/Users/johndoe/Projects/XYZ/someproject/tests/test_extractors.py'
    at crates/ty_python_semantic/src/types.rs:92 on ThreadId(11)
    in ty_project::check_file with file=file(Id(c12))
    in ty_project::Project::check


thread '<unknown>' has overflowed its stack
fatal runtime error: stack overflow
```

Please let me know if you need to know the content of the last checked file.

### Version

ty 0.0.1-alpha.11 (1ae703836 2025-06-17)

---

_Comment by @nzedler on 2025-06-18 18:51_

I just checked, excluding the files inside the /tests folder leads to the same result. I had the suspicion, that it may occur from the invalid import statement (due to not maintained unit tests hehe). 

---

_Comment by @carljm on 2025-06-18 18:52_

Thanks for the report! Probably this is #93, which I'm working on, but hard to say for sure without seeing the code.

---

_Label `fatal` added by @carljm on 2025-06-18 18:52_

---

_Label `needs-mre` added by @AlexWaygood on 2025-06-19 20:45_

---

_Comment by @carljm on 2025-08-15 01:24_

Going to close this for now since we don't have any way to reproduce; @nzedler if you want to try with the latest ty and let us know if you still see the stack overflow, that would be good to know.

---

_Closed by @carljm on 2025-08-15 01:24_

---
