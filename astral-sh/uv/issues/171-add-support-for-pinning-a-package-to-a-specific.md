```yaml
number: 171
title: Add support for pinning a package to a specific index
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - projects
assignees: []
created_at: 2023-10-23T15:59:07Z
updated_at: 2025-12-15T03:55:17Z
url: https://github.com/astral-sh/uv/issues/171
synced_at: 2026-01-12T15:58:22Z
```

# Add support for pinning a package to a specific index

---

_@charliermarsh_

Discussed this with Armin -- pip doesn't support it, and it seems like a big problem? If you have an internal index, but also want to get some packages from PyPI, there's no way to ensure that your internal packages come from your internal index. Packages on PyPI could even shadow them.

---

_Added to milestone `Initial release` by @charliermarsh on 2023-10-24 19:18_

---

_Label `enhancement` added by @charliermarsh on 2023-10-24 19:18_

---

_Label `wish` added by @charliermarsh on 2023-10-24 19:31_

---

_Comment by @charliermarsh on 2023-10-26 05:13_

https://github.com/pypa/pip/issues/8606

---

_Comment by @charliermarsh on 2023-10-26 05:16_

https://github.com/python-poetry/poetry/issues/6713

---

_Comment by @charliermarsh on 2023-10-26 05:34_

I don't quite understand why this is so hard (as per the pip issue), I can't tell if it's hard because it's a large conceptual change for pip specifically, if it's hard because it's pip is large and complicated and any changes are hard, or if there's inherent complexity.

---

_Comment by @charliermarsh on 2023-10-26 05:35_

Poetry's design is interesting: https://python-poetry.org/docs/repositories/. It feels a bit more complex than is necessary though.

---

_Comment by @groodt on 2024-02-19 02:43_

Please consider dependency confusion attacks: https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610

Use of `--extra-index-url` as they are presently used are a security vulnerability. 

[PEP 708](https://peps.python.org/pep-0708/) is a yet-to-be-implemented approach to improving the security posture.

---

_Comment by @charliermarsh on 2024-02-19 04:04_

I would love to implement improvements to this (like the ability to pin a dependency to a specific index)… We specifically held off and implemented indexes as-is to be spec-compliant. I’ll implement this as soon as it’s supported and there’s clarity on how installers should handle it!

---

_Comment by @groodt on 2024-02-19 04:16_

Makes sense. 

I think you may receive a lot of duplicate feature requests from the folks who do misuse `--extra-index-url` and who aren't aware that it is not currently intended to be used to append additional sources of dependencies, it's purpose is to provide a set of fallback mirrors of the primary index (`--index-url`) which is PyPI in the general case.

We may need to consider offering some help as mentioned in the PEP to move this along.

```
This PEP has been provisionally accepted, with the following required conditions before the PEP is made Final:

An implementation of the PEP in PyPI (Warehouse) including any necessary UI elements to allow project owners to set the tracking data.
An implementation of the PEP in at least one repository other than PyPI, as you can’t really test merging indexes without at least two indexes.
An implementation of the PEP in pip, which supports the intended semantics and can be used to demonstrate that the expected security benefits are achieved. This implementation will need to be “off by default” initially, which means that users will have to opt in to testing it. Ideally, we should collect explicit positive reports from users (both project owners and project users) who have successfully tried out the new feature, rather than just assuming that “no news is good news”.
```

In the short-term, if you don't want to bug-for-bug implement pip, we may need to point people at alternatives like https://github.com/uranusjr/simpleindex to help them merge indexes behind the scenes on localhost. I don't think many will like it 😂 

---

_Comment by @pawamoy on 2024-02-19 07:58_

@groodt can you point at official docs that confirm this use of extra index is indeed misuse? To me it fits well with local versions for example?

In any case, thanks for the links to simple index, this will be useful if case you're right and UV does not allow this use of extra index :) A bit sad to have to run two local servers instead of one, but that's not that bad.

---

_Comment by @pfmoore on 2024-02-19 08:12_

> I don't quite understand why this is so hard

From what I recall (I quickly refreshed my memory on the issue but didn’t review the details) it’s a “design decision that was made a long time ago” type of complexity. Pip has built a bunch of choices on top of the “all indexes are equal and get merged” model, and it’s hard to know what we’d need to change if we were to revisit that decision. Add to that the need for backward compatibility and it’s a much bigger question than people saying “just pick the index I ask for” will accept.

For uv, I foresee the following issues:

1. Compatibility - you’ll diverge from pip in both subtle and not-so-subtle ways. This isn’t a bad thing, but your stated intention is to be compatible with pip, so you need to make choices here.
2. Being an outlier - the “all indexes are equal” model is not just something pip does. Standards like PEP 708 are built on the idea, so you may have trouble fitting that standard into your implementation. More generally, you’ll have to make a bunch of UI and design issues where there is less pre-existing experience to draw on.
3. Scope creep - people start wanting things like index priorities (“get these from piwheels when you can, but fall back to PyPI if there’s nothing at all published on piwheels”) which have their own issues.
4. You open yourself up to the idea that a package is no longer identified by just name and version - where it came from becomes important as well. This may have wider implications - for example, SBOM data might need extra information like source URL.

On the plus side, you’ll be offering a solution to something users have been wanting for a *long* time, and which is often characterised as a security issue. And practicality may well beat purity here.

>  can you point at official docs that confirm this use of extra index is indeed misuse

It doesn’t describe it as “misuse”, but the pip docs are clear that we treat all indexes as equal: “There is no ordering in the locations that are searched” from [here](https://pip.pypa.io/en/stable/cli/pip_install/). We (pip) could add a guide-type article discussing this in more depth, but we don’t have one currently.

---

_Comment by @groodt on 2024-02-19 09:12_

There’s a small security warning in the pip docs [here](https://pip.pypa.io/en/stable/cli/pip_install/#examples)

`
Using this option to search for packages which are not in the main repository (such as private packages) is unsafe, per a security vulnerability called [dependency confusion](https://azure.microsoft.com/en-us/resources/3-ways-to-mitigate-risk-using-private-package-feeds/): an attacker can claim the package on the public repository in a way that will ensure it gets chosen over the private package.
`

There is also an in progress pip PR to make this more explicit here https://github.com/pypa/pip/issues/11694

Here’s a major recent dependency confusion attack that impacted PyTorch (caused by instructions to use —extra-index-url) https://news.ycombinator.com/item?id=34202662





---

_Comment by @pawamoy on 2024-02-19 09:25_

@groodt I'm not sure this is to answer my previous question ("can you point at official docs that confirm this use of extra index is indeed misuse"), but I was especially referring to "it's purpose is to provide a set of fallback mirrors of the primary index".

I distinguish three use-cases for extra indexes:

1. extend main index with additional projects (subject to dependency confusion)
2. extend specific projects in main index with additional versions (not subject to dependency confusion)
3. mirror PyPI.org 

My understanding of your "append additional sources of dependencies" was that it referred to case 2, but now I think you were speaking of case 1.

So, to rephrase, and given pip currently considers all indexes to be equal, is case 2 is a misuse too?

(Anyway, after reading PEP 708, I also agree it's the way forward and not index ordering, as I commented [here](https://github.com/astral-sh/uv/issues/1377#issuecomment-1951948003) :+1: )

---

_Comment by @pfmoore on 2024-02-19 09:26_

Stepping back from the question of "why is this so hard", @groodt is correct that PEP 708 is the better solution here. Otherwise, does the user need to specify an explicit index for the whole of their internal package's dependency tree? What if someone adds a new internal module, and forgets to add it to the list of "must come from the internal index" list in all of their install jobs? Are we going to consider that as "user error"? The torchtriton attack took exactly this form. Or an attacker could compromise a *public* dependency of your internal project.

Having an index pin option doesn't prevent you from needing to handle the consequences of a "all distributions with the same name and version are interchangeable" model. It just gives users a manual way of firefighting issues with that model.

---

_Comment by @pfmoore on 2024-02-19 09:51_

> So, to rephrase, and given pip currently considers all indexes to be equal, is case 2 is a misuse too?

It's not that "pip considers all indexes to be equal" but rather that "pip considers all distributions with the same name and version to be interchangeable *regardless of which index they came from*". The difference is subtle but important. Whether case 2 is a problem depends on whether you trust the "main index", just the same as with case 1. The trust issue is what's important here rather than what is "considered equal"[^1].

The `--extra-index-url` argument for pip was added long ago, in simpler times when people weren't anywhere near as worried about attackers targetting PyPI, and nobody was running multinational businesses based on Python code. Trust was generally assumed to be present, and in particular no-one was worrying about *relative* trust between indexes. Things have changed, for better or worse, and `--extra-index-url` doesn't match the new reality. This is why one of the options pip is considering is simply *removing* it, and demanding that users pick a single index and ensure that they trust that one index. That's unlikely to ever happen because the breakage would be significant, but it's absolutely an option that a new project like `uv` could (and should) consider.

To answer your question more explicitly, your case (2) is a "misuse" in the sense that it has risks (the same as case 1). The risks may not be a dependency confusion attack, but they do include compromise of the PyPI account owning of the code you're extending. Is that a more acceptable risk? Only you can decide that.

The point is that *mixing indexes with different trust levels*[^2] is the problem here. Even the "mirror PyPI" case involves a risk if the mirror is compromised.

[^1]: You could argue that if you don't trust two indexes equally, you don't "consider them equal", I guess...
[^2]: Within the same install command, which is why pinning for a set of named packages isn't a sufficient solution.

---

_Comment by @pawamoy on 2024-02-19 11:18_

Thanks @pfmoore!

(Side note: I just discovered that PDM supports respecting the order of indexes: https://pdm-project.org/latest/usage/config/#respect-the-order-of-the-sources.)

---

_Comment by @pfmoore on 2024-02-19 11:53_

Just as an example (and yes, it's a pathological case) if you have ordering, suppose you have index1 and index2 (ordered with 1 having priority over 2). Index 1 contains A 1.0 and B 1.0, with A 1.0 depending on B. Index 2 has B 2.0. If you install A, do you get B 1.0 or B 2.0? If the answer is 1.0, why did you bother specifying index 2? If you get B 2.0, and someone now adds A 2.0 to index 2, and changes B 2.0 to depend on A > 1.0, is the correct thing to upgrade A to 2.0, or downgrade B to 1.0?

The point here is that there's multiple possible choices, and if you don't factor index priority into the core of your resolution algorithm, you end up with a system that users won't have a good intuition about, and which might depend on implementation details. Both of which can lead to security issues. I'm not saying that PDM has such a problem - they may well have considered all of this. Just that "which index did this come from" is an extra axis you have to consider as part of resolution, *not* just something you can keep separate from the resolver.

Anyway, the key point for `uv`, and in answer to the question @charliermarsh asked above, is that *this* is why pip is still trying to work out a good answer to how we could allow users to choose which index to use at a more fine grained level than "per install".

---

_Comment by @pawamoy on 2024-02-19 13:09_

> If the answer is 1.0, why did you bother specifying index 2?

To be able to find package C, D, E, etc. :smile: In my case, all packages from index 1 take precedence, even if higher versions are available in index 2. Index 1 contains just a tiny few packages, index 2 contains all the rest (PyPI.org in my case). Well, `pypiserver` can redirect to a fall back index when a package isn't found, so a single index (the local `pypiserver` one) is enough for me, but if it didn't have this feature, I'd have to continue relying on index-url + extra-index-url, with the limitation that I cannot enforce packages to be fetched from one of the two.

PEP 708 will bring the same ability with finer-grain control (per-project fallback). I'll see if `pypiserver` maintainers are interested in supporting it :slightly_smiling_face: 

Could also be interesting to know how PDM handles ordering. If @frostming wants to chime in :smile: 

---

_Comment by @pfmoore on 2024-02-19 13:18_

> In my case, all packages from index 1 take precedence, even if higher versions are available in index 2.

Cool, so your approach to priority is like pinning, but with the decision on whether to pin being "if package A is in index 1, then pin to index 1, else fall through". That works, but it doesn't support the piwheels case where they *supplement* PyPI with wheels for the raspberry pi architecture, letting installers fall back to PyPI if the wheel isn't valid for the user's architecture (at least that's how I understand what they do...)

Getting into this much detail may be more than the `uv` maintainers want, though. Let's see if this is useful to them. I don't want to come across as saying that this is too hard to do - it's too hard for *pip* to easily do (which is what triggered my original comment) but `uv` may well have different priorities and choose different trade-offs. They also don't have backward compatibility to deal with (unless they want to match pip feature for feature).

---

_Assigned to @BurntSushi by @BurntSushi on 2024-02-26 15:51_

---

_Comment by @BurntSushi on 2024-02-29 12:52_

@pfmoore Thank you very much for your detailed comments here. They've been _super_ helpful.

I'd like to make a small tweak to how `uv` behaves today that will hopefully resolve at least some of the issues users are hitting (#1377, #1600, #1451) in practice, but do it in a way that doesn't make `uv` behave like `pip` (which, AIUI, is to consider all available packages from all indexes, without any guaranteed priority order).

So today, our implementation works by giving a preference order to the indexes made available to `uv`:

https://github.com/astral-sh/uv/blob/995fba8fec6ac6569e5f7ffe8d244faddef98692/crates/uv-client/src/registry_client.rs#L191-L210

That is, given `uv pip install --index-url foo --extra-index-url bar --extra-index-url quux package`, it will first try `foo`, then `bar` and then `quux`. Once a package is found, `uv` will stop looking for it in any other indexes. So for example, if `package` is found in `bar`, then that means it definitely wasn't found in `foo`, and it may or may not be in `quux`. We never check.

Since `--index-url` defaults to PyPI, that means something like `uv pip install --extra-index-url bar package` will check PyPI first for `package`, and if it's found, stop. I think this turns out to be the reverse of what the commonly desired behavior is. And while it won't address every use case, I think a nice stop-gap solution here would be to flip the preference order with respect to `--index-url` and `--extra-index-url`. That is, we check PyPI after all other extra index URLs given.

This will not match pip's behavior in every case, but I think it does help address some of the common cases and I think also helps to mitigate the dependency confusion concerns. That is, if `package` is in `bar`, then `uv` would completely ignore any packages named `bar` on PyPI. (Today, that's flipped. If `package` is on PyPI, then it will completely ignore any `package` on `bar`.)

Otherwise, I do agree that if we can get away with it, we should probably avoid encoding `pip`'s behavior of including packages from all indexes without regard to priority into `uv`. But we can certainly revisit this if my tweak above doesn't pan out.

And popping up a level, I do think we'll want to absolutely address the multi-registry issue by giving users more control when we build out more project management features. But I think until then, we'll probably want to avoid adding too many additional abstractions into `uv pip install` for dealing with multiple registries. And of course, I suspect we will want PEP 708 support eventually too.

---

_Comment by @pawamoy on 2024-02-29 13:01_

Nice! Worth noting is users who want the flipped behavior can do `uv pip install --index-url https://private-index.com/simple --extra-index-url https://pypi.org/simple`, right?

---

_Comment by @BurntSushi on 2024-02-29 13:02_

> Nice! Worth noting is users who want the flipped behavior can do `uv pip install --index-url https://private-index.com/simple --extra-index-url https://pypi.org/simple`, right?

Ah yes! I meant to call that out, but yes indeed.

---

_Comment by @vlad-ivanov-name on 2024-03-01 16:04_

While setting priority is good, in many cases it's only a bandaid while a proper solution would be enforcing association between specific packages and registries. This is not something that can be fixed server side; this has to be on the client, without any fallbacks or flexible behaviours. So if a package is not found in the registry, install (or any other operation) has to fail. This is to prevent supply chain attacks -- any other behaviour creates scenarios where due to intermittent problems (e. g. network) a malicous package can be installed.

---

_Comment by @BurntSushi on 2024-03-01 16:07_

> ... while a proper solution would be enforcing association between specific packages and registries.

I think this is roughly what I meant above with:

> And popping up a level, I do think we'll want to absolutely address the multi-registry issue by giving users more control when we build out more project management features.

---

_Comment by @vlad-ivanov-name on 2024-03-01 16:23_

Thank you for the reply. Do you think this feature could be implemented in API sooner than implementing the UX for it in the command line? `pixi` recently switched to `uv` as a Python backend, and as they don't implement `pip` UX/UI, just an API that would be more detailed than the current "flat" package source from multiple URLs would already enable it there.

---

_Comment by @atti92 on 2024-03-05 19:31_

I have a similar problem. 
**CASE A:**
- Lets say you have a PKG with 1 yanked version available in UV_INDEX_URL or pypi.org
- You have the same package in your UV_EXTRA_INDEX_URL, but with valid versions.

1. when PKG is a dependency of something that we try to install, it works with latest uv version. :ok: 
2. trying to install the PKG directly, gives an error: :no_entry: 
```
error: Missing `Content-Type` header for <EXTRA_INDEX_URL>/PKG
```

**CASE B:**
- When you have multiple indexes in UV_EXTRA_INDEX_URL and the PKG is in the first extra:
1) same behaviour as with just 1 extra  :no_entry: 

**CASE C:**
- When the PKG is not in the first extra, but in subsequent extra indexes:
1) installation is not possible, it only looks for the package in the UV_INDEX_URL  :no_entry: 
```
× No solution found when resolving dependencies:
  ╰─▶ Because only PKG==0.0.0a1 is available and PKG==0.0.0a1 is unusable because it was yanked (reason: not stable), we can conclude that all versions of PKG cannot be used.
      And because you require PKG, we can conclude that the requirements are unsatisfiable.

      hint: PKG was requested with a pre-release marker (e.g., any of:
          PKG<0.0.0a1
          PKG>0.0.0a1
      ), but pre-releases weren't enabled (try: `--prerelease=allow`)
```


---

_Comment by @charliermarsh on 2024-03-05 19:33_

What's the case in which you're seeing `error: Missing `Content-Type` header for <EXTRA_INDEX_URL>/PKG`? That seems like a bug in the registry, but perhaps we can handle it gracefully.

---

_Comment by @atti92 on 2024-03-05 19:37_

> What's the case in which you're seeing `error: Missing `Content-Type` header for <EXTRA_INDEX_URL>/PKG`? That seems like a bug in the registry, but perhaps we can handle it gracefully.

- UV_INDEX_URL is undefined.
- UV_EXTRA_INDEX_URL points to a NEXUS instance with pypi repo

It only happens if I try to install a package that is available in both pypi.org and in the nexus.
Installing packages that are only in the nexus or pypi works.
Also it only happens when installing the package directly with `uv pip install PKG`

---

_Comment by @charliermarsh on 2024-03-05 19:41_

I think you want https://github.com/astral-sh/uv/issues/1754.

---

_Comment by @atti92 on 2024-03-05 19:47_

Yes I can confirm using `--no-cache` removes the error message.

Do you know anything about using multiple extra-index-urls, is that supposed to work?
it only resolved the package if it was in the first one, tried --extra-index-url and env too.

we have multiple gitlab registries, on top of the nexus and pypi, and we need to install many packages from all the different repositories.

---

_Comment by @BurntSushi on 2024-03-05 19:56_

> Do you know anything about using multiple extra-index-urls, is that supposed to work?
> it only resolved the package if it was in the first one, tried --extra-index-url and env too.

See https://github.com/astral-sh/uv/pull/2083

---

_Comment by @atti92 on 2024-03-05 20:00_

> > Do you know anything about using multiple extra-index-urls, is that supposed to work?
> > it only resolved the package if it was in the first one, tried --extra-index-url and env too.
> 
> See #2083

My package is only available in one of the extra-index-urls outside of index-url, but it still cannot find it, if it's not the first.

---

_Comment by @BurntSushi on 2024-03-05 21:00_

> > > Do you know anything about using multiple extra-index-urls, is that supposed to work?
> > > it only resolved the package if it was in the first one, tried --extra-index-url and env too.
> > 
> > 
> > See #2083
> 
> My package is only available in one of the extra-index-urls outside of index-url, but it still cannot find it, if it's not the first.

It might be worth opening a new issue with a repro.

---

_Comment by @atti92 on 2024-03-05 21:24_

It seems like gitlab is doing an automatic pypi lookup:
https://docs.gitlab.com/ee/user/packages/pypi_repository/#install-a-pypi-package
```
In [GitLab 14.2 and later](https://gitlab.com/gitlab-org/gitlab/-/issues/233413), when a PyPI package is not found in the package registry, the request is forwarded to [pypi.org](https://pypi.org/).
```
Probably that's why it finds the yanked version again and stops looking at the subsequent extra-index-urls
gonna comment on this, as that seems the most relevant issue: https://github.com/astral-sh/uv/issues/2205

---

_Comment by @charliermarsh on 2024-04-03 23:29_

The next version of uv will include an opt-in flag to allow users to search for packages across multiple indexes. This is less secure, but closer to pip's behavior.

https://github.com/astral-sh/uv/pull/2815

---

_Comment by @vlad-ivanov-name on 2024-04-04 07:39_

Since there's a flag and the logic behind it already, perhaps it could accept something like `--index-strategy=pin --index-pin=pytest=pypi` 👀 although ideally this should go in requirement files

---

_Unassigned @BurntSushi by @konstin on 2024-07-09 12:38_

---

_Comment by @smphhh on 2024-08-21 13:42_

FWIW I quite like the feature in PDM that allows one to pin all packages with a specific prefix to a particular index, see: https://pdm-project.org/latest/usage/config/#specify-index-for-individual-packages. This allows easy pinning of all company internal dependencies (including transitive ones) to a private repository as long as you are able to name all the packages using some unique-enough prefix.

---

_Comment by @corleyma on 2024-08-22 19:44_

@smphhh me too, and I've used poetry plugin to accomplish something similar.  I know it's funky but it's a big 80/20 for corporate usecases.  It solves the problem of "if I pin a dependency to an index, from which index do I get the transitive dependencies?" for a narrow but useful set of cases.

---

_Comment by @zanieb on 2024-08-23 18:02_

Removing with `wish` label here, as we expect to implement this in `tool.uv.sources`

---

_Label `wish` removed by @zanieb on 2024-08-23 18:02_

---

_Label `projects` added by @zanieb on 2024-08-23 18:03_

---

_Removed from milestone `Feature complete` by @zanieb on 2024-08-23 18:03_

---

_Comment by @colinjc on 2024-09-05 19:07_

Current uv docs for dependencies make it sound like this feature is already available. Let to a bit of churn trying to figure out how to use it 😅 

https://docs.astral.sh/uv/concepts/dependencies/


> tool.uv.sources enriches the dependency metadata with additional sources, incorporated during development. A dependency source can be a Git repository, a URL, a local path, or an **alternative registry**.


---

_Comment by @msabramo on 2024-09-13 13:57_

Does uv not expand environment variables in the `extra-index-url` setting in `pyproject.toml`? I couldn't get this to work:

```toml
[tool.uv]
# uv doesn't expand ARTIFACTORY_USER and ARTIFACTORY_API_TOKEN?
extra-index-url = ["https://${ARTIFACTORY_USER}:${ARTIFACTORY_API_TOKEN}@pythonpackages.corp.example.com/artifactory/api/pypi/pypi-colorado-tools-snapshot/simple"]
```

It _does_ work if I set `UV_EXTRA_INDEX_URL` in the shell from which I invoke `uv`, presumably because the shell is expanding the environment variables.

And as a point of reference, we are currently using pdm and this works:

```toml
[[tool.pdm.source]]
name = "colorado-tools-snapshot"
url = "https://${ARTIFACTORY_USER}:${ARTIFACTORY_API_TOKEN}@pythonpackages.corp.example.com/artifactory/api/pypi/pypi-colorado-tools-snapshot/simple"
include_packages = ["venice", "venice-*"]
```

The packages in `include_packages` are proprietary packages, so they only exist in this internal Artifactory and not on PyPI. There's no mirroring and no choice of where to get them from; they have to come from a specific Artifactory package index.

Great work on uv!

---

_Comment by @vlad-ivanov-name on 2024-09-13 14:03_

this is probably something that could be solved via keyring and --keyring-provider=subprocess (and is also unrelated to this issue)

---

_Comment by @zanieb on 2024-09-14 03:14_

Please comment on https://github.com/astral-sh/uv/issues/5734 instead for environment variable expansion.

---

_Comment by @charliermarsh on 2024-09-18 04:06_

This is starting to come together in https://github.com/astral-sh/uv/pull/7481.

---

_Comment by @samypr100 on 2024-09-20 01:49_

> Poetry's design is interesting: https://python-poetry.org/docs/repositories/. It feels a bit more complex than is necessary though.

Here's an interesting pattern w/ poetry I've seen used to allow local dev on osx with pytorch but also enable GPU usage on linux specifically on x86_64.

```
[tool.poetry.dependencies]
...
torch = [
    ...
    { markers = "sys_platform == 'darwin' and platform_machine == 'arm64'", url = "https://download.pytorch.org/whl/cpu/foo_bar_cpu_wheel.whl"},
    { markers = "sys_platform == 'linux' and platform_machine == 'aarch64'", url = "https://download.pytorch.org/whl/cpu/foo_bar_cpu_wheel.whl"},
    { markers = "sys_platform == 'linux' and platform_machine == 'x86_64'", url = "https://download.pytorch.org/whl/cu121/foo_bar_gpu_wheel.whl"}
]


---

_Comment by @groodt on 2024-09-20 23:27_

Good news! 🎉

[Initial PEP 708 support has arrived on pypi](https://discuss.python.org/t/pep-708-extending-the-repository-api-to-mitigate-dependency-confusion-attacks/24179/94)



---

_Closed by @charliermarsh on 2024-10-15 22:24_

---

_Closed by @charliermarsh on 2024-10-15 22:24_

---

_Comment by @k0t3n on 2024-10-21 11:26_

What about CLI? Will uv support adding dependencies with custom registries via `uv add`?

I with I could do 
```shell
uv add --index internal somepackage
```

---

_Comment by @gazpachoking on 2024-10-21 13:54_

> What about CLI? Will uv support adding dependencies with custom registries via `uv add`?

It looks like #7747 sorta does that. It doesn't allow just giving the index name though, you have to give the name and url. `uv add --index internal=https://internalindex.url somepackage` Allowing just using an existing index name would be a nice improvement.

---

_Comment by @matteius on 2025-12-15 03:55_

fwiw, this is something pipenv actually does right -- we patch pip to force index restricted packages.   I was exploring what a uv resolver backend for pipenv would look like tonight and ran across this closed issue.

---
