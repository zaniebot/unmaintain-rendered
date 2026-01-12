```yaml
number: 451
title: "Cannot resolve imported module <module_name> for all external modules"
type: issue
state: closed
author: rikkarth
labels:
  - question
  - imports
assignees: []
created_at: 2025-05-19T14:29:41Z
updated_at: 2025-05-19T15:37:01Z
url: https://github.com/astral-sh/ty/issues/451
synced_at: 2026-01-12T15:54:23Z
```

# Cannot resolve imported module <module_name> for all external modules

---

_@rikkarth_

### Summary

After installing VS Code extension of ty all built-in dependencies are detected but not the 3rd party ones.
![Image](https://github.com/user-attachments/assets/3744caaa-adcf-41b2-bdd4-80ea9e0bf74d)

# How to Reproduce

1. Install VS Code on Windows 11
2. Install WSL Ubuntu 24.04 or similar
3. Install pyenv
3.1 install python 3.13.2 or higher with pyenv
3.2 create new virtualenv with installed python version
3.3 activate environment, you can use VS Code with Select Interpreter
4. create new repo add a few external dependencies
```bash
pandas
google-generativeai
```
5. create a `src` structure src/main.py for example


### Version
ty 0.0.1a5


---

_Comment by @rikkarth on 2025-05-19 14:40_

Apprently not finding my pyenv environment
```bash
0.000241340s DEBUG main ty_python_semantic::module_resolver::resolver: Adding first-party search path '/home/rikkarth/repoland/test-proj/src'
   0.000252450s DEBUG main ty_python_semantic::module_resolver::resolver: Using vendored stdlib
   0.000700700s DEBUG main ty_python_semantic::module_resolver::resolver: Discovering virtual environment in `/home/rikkarth/repoland/test-proj`
   0.000710380s DEBUG main ty_python_semantic::module_resolver::resolver: No virtual environment found
   0.003365340s DEBUG ty:worker:0 check_file{file=file(Id(800))}: ty_python_semantic::types: Checking file '/home/rikkarth/repoland/test-proj/src/analyzer.py'
   0.003623180s DEBUG ty:worker:0 check_file{file=file(Id(800))}: ty_python_semantic::module_resolver::resolver: Resolving dynamic module resolution paths
   0.003630440s DEBUG ty:worker:0 check_file{file=file(Id(800))}: ty_python_semantic::module_resolver::resolver: Module `langchain_core.messages` not found in search paths
   0.004189350s  INFO     ty:main ty_server::server: File watcher successfully registered
   0.007303780s  INFO ty:worker:0 check_file{file=file(Id(800))}:Project::index_files{project=test-proj}: ty_project: Indexed 6 file(s)
   0.007432810s DEBUG ty:worker:0 check_file{file=file(Id(800))}: ty_python_semantic::module_resolver::resolver: Module `langchain_google_genai` not found in search paths
```

---

_Label `imports` added by @AlexWaygood on 2025-05-19 14:42_

---

_Label `question` added by @MichaReiser on 2025-05-19 14:58_

---

_Comment by @rikkarth on 2025-05-19 15:10_

After checking with @MichaReiser we figured VS Code wasn't setting the VIRTUAL_ENV variable

i hardcoded env path in `ty.toml` with `environment.python = "<path-to-your-venv>"` and it worked

logs for working
```
 0.000114070s DEBUG main ty_project::metadata: Searching for a project in '/home/rikkarth/repoland/test-proj'
   0.000214170s  WARN main ty_project::metadata: Ignoring the `tool.ty` section in `/home/rikkarth/repoland/test-proj/pyproject.toml` because `/home/rikkarth/repoland/test-proj/ty.toml` takes precedence.
   0.000218130s DEBUG main ty_project::metadata: Found project at '/home/rikkarth/repoland/test-proj'
   0.000229240s DEBUG main ty_project::metadata::configuration_file: Searching for a user-level configuration at `/home/rikkarth/.config/ty/ty.toml`
   0.000250800s  INFO main ty_project::metadata::options: Defaulting to python-platform `linux`
   0.000265210s  INFO main ty_python_semantic::program: Python version: Python 3.13, platform: linux
   0.000271260s DEBUG main ty_python_semantic::module_resolver::resolver: Adding first-party search path '/home/rikkarth/repoland/test-proj'
   0.000283030s DEBUG main ty_python_semantic::module_resolver::resolver: Adding first-party search path '/home/rikkarth/repoland/test-proj/src'
   0.000286110s DEBUG main ty_python_semantic::module_resolver::resolver: Using vendored stdlib
   0.000843480s DEBUG main ty_python_semantic::module_resolver::resolver: Resolving `--python` argument: /home/rikkarth/.pyenv/versions/3.13.2/envs/test-proj
   0.000865150s DEBUG main ty_python_semantic::site_packages: Attempting to parse virtual environment metadata at '/home/rikkarth/.pyenv/versions/3.13.2/envs/test-proj/pyvenv.cfg'
   0.000880330s DEBUG main ty_python_semantic::site_packages: Searching for site-packages directory in `sys.prefix` path `/home/rikkarth/.pyenv/versions/3.13.2/envs/test-proj`
   0.000886160s DEBUG main ty_python_semantic::site_packages: Resolved site-packages directories for this virtual environment are: ["/home/rikkarth/.pyenv/versions/3.13.2/envs/test-proj/lib/python3.13/site-packages"]
   0.000890890s DEBUG main ty_python_semantic::module_resolver::resolver: Adding site-packages search path '/home/rikkarth/.pyenv/versions/3.13.2/envs/test-proj/lib/python3.13/site-packages'
   0.008383540s  INFO ty:main ty_server::server: File watcher successfully registered
   8.522525880s DEBUG ty:worker:0 check_file{file=file(Id(c00))}: ty_python_semantic::types: Checking file '/home/rikkarth/repoland/test-proj/src/analyzer.py'
   8.522984910s DEBUG ty:worker:1 ty_python_semantic::module_resolver::resolver: Resolving dynamic module resolution paths
   8.578703210s  INFO ty:worker:1 Project::index_files{project=test-proj}: ty_project: Indexed 6 file(s)
```


---

_Closed by @MichaReiser on 2025-05-19 15:37_

---
