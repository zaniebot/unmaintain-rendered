```yaml
number: 7062
title: "perf: `uv build` is slower on the second run compared to `uvx hatch build`"
type: issue
state: closed
author: pplmx
labels:
  - performance
assignees: []
created_at: 2024-09-05T02:08:31Z
updated_at: 2025-02-16T19:20:19Z
url: https://github.com/astral-sh/uv/issues/7062
synced_at: 2026-01-12T15:59:10Z
```

# perf: `uv build` is slower on the second run compared to `uvx hatch build`

---

_@pplmx_

## Reproduce Steps

### 1. Using `uv build`

```bash
cookiecutter gh:x-pt/template --directory template/py --no-input && \
cd my-awesome-project && \
uv sync && \
time uv build && \
time uv build && \
time uv build && \
time uv build && \
cd .. && rm -fr my-awesome-project
```

### 2. Using `uvx hatch build`

```bash
cookiecutter gh:x-pt/template --directory template/py --no-input && \
cd my-awesome-project && \
uv sync && \
time uvx hatch build && \
time uvx hatch build && \
time uvx hatch build && \
time uvx hatch build && \
cd .. && rm -fr my-awesome-project
```

### Additional Information

- **Inconsistent Performance:** It's worth noting that the first build with `uv build` is **not always faster** compared to `uvx hatch build`.



---

_Comment by @charliermarsh on 2024-09-05 02:18_

I don't know if there's a ton for us to optimize here but we can look into it. The majority of the time is in invoking the Python interpreter and calling the build backend.

---

_Label `performance` added by @charliermarsh on 2024-09-05 02:18_

---

_Comment by @charliermarsh on 2024-09-05 02:27_

There's so little work happening here that my guess is Hatch benefits from building a project that uses `hatchling` as its own backend, so there's less overhead? Like, we're probably paying overhead to invoke Python several times.

If you compare `uv build` to `python -m build --installer uv`, I believe `uv build` is faster (so, e.g., if you try the same thing with a setuptools project, I would guess uv will be faster).

We'll get similar benefits when we build our own build backend!

---

_Comment by @pplmx on 2024-09-05 02:44_

Expect the UV's own build backend to be used. ❤️ 

---

_Comment by @henryiii on 2024-09-06 20:50_

> If you compare `uv build` to `python -m build --installer uv`, I believe uv build is faster

Hopefully @matthewfeickert doesn't mind me stealing this from him:

```console
$ time pipx run build .
* Creating isolated environment: venv+pip...
* Installing packages in isolated environment:
  - hatch-vcs>=0.3.0
  - hatchling>=1.13.0
* Getting build dependencies for sdist...
* Building sdist...
* Building wheel from sdist
* Creating isolated environment: venv+pip...
* Installing packages in isolated environment:
  - hatch-vcs>=0.3.0
  - hatchling>=1.13.0
* Getting build dependencies for wheel...
* Building wheel...
Successfully built pyhf-0.7.1.dev273.tar.gz and pyhf-0.7.1.dev273-py3-none-any.whl

real	0m3.087s
user	0m2.299s
sys	0m0.282s
$ rm -rf dist && time pipx run build --installer uv .
* Creating isolated environment: venv+uv...
* Using external uv from /home/feickert/.pyenv/shims/uv
* Installing packages in isolated environment:
  - hatch-vcs>=0.3.0
  - hatchling>=1.13.0
* Getting build dependencies for sdist...
* Building sdist...
* Building wheel from sdist
* Creating isolated environment: venv+uv...
* Using external uv from /home/feickert/.pyenv/shims/uv
* Installing packages in isolated environment:
  - hatch-vcs>=0.3.0
  - hatchling>=1.13.0
* Getting build dependencies for wheel...
* Building wheel...
Successfully built pyhf-0.7.1.dev273.tar.gz and pyhf-0.7.1.dev273-py3-none-any.whl

real	0m1.214s
user	0m0.971s
sys	0m0.278s
$ rm -rf dist
$ time uv build
Building source distribution...
Building wheel from source distribution...
Successfully built dist/pyhf-0.7.1.dev273.tar.gz and dist/pyhf-0.7.1.dev273-py3-none-any.whl

real	0m0.799s
user	0m0.634s
sys	0m0.193s
```

And if there was a Rust build backend (like maturin) and uv could skip Python entirely when calling it (like `maturin` the command does), I'd expect `uv build` would be ridiculously fast in that case. But under a second isn't bad. :) For a built package though, the compiler takes most of the time.

---

_Comment by @charliermarsh on 2024-09-06 20:59_

> ...uv could skip Python entirely when calling it (like maturin the command does), I'd expect uv build would be ridiculously fast in that case.

Yeah this is one of the motivations. Would be really cool!

---

_Comment by @charliermarsh on 2025-02-16 19:20_

I think we can probably close this now? The uv-native build backend is basically instant (<10ms):

```
❯ hyperfine "uv build"
Benchmark 1: uv build
  Time (mean ± σ):       7.2 ms ±   0.2 ms    [User: 3.4 ms, System: 3.3 ms]
  Range (min … max):     6.8 ms …   9.1 ms    299 runs
```

(It's still in `--preview`, we don't recommend using it for real projects yet.)

Meanwhile, comparing `hatchling` with `uv build` vs. `uvx hatch build` is still pretty decent given that we can't use a fast-path:

```
❯ hyperfine "uv build"
Benchmark 1: uv build
  Time (mean ± σ):     300.9 ms ±   2.2 ms    [User: 215.5 ms, System: 78.7 ms]
  Range (min … max):   297.4 ms … 304.5 ms    10 runs

my-awesome-project on  main [?] is 📦 v0.0.1 via 🐍 v3.11.6 took 3s
❯ hyperfine "uvx hatch build"
Benchmark 1: uvx hatch build
  Time (mean ± σ):     232.6 ms ±   1.6 ms    [User: 184.0 ms, System: 60.6 ms]
  Range (min … max):   230.5 ms … 235.4 ms    12 runs
```


---

_Closed by @charliermarsh on 2025-02-16 19:20_

---
