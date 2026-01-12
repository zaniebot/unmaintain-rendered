```yaml
number: 846
title: Use a single python process across all steps of building a source dist
type: issue
state: closed
author: konstin
labels:
  - performance
assignees: []
created_at: 2024-01-09T14:21:36Z
updated_at: 2024-03-18T14:42:33Z
url: https://github.com/astral-sh/uv/issues/846
synced_at: 2026-01-12T15:58:24Z
```

# Use a single python process across all steps of building a source dist

---

_@konstin_

Starting python is slow (10ms on my machine), and importing a build backend is even slower. Instead, we should start a python interpreter once and keep reusing it. We avoid the startup cost and cached import are free.

PEP 517 says you _should_ use a fresh subprocess each time, but i don't see why keeping the process running shouldn't work.

> Frontends should call each hook in a fresh subprocess, so that backends are free to change process global state (such as environment variables or the working directory). A Python library will be provided which frontends can use to easily call hooks this way.

Ad-hoc Benchmarks:

```
$ hyperfine 'python3.12 -c "import sys; print(sys.version)"'
Benchmark 1: python3.12 -c "import sys; print(sys.version)"
  Time (mean ± σ):       8.8 ms ±   3.9 ms    [User: 7.1 ms, System: 1.9 ms]
  Range (min … max):     3.8 ms …  21.8 ms    128 runs
$ hyperfine 'python -c "import setuptools; print(setuptools.__version__)"'
Benchmark 1: python -c "import setuptools; print(setuptools.__version__)"
  Time (mean ± σ):      48.9 ms ±   3.8 ms    [User: 44.3 ms, System: 4.7 ms]
  Range (min … max):    46.5 ms …  71.9 ms    40 runs
$ hyperfine 'python -c "import poetry.core.masonry.api"'
Benchmark 1: python -c "import poetry.core.masonry.api"
  Time (mean ± σ):      30.3 ms ±   2.6 ms    [User: 26.1 ms, System: 4.3 ms]
  Range (min … max):    29.0 ms …  49.2 ms    59 runs
```

---

_Label `performance` added by @konstin on 2024-01-09 14:21_

---

_Comment by @BurntSushi on 2024-01-09 14:32_

> but i don't see why keeping the process running shouldn't work.

Doesn't this depend on what the hook itself is doing?

---

_Comment by @konstin on 2024-01-09 14:35_

If the backend e.g. sets global variables and expects them to be fresh anytime a hook is called, then this would fail, but if the hooks are implemented properly i see no problem.

---

_Comment by @zanieb on 2024-01-09 14:51_

We could also reset the environment variables and working directory if needed.

I don't know if people would be good about not mutating those (or other global state).

---

_Comment by @zanieb on 2024-01-09 18:55_

Would we be using the process concurrently?

---

_Comment by @charliermarsh on 2024-01-09 18:55_

No, serially.

---

_Comment by @zanieb on 2024-01-09 21:51_

I'm confident in my ability to do something robust on the Python side, although I'm not sure what all the requirements are :)

---

_Comment by @konstin on 2024-01-09 22:51_

It needs an async interface you can tell it to invoke one of the [PEP 517](https://peps.python.org/pep-0517/#build-wheel) hooks with it's respective argument(s) and get a `Result<(return value, stderr), (stdout, stderr with backtrace)>` on the rust side. It needs to be resilient to the child process aborting (it should retain stderr) and large amount of output (will not fit in a 64KB pipe buffer).

---

_Renamed from "Use a single python process all step of building a source dist" to "Use a single python process across all steps of building a source dist" by @konstin on 2024-02-02 20:11_

---

_Comment by @bschoenmaeckers on 2024-03-18 12:37_

Is it possible to dynamically load the correct libpython.so using dlopen (or some platform variant) and run the python code in the current process?

---

_Comment by @konstin on 2024-03-18 12:54_

Not really unfortunately. We would attach the python interpreter state to the current process (breaking isolation between builds) and we'd still pay the python initialization cost.

---

_Comment by @zanieb on 2024-03-18 14:25_

For bookkeeping I implemented this in #895 and we did not see a significant improvement.

Should we close this?

---

_Closed by @konstin on 2024-03-18 14:30_

---

_Comment by @charliermarsh on 2024-03-18 14:36_

It’s a minor optimization. It should still save ~30-50ms from source distribution building but I don’t feel strongly about doing it (and it shouldn’t be prioritized).

---

_Comment by @zanieb on 2024-03-18 14:42_

The problem is that it's costly (and technically dubious) to reset the Python modules on each call which is necessary because new packages are installed in-between calls and libraries (e.g. setuptools) have dynamic behavior depending on what is available at import-time.

---
