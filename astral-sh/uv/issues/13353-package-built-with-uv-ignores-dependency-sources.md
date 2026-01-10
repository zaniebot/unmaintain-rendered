---
number: 13353
title: Package built with uv, ignores dependency sources specified in tool.uv.sources
type: issue
state: open
author: mvechtomova
labels:
  - documentation
  - question
assignees: []
created_at: 2025-05-08T18:20:11Z
updated_at: 2025-07-07T08:42:23Z
url: https://github.com/astral-sh/uv/issues/13353
synced_at: 2026-01-10T01:25:32Z
---

# Package built with uv, ignores dependency sources specified in tool.uv.sources

---

_Issue opened by @mvechtomova on 2025-05-08 18:20_

### Summary

@charliermarsh 
We used tool.uv.sources to specify our dependency source (GitHub) and then built the package using uv build. 

I realized then when we tried to install the package somewhere else, the source from tool.uv.sources was ignored - instead, the package was downloaded from PyPi (and a package with that name existed too!)

If tool.uv.sources is used to specify our dependency source,  the installation of the wheel with dependencies will fail in the best case. In the worst case, the package with the same name might be available on PyPi. Then it will be installed instead of the package from the specified source (and may contain malware).

Not obvious and dangerous.

![Image](https://github.com/user-attachments/assets/2aeb1bd1-392a-413a-a795-9ceff6cc9a41)

### Platform

all

### Version

0.7.2

### Python version

3.11.9

---

_Label `bug` added by @mvechtomova on 2025-05-08 18:20_

---

_Label `bug` removed by @zanieb on 2025-05-08 18:27_

---

_Label `documentation` added by @zanieb on 2025-05-08 18:27_

---

_Label `question` added by @zanieb on 2025-05-08 18:27_

---

_Comment by @zanieb on 2025-05-08 18:28_

Where would you expect to see this highlighted?

---

_Comment by @zanieb on 2025-05-08 18:28_

(and separately, what build backend are you using?)

---

_Comment by @mvechtomova on 2025-05-08 18:36_

> (and separately, what build backend are you using?)

Setuptools build backend. I need to try another one to see whether the problem persists then.

---

_Comment by @mvechtomova on 2025-05-08 18:36_

> Where would you expect to see this highlighted?

Good question, I think this should be removed all together if this is a possibility that this behaviors occurs. I noticed you marked it as question. But it is too serious to just be a question.

---

_Comment by @zanieb on 2025-05-08 18:37_

> Setuptools build backend. I need to try another one to see whether the problem persists then.

No build backend will respect `tool.uv.sources`. It's both uv-specific, and development-only.

> I think this should be removed all together if this is a possibility that this behaviors occurs.

I think you're just misunderstanding the purpose of the feature.


---

_Comment by @zanieb on 2025-05-08 18:39_

The first paragraph of documentation for this feature is clear that it's for development https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-sources

And the next paragraph is clear that its purpose is to add support for things not covered by the `project.dependencies` standard (i.e., the _published_ metadata). 

---

_Comment by @mvechtomova on 2025-05-08 18:41_

> > Setuptools build backend. I need to try another one to see whether the problem persists then.
> 
> No build backend will respect `tool.uv.sources`. It's both uv-specific, and development-only.
> 
> > I think this should be removed all together if this is a possibility that this behaviors occurs.
> 
> I think you're just misunderstanding the purpose of the feature.

This is not clear at all that this is development-only. Hence it is dangerous.

---

_Comment by @mvechtomova on 2025-05-08 18:44_

> The first paragraph of documentation for this feature is clear that it's for development https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-sources
> 
> And the next paragraph is clear that its purpose is to add support for things not covered by the `project.dependencies` standard (i.e., the _published_ metadata).

Many people come from poetry, in poetry these are not development dependencies! So it is very logical to assume these are not development dependencies either. It is crucial to highlight the consequences of not being published metadata for the built wheel.

Since too many people may misunderstand it (and already do!), it creates serious vulnerability risks.

---

_Comment by @konstin on 2025-05-08 18:49_

> > Where would you expect to see this highlighted?
> 
> Good question, I think this should be removed all together if this is a possibility that this behaviors occurs. I noticed you marked it as question. But it is too serious to just be a question.

We're limited by the rules that PEP 621 imposes on the `[project]` table, a split declaration with fields that apply only locally otherwise wouldn't have been our choice. Given the current (external) constraints that apply, we're using the fields during uv development and document the behavior. We generally found it to be well received by users and is the underpinning for advanced features such as workspaces.

Note that indexes generally reject all non-index dependencies, so it is not possible to upload a package with a e.g. Git source to PyPI, independent of whether it's `[tool.uv.sources]` or in `[project]`.

With Poetry 2.0 and its switch to PEP 621, Poetry faces the same problem that source is used locally, but only the version constraint is published:

```toml
[project]
# ...
dependencies = [
    "requests>=2.13.0",
]

[tool.poetry.dependencies]
requests = { source = "private-source" }
```

---

_Comment by @mvechtomova on 2025-05-08 18:57_

> > > Where would you expect to see this highlighted?
> > 
> > 
> > Good question, I think this should be removed all together if this is a possibility that this behaviors occurs. I noticed you marked it as question. But it is too serious to just be a question.
> 
> We're limited by the rules that PEP 621 imposes on the `[project]` table, a split declaration with fields that apply only locally otherwise wouldn't have been our choice. Given the current (external) constraints that apply, we're using the fields during uv development and document the behavior. We generally found it to be well received by users and is the underpinning for advanced features such as workspaces.
> 
> Note that indexes generally reject all non-index dependencies, so it is not possible to upload a package with a e.g. Git source to PyPI, independent of whether it's `[tool.uv.sources]` or in `[project]`.
> 
> With Poetry 2.0 and its switch to PEP 621, Poetry faces the same problem that source is used locally, but only the version constraint is published:
> 
> [project]
> # ...
> dependencies = [
>     "requests>=2.13.0",
> ]
> 
> [tool.poetry.dependencies]
> requests = { source = "private-source" }

Quite often people do not upload packages to PyPi (as they are private) and just use GitHub link. Hence it is logical to include that in dependencies. Seems like poetry also does not look at tool.poetry.dependencies anymore:


> When both are specified, project.dependencies are used for metadata when building the project, > tool.poetry.dependencies is only used to enrich project.dependencies for locking.

> Alternatively, you can add dependencies to dynamic and define your dependencies completely in the tool.poetry > section. Using only the tool.poetry section might make sense in non-package mode when you will not build an sdist > or a wheel.

---

_Comment by @mvechtomova on 2025-05-09 06:31_

@zanieb @konstin I guess my main problem with this is that the way requirements are defined must be different for development and when the wheel must be built. Inconsistency by design.

In the case I shared it will not fail, but will cause some unexpected behavior. As dependency is specified in dependencies, it will actually be included in metadata - but not the source, hence if the package exists in PyPi, a different package will be installed. Something which is very hard to debug.

There is no way to isolate local development and build as it seems.

https://docs.astral.sh/uv/concepts/projects/dependencies/#build-dependencies. This documentation is also very unclear:

> By default, uv will respect tool.uv.sources when resolving build dependencies. For example, to use a local version of setuptools for building, add the source to tool.uv.sources

> When publishing a package, we recommend running uv build --no-sources to ensure that the package builds correctly when tool.uv.sources is disabled, as is the case when using other build tools, like [pypa/build](https://github.com/pypa/build).

The second kind of implies that uv build may use sources. 

---

_Comment by @JenspederM on 2025-05-11 09:52_

I will second @mvechtomova concerns. I was also very surprised the first time I encountered this issue, and I don't find the documentation to be clear on this. 

I wonder, why we couldn't just resolve the dependencies though. Since uv is effectively in control of the build process, why not just copy local dependencies into site-packages at build time?

@mvechtomova As for git dependencies, I've found that manually specifying these, as you would have in a requirements.txt, will actually resolve correctly. But I do find it unfortunate that this has to be done manually.

---

_Comment by @mavwolverine on 2025-06-10 22:17_

Me too. I came here looking for exactly the same thing.
I have a core project and an app project. When I do a `uv build` on the app project, I want it to run `uv build` on core, install it in app, then run `uv build` on app so that the final wheel contains all necessary packages for app.

---

_Comment by @konstin on 2025-06-11 20:43_

@mavwolverine That sounds like an unrelated problem, wheels only contain one project, independent of whether `tool.uv.sources` are used.

---

_Comment by @offsetcyan on 2025-06-22 21:38_

I've just encountered this, same as @mvechtomova, while trying to build wheels and it's a clear security risk. To reiterate mvechtomova's point: a malicious actor (say, working at an organisation) could intercept the build process with a name collision via PyPi and install malicious code from out-of-tree.

@zanieb post-hoc the leading sentence and trailing "important" paragraph in that section are clear, but I wouldn't have guessed this was the behaviour after reading that the first time. It's only after debugging unexpected dependencies while installing a wheel did I realise what was happening.

---

_Comment by @nathanpainchaud on 2025-07-04 17:52_

I understand that respecting the specified `tool.uv.sources` is not something uv can force on any build backend, since it relies on a uv-specific field in `pyproject.toml`.

However, I was wondering if it was something that could be supported when using the (newly released) `uv_build` backend? At first glance, it doesn't seem unreasonable to me for uv's own backend to be aware of uv-specific fields in `pyproject.toml`...

Or maybe I'm missing some trickier issues with python standards compliance that make this inadvisable, even for `uv_build`?

---

_Comment by @konstin on 2025-07-07 08:28_

Unfortunately, PEP 621 requires us to keep `project.dependencies` exactly as they were, so we're bound to that with `uv_build`.

---

_Comment by @nathanpainchaud on 2025-07-07 08:42_

> Unfortunately, PEP 621 requires us to keep `project.dependencies` exactly as they were, so we're bound to that with `uv_build`.

I was afraid that was the case, but thank you for the clear and direct answer 👍 

---

_Referenced in [aptible/unpage#18](../../aptible/unpage/pulls/18.md) on 2025-07-28 17:13_

---
