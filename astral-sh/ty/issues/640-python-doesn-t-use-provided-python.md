```yaml
number: 640
title: "`--python` doesn't use provided python"
type: issue
state: closed
author: remiconnesson
labels:
  - imports
assignees: []
created_at: 2025-06-12T07:36:25Z
updated_at: 2025-06-21T19:28:48Z
url: https://github.com/astral-sh/ty/issues/640
synced_at: 2026-01-12T15:54:23Z
```

# `--python` doesn't use provided python

---

_@remiconnesson_

### Summary

`uvx ty check . --python="/home/remi/work/acme/app/.venv/bin/python"`

gives error `[unresolved-import]: Cannot resolve imported module acme.Acme`

where `acme` is an editable installed package

whereas running `/home/remi/work/acme/app/.venv/bin/python` does work (as shown below)
```
>>> from acme import Acme
```

So it looks like `ty` is not picking up the .venv from the `--python` argument

---

I also tried initially using `uvx ty check` having the `.venv` activated but it seems to me more significative that explicitly setting the python still doesn't work

My system is WSL2 ubuntu


### Version

ty 0.0.1-alpha.9

---

_Comment by @MichaReiser on 2025-06-12 07:39_

Thanks for the report. Can you try running `uvx ty check . --python="/home/remi/work/acme/app/.venv/bin/python"` with `-vv` and share the logs here?  

---

_Comment by @AlexWaygood on 2025-06-12 07:40_

Is ty able to resolve imports that refer to non-editable installs in that Python installation? And does the editably installed package use setuptools as its build backend? (Setuptools is the default build backend if no backend has been specified in your pyproject.toml.)

If the answer to both of those is "yes" then this is probably another instance of #585 / #475. It's a priority for us to improve our diagnostics here to make it clearer what the issue is. 

---

_Label `needs-info` added by @AlexWaygood on 2025-06-12 07:41_

---

_Label `imports` added by @AlexWaygood on 2025-06-12 07:41_

---

_Comment by @remiconnesson on 2025-06-12 08:22_

@AlexWaygood 
> Is ty able to resolve imports that refer to non-editable installs in that Python installation? 

No it isn't actually `error[unresolved-import]: Cannot resolve imported module ``numpy`` `

> And does the editably installed package use setuptools as its build backend? (Setuptools is the default build backend if no backend has been specified in your pyproject.toml.)

Yes I think so (using setup.py which imports setuptools)



---

_Comment by @remiconnesson on 2025-06-12 08:27_

@MichaReiser 

> Thanks for the report. Can you try running uvx ty check . --python="/home/remi/work/project/app/.venv/bin/python" with -vv and share the logs here?

A weird thing, not sure if that's the issue, It seems it's using src but it shouldn't, (monorepo), `src` and `app` are neighbors in my case

 

```
➜  alerting git:(user/alerting/queries) ✗ uvx ty check . --python=/home/user/work/project/app/.venv/bin/python -vv
2025-06-12 10:23:30.127431482 WARN  ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
2025-06-12 10:23:30.128089456 DEBUG Version: 0.0.1-alpha.9
2025-06-12 10:23:30.128119473 DEBUG Architecture: x86_64, OS: linux, case-sensitive: case-sensitive
2025-06-12 10:23:30.128233015 DEBUG Searching for a project in '/home/user/work/project/app/org/alerting'
2025-06-12 10:23:30.128366416 DEBUG Project without `tool.ty` section: '/home/user/work/project'
2025-06-12 10:23:30.128400209 DEBUG Searching for a user-level configuration at `/home/user/.config/ty/ty.toml`
2025-06-12 10:23:30.128432289 INFO  Defaulting to python-platform `linux`
2025-06-12 10:23:30.128458298 DEBUG Including `./src` in `src.root` because a `./src` directory exists
2025-06-12 10:23:30.128507220 DEBUG Adding first-party search path '/home/user/work/project'
2025-06-12 10:23:30.128534070 DEBUG Adding first-party search path '/home/user/work/project/src'
2025-06-12 10:23:30.128540963 DEBUG Using vendored stdlib
2025-06-12 10:23:30.129026513 DEBUG Resolving `--python` argument: /home/user/work/project/app/.venv/bin/python
2025-06-12 10:23:30.129086235 DEBUG Attempting to parse virtual environment metadata at '/home/user/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/pyvenv.cfg'
2025-06-12 10:23:30.129112264 DEBUG Searching for site-packages directory in sys.prefix /home/user/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu
2025-06-12 10:23:30.129156347 DEBUG Resolved site-packages directories for this environment are: SitePackagesPaths({"/home/user/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/lib/python3.12/site-packages"})
2025-06-12 10:23:30.129176555 DEBUG Adding site-packages search path '/home/user/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/lib/python3.12/site-packages'
2025-06-12 10:23:30.129211881 INFO  Python version: Python 3.13, platform: linux
2025-06-12 10:23:30.129303082 DEBUG Setting included paths: 1
2025-06-12 10:23:30.129334421 DEBUG Reloading files for project `project`
2025-06-12 10:23:30.129416294 DEBUG Starting main loop
2025-06-12 10:23:30.129456189 DEBUG Waiting for next main loop message.
2025-06-12 10:23:30.129524597 DEBUG Checking project 'project'
2025-06-12 10:23:30.133619416 INFO   Indexed 13 file(s)
2025-06-12 10:23:30.134071504 DEBUG Checking file '/home/user/work/project/app/org/alerting/__init__.py'
2025-06-12 10:23:30.134084037 DEBUG Checking file '/home/user/work/project/app/org/alerting/stats.py'
2025-06-12 10:23:30.134088406 DEBUG Checking file '/home/user/work/project/app/org/alerting/generator_history/run_data/queries.py'
2025-06-12 10:23:30.134070973 DEBUG Checking file '/home/user/work/project/app/org/alerting/generator_history/run_data/service.py'
2025-06-12 10:23:30.134133189 DEBUG Checking file '/home/user/work/project/app/org/alerting/generator_history/__init__.py'
2025-06-12 10:23:30.134102392 DEBUG Checking file '/home/user/work/project/app/org/alerting/generator_history/generator.py'
2025-06-12 10:23:30.134987842 DEBUG Checking file '/home/user/work/project/app/org/alerting/anomaly_detection/__init__.py'
2025-06-12 10:23:30.135000857 DEBUG Checking file '/home/user/work/project/app/org/alerting/exceptions.py'
2025-06-12 10:23:30.135036183 DEBUG Checking file '/home/user/work/project/app/org/alerting/generator_history/run_data/__init__.py'
2025-06-12 10:23:30.135524779 DEBUG Checking file '/home/user/work/project/app/org/alerting/generator_history/run_data/dto.py'
2025-06-12 10:23:30.135715577 DEBUG Checking file '/home/user/work/project/app/org/alerting/generator_history/repository.py'
2025-06-12 10:23:30.136774743 DEBUG Checking file '/home/user/work/project/app/org/alerting/generator_history/run_data/mapper.py'
2025-06-12 10:23:30.136844424 DEBUG Resolving dynamic module resolution paths
2025-06-12 10:23:30.137459176 DEBUG Module `sqlalchemy.orm` not found in search paths
2025-06-12 10:23:30.137598067 DEBUG Module `org.exceptions` not found in search paths
2025-06-12 10:23:30.137708183 DEBUG Checking file '/home/user/work/project/app/org/alerting/anomaly_detection/core.py'
2025-06-12 10:23:30.137855249 DEBUG Module `org.alerting.generator_history.generator` not found in search paths
2025-06-12 10:23:30.139983851 DEBUG Module `sqlalchemy` not found in search paths
2025-06-12 10:23:30.140052580 DEBUG Module `org.tracking.models` not found in search paths
2025-06-12 10:23:30.151783237 DEBUG Module `org.alerting.stats` not found in search paths
2025-06-12 10:23:30.152840891 DEBUG Module `numpy` not found in search paths
2025-06-12 10:23:30.152974671 DEBUG Module `pandas` not found in search paths
2025-06-12 10:23:30.155960851 DEBUG Module `org.alerting` not found in search paths
2025-06-12 10:23:30.156015794 DEBUG Module `org.alerting.generator_history` not found in search paths
```

---

_Comment by @AlexWaygood on 2025-06-12 08:32_

Is `/home/user/work/project/app/.venv/bin/python` a symlink to `/home/user/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/bin/python`? It looks like we might be resolving symlinks too soon/eagerly if so (which is causing us to add the uv-managed system installation's `site-packages` directory as a search path instead of the virtual environment's `site-packages` directory)

---

_Comment by @remiconnesson on 2025-06-12 08:36_

> Is /home/user/work/project/app/.venv/bin/python a symlink to /home/user/.local/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/bin/python?

Yes it is, I just checked

---

_Comment by @AlexWaygood on 2025-06-12 08:37_

Great, thank you!

---

_Label `needs-info` removed by @AlexWaygood on 2025-06-12 08:37_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-06-20 17:36_

---

_Closed by @AlexWaygood on 2025-06-21 19:28_

---
