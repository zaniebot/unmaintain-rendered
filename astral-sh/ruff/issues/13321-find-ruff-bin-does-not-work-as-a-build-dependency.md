```yaml
number: 13321
title: find_ruff_bin does not work as a build dependency with pip
type: issue
state: closed
author: gaborbernat
labels:
  - question
assignees: []
created_at: 2024-09-10T23:33:20Z
updated_at: 2024-10-03T17:38:08Z
url: https://github.com/astral-sh/ruff/issues/13321
synced_at: 2026-01-12T15:54:53Z
```

# find_ruff_bin does not work as a build dependency with pip

---

_@gaborbernat_

See reproducible at https://github.com/gaborbernat/ruff-find-bin-during-pip-build/actions/runs/10802198412/job/29963805989

With code https://github.com/gaborbernat/ruff-find-bin-during-pip-build/commit/fd532e8eff45ce7c416d0563a52871ed7a0208e7

In this case, the host Python is the installer Python, but the expectation is to find it in the pip isolated environment 

cc @pradyunsg I am not sure if this is a pip bug or a uv bug... but the binary gets discovered correctly when installing with uv, but discovers in host Python when using pip.

cc @charliermarsh I believe this is also a bug for uv itself for find_uv_bin...

---

_Comment by @zanieb on 2024-09-11 20:47_

This doesn't seem like a Ruff bug? It looks like pip isn't installing the build requirement (`ruff`) before calling the hook. In uv, we install the build requirement into the isolated build environment. I'm not sure what pip is doing here or if it's intentional. I also tried enabling PEP 517 with the same result:

https://github.com/zanieb/ruff-find-bin-during-pip-build/commit/2cd719e36be250a82b9b670f63f585b89aceaccb
https://github.com/zanieb/ruff-find-bin-during-pip-build/actions/runs/10819384824/job/30017135074

---

_Label `question` added by @zanieb on 2024-09-11 20:47_

---

_Comment by @gaborbernat on 2024-09-11 22:02_

I do not believe what you say is correct. If you check the logs, you can see that Pip actually is installing the package:

> Processing /home/runner/work/ruff-find-bin-during-pip-build/ruff-find-bin-during-pip-build
>  Installing build dependencies: started
> Installing build dependencies: finished with status 'done'

And actually the stack trace confirms this:

```
         File "/home/runner/work/ruff-find-bin-during-pip-build/ruff-find-bin-during-pip-build/build.py", line 7, in prepare_metadata_for_build_wheel
          raise RuntimeError(find_ruff_bin())
                             ^^^^^^^^^^^^^^^
        File "/tmp/pip-build-env-dkih1szy/overlay/lib/python3.12/site-packages/ruff/__main__.py", line 36, in find_ruff_bin
          raise FileNotFoundError(scripts_path)
``` 
See ruff is installed into `/tmp/pip-build-env-dkih1szy/overlay/`.

The problem is that Pip does not use virtual environments for building packages, but rather isolated environments. And the discovery mechanism here assumes this is going to be a virtual environment.

I will leave it to you to argue with the PIP maintainers if PEP 517 requires a virtual environment or an isolated environment should suffice. Actually, the PEP explicitly calls it out that virtual environments not required: https://peps.python.org/pep-0517/#build-environment

> We do not require that any particular “virtual environment” mechanism be used; a build frontend might use virtualenv, or venv, or no special mechanism at all.

Which then makes it a ruff bug. `find_ruff_bin` must handle pips isolated environment.

You enabling PEP-517 is a no op because PEP-517 is enabled by default in pip.

---

_Comment by @zanieb on 2024-09-12 01:07_

What is the isolated environment you refer to? How are they creating it? Does it follow a specification somewhere? Can its root be discovered using `sysconfig`?

> You enabling PEP-517 is a no op because PEP-517 is enabled by default in pip.

Is this true? I think it's enabled by default sometimes, but not always? We get issues every day of people reporting different uv vs pip behavior because pip isn't using build isolation on their system.

Just wondering, why are you using Ruff as a build dependency?

---

_Comment by @charliermarsh on 2024-09-12 01:10_

> You enabling PEP-517 is a no op because PEP-517 is enabled by default in pip.

(In general, I don't believe this is true. It's enabled by default for `pyproject.toml`-based projects. It is not enabled by default for projects that only contain a `setup.py`. See https://github.com/pypa/pip/issues/9175. But this is a `pyproject.toml`, so yes, shouldn't be affected by the flag.)

---

_Comment by @gaborbernat on 2024-09-12 01:14_

> Is this true? I think it's enabled by default sometimes, but not always? We get issues every day of people reporting different uv vs pip behavior because pip isn't using build isolation on their system.

I think for all modern packages. Which is the case here too.

> Just wondering, why are you using Ruff as a build dependency?

Have an app that generates during build some code from a JSON schema, and the library uses ruff afterward to format the generated code.

> What is the isolated environment you refer to?

From the above stack trace:
```
/tmp/pip-build-env-dkih1szy/overlay/
```

> How are they creating it? Does it follow a specification somewhere? Can its root be discovered using `sysconfig`?

A pip maintainer would be able to answer these questions better than me. Note, that PEP-517 never requires the isolated environment to be sysconfig discoverable and I assume is not.

---

_Comment by @gaborbernat on 2024-09-12 01:23_

https://github.com/pypa/pip/blob/main/src/pip/_internal/build_env.py#L81-L139 I believe contains the implementation detail answers... perhaps a cheap solution would be to look for `ruff` binary on the last entry of `PATH`... FWIW.

---

_Comment by @zanieb on 2024-09-12 01:28_

Thanks for pulling that up. I think we've explicitly avoided finding the Ruff binary on the PATH because we can't really know that it's the correct Ruff executable associated with the module. I'm looking at the `site` code trying to figure out if there's a clear way to guarantee that.

---

_Comment by @gaborbernat on 2024-09-12 01:32_

FWIW using uv over pip fixes the issue, but some of our internal systems are not uv ready yet...

---

_Comment by @zanieb on 2024-09-12 02:21_

I guess I'd just recommend using a different strategy to find the Ruff binary in the meantime, like `shutil.which` — since you know in this situation it will be at the front of the path. I'm not sure this is an intended use-case for `find_ruff_bin`. If there was an obvious fix that retained the existing semantics I'd accept it though.

---

_Comment by @gaborbernat on 2024-09-12 02:31_

I don't control the package doing the generation and this method is name for solving this problem, needing another solution doesn't feels right...

---

_Comment by @pradyunsg on 2024-09-12 14:20_

Yea, pip's isolation is a weird setup that's not quite a virtual environment for $legacy reasons around how things worked with Python 2 and all that IIRC. I've wanted to clean that up for a while but haven't come around to it due to a lack of time.

I don't think you can get the information about the isolated environment's paths from `sysconfig`.

---

_Comment by @gaborbernat on 2024-09-20 15:09_

So @zanieb what is the action plan here?

---

_Comment by @gaborbernat on 2024-09-24 16:40_

@zanieb ping 😊 

---

_Comment by @zanieb on 2024-09-24 16:44_

Please don't ping us repeatedly — it's bad form.

I don't have an action plan here — it's a weird use case and there's not a clear solution. The upstream package should probably switch from using `find_ruff_bin` to read the `PATH` directly.

If there's a heuristic we can use to understand we're in a bespoke pip build environment, I'm all ears.

---

_Comment by @gaborbernat on 2024-09-26 16:51_

How about this close-enough approximation: 
- when all other discovery fails, 
- if the first entry on the `PATH` contains a folder named `pip-build-env-` (starting with) and within `overlay`
- a ruff executable is present on the first entry of `PATH`

We take a ruff from the first entry of `PATH`. If you agree with this I can create a PR.

If we really want to be safe, we can also check the presence of the `overlay` and `normal` entries at the end of `sys.path`. 

Thanks! 

---

_Comment by @zanieb on 2024-09-26 16:56_

It seems reasonable from a correctness perspective, but a bit dubious for long-term maintenance. I'm not sure. Let me see what other people on the team think.

---

_Comment by @gaborbernat on 2024-09-26 16:59_

FWIW `pip` at some point will switch over to using virtual environments, at which point we can remove it. (or pip will die and be replaced by `uv` at which point again we can remove it). So either way is a temporary heuristic to make it work in the current (inperfect) world.

---

_Comment by @charliermarsh on 2024-09-26 18:42_

I suppose I'm alright with it. (Is that a reliable heuristic on all platforms?)

---

_Comment by @gaborbernat on 2024-09-26 19:34_

Yes, following their implementation of the isolated build environment, this should work reliably on all platforms.

---

_Closed by @charliermarsh on 2024-10-03 17:38_

---

_Closed by @charliermarsh on 2024-10-03 17:38_

---
