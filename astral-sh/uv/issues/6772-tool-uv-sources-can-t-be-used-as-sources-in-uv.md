---
number: 6772
title: "`[tool.uv.sources]` can't be used as `[sources]` in `uv.toml`"
type: issue
state: open
author: johnpyp
labels:
  - configuration
assignees: []
created_at: 2024-08-28T19:02:44Z
updated_at: 2025-12-20T17:25:00Z
url: https://github.com/astral-sh/uv/issues/6772
synced_at: 2026-01-10T01:24:05Z
---

# `[tool.uv.sources]` can't be used as `[sources]` in `uv.toml`

---

_Issue opened by @johnpyp on 2024-08-28 19:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

As mentioned in the docs, `tool.uv.sources` works fine for specifying sources overrides, but it seems that `[sources]` in `uv.toml` does not.

Confusingly, using `tool.uv.sources` and a `uv.toml` *doing other things* throws this warning:
```
warning: Found both a `uv.toml` file and a `[tool.uv]` section in an adjacent `pyproject.toml`. The `[tool.uv]` section will be ignored in favor of the `uv.toml` file.
```

However, it actually works fine to have it split among the two files.

Let me know if you need more to reproduce, but it should be straightforward as it's just a config file distinction for all forms of `[tool.uv.sources]`.

---
**Version:** uv 0.3.3
**Platform**: MacOS / Linux
**Command**: `uv sync`

---

_Label `bug` added by @zanieb on 2024-08-28 19:08_

---

_Label `configuration` added by @zanieb on 2024-08-28 19:08_

---

_Assigned to @charliermarsh by @zanieb on 2024-08-28 19:08_

---

_Comment by @charliermarsh on 2024-08-28 19:25_

Can you share an example of the exact setup you're trying to achieve?


---

_Label `bug` removed by @charliermarsh on 2024-08-28 19:25_

---

_Comment by @charliermarsh on 2024-08-28 19:27_

I believe sources need to be in the `pyproject.toml` alongside the dependencies they annotate.

---

_Comment by @johnpyp on 2024-08-29 22:12_

> I believe sources need to be in the pyproject.toml alongside the dependencies they annotate.

Yeah totally, this makes sense generally. The two things that are weird are:

1. it seems like `sources` is uniquely inconsistent with the rest of the `tool.uv` namespace, in that it doesn't support configuration in `uv.toml`, whereas basically every other property does.
2. Even if I keep `tool.uv.sources` in `pyproject.toml` but put the rest into `uv.toml`, it still gives that warning.

I'm just using `sources` for a normal path dependency, nothing special, but I started with using `uv.toml` as my configuration for `index-url` and `extra-index-url` - hence putting `sources` there too. These index urls are generated on-demand, not statically, so I don't want to put them into source control. Of course, that's another issue because I can't mix `pyproject.toml` and `uv.toml` generally, but that was the impotence to start using `uv.toml` in the first place.

---

_Comment by @colinjc on 2024-09-10 13:42_

I also have a use case for this. Internally we have a private registry that contains a number of shadowed package versions for legacy tools, which is causing the resolver to not find the OSS packages with the default index strategy. 

We could resolve it by setting the default strategy to unsafe-first-index in the global uv.toml we provide, but it would be more security friendly to include a list of overrides for the effected packages in sources instead. Long term we want to purge all of these shadowed packages, but this would really help our current efforts to adopt uv. 

---

_Comment by @charliermarsh on 2024-09-10 13:46_

@colinjc -- Can you not put that configuration in a `pyproject.toml`?

---

_Comment by @colinjc on 2024-09-10 15:05_

We can, but each project (dozens) would need to do it. Since our `uv.toml` is generated and shared automatically to all users to keep their private registry auth in sync, it would let us maintain a single list of dependencies to override.

It's not a huge deal for us, just a nice workaround while we hunt down the legacy projects shadowing dependencies when they really shouldn't be. 

---

_Comment by @johnpyp on 2024-09-10 16:46_

Echoing Colin's usecase - generated, often not source-controlled config is very common in large environments for things like registry configuration that may not be static.

In other ecosystems, we use git-untracked files like .npmrc, .cargo/config.toml, settings.gradle.

Tools which support multiple of these merged together are the *best* 🙏

---

_Comment by @charliermarsh on 2024-11-20 00:29_

I believe we intentionally don't support this and it will now error (instead of silently ignoring).

---

_Unassigned @charliermarsh by @charliermarsh on 2024-11-20 00:29_

---

_Comment by @befelix on 2024-11-22 07:35_

I would have another use-case for `sources` in `uv.toml`: My laptop doesn't have a GPU, so I generally want any `pytorch` package to be the cpu-only variant. While we can handle this for our local projects via `pyproject.toml`, if I install external dependencies directly into a fresh venv I have to manually pass the corresponding index url again. Adding the pytorch cpu-only index (without marking it at explicit) is not an option, since it has unintended side-effects unless we use an unsafe index strategy. Being able to explicitly pin `pytorch` to the cpu index globally via a `sources` field in the `uv.toml` would resolve that issue.


---

_Comment by @ipeevski on 2025-02-21 22:30_

Along the lines of what has been said before, this is my use case (that gives the warning anytime I run `uv`):

I want to specify torch sources/indexes (I don't mind where, preferrably in the `uv.toml` file, but currently it only allows in `project.toml`)
I want to specify my `cache-dir` to be on the same drive as my project (so hard links work)
I don't want to commit potentially sensitive information about where my cache-dir is and more to the point, it will not be a valid location for most people (as not everyone has a P: drive). 

So, currently, I don't see a way to reconcile those two requirements (my wants above)

---

_Comment by @charliermarsh on 2025-02-21 22:36_

Why can’t you put the cache-dir setting in a user-level uv.toml?

---

_Comment by @ipeevski on 2025-02-21 22:40_

> Why can’t you put the cache-dir setting in a user-level uv.toml?

I didn't know that's an option. It looks like that works and doesn't give the warning above, therefore that works for what I needed.
Thank you @charliermarsh 

---

_Referenced in [astral-sh/uv#11860](../../astral-sh/uv/issues/11860.md) on 2025-02-28 16:38_

---

_Comment by @orishamir on 2025-03-05 17:35_

> I believe we intentionally don't support this and it will now error (instead of silently ignoring).

May I add, that while having `[sources]` inside `uv.toml` of the project indeed yields an error stating `sources` field is not supported in a `uv.toml` file, it does not error in the case where `[sources]` appear in a `uv.toml` of system-level configuration (in _%SYSTEMDRIVE%\ProgramData\uv\uv.toml_ in windows at least).   
So the `[sources]` inside system-level configuration silently gets ignored.
A warning in that case would be helpful since it caused significant headaches 

_By the way - you are doing an incredible job with UV, I can't thank you enough!_

---

_Referenced in [astral-sh/uv#12013](../../astral-sh/uv/issues/12013.md) on 2025-03-06 19:14_

---

_Comment by @cylixlee on 2025-03-11 10:30_

My usecase is very close to what @befelix describes.

I'm writing a lib based on PyTorch on my laptop, which does not have GPU, and I configured the `[tool.uv.sources]` section with an explicit `pytorch-cpu` index. However, because the `[tool.uv.sources]` section can only be specified in `pyproject.toml`, I have to **comment out the section every time** I publish it to PyPI.

If the `[sources]` section is allowed in `uv.toml`, I can easily use `pytorch-cpu` index locally -- I can add `uv.toml` to `.gitignore` and there's no need to comment out those configuration again!

uv is the best package manager I've ever met and I appreciate your work so much, and I can't be too grateful if this feature is accepted😊.

---

_Comment by @konstin on 2025-03-11 11:04_

@cylixlee Have you seen https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-environment-markers? We added this feature specifically for that torch use case.

---

_Comment by @cylixlee on 2025-03-11 13:00_

@konstin Yeah, the markers is a good choice, however it may influence downstream: I personally use Windows+PyTorch CPU and Linux+GPU, however the downstream may not want to pair up like that...

I also tried the extras, but writing a lot of optional dependencies for every PyTorch-related lib is not a very pleasant work.😂

---

_Comment by @johnpyp on 2025-03-24 02:16_

@konstin For the pytorch specific usecase, I'm in a situation where I'm still eager for developer-specific `uv.toml` sources, mainly because using `extras` doesn't work very well for this. Some developers have AMD gpus, some NVIDIA gpus, etc. and we want to be easily accessible for them. As far as I can tell, even if I run `uv sync --extras rocm` (e.g for an AMD gpu), running `uv run ...` then tries to sync what it thinks is the default extras set. i.e the developer now has to specifc `--extras` for every single command they run...

`sys_platform` markers can't account for this unless I'm mistaken.

(This is a different motivation for this same thing than what I first described last year, just another one running into a similar flexibility issue)

---

_Referenced in [astral-sh/uv#12290](../../astral-sh/uv/issues/12290.md) on 2025-03-24 02:47_

---

_Comment by @charliermarsh on 2025-03-24 13:03_

(If you use `uv run` in lieu of `uv sync`, it will never remove any installed extras.)

---

_Referenced in [astral-sh/uv#12753](../../astral-sh/uv/issues/12753.md) on 2025-04-08 17:23_

---

_Referenced in [astral-sh/uv#13493](../../astral-sh/uv/issues/13493.md) on 2025-05-20 13:35_

---

_Comment by @amitavaghosh2 on 2025-05-28 07:40_

Off all this nonsense what i don't understand is, `uv add --active ../../commons` , does nothing and the solution is to `uv pip install -e` .
Python and its language is a different kind of broken than Java's XML horeshit.

---

_Comment by @rkechols on 2025-08-28 13:47_

I'd also like to put a vote in for being able to specify `tool.uv.sources` config outside of `pyproject.toml`. My use-case is also pytorch; some developer machines have GPUs, some don't, and it's independent of OS. To me the intuitive solution was to put `[[index]]` and `[sources]` entries in my `uv.toml` file, and I was surprised that the `[sources]` config was rejected.

Do we have any news whether this is on the roadmap?

---

_Comment by @zanieb on 2025-08-28 14:51_

It's not on the roadmap, no. Defining sources outside the `pyproject.toml` would invalidate the lockfile. We'd rather invest in things like being able to toggle sources based on the presence of a GPU.

---

_Comment by @ncoghlan on 2025-09-12 12:08_

I believe I just ran into this restriction trying to enable `uv` source overrides in `venvstacks`. That uses `uv pip compile` and `uv pip install` from input requirements files, with nary a `pyproject.toml` in sight.

There isn't any lock file to inadvertently invalidate in that usage model.

---

_Comment by @zanieb on 2025-09-12 13:20_

That's fair, but allowing it in that context but not other ones adds a bunch of complexity! If we choose to do so, we'll need to be careful about it.

---

_Comment by @ncoghlan on 2025-09-15 00:37_

Would it be viable to add a `pip.sources` table to constrain the non-project source definitions to just the use cases that aren't using `uv.lock`? (although that may be the extra complexity you're referring to) 

Independently of that question, I'll see if I can rework `venvstacks` to use pseudo-projects instead of requirements files.

---

_Referenced in [lmstudio-ai/venvstacks#253](../../lmstudio-ai/venvstacks/pulls/253.md) on 2025-09-15 06:29_

---

_Comment by @ncoghlan on 2025-09-15 07:01_

@charliermarsh In relation to https://github.com/astral-sh/uv/issues/6772#issuecomment-2487042845, does "it will now error" refer to attempting to specify `[sources]` in `uv.toml` at all, or more specifically to attempting to install a package that has an override specified in `uv.toml`?

Or is it only when `pyproject.toml` exists (and has a `tools.uv.sources` field?), so `uv pip compile` will never trigger it?

---

_Referenced in [lmstudio-ai/venvstacks#256](../../lmstudio-ai/venvstacks/issues/256.md) on 2025-09-16 05:00_

---

_Referenced in [astral-sh/uv#16117](../../astral-sh/uv/issues/16117.md) on 2025-10-03 17:41_

---

_Comment by @TheTerrasque on 2025-10-11 14:16_

> It's not on the roadmap, no. Defining sources outside the `pyproject.toml` would invalidate the lockfile. We'd rather invest in things like being able to toggle sources based on the presence of a GPU.

I just came over this problem and I believe this is unwise. The sources settings in relation to pytorch is clearly a system-wide and not project setting. You not only have gpu / cpu, but type of gpu and with nvidia version of cuda installed. Furthermore, as some have pointed out here there's many situations where it's organization wide settings and again not project-scope settings.



---

_Comment by @patrontheo on 2025-10-27 16:01_

> My usecase is very close to what [@befelix](https://github.com/befelix) describes.
> 
> I'm writing a lib based on PyTorch on my laptop, which does not have GPU, and I configured the `[tool.uv.sources]` section with an explicit `pytorch-cpu` index. However, because the `[tool.uv.sources]` section can only be specified in `pyproject.toml`, I have to **comment out the section every time** I publish it to PyPI.
> 
> If the `[sources]` section is allowed in `uv.toml`, I can easily use `pytorch-cpu` index locally -- I can add `uv.toml` to `.gitignore` and there's no need to comment out those configuration again!
> 
> uv is the best package manager I've ever met and I appreciate your work so much, and I can't be too grateful if this feature is accepted😊.

I have exactly the same use-case as @cylixlee and was hoping for the same solution (put the sources in `uv.toml` so that it applies locally only). If not possible in `uv.toml`, could it be in something like `pyproject.dev.toml`, that would get merged with the main pyproject when running `uv sync` ? Or do you see any other solution for that issue ? @zanieb 

---

_Referenced in [astral-sh/uv#16471](../../astral-sh/uv/issues/16471.md) on 2025-10-27 16:02_

---

_Comment by @Ruhrpottpatriot on 2025-12-19 09:35_

Adding to the people who would like this feature added.
I work in a company where we have some strict requirements which machine can access our internal network. However the projects I work on are sometimes developed in the field (quite literally) on machines which don't have access to the internal network. At that point we need to use local copies of our in-house packages.

Having the option to use the [sources] section in a uv.toml configuration would allow us to configure the machines we take with us to always use local copies and machines that are internal to always use our internal package registry. Much less hassle and much less error prone.

---

_Comment by @zanieb on 2025-12-20 10:54_

>  The sources settings in relation to pytorch is clearly a system-wide and not project setting. 

We already allow markers on sources, which are for system-wide changes https://docs.astral.sh/uv/concepts/projects/dependencies/#platform-specific-sources

> Having the option to use the [sources] section in a uv.toml configuration would allow us to configure the machines we take with us to always use local copies and machines that are internal to always use our internal package registry. 

Unfortunately, this would still change the resolution and consequently the `uv.lock` file though. I think we may need a different concept for your use-case. What do the dependencies look like in the online case? e.g., are they on a package index? or path dependencies on a NFS?

---

_Comment by @Ruhrpottpatriot on 2025-12-20 17:03_

> What do the dependencies look like in the online case? e.g., are they on a package index? or path dependencies on a NFS?

When working in-house they're included either from an internal package index, or git repository (in case we're still in the process of developing them). Both package index and git are internal only, this is a non negotiable requirement from our security department.

When we are in the field we use provisioned devices for that purpose and which get wiped after we're finished. At the moment we're cloning the required repositories to each device and then set `[tool.uv.sources]` to the local path of each package manually. For longer excursions we _may_ have an NFS, but usually this isn't the case. Sometimes we also have packages that come from project-partners which obviously are not giving us access to their internal network and usually give us code as a folder on a USB-Stick.


---

_Comment by @zanieb on 2025-12-20 17:07_

So for the Git dependencies, I presume the local path is just a checkout of the repository. For the index dependencies, do you similarly use a local clone of the source tree? Or do you put the sdist / wheel in a local directory?

---

_Comment by @Ruhrpottpatriot on 2025-12-20 17:14_

Usually it's a clone of the source tree for every (internal) dependency we have. I know that if we had wheels/sdists we could use a flat package index and iirc we did this in the past. However, we found that sometimes we have to extend/change packages that we internally pull from the index. That's very rare, but it happens occasionally. In-house this is no problem as we can change the packages and push a new version quickly, but since we don't have any access to the internal network (not even through a VPN) this isn't possible, so we usually clone the source for every internal package.


---

_Comment by @zanieb on 2025-12-20 17:25_

Yeah I was hoping we could just add a "mirror" concept for indexes which would let you fetch the artifact from a local directory instead of the remote index, but if you're changing to source trees that's not going to work.

---
