---
number: 8214
title: Private Projects
type: issue
state: open
author: johnthagen
labels:
  - documentation
assignees: []
created_at: 2024-10-15T12:59:11Z
updated_at: 2024-12-22T13:45:40Z
url: https://github.com/astral-sh/uv/issues/8214
synced_at: 2026-01-07T13:12:17-06:00
---

# Private Projects

---

_Issue opened by @johnthagen on 2024-10-15 12:59_

Is it possible to mark a uv project as `private` such that `uv publish` is completely disabled? For private code this is a desirable feature.

Something like:

```toml
[tool.uv]
private = true
```

NPM has a similar `"private": true`:

- https://docs.npmjs.com/cli/v10/configuring-npm/package-json#private

> If you set `"private": true` in your package.json, then npm will refuse to publish it.
>
> This is a way to prevent accidental publication of private repositories. 

In Cargo, this is controlled by the [`publish` flag](https://doc.rust-lang.org/cargo/reference/manifest.html#the-publish-field).

> To prevent a package from being published to a registry (like crates.io) by mistake, for instance to keep a package private in a company, you can omit the [version](https://doc.rust-lang.org/cargo/reference/manifest.html#the-version-field) field. If you’d like to be more explicit, you can disable publishing:

```toml
[package]
# ...
publish = false
```

For some additional motivation, see a similar feature request for Poetry

- https://github.com/python-poetry/poetry/issues/1537

---

_Referenced in [johnthagen/python-blueprint#238](../../johnthagen/python-blueprint/issues/238.md) on 2024-10-15 13:01_

---

_Comment by @zanieb on 2024-10-15 14:05_

This exists already as a `Private :: Do Not Upload` [classifier](https://pypi.org/classifiers/)

---

_Comment by @zanieb on 2024-10-15 14:06_

This is mentioned over in https://github.com/python-poetry/poetry/issues/1537#issuecomment-1154642727 — we might want to include a private mode that avoids attempting to upload to the registry at all.

cc @konstin 

---

_Label `enhancement` added by @zanieb on 2024-10-15 14:06_

---

_Label `needs-decision` added by @zanieb on 2024-10-15 14:06_

---

_Comment by @johnthagen on 2024-10-15 14:27_

@zanieb Yeah, I personally feel like the `Private :: Do Not Upload` classifier is not 100% ideal and having something built in at the package manager level like NPM or Cargo would be more reliable (and also probably easier to teach).

---

_Comment by @konstin on 2024-10-15 14:47_

I'd love to support something like `tool.uv.private = true`. The only blocker for this is that currently, `uv publish` is independent of `pyproject.toml`: We will upload the files you hand `uv publish` and if you hand it a wheel, there's no `pyproject.toml` anymore. We also aren't allowed to set `"Private :: Do Not Upload"` for you when `tool.uv.private = true` is set, since we have to respect `project.classifiers`.

For now, we should start hinting users towards the `Private :: Do Not Upload` classifier in the docs. It's not the most elegant solution, but it's a reliable way to prevent accidental publishing.

---

_Comment by @zanieb on 2024-10-19 18:24_

> The only blocker for this is that currently, uv publish is independent of pyproject.toml: We will upload the files you hand uv publish and if you hand it a wheel, there's no pyproject.toml anymore.

I understand that in some contexts we may not know where the wheels come from, but I feel like we should still be able to read project metadata and configuration when publishing artifacts for builds in a project. This seems like a fairly important base capability for `uv publish`. 

>  We also aren't allowed to set "Private :: Do Not Upload" for you when tool.uv.private = true is set, since we have to respect project.classifiers.

How "not allowed" is this? Couldn't we append this when building the wheel and source dists?

---

_Comment by @mahyarmirrashed on 2024-10-19 18:33_

I agree with @zanieb on this. It's fairly important to protect users against shooting themselves in the foot when they accidentally attempt to publish something that is marked as private.
As a user, I can provide the following perspective/expected workflow.

1. If a project is created with `uv init`, add the `private = true` by default. The vast majority of projects are not public. Being proactive here brings less harm than the contrary;
2. If a project has the "Private :: Do Not Upload" classifier, warn the user that it must be removed in order to continue with publishing;
3. If a project is marked with `private = true` and the "Private :: Do Not Upload" classifier **is not** specified, warn the user that they should add it, link to https://pypi.org/classifiers/, and do not publish;
4. If a project is marked with `private = true` and the "Private :: Do Not Upload" classifier **is** specified, warn the user that it is set and therefore, do not publish.

This respects the apparent rule of not setting the classifier automatically while still protecting the user as much as possible. I strongly encourage not adding any `--override` parameters either. Those usually are a bad idea for users that blindly use them without understanding the consequences and reasons for their errors.

---

_Comment by @konstin on 2024-10-19 20:44_

There's two options to implementing private project: METADATA and pyproject.toml.

## Metadata

One option is going through the METADATA file and making sure `"Private :: Do Not Upload"` ends up in the source dist and wheel.

The non-uv solution is adding `classifiers = ["Private :: Do Not Upload"]` to your pyproject.toml:

```toml
[project]
name = "foo-internal"
version = "0.1.0"
classifiers = ["Private :: Do Not Upload"]
```

This prevents publishing to PyPI.

(There is a separate question that alternative registries such as GitLab have options to host packages both as public and private, but I'm skipping over that here.)

> How "not allowed" is this? Couldn't we append this when building the wheel and source dists?

PEP 621 is unambiguous that we must not append to static parts of `[project]` when building the wheel and source dists, this is required for the ability to read `pyproject.toml` without a build. There is an escape hatch that requires explicitly declaring specifiers as dynamic, we set the actual classifier on build depending on the value of `tool.uv.private`, i.e., if we see `tool.uv.private = true` we transform the metadata on build as if we had `classifiers = ["Private :: Do Not Upload"]`:

```toml
[project]
name = "foo-internal"
version = "0.1.0"
dynamic = ["classifier"]

[tool.uv]
private = true
```

Through `dynamic`, we are allowed to emit the following as METADATA:

```text
Metadata-Version: 2.3
Name: foo-internal
Version: 0.1.0
Version: Private :: Do Not Upload
```

I prefer `classifiers = ["Private :: Do Not Upload"]` over the above for being self-documenting and more concise.

## pyproject.toml

The other option is requiring pyproject.toml to be present.

To check for `tool.uv.private`, we need to require the pyproject.toml of the (forbidden to be) published packages to be present for _all_ `uv publish` invocations, for workspace packages this means having all workspace pyproject.toml. If we were to only check if the respective pyproject.toml is present, we would risk ignoring a `tool.uv.private`.

The other, bigger downside is that this will be ignored when using a standard workflow such as `python -m build && twine upload dist/*`: Twine, `@pypa/gh-action-pypi-publish` and other tools will publish this package to pypi with no regards to custom uv configuration, while the classifier protects no matter the tooling mix.

There are other advantages too for requiring pyproject.toml for publish, e.g., reading configuration from it. In the current trade-off, that doesn't tip the scales for me towards switching to a poetry-like `uv publish`, given that the currently implemented solution is a universally supported self-documenting one-liner:

```toml
[project]
name = "foo-internal"
version = "0.1.0"
classifiers = ["Private :: Do Not Upload"]
```

I agree that cargo and npm have a clearly better state, and would favor the addition of `project.publish` or `project.private` to the pyproject.toml spec.


---

_Comment by @johnthagen on 2024-10-20 00:24_

> The other option is requiring pyproject.toml to be present.

Is there a third option where _if_ pyproject.toml is present, then `uv publish` inspects it and if `private = true`  is there, it prevents publishing? So normal `uv` projects will respect it, but if you're using `uv publish` without a pyproject.toml, then it's back on you to set the classifiers and it works more like twine?

---

_Comment by @zanieb on 2024-10-20 14:09_

Yeah I agree, I don't think we need to _require_ it. We could always add a flag that requires we can find it; for people that are concerned about accidentally ignoring their configuration. 

---

_Referenced in [astral-sh/uv#7839](../../astral-sh/uv/issues/7839.md) on 2024-11-03 18:09_

---

_Referenced in [astral-sh/uv#8783](../../astral-sh/uv/pulls/8783.md) on 2024-11-03 20:37_

---

_Label `needs-decision` removed by @konstin on 2024-11-03 20:37_

---

_Label `enhancement` removed by @konstin on 2024-11-03 20:37_

---

_Label `documentation` added by @konstin on 2024-11-03 20:37_

---

_Comment by @ncoghlan on 2024-11-05 23:49_

I know packaging tool authors are generally against putting UX details into PEPs, but maybe this is a case where it would make sense to propose an optional `private = true` flag in the `project` table that fails the build if `classifiers = ["Private :: Do Not Upload"]` is missing from the classifier list? (@konstin considered a flag above, but in the context of reading it at publication time, rather than using it as a build time cross check on the classifier list)

That way as long as the project developers are running their CI builds with a version that respects the flag, publication tools will still see the classifier in the metadata.

---

_Comment by @johnthagen on 2024-11-06 12:18_

> an optional private = true flag in the project table that fails the build if classifiers = ["Private :: Do Not Upload"] is missing from the classifier list?

That is an interesting idea. The downsides I see to this are:

- Simplicity for the user. Now we need to teach users that ideally they need to state their intention that a project is private _twice_
- The `"Private :: Do Not Upload"` still seems like a non-ideal solution in the first place because it's only _after_ your code is shipped through an HTTPS connection over the network somewhere that _then_ it's blocked from being saved. I get that this is a bit pedantic, but in the presence of SSL proxies, etc it's possible that your code is revealed _somewhere_
  - You then also need to trust that whatever package repository you have configured (since it could be something other than PyPI like Artifactory - again in the very slim edge case) has properly implemented `"Private :: Do Not Upload"` and rejects it.
- What I like about `private = true` is that (just like NPM and Cargo) it can prevent _any_ transmission of the code from package managers that understand it

But I do like the overall idea of proposing that `private = true` be in a real PEP eventually. That could resolve some of the concerns that "well Twine will upload the `.whl` anyway". As Twine, Poetry, etc could also be updated to look for this common field in the current directory (or, bikeshedding, could look upwards for a `project.toml` in upper directories too) and then all refuse to upload.

---

_Comment by @ncoghlan on 2024-11-07 07:07_

> What I like about private = true is that (just like NPM and Cargo) it can prevent any transmission of the code from package managers that understand it

Yeah, that's one of the two main advantages I see in defining `project.private` formally. The other advantage is this:

> That could resolve some of the concerns that "well Twine will upload the .whl anyway".

If the invalid classifier is explicitly present in the metadata, even old versions of Twine (or other tools) won't *successfully* upload the package.

So even though people (or their dev tools) would need to express "this is private" twice, they'd be doing it for a specific backwards compatibility reason. (and if `uv init` accepted a `--private` flag, the dev themselves would still only be marking the package as private once)

---

_Comment by @pawamoy on 2024-12-22 13:25_

Use-case here!

I maintain private versions of my open-source projects (sponsorware strategy). In the past, I accidentally uploaded a private version to PyPI, which I then deleted when I noticed the mistake on pepy.tech (a few weeks later IIRC 😱).

I recently learned about `Private :: Do Not Upload` (classifiers starting with `Private ::`, really), and wondered if I could use it to prevent future accidents.

The thing is: although the private versions of my open-source projects must not end up on PyPI, my users must be able to publish them to their own private registries. I myself upload these private packages to a locally-served `pypiserver` instance.

The official docs (packaging guide, PEP 301) do not mention whether `Private ::` is a PyPI thing only, or if other registries should or should not (must or must not) support it.

Given my use-case above, I hope *they explicitly don't support it*, but cannot confirm that ([maybe you can?](https://github.com/astral-sh/uv/issues/10092)).

So, in my case, I think a `project.private = true` value in `pyproject.toml` would be ideal:

- it prevents me from accidentally publishing a private project to PyPI from my own machine
- it doesn't add the `Private ::` classifier to my artifacts, which could prevent upload on alternate/private registries
- and therefore lets users (including myself) upload built sdists and wheels to alternate/private registries
- ...even with `uv publish <artifact>`, since at that point, uv won't be able to see pyproject.toml, only built artifacts (unless it searches for it in the sdist: don't do that 😅!)

I realize `project.private` would need a PEP update, so it might take time, or never happen. It would also mean existing tools like Twine would have to start taking that into consideration, maybe (depending on how it is specified), something they could be against. In the meantime `tool.uv.private = true` would work too, given I use `uv publish` exclusively (I'm not, but I could).

Obviously, none of this would actually prevent someone from publishing the built artifacts to a *public registry other than PyPI*, or sharing the private code via other means, but this is another story 🙂 

---

_Referenced in [astral-sh/uv#10092](../../astral-sh/uv/issues/10092.md) on 2024-12-22 13:27_

---

_Referenced in [astral-sh/uv#10204](../../astral-sh/uv/issues/10204.md) on 2024-12-27 18:01_

---

_Referenced in [johnthagen/python-blueprint#95](../../johnthagen/python-blueprint/issues/95.md) on 2025-03-21 18:23_

---
