---
number: 6783
title: "Consider defaulting to `~=` version_cmp instead of `>=`"
type: issue
state: closed
author: lucaspar
labels:
  - projects
assignees: []
created_at: 2024-08-29T00:15:25Z
updated_at: 2025-05-28T13:11:32Z
url: https://github.com/astral-sh/uv/issues/6783
synced_at: 2026-01-07T13:12:17-06:00
---

# Consider defaulting to `~=` version_cmp instead of `>=`

---

_Issue opened by @lucaspar on 2024-08-29 00:15_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

> This "issue" might be the intended behavior of `uv`, but I believe it should be changed.

Currently, when adding a dependency without an explicit version constraint e.g. `uv add numpy`, `uv` adds a `numpy>=2.1.0` to the `pyproject.toml` in order to track this requirement.

Most semver upgrades are minors and patches, so this is usually fine, but `>=` can be problematic when said package introduces a new major version (e.g. 3.0.0).

The default behavior of Poetry, for example, is to use the [caret](https://python-poetry.org/docs/dependency-specification/), where `^2.1.0` is equivalent to `>=2.1.0 <3.0.0`, thus protecting the project from an unintended breaking upgrade.

The uv's specifier `>=` however, will upgrade to the most recent major by default. PEP 440 introduced the `~=` "[compatible release clause](https://packaging.python.org/en/latest/specifications/version-specifiers/#id5)" / tilde, which - IMO - makes more sense to serve as the default version constraint:

```
~= 2.1.0
# equals to
>= 2.1.0, == 2.1.*
```

Note this behavior is different from Poetry's caret notation, so, unless specified, the patch version could be safely omitted by default to allow minor upgrades, while still preventing major ones:

```
~= 2.1
# equals to
>= 2.1, == 2.*
```

This is a default behavior I'd like to see from `uv` to ease future project upgrades.


---

_Comment by @zanieb on 2024-08-29 03:33_

Now that we have an application / library split I wonder if we can use that to determine if we should add upper bounds by default. Previously, we were quite opposed to this due to its effect on the ecosystem of published packages.

Some prior discussion at https://github.com/astral-sh/uv/issues/5178 — I think the rest of it was private. I previously compiled some [references](https://github.com/zanieb/poetry-relax?tab=readme-ov-file#references) on the topic though.

---

_Comment by @lucaspar on 2024-08-29 04:18_

ha, I figured this must had already been discussed before. Reading some of that convinced me that not setting upper bounds might be the best default for libraries. Most of my projects are applications though, and I often prefer to reserve some time to handle major upgrades manually. I see good points for either option though.


---

_Label `projects` added by @zanieb on 2024-08-29 04:25_

---

_Label `needs-decision` added by @zanieb on 2024-08-29 04:25_

---

_Assigned to @zanieb by @zanieb on 2024-08-29 04:25_

---

_Comment by @palotasb on 2024-08-29 08:28_

I hope this is useful: https://iscinumpy.dev/post/bound-version-constraints/ It's a very detailed (and thus long...) article arguing for why the `>=` default is better than `~=` for Python dependency constraints.

IMHO while `>=` is a no-contest winner for libraries, it makes sense for applications too. Develop using the `>=` constraints by default, explicitly update the lockfile regularly, test the changes, and use the lockfile when needing the stability in prod. Only add `~=`/`<=` constraints when updating breaks something in practice.

---

_Comment by @Zaczero on 2024-10-29 14:41_

Many popular packages (fastapi, httpx, ..) use 0.X.Y versioning schema which is very difficult to work with when using `~=`. I also really dislike packages that enforce upper bound by default. I sometimes find myself stuck not being upgrade to a next major release (mostly backwards compatible) because of some random dependency that decided to safeguard me against it preemptively.

---

_Comment by @amoralesc on 2024-11-13 23:45_

Hey @zanieb, for users exclusively using `uv` as an application management tool, is there a way to change the default behavior of adding the lower bound specifier when `uv add` is used without a explicit constraint?

I like to stick with semver patch versions parity between the `pyproject.toml` and `uv.lock` files (the `~=` version specifier), and let the CI upgrade the dependencies if the tests pass. The problem I find with the default `>=` version specifier is that I really can't tell which version of a package I have installed unless I manually check the lock file. If for some reason the lock file is updated for a package that introduces breaking changes, I'm forced to manually downgrade the package (from which the `~=` specifier would save me).

Consider for example that [uv](https://docs.astral.sh/uv/reference/versioning/#versioning) itself introduces breaking changes when the minor version is updated. This is the case too for [FastAPI](https://fastapi.tiangolo.com/az/deployment/versions/).

A configuration option that allowed users to set the default version specifier strategy (set lower bound `>=`, set strict version `==`, patch versions only `~=`, set upper bound `<=`) when `uv add` is called would be really helpful.

---

_Comment by @frenata on 2024-12-26 16:43_

Yeah I'd *at least* like a way to control `uv`'s behavior here, ala `rye`'s "dependency-operator"[ config file](https://rye.astral.sh/guide/config/#config-file) option.

---

_Comment by @zanieb on 2024-12-28 17:01_

I'm not opposed to adding an option, I think. We'll need to reach consensus as a team though. cc @konstin 

---

_Referenced in [astral-sh/uv#10247](../../astral-sh/uv/issues/10247.md) on 2024-12-31 06:13_

---

_Comment by @konstin on 2025-01-03 10:09_

I usually recommend operators like `>=2.1.0 <3.0.0` (or `>=0.2.1 <0.3.0` for `0.x` versions), since they avoid code breaking when a new major version of a dependencies is released, assuming that the dependency uses somewhat semantic versioning, so an option for that would be nice.

---

_Comment by @frenata on 2025-01-03 15:46_

@konstin , I *think* that's equivalent to ~2.1. Either way, an option to make that the default would be nice.

---

_Referenced in [astral-sh/uv#7810](../../astral-sh/uv/issues/7810.md) on 2025-01-03 17:55_

---

_Comment by @benlindsay on 2025-01-05 19:35_

Confirmed. `>=2.1.0, <3.0.0` is equivalent to `~=2.1`, and `>=2.1.0, <2.2.0` would be equivalent to `~=2.1.0` [link](https://packaging.python.org/en/latest/specifications/version-specifiers/#compatible-release). But since `~=` is not as broadly understood as `>, >=, <, <=` operators, I'd be happy with either of those options. Or the `>=2.1.0, ==2.*` variant used in the link I provided. Any of those are preferable to straight `>=`

---

_Comment by @yakMM on 2025-01-07 19:51_

Coming from https://github.com/astral-sh/uv/issues/10247.

Imo the current `>=` operator is a perfect default for library. As for applications, other than being able to configure the default, adding a command line option would work as well.

For example PDM has the `--save-exact` option to the `pdm add` command that can be useful for some applications use cases.



---

_Referenced in [bakdata/kpops#578](../../bakdata/kpops/pulls/578.md) on 2025-01-09 15:46_

---

_Comment by @yourcelf on 2025-01-10 04:07_

The "compatible" range specifier `~=` isn't great as a default.  While `~=2.1` is equivalent to `>=2.1.0`, it is *not* equivalent to `>=2.1.1`, since `~=2.1` allows `2.1.0`.  The poetry style `^2.1.1` can be expressed precisely as `>=2.1.1, <3.0.0`, which works, but is verbose.  The difference would be if I'm installing second library that expresses a dependency on `==2.1.0`, I will get a dependency solver error (which is desirable), instead of silently allowing a (potentially with security bugs) earlier patch version into the solution.

As someone coming from a long time working with poetry and `^`-style ranges, the urge I feel is the desire for a compact or canonical way to express a "compatible" version spec, but with a lower bound to exclude earlier buggy releases.  But if we stick strictly to [PEP 508](https://peps.python.org/pep-0508/), I don't think there's a way to get that without expressing both bounds explicitly.

So I'd advocate for a double inequality like `>=2.1.1, <3.0.0` as the clearest semver "compatible with lower bound" style.

---

_Comment by @AmmoniumX on 2025-02-24 14:52_

I think frenata's suggestion of implementing rye's "dependency-operator" config would satisfy every party. Keep the `>=` as default, since there is good justification for having it there, but people who aren't developing public libraries should have the _option_ to explicitly state they want to limit themselves to the current major version. I hope we can see something similar added to uv, as this wouldn't really change the majority's opinion of "having >= by default is better", but still gives other people the option to change the default behavior.

---

_Referenced in [astral-sh/uv#12401](../../astral-sh/uv/issues/12401.md) on 2025-03-23 22:50_

---

_Comment by @tomaszbk on 2025-04-09 15:32_

Considering @Zaczero 's thought that many packages use 0.X.Y, I think the best for uv _**applications**_ is to use pinned versions by default in the toml (or a cli parameter on uv init that makes every `uv add` afterward use `==` on the toml instead of `>=`), and make upgrades manually for now, and using `uv upgrade` in the future, once #6794 is approved.

I work on some python apps and me and my team don't want to deal with multiple ranges and application versions, we want to have only one and use that one on prod.

pd: the `==` would not only be used for dependencies, but also for the `requires-python` field, meaning that _**application**_ projects wouldn't require a `.python-version` anymore, because the exact version would be already specified in the toml.

---

_Comment by @palotasb on 2025-04-09 16:16_

> I think the best for uv applications is to use pinned versions by default in the [pyproject.]toml

I think this is what you should use the `uv.lock` lockfile for both when developing the application and deploying it. Keep the constraints in your `pyproject.toml` relaxed, only list your direct dependencies, and don't worry about exactly specifying the minimum dependency version your app's supposed to work with. But then generate a `uv.lock` file and use it both in your development environment (make `uv sync --frozen` part of your dev env set up, include it in your readme or your onboarding training), and configure your CI to also run `uv sync --frozen`. When deploying your application also make sure it's based on `uv sync --frozen`, perhaps this is a line you need to add to your `Dockerfile`.

All of this ensures that everyone working on a specific commit (both your colleagues and your CI) will install the exact same version of all dependencies – no "works on my machine" issues. But it also enables easy upgrades. Just run `uv sync --upgrade`, and commit and push the changes to `uv.lock` to upgrade in a well-controlled way.



---

_Comment by @tomaszbk on 2025-04-09 19:40_

> don't worry about exactly specifying the minimum dependency version your app's supposed to work with

the idea here is in case we want to do a dependency version rollback? For our application, we find it more readable to have the same dependency versions in both the pyproject.toml and uv.lock. It's much simpler to look at the pyproject.toml than having to search through a huge uv.lock to find out which dependency version we are currently using.
Again, if we were to create a library, we would definitely used `uv sync --frozen` and `uv sync --upgrade` as you mentioned, but we are making an app and we, at least, find it easier to use `uv remove` and `uv add` to upgrade packages, and replace `>=` with `==` to avoid auto upgrades and avoid the need of a `.python-version` file.

---

_Comment by @AmmoniumX on 2025-04-10 13:34_

> I think this is what you should use the uv.lock lockfile for both when developing the application and deploying it.

This "forces" a dependency in uv for deployment, though. What if we just want uv for development, but still want to support an installation as simple as `python -m venv; pip install` or other tools? For this we'd want pyproject.toml to contain all necessary deployment requirements, as pyproject.toml is the "universal" file read by all tools

---

_Comment by @notatallshaw on 2025-04-10 13:36_

> This "forces" a dependency in uv for end users

Once [PEP 571 lock files](https://peps.python.org/pep-0751/) are widely implemented then you can use that lock file format instead and it won't force a dependency on uv.

---

_Comment by @charliermarsh on 2025-04-10 13:38_

Yes, though note that PEP 571 lock files won't have any effect on a workflow like `pip install application`. That would still use the published metadata (not the `pyproject.toml` as suggested above, but the published package metadata).


---

_Comment by @palotasb on 2025-04-10 17:38_

> > I think this is what you should use the uv.lock lockfile for both when developing the application and deploying it.
> 
> This "forces" a dependency in uv for deployment, though. What if we just want uv for development, but still want to support an installation as simple as `python -m venv; pip install` or other tools?

You're right, my suggestion did imply an `uv` dependency for deployment. You can sidestep this by generating a `requirements.txt` during development:

```shell
uv export >requirements.txt
```

And then for the deployment:

```shell
python3 -m venv .venv
./.venv/bin/pip install -r requirements.txt
```

> For this we'd want pyproject.toml to contain all necessary deployment requirements, as pyproject.toml is the "universal" file read by all tools

Having complete and pinned requirements as part of your built package's published [core metadata](https://packaging.python.org/en/latest/specifications/core-metadata/#requires-dist-multiple-use) is the way this is generally recommended for Python. A long-standing recommendation has been to separate your minimal requirements (traditionally `install_requires` in a `setup.py`) from your requirements files (or more recently, lockfiles). The Python Packaging User Guide has a good overview on this: https://packaging.python.org/en/latest/discussions/install-requires-vs-requirements/

Even with PEP 751 lockfiles implemented, the recommendation will be to keep your dependencies in `pyproject.toml` and your published package metadata minimal and abstract. Don't expect `pip install my-application==1.0.0` to be reproducible, it's not intended to be.

---

_Label `needs-decision` removed by @konstin on 2025-04-17 14:20_

---

_Assigned to @konstin by @konstin on 2025-04-17 14:20_

---

_Unassigned @zanieb by @konstin on 2025-04-17 14:20_

---

_Referenced in [astral-sh/uv#12946](../../astral-sh/uv/pulls/12946.md) on 2025-04-17 14:26_

---

_Referenced in [astral-sh/uv#13381](../../astral-sh/uv/issues/13381.md) on 2025-05-10 20:20_

---

_Referenced in [goauthentik/authentik#14469](../../goauthentik/authentik/pulls/14469.md) on 2025-05-11 00:40_

---

_Closed by @konstin on 2025-05-28 13:11_

---
