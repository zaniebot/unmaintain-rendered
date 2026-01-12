```yaml
number: 16904
title: Question on migrating a uv-synced environment to an offline machine
type: issue
state: open
author: mytk2012
labels:
  - needs-mre
assignees: []
created_at: 2025-12-01T07:59:14Z
updated_at: 2025-12-18T17:14:20Z
url: https://github.com/astral-sh/uv/issues/16904
synced_at: 2026-01-12T16:02:40Z
```

# Question on migrating a uv-synced environment to an offline machine

---

_@mytk2012_

## We need to migrate the environment to an offline machine using uv sync, as the current development environment is offline. </br></br>
### We have attempted three methods (1-3), all of which failed:</br></br>
- 1.Export the uv environment via pip freeze and then run uv add.</br>
However, the uv add operation failed because the TOML file contains Git URLs without specifying explicit Git versions or Git commit hashes, making it impossible to execute the add command successfully.
- 2.Directly run uv sync. This method also failed due to the presence of HTTPS links in the dependencies, which prevent direct synchronization in an offline environment.
- 3.Install dependencies using uv pip (requiring dependency resolution in the order from low-level dependencies to high-level dependencies) with the command:
    ```
    pip download -r requirements.txt -d ./dist
    ```
     The core issue here is that the dependencies cannot be sorted in the order of "low-level → high-level". As a result, installation of high-level dependencies throws errors because their required low-level dependencies are not yet installed.</br>
##### We are currently trying the fourth method:4. Set up a new environment in Docker and then migrate it to the offline machine.


---

_Label `question` added by @mytk2012 on 2025-12-01 07:59_

---

_Comment by @konstin on 2025-12-01 08:33_

Have you tried copying the cache directory instead, and using `uv sync --offline` with an up-to-date lockfile?

---

_Comment by @mytk2012 on 2025-12-01 09:41_

> Have you tried copying the cache directory instead, and using `uv sync --offline` with an up-to-date lockfile?

Yeah, among the packages missing from the new machine, a large number are from the dev dependency group, with some others coming from the sub-dependencies of the dev group plus additional groups that may have been activated on the old machine.

Even though the --offline flag is added to enforce offline mode, the core cause of the error is clear: the corresponding file for the package ray==2.52.1 is not present in the uv cache on the new machine. The --offline flag only disables downloading new packages from the internet, but if the cache lacks the packages required for installation by the command, uv will still throw an error — this issue is related to "installation triggered by sub-dependencies or dependency groups".

What should I do?


---

_Comment by @mytk2012 on 2025-12-01 10:54_

> Have you tried copying the cache directory instead, and using `uv sync --offline` with an up-to-date lockfile?

Can u provide some solutions?


---

_Comment by @konstin on 2025-12-01 11:45_

To prepare the cache, you need to run the same installation/sync commands on the online machine as you want to run on the offline machine. This way, all the packages will be in the cache and you can use `offline`. For example, if you run `uv sync` on the online machine with the same project and the same uv version, copy the cache to the offline machine and run `uv sync --offline`, all packages will be installed from the cache.

---

_Comment by @mytk2012 on 2025-12-01 18:31_

> To prepare the cache, you need to run the same installation/sync commands on the online machine as you want to run on the offline machine. This way, all the packages will be in the cache and you can use `offline`. For example, if you run `uv sync` on the online machine with the same project and the same uv version, copy the cache to the offline machine and run `uv sync --offline`, all packages will be installed from the cache.

uv sync --group dev --offline:
 × Failed to build `agentlightning @
  │ file:///home/ch/Agentic_RL/agent-lightning/env`
  ├─▶ Failed to install requirements from `build-system.requires`
  ├─▶ Failed to download `pathspec==0.12.1`
  ╰─▶ Network connectivity is disabled, but the
      requested data wasn't found in the cache for:
      `https://pypi.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl`


---

_Comment by @konstin on 2025-12-03 09:16_

Can you try cleaning the cache on the online machine first? We're failing to install build dependencies, maybe there's a cached build for agentlightning but `pathspec`, its build dependency, somehow isn't in the cache.

---

_Comment by @mytk2012 on 2025-12-04 03:49_

I have cleared the cache, and after deleting it and redoing the previous steps, the issue still persists.

---

_Comment by @mytk2012 on 2025-12-04 06:05_

uv sync --python /home/ch/miniconda3/envs/agent_train/bin/python --group dev --offline 

errors: 
Creating virtual environment at: .venv
warning: The `extra-build-dependencies` option is experimental and may change without warning. Pass `--preview-features extra-build-dependencies` to disable this warning.
Resolved 536 packages in 1ms
  × Failed to download `polars-runtime-32==1.35.2`
  ╰─▶ Network connectivity is disabled, but the
      requested data wasn't found in the cache for:
      `https://files.pythonhosted.org/packages/e2/c8/fd9f48dd6b89ae9cff53d896b51d08579ef9c739e46ea87a647b376c8ca2/polars_runtime_32-1.35.2-cp39-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl`
  help: `polars-runtime-32` (v1.35.2) was included because `agentlightning`
        (v0.3.0) depends on `litellm[proxy]` (v1.80.0) which depends on
        `polars` (v1.35.2) which depends on `polars-runtime-32`

---

_Comment by @konstin on 2025-12-18 17:14_

Can you provide a minimal reproducible example, a set of commands or input files to run that reproduce the problem? I'm having trouble reproducing your problem, the caching is working for me.

---

_Label `question` removed by @konstin on 2025-12-18 17:14_

---

_Label `needs-mre` added by @konstin on 2025-12-18 17:14_

---
