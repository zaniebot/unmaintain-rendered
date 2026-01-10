---
number: 1853
title: "`uv pip install` does not install `git+ssh://` dependency declared in `setup.cfg`"
type: issue
state: closed
author: SnoopJ
labels:
  - enhancement
assignees: []
created_at: 2024-02-22T03:50:40Z
updated_at: 2024-03-27T22:17:10Z
url: https://github.com/astral-sh/uv/issues/1853
synced_at: 2026-01-10T01:23:09Z
---

# `uv pip install` does not install `git+ssh://` dependency declared in `setup.cfg`

---

_Issue opened by @SnoopJ on 2024-02-22 03:50_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Platform: Ubuntu 22.04
`uv` version: 0.1.6 (da3a7ec8)

When checking that #1781 resolves the problem with cloning a private repository (and it does seem to), I encountered a new (to me) problem with resolving a `git` requirement declared in `setup.cfg`. Installing the requirement given in `setup.cfg` directly in the `pip install` invocation does succeed, so it seems likely this is a problem with understanding the project metadata.

## Reproduction

Run `uv pip install "acmelib @ /path/to/acmelib_repo"` to install a package on the local filesystem that declares a `git+ssh://` dependency in `setup.cfg` against a private GitHub repository.

Sharing below the best reproduction details that I can, with some of the names changed to sanitize the private stuff.

```
$ uv venv
Using Python 3.9.14 interpreter at /home/jgerity/.pyenv/versions/3.9.14/bin/python3.9
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
$ source .venv/bin/activate
$ uv pip install "acmelib @ /home/jgerity/acme/acmelib_repo"  # attempting a direct install of the parent package fails
error: Package `metrics` attempted to resolve via URL: git+ssh://git@github.com/acme/python-metrics.git@develop. URL dependencies must be expressed as direct requirements or constraints. Consider adding `metrics @ git+ssh://git@github.com/acme/python-metrics.git@develop` to your dependencies or constraints file.
$ grep "python-metrics" /home/jgerity/acme/acmelib_repo/setup.cfg
    metrics @ git+ssh://git@github.com/acme/python-metrics.git@develop
$ uv pip install "acmelib @ /home/jgerity/acme/acmelib_repo"
error: Package `metrics` attempted to resolve via URL: git+ssh://git@github.com/acme/python-metrics.git@develop. URL dependencies must be expressed as direct requirements or constraints. Consider adding `metrics @ git+ssh://git@github.com/acme/python-metrics.git@develop` to your dependencies or constraints file.
$ uv pip install -v "acmelib @ /home/jgerity/acme/acmelib_repo"
 uv::requirements::from_source source=acmelib @ /home/jgerity/acme/acmelib_repo
    0.005665s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: /tmp/.venv
    0.005922s DEBUG uv_interpreter::interpreter Using cached markers for: /tmp/.venv/bin/python
    0.005941s DEBUG uv::commands::pip_install Using Python 3.8.10 environment at /tmp/.venv/bin/python
    0.006449s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries 
 uv_resolver::resolver::solve 
      0.008116s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.8.10
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.008216s   0ms DEBUG uv_resolver::resolver Adding direct dependency: acmelib*
   uv_resolver::resolver::choose_version package=acmelib
        0.008311s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of acmelib @ file:///home/jgerity/acme/acmelib_repo (*)
 uv_resolver::resolver::process_request request=Metadata acmelib @ file:///home/jgerity/acme/acmelib_repo
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=acmelib @ file:///home/jgerity/acme/acmelib_repo
        0.008882s   0ms DEBUG uv_distribution::source Using cached metadata for acmelib @ file:///home/jgerity/acme/acmelib_repo
   uv_resolver::resolver::get_dependencies package=acmelib, version=2.1.0rc1+10.g83f70bea
     uv_resolver::resolver::distributions_wait package_id=de82c87359640496
error: Package `metrics` attempted to resolve via URL: git+ssh://git@github.com/acme/python-metrics.git@develop. URL dependencies must be expressed as direct requirements or constraints. Consider adding `metrics @ git+ssh://git@github.com/acme/python-metrics.git@develop` to your dependencies or constraints file.
$ uv pip install "metrics @ git+ssh://git@github.com/acme/python-metrics.git@develop"  # installing the requirement directly *does* succeed
 Updated ssh://git@github.com/acme/python-metrics.git (4a3a73f)                                                                                                      Resolved 42 packages in 1.34s
Installed 42 packages in 378ms
... package list ...
```

---

_Comment by @charliermarsh on 2024-02-22 04:13_

If I'm reading the trace correctly, I believe this is by design and indicated by the error message itself -- we require that all direct URL dependencies are declared as direct, and not transitive dependencies. If you install as an editable install, we'll pick them up as expected.

---

_Label `question` added by @zanieb on 2024-02-22 04:15_

---

_Comment by @SnoopJ on 2024-02-22 04:26_

Ah, maybe I've misunderstood `uv` as being more drop-in here than I'd thought. This is basically a show-stopper for my own org's interest in using the tool since we have enough such dependencies that declaring them in the same invocation is not going to happen. I would have found it helpful if the error message was more clearly telling me _"`uv` doesn't support that by choice"_.

Adding `--editable` to my invocation (and reworking the requirement to drop the `acmelib @` prelude) does pick up these requirements correctly, so I believe you're right, there is no bug here, just a surprising (to me) design.

---

_Comment by @zanieb on 2024-02-22 04:28_

We made this as a safety choice, but perhaps we can reconsider or allow opt-in to the relaxed behavior. Generally you'd put them all in a requirements file then install from there so you don't need to list them all in each invocation.

---

_Referenced in [astral-sh/uv#1855](../../astral-sh/uv/issues/1855.md) on 2024-02-22 04:34_

---

_Comment by @charliermarsh on 2024-02-22 04:35_

I also consider it a correctness issue. Imagine you declare a direct dependency on Flask at 3.0.0, but then some other package deep in the tree declares a direct URL dependency on some other Flask, that also resolves to a version called 3.0.0. What happens?

---

_Comment by @SnoopJ on 2024-02-22 05:01_

> I also consider it a correctness issue. Imagine you declare a direct dependency on Flask at 3.0.0, but then some other package deep in the tree declares a direct URL dependency on some other Flask, that also resolves to a version called 3.0.0. What happens?

My naive expectation would have been that there's some sort of first-past-the-post behavior there, but a quick check against what `pip` does suggests that this isn't the case, and the VCS requirement wins over the "plain" one. That makes some sense to me inasmuch as some dependency of mine may want `flask==3.0.0`, but maybe I want to install `flask==3.0.0+snoopj.fix` that has some patch I've applied, but which still satisfies their declared dependency. I'm sure someone with more archaeological knowledge of `pip` would have more insight on that behavior.

It seems that if `pip` is presented with two conflicting VCS requirements, it throws a `ResolutionImpossible` and brings the issue to the user's attention, which seems reasonable to me.

> Generally you'd put them all in a requirements file then install from there so you don't need to list them all in each invocation.

The project I have been most interested in pointing `uv` at (which has created most of my filed issues in one way or another) does actually have a requirements workflow, but it isn't really load-bearing in this way yet. That might end up being okay since my interest is less in `uv pip install` than in `uv pip compile` _anyway_, but I'm still assessing where the sharp edges of that speculative workflow are going to be once the "just bugs" stuff is sorted out.

As a user, I'd say that if `uv` has a different philosophy from `pip` about whether "transitive" (in the sense of _"it's in the project metadata"_ as distinguished from _"my project depends on something else that has a VCS dep"_) dependencies of this sort are allowed, it's definitely something I would want flagged to me as a fundamentally different behavior.

---

_Referenced in [astral-sh/uv#1808](../../astral-sh/uv/issues/1808.md) on 2024-02-22 14:49_

---

_Comment by @pablodz on 2024-02-26 16:16_

The current workaround will be using a old version of uv?

This version is working
```
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/uv/releases/download/0.1.5/uv-installer.sh | sh
```

---

_Comment by @charliermarsh on 2024-02-26 19:00_

Re-reading, I think `uv pip install "acmelib @ /path/to/acmelib_repo"` _should_ allow URL requirements that are defined in `acmelib`, and I'd consider this a limitation in the current implementation. What we _don't_ allow is if `acmelib` were itself a package hosted at a remote URL.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-26 03:07_

---

_Label `question` removed by @charliermarsh on 2024-03-26 03:07_

---

_Label `enhancement` added by @charliermarsh on 2024-03-26 03:07_

---

_Comment by @charliermarsh on 2024-03-26 03:08_

This will have the same fix as https://github.com/astral-sh/uv/issues/2643.

---

_Referenced in [astral-sh/uv#2671](../../astral-sh/uv/pulls/2671.md) on 2024-03-26 16:03_

---

_Closed by @charliermarsh on 2024-03-27 22:17_

---

_Closed by @charliermarsh on 2024-03-27 22:17_

---

_Referenced in [thinkingmachines/geowrangler#236](../../thinkingmachines/geowrangler/pulls/236.md) on 2024-07-04 11:02_

---
