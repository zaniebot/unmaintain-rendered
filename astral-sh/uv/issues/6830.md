```yaml
number: 6830
title: "Feature: add --lockfile flag to allow users to customize lock file name"
type: issue
state: open
author: jensqin
labels:
  - needs-decision
  - cli
assignees: []
created_at: 2024-08-29T19:40:39Z
updated_at: 2025-12-19T20:08:40Z
url: https://github.com/astral-sh/uv/issues/6830
synced_at: 2026-01-10T03:11:31Z
```

# Feature: add --lockfile flag to allow users to customize lock file name

---

_Issue opened by @jensqin on 2024-08-29 19:40_

# Usecase

When a project needs to have multiple lock files for different users, it is convenient to be able to rename the lockfile.

## API Suggestion
```bash
uv lock --lockfile my_uv_lock_file.lock
```

## Context
[pdm](https://pdm-project.org/en/latest/usage/lockfile/#specify-another-lock-file-to-use) has this feature.

---

_Comment by @zanieb on 2024-08-29 20:24_

Can you share more details about "different users"?

---

_Label `needs-decision` added by @zanieb on 2024-08-29 20:24_

---

_Label `cli` added by @zanieb on 2024-08-29 20:24_

---

_Comment by @jensqin on 2024-08-30 14:11_

I should have said "different resolution". For example, one user wants to use "highest", and another wants to use "lowest-direct".

---

_Comment by @SamyAB on 2024-08-30 22:49_

I agree that the feature would be very useful in different situations. In addition to the case that @jensqin mentioned, I personally use the `--lockfile` option from `pdm` to have different locks based on different sources. Right now with `uv` that might be translated as different lockfiles based on different sets of `--index-url` and `--extra-index-url` 

---

_Comment by @nikhilweee on 2024-09-11 19:50_

Another use case is to support conflicting dependencies if/when #6981 is implemented.

---

_Comment by @jsiverskog on 2025-03-19 10:20_

i want this for the sole purpose of keeping my repository roots clean - i prefer to have as many as possible of environment related files in a separate directory.

---

_Comment by @vladidobro on 2025-08-17 10:35_

Hi,
I would say that support for multiple lockfiles is extremely important for scalable monorepos, and is the last missing piece for us to fully commit to a uv-based monorepo.

For example in our setup of some shared libs + many domain-specific applications with data pipelines, I would like to be able to use a new feature of some library without being forced to upgrade and test it and fix it in all of the other applications (100+ mostly completely unrelated pipelines).

Our current options seem to be
- use pants
- dont use workspaces, treat is as unrelated uv packages that just happen to be in one repo (very inconvenient)

Or is there already a way to achieve it with uv workspaces?
Does uv plan some "environments" support for these usecases?

Thank you!

---

_Comment by @g0di on 2025-09-17 08:08_

I'm using nox for running tests on my libraries against multiple versions of Python. On my nox test session I'm using the uv `--resolution` flag to run my tests against highest and lowest versions of required dependencies which is super useful. However, this cause uv to re-lock and override the `uv.lock` on each session. I would find it useful to tell uv to use separated lock files for both resolution strategies as well as using the default one in day-to-day situations.

---

_Comment by @konstin on 2025-09-17 10:27_

@g0di Can you use `uv pip install --resolution lowest .` for your tests, or do you need a lockfile for lowest, too`

---

_Comment by @zanieb on 2025-09-17 10:55_

You can use `--isolated` for an isolated lockfile.

---

_Comment by @g0di on 2025-09-17 11:16_

Those are two interesting options I did not think about. Let me give it a try to see if it fits my needs! Thanks!

---

_Comment by @g0di on 2025-09-17 12:34_

> You can use `--isolated` for an isolated lockfile.

I was wondering why I did not see this one but using the latest uv version gives me this:
```
warning: The `--isolated` flag is deprecated and has no effect. Instead, use `--no-config` to prevent uv from discovering configuration files.
```
I tried with `--isolated` and `--no-config` but it did not work (the first is ignored and the second ignore my whole configuration breaking my builds).

---

_Comment by @zanieb on 2025-09-17 12:57_

Sorry, what command are you using? It's available on `uv run` to use an isolated environment and lockfile.

---

_Comment by @g0di on 2025-09-17 13:48_

> Sorry, what command are you using? It's available on `uv run` to use an isolated environment and lockfile.

I was trying to use `uv sync` because it fits better in my nox file. The idea is that nox creates me a venv, then I sync my project and lowest/highest dependencies in it and finally run my tests. So I end up with one venv per combinations or python version and resolution strategy. Not having the ability to use different locks file cause this to relock everything each time.

> @g0di Can you use uv pip install --resolution lowest . for your tests, or do you need a lockfile for lowest, too`

This is what I tried to achieve and ended up with the following nox session, leveraging compile and install. It looks like its working as expected even if its a bit verbose (considering I'll have to copy paste this in all my company projects).

```python
@nox.session(python=["3.9", "3.12"], reuse_venv=True, venv_backend="uv")
@nox.parametrize("resolution", ["lowest-direct", "highest"])
def tests(session: nox.Session, resolution: str) -> None:
    session.run_install(
        "uv",
        "pip",
        "compile",
        "pyproject.toml",
        "--group=dev",
        "--format=pylock.toml",
        f"--resolution={resolution}",
        f"--output-file=pylock.{resolution}.toml",
        external=True,
        silent=True,
    )
    session.install("--no-deps", "--exact", f"-r=pylock.{resolution}.toml")
    session.install(".", "--no-deps")

    session.run("coverage", "run", "--parallel-mode", "-m", "pytest")
    session.notify("coverage")
```

I'm using `uv pip compile` because I can use the pylock format and include hashes automatically. Also, I suppose that when I rerun this command, uv reuse the existing lock file reducing the compile time (to be confirmed).

I'm open to feedback!

---

_Comment by @NicholasPini on 2025-09-18 19:59_

Just adding my possible use case: I'm building a ci/cd pipeline which needs to build and push docker containers. These containers are used to setup a python environment for an application to eventually run inside. My idea is to use `uv lock --check` using the uv.lock of the previous commit to check if dependencies have changed: if so, rebuild the container, otherwise I skip this step entirely. Not sure if there's a way to do this right now.
My workaround is to rename the current uv.lock to something else, get the previous uv.lock, run `uv lock --check` and then replace the old file with the file I previously renamed.

---

_Comment by @Gattocrucco on 2025-12-17 12:23_

Anecdote of last time where I would have liked `UV_LOCKFILE=...` or similar: I launched a Python shell with `uv run --resolution lowest-direct ...` to debug some changes I was making with the oldest supported dependencies, after that as usual I did `git restore uv.lock` because it gets overridden when I change resolution, then forgot to do `uv sync` before committing, which broke CI because in the change I added a new dependency.

Of course this can be programmed with make targets or custom scripts that backup then restore `uv.lock` for these cases, but handling all edge cases this way is error-prone and takes time, and makes it harder to just type commands when I need something different than the pre-written debug command.

---

_Comment by @Kludex on 2025-12-19 15:55_

I'm also interested in this one.

---

_Comment by @zanieb on 2025-12-19 16:57_

@Kludex please share your use-case

---

_Comment by @jiridanek on 2025-12-19 17:03_

My use case: we're maintaining separate Python indices per hardware profile ([ROCm](https://console.redhat.com/api/pypi/public-rhai/rhoai/3.2/rocm-ubi9/simple/) vs [CUDA](https://console.redhat.com/api/pypi/public-rhai/rhoai/3.2/cuda-ubi9/simple/)) and we would like to sometimes resolve one `pyproject.toml` against both, and thereby having both `uv.lock.rocm` and `uv.lock.cuda` for the repo.

---

_Comment by @Kludex on 2025-12-19 17:06_

> [@Kludex](https://github.com/Kludex) please share your use-case

I test both `lowest-direct` and current `uv.lock` in `pydantic-ai`, and by mistake we were not really testing the `lowest-direct`, so we find ourselves in a situation that the `lowest-direct` is actually not valid anymore because the transitive dependencies are not compatible with the lowest direct dependencies. If we had a lock file (we did make the mistake of installing highest instead of `lowest-direct` sometimes already because of some mistakes with uv over time) we wouldn't have this problem.

---

_Comment by @zanieb on 2025-12-19 20:08_

@jiridanek that makes sense. we would love to solve that problem within a single lockfile because lots of people have to deploy against different accelerators.

@Kludex I'm sorry, I don't follow? If you weren't testing `lowest-direct` correctly I don't see how a separately lockfile would have saved you? It'd just also have drifted from your main `uv.lock` file. Also, you wouldn't be able to assert that your `lowest-directly` file actually represents the same content as your main `uv.lock` file which makes it sort of useless?

---
