---
number: 8870
title: "Allow hiding \"Missing version constraint\" warning via configuration, or only warn for entire workspace"
type: issue
state: closed
author: vvuk
labels:
  - needs-decision
assignees: []
created_at: 2024-11-06T17:55:38Z
updated_at: 2025-02-23T23:59:03Z
url: https://github.com/astral-sh/uv/issues/8870
synced_at: 2026-01-07T13:12:18-06:00
---

# Allow hiding "Missing version constraint" warning via configuration, or only warn for entire workspace

---

_Issue opened by @vvuk on 2024-11-06 17:55_

The configuration:
- virtual workspace with A & B
- package A that depends on "torch"
- package B that depends on "torch==2.5.1"

doing a `uv sync` gives a warning about a missing version constraint for torch, I assume while it's processing A's dependencies. I'd like to hide these warnings, since they're just noise for my developers. I also tried with `--all-packages` by turning my workspace root into a non-virtual workspace and adding a `torch>1` dependency into the workspace root; but I still got the warning, so this seems like it's a per-package warning, not an overall workspace warning.

Alternatively, this request could be to wait until the entire workspace constraints are collected and only then print the warnings.

(I've got a bunch of pre-pyproject external python projects in a monorepo; I'm creating the pyproject to mirror their requirements.txt files exactly, so I'd rather go in and fix the dependencies in their individual pyproject files)

Also -- thank you for `uv`! It's brought a lot of sanity to my developers :)
Also also -- I'm happy to make a PR for stuff like this; just let me know if it's a PR you'd be OK with accepting

---

_Label `needs-decision` added by @charliermarsh on 2024-11-07 13:18_

---

_Comment by @charliermarsh on 2024-11-07 13:18_

Thanks for the kind words and the offer :)

I'm open to this but can I ask why you can't add a lower-bound on torch in package A? Is it inconvenient, or is it impossible?


---

_Comment by @konstin on 2024-11-07 14:05_

Another question for my understanding: Are the packages in the workspace intended to be published, or do the run without publishing (e.g. deploying in docker container)?

---

_Comment by @vvuk on 2024-11-07 17:36_

> I'm open to this but can I ask why you can't add a lower-bound on torch in package A? Is it inconvenient, or is it impossible?

It's not impossible, but very inconvenient -- since it's not just package A, it's package A...Z (currently around two dozen in our monorepo) :) Whenever we pull an update to one of those packages I've been just doing a `uv add -r requirements.txt` to pull in updated dependencies into our pyproject files (since those are not in the upstream packages).

Easiest thing would be to add a uv.tool config option for missing_version_constraint=ignore vs. warn; more complex would be to add the ability to warn only if the workspace is missing a constraint (which would be the most useful I think). I'd probably do the config first, and then extend it to missing_version_constraint=workspace or something to do the second.

---

_Comment by @ods on 2024-12-06 17:43_

Lower bounds may be useful for libraries, but they are entirely irrelevant for applications that always work with all packages pinned to exact versions in a lock file. In my typical application, I encounter around 50 warnings of this type, creating significant noise and increasing the likelihood of missing important warnings.

---

_Comment by @charliermarsh on 2024-12-06 17:47_

Why is a lower-bound irrelevant for an application?

---

_Comment by @ods on 2024-12-06 18:13_

An application (like microservice), in contrast to a library, does not need to work with multiple versions of its dependencies. The typical workflow involves updating some or all dependencies (manually or using a tool like Renovate), testing the application with the updated versions, and deploying if everything works fine. If issues arise, the problematic dependency version is restricted (typically using `!=`, rarely with `<`), and the process is repeated. Since updates generally involve moving forward rather than downgrading versions, lower bounds have no impact on the outcome.

The best practice is to justify every version restriction on dependencies. For a library, this typically involves specifying the oldest version that provides the required features or, at the very least, the lowest version tested in a matrix. For an application, however, there is no need to support older versions of dependencies, and testing in a matrix is often too resource-intensive. Consequently, there is little reason to test with older versions. If lower bounds are set, they often become arbitrary numbers that cannot be justified or guaranteed to work with the application. Alternatively, if the lower bounds are updated along with the versions in the lock file, they end up duplicating the lock file’s role.


---

_Comment by @zanieb on 2024-12-06 19:51_

> ...however, there is no need to support older versions of dependencies, and testing in a matrix is often too resource-intensive. Consequently, there is little reason to test with older versions. If lower bounds are set, they often become arbitrary numbers that cannot be justified or guaranteed to work with the application...

Unfortunately these lower bounds are important for resolution purposes. For example, prevent one package from being downgraded to an unsupported version when another is upgraded. They're also not entirely devoid of meaning, e.g., updating your lower bounds on semver boundaries is normal and comes with a set of expectations.



---

_Comment by @samsja on 2025-01-06 10:07_

This lower bound warning only happens when adding input in `tool.uv.sources`. Example

```
[tool.uv.sources]
torch-shampoo = { path = "third_party/optimizers", editable = true }
```

will trigger the lower bound warning even for dependencies that are not related to this source

---

_Comment by @MrMarvel on 2025-01-29 20:28_

I need workaround for that. (ignore warning: Missing version constraint (e.g., a lower bound) for `torch`)
Do not force people to do smth. Even if it's bad way to do that sometimes it makes someone happy.
I change poetry to uv because poetry has universal limitation to platform specific rules. They seems to me like putting sticks in a wheel.

---

_Comment by @samsja on 2025-01-29 20:31_

> I need workaround for that. (ignore warning: Missing version constraint (e.g., a lower bound) for `torch`) Do not force people to do smth. Even if it's bad way to do that sometimes it makes someone happy. I change poetry to uv because poetry has universal limitation to platform specific rules. They seems to me like putting sticks in a wheel.

I updated to the latest uv version and the issue does not appear anymore when doing `uv run ...` only doing `uv sync ` for which its probably not a real issue

---

_Comment by @MrMarvel on 2025-01-29 20:33_

I have an example from my project.
I repeat version 3x times. It's annoying. But it is another problem.
I just wanted to remove some of versions for optional dependencies but I face warning `Missing version constraint (e.g., a lower bound) for torch` when I call `uv sync` or `uv lock`
```Toml
[project]
name = "a"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = [
    "requests>=2.32.3",
    "torch>=2.6.0",
]

[project.optional-dependencies]
cpu = [
    "torch",
]
gpu = [
    "torch",
]

[tool.uv]
package = false
conflicts = [
  [
    { extra = "cpu" },
    { extra = "gpu" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "torch_cuda", extra = "gpu" },
  { index = "torch_cpu", extra = "cpu" },
]


[[tool.uv.index]]
name = "torch_cuda"
url = "https://download.pytorch.org/whl/cu124"
explicit = true


[[tool.uv.index]]
name = "torch_cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

---

_Comment by @MrMarvel on 2025-01-29 20:35_

[comment](https://github.com/astral-sh/uv/issues/8870#issuecomment-2622780914)
> I updated to the latest uv version and the issue does not appear anymore when doing uv run ... only doing uv sync  for which its 
> probably not a real issue

@samsja, I want to specificaly define property in my project (`setting`) so it hides warning for `sync` or `lock` (**FEATURE**)

---

_Comment by @ods on 2025-01-30 07:51_

I find this alias handy for solving the issue:
```shell
alias uv='2> >(grep -vF "Missing version constraint") uv --color=always'
```

---

_Referenced in [astral-sh/uv#11091](../../astral-sh/uv/pulls/11091.md) on 2025-01-30 11:58_

---

_Comment by @konstin on 2025-01-30 11:58_

@MrMarvel thank you for the reproducer, fixed in https://github.com/astral-sh/uv/pull/11091.

---

_Comment by @charliermarsh on 2025-02-23 23:59_

I believe we removed these.

---

_Closed by @charliermarsh on 2025-02-23 23:59_

---

_Referenced in [PufferAI/PufferLib#337](../../PufferAI/PufferLib/issues/337.md) on 2025-08-21 00:43_

---
