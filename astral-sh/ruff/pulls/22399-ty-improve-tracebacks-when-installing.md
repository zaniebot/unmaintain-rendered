```yaml
number: 22399
title: "[ty] Improve tracebacks when installing dependencies fails in ty_benchmark"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/ty-benchmark-exceptions
created_at: 2026-01-05T14:46:47Z
updated_at: 2026-01-05T14:55:10Z
url: https://github.com/astral-sh/ruff/pull/22399
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Improve tracebacks when installing dependencies fails in ty_benchmark

---

_Pull request opened by @AlexWaygood on 2026-01-05 14:46_

## Summary

I tried running ty_benchmark locally, and installation of homeassistant's dependencies kept failing. However, the traceback didn't tell me which project was the problem, and also implied that the exception handling itself was buggy ("during handling of the exception, another exception occurred"):

```pytb
Traceback (most recent call last):
  File "/Users/alexw/dev/ruff/scripts/ty_benchmark/src/benchmark/venv.py", line 82, in install
    subprocess.run(
    ~~~~~~~~~~~~~~^
        command,
        ^^^^^^^^
    ...<3 lines>...
        text=True,
        ^^^^^^^^^^
    )
    ^
  File "/Users/alexw/Library/Application Support/uv/python/cpython-3.14.0-macos-aarch64-none/lib/python3.14/subprocess.py", line 577, in run
    raise CalledProcessError(retcode, process.args,
                             output=stdout, stderr=stderr)
subprocess.CalledProcessError: Command '['uv', 'pip', 'install', '--python', '/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmp1_b6us7f/venv/bin/python', '--quiet', '--exclude-newer', '2025-12-13T00:00:00Z', 'mypy', '-r', 'requirements_test_all.txt', '-r', 'requirements.txt']' returned non-zero exit status 1.

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/Users/alexw/dev/ruff/scripts/ty_benchmark/.venv/bin/benchmark", line 10, in <module>
    sys.exit(main())
             ~~~~^^
  File "/Users/alexw/dev/ruff/scripts/ty_benchmark/src/benchmark/run.py", line 151, in main
    venv.install(project.install_arguments)
    ~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/dev/ruff/scripts/ty_benchmark/src/benchmark/venv.py", line 90, in install
    raise RuntimeError(f"Failed to install dependencies: {e.stderr}")
RuntimeError: Failed to install dependencies:   × Failed to download `mbddns==0.1.2`
  ├─▶ Failed to write to the client cache
  ╰─▶ Too many open files (os error 24) at path "/Users/alexw/.cache/uv/wheels-v5/pypi/mbddns/.tmpM2R0B4"
```

This PR improves the traceback when dependencies fail to install, fixing both of the above issues; it is now:

```pytb
Traceback (most recent call last):
  File "/Users/alexw/dev/ruff/scripts/ty_benchmark/src/benchmark/venv.py", line 83, in install
    subprocess.run(
    ~~~~~~~~~~~~~~^
        command,
        ^^^^^^^^
    ...<3 lines>...
        text=True,
        ^^^^^^^^^^
    )
    ^
  File "/Users/alexw/Library/Application Support/uv/python/cpython-3.14.0-macos-aarch64-none/lib/python3.14/subprocess.py", line 577, in run
    raise CalledProcessError(retcode, process.args,
                             output=stdout, stderr=stderr)
subprocess.CalledProcessError: Command '['uv', 'pip', 'install', '--python', '/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmph9w6alp7/venv/bin/python', '--quiet', '--exclude-newer', '2025-12-13T00:00:00Z', 'mypy', '-r', 'requirements_test_all.txt', '-r', 'requirements.txt']' returned non-zero exit status 1.

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "/Users/alexw/dev/ruff/scripts/ty_benchmark/.venv/bin/benchmark", line 10, in <module>
    sys.exit(main())
             ~~~~^^
  File "/Users/alexw/dev/ruff/scripts/ty_benchmark/src/benchmark/run.py", line 153, in main
    venv.install(project.install_arguments)
    ~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/dev/ruff/scripts/ty_benchmark/src/benchmark/venv.py", line 92, in install
    raise RuntimeError(msg) from e
RuntimeError: Failed to install dependencies for homeassistant:

  × Failed to build `motionblindsble==0.1.3`
  ├─▶ Failed to run `/Users/alexw/.cache/uv/builds-v0/.tmpjjjo8s/bin/python`
  ╰─▶ Too many open files (os error 24)
```

## Test Plan

- See above
- `uvx prek run -a`
- `uv run --project=./scripts/ty_benchmark cargo run -p ty check --project=./scripts/ty_benchmark`


---

_Review requested from @carljm by @AlexWaygood on 2026-01-05 14:46_

---

_Review requested from @MichaReiser by @AlexWaygood on 2026-01-05 14:46_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-05 14:46_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-05 14:46_

---

_Label `testing` added by @AlexWaygood on 2026-01-05 14:46_

---

_Label `ty` added by @AlexWaygood on 2026-01-05 14:46_

---

_Review comment by @MichaReiser on `scripts/ty_benchmark/src/benchmark/venv.py`:10 on 2026-01-05 14:52_

Nooo, don't make it fancy lol. Now I need to learn new Python features when I want to maintain this next time

---

_@MichaReiser approved on 2026-01-05 14:52_

---

_@Gankra approved on 2026-01-05 14:53_

---

_Merged by @AlexWaygood on 2026-01-05 14:55_

---

_Closed by @AlexWaygood on 2026-01-05 14:55_

---

_Branch deleted on 2026-01-05 14:55_

---
