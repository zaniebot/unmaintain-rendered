```yaml
number: 5815
title: "Add option to force use of `uv.lock` file when adding dependency or installing a tool"
type: issue
state: open
author: my1e5
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2024-08-06T14:50:55Z
updated_at: 2026-01-09T15:48:50Z
url: https://github.com/astral-sh/uv/issues/5815
synced_at: 2026-01-10T03:11:31Z
```

# Add option to force use of `uv.lock` file when adding dependency or installing a tool

---

_Issue opened by @my1e5 on 2024-08-06 14:50_

From 'The Cargo Book' - https://doc.rust-lang.org/cargo/commands/cargo-install.html#dealing-with-the-lockfile

> ### Dealing with the Lockfile
> By default, the Cargo.lock file that is included with the package will be ignored. This means that Cargo will recompute which versions of dependencies to use, possibly using newer versions that have been released since the package was published. The --locked flag can be used to force Cargo to use the packaged Cargo.lock file if it is available. This may be useful for ensuring reproducible builds, to use the exact same set of dependencies that were available when the package was published.

It would be nice if `uv` had a similar option which allowed you to force the use of the `uv.lock` file rather than recomputing the dependencies from the `pyproject.toml` file. This would enable you to get exact reproducible builds.

Consider this simple use-case
```
$ uv init --name mylib
$ uv add numpy
```
The `pyproject.toml` looks like
```
dependencies = [
    "numpy>=2.0.1",
]
```
and the `uv.lock` file is
```
[[distribution]]
name = "numpy"
version = "2.0.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/1c/8a/0db635b225d2.......
```
You then write some code and once ready you commit and push the project to your Git server (including the `uv.lock` file).

Some time later, you want to re-use this project in another codebase. So you use something like
```
$ uv add git+ssh://git@mygitserver/user/mylib
```
But because a new version of `numpy` has been released it doesn't install version 2.0.1 and instead installs a newer version (potentially causing some issues). Having something like a `--use-lock-file` flag would solve this issue by directing `uv` to follow the lock file exactly. 

The same thing would be useful for `uv tool install`. For example if I've developed an executable app and want the exact same dependencies installed as specified in the lock file. 
```
$ uv tool install git+ssh://git@mygitserver/user/myapp
```
I understand that some might say, well you should have made your dependencies explicit:
```
dependencies = [
    "numpy==2.0.1",
]
```
But as you can see, the default behaviour of `uv add` is to use version ranges (`>=`). And there may be situations where you are working with someone else's uv-managed project, in which case going by the `uv.lock` file might be essential to ensure a reproducible build.

---

_Comment by @charliermarsh on 2024-08-06 14:55_

Is this not `--frozen` and `--locked`? I believe we already support those.

---

_Comment by @zanieb on 2024-08-06 14:55_

> But because a new version of numpy has been released it doesn't install version 2.0.1 and instead installs a newer version

We should prefer the existing versions in the lockfile when adding dependencies, if we aren't it sounds like either the package you're adding depends on a newer version of numpy or we have an unexpected bug.



---

_Comment by @my1e5 on 2024-08-06 14:57_

> Is this not `--frozen` and `--locked`? I believe we already support those.

My understanding was these flags had a different meaning
```
      --locked                                 Assert that the `uv.lock` will remain unchanged
      --frozen                                 Add the requirements without updating the `uv.lock` file
```

---

_Comment by @charliermarsh on 2024-08-06 14:59_

Oh, you're talking about using the `uv.lock` from Package A when you add Package A as a dependency of Package B?

---

_Comment by @zanieb on 2024-08-06 15:02_

Ohhh that makes more sense. Hm.

---

_Comment by @my1e5 on 2024-08-06 15:12_

Ah sorry, I can see how the confusion came about. Yes, I mean this exactly
> using the uv.lock from Package A when you add Package A as a dependency of Package B?

I'm not aware of other Python tooling and project managers offering this kind of feature. For example `pipx` will ignore the lock files. There is some related discussion here - https://github.com/pdm-project/pdm/discussions/2450

---

_Comment by @zanieb on 2024-08-06 16:05_

This seems particularly reasonable when performing `uv tool install`. It seems a bit more complicated with `uv add`.

---

_Comment by @charliermarsh on 2024-08-06 16:39_

Yeah we should support something like this eventually but I don't think we should try to solve it for the upcoming release -- requires some design.

---

_Label `enhancement` added by @charliermarsh on 2024-08-06 19:38_

---

_Label `needs-design` added by @charliermarsh on 2024-08-06 19:38_

---

_Label `preview` added by @charliermarsh on 2024-08-06 19:38_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---

_Comment by @eddie-dunn on 2025-10-07 11:04_

Forcing an installation _exactly_ as the lockfile specifies will protect against supply chain attacks, so there are security implications for wanting to add this functionality.

What kind of design requirements remain?

---

_Comment by @helmut-hoffer-von-ankershoffen on 2025-12-09 18:46_

I hope this is the right issue @zanieb 

`uvx <tool>` ignores `override-dependencies` in the pyproject.toml. I.e. users using `uvx` to install and run a tool will install vulnerable (transitive) dependencies in cases where override-dependencies is used to force updated versions. 

(1) Was this considered when determining the priority of if and when to support uv.lock in uvx?
(2) Should `uvx --use-lock` actually be the **default** when it's supported. 

Should this not so obvious security gap be mentioned on this page? https://docs.astral.sh/uv/guides/tools/

---

_Comment by @MattiasDC on 2026-01-09 15:30_

Just want to hop in here, mentioning that I'd like `uvx` to use the lockfile of the tool as I need my workflow to be reproducible. I tried a workaround with a requirement.txt and use it with `--with-requirements`

---
