```yaml
number: 8783
title: Add note on private classifier
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: konsti/private-project-note
created_at: 2024-11-03T20:37:17Z
updated_at: 2024-11-07T08:41:49Z
url: https://github.com/astral-sh/uv/pull/8783
synced_at: 2026-01-12T16:08:29Z
```

# Add note on private classifier

---

_@konstin_

Users are concerned that they might accidentally publish their private package to PyPI, thereby exposing company internals and their private source code.

However, for this to happen, the user needs to call `uv publish` without an index option, while having credentials for PyPI set either in environment variables (and the private index credentials not set to those env vars) or in the keyring (without scoping to a package), and that token having the project in scope.

As easiest protection, we recommend **never generating a PyPI token scoped to all projects**, only generating one that is matched to the specific, public project: Without a matching token, no accidental publishing can happen.

---

The `Private :: Do Not Upload` prevent packages from being uploaded to PyPI (https://pypi.org/classifiers/). This is unergonomic: The classifiers are a system for metadata, not for configuration, they are not part of uv's docs and it doesn't have the discoverability of regular settings.

The most evident idea for solving this is `tool.uv.publish`:

```
[tool.uv]
private = true
```

Unfortunately, this doesn't actually offer reliable protection: It only works when the pyproject.toml is present (we currently don't require checkouts for `uv publish`) and it doesn't work with twine or any other publishing tool.

The second idea is translating this to `classifiers = ["Private :: Do Not Upload"]`. Unfortunately, PEP 621 forbids this, it requires that we must not append to static parts of `[project]` when building the wheel and source dists. This is needed for the ability to read `pyproject.toml` without a build. There is an escape hatch that requires explicitly declaring specifiers as dynamic, we set the actual classifier on build depending on the value of `tool.uv.private`, i.e., if we see `tool.uv.private = true` we transform the metadata on build as if we had `classifiers = ["Private :: Do Not Upload"]`:

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

Just setting `classifiers = ["Private :: Do Not Upload"]` is, unlike the above, self-documenting and more concise.

---

Given all that we document that you shouldn't create unscoped tokens and that you can use `classifiers = ["Private :: Do Not Upload"]`.

Note that we cannot handle e.g. a GitLab repository that is set to public accidentally. It falls into the domain of registry vendors to have guardrails (e.g. private-by-default registries when the status of the source is unknown)

---

_Label `documentation` added by @konstin on 2024-11-03 20:37_

---

_Comment by @johnthagen on 2024-11-04 00:56_

As OP of #8214, this isn't what I was hoping for but I can understand your rationale and the constraints on `[project]` metadata mandated outside the scope of uv.

---

_Comment by @samypr100 on 2024-11-04 01:12_

> As OP of #8214, this isn't what I was hoping for but I can understand your rationale and the constraints on `[project]` metadata mandated outside the scope of uv.

It might be worth leaving #8214 open to explore the possibility of adding support for `[tool.uv] private = true`. While PEP 621 forbids appending to static parts of `[project]`, I wonder if `uv` can make an exception only in this situation for better alignment with user's expectations and other package managers (e.g. cargo).

---

_@charliermarsh reviewed on 2024-11-06 15:01_

---

_Review comment by @charliermarsh on `docs/guides/publish.md`:50 on 2024-11-06 15:01_

I think this should just be at the bottom of this section, and not as a note. (Is it really the most important content?)

---

_@konstin reviewed on 2024-11-06 15:21_

---

_Review comment by @konstin on `docs/guides/publish.md`:50 on 2024-11-06 15:21_

The trouble with placing this section is that it's a negative advice, it's about not doing something rather than doing something. Users with private packages that don't get published at all aren't likely to read through the publish guide, but I haven't found a place they would come through either.

---

_@zanieb reviewed on 2024-11-06 15:29_

---

_Review comment by @zanieb on `docs/guides/publish.md`:65 on 2024-11-06 15:29_

This doesn't quite make sense in the publishing guide, which is explicitly about how to publish your package. I guess I'll move towards having a publish concept document, but in the meantime, it can live here. It's a little awkward in the middle of this workflow, maybe it should be in `Preparing your project for packaging` as a `!!! tip`?

Could you add a sentence at the top of this section which explains what a private package is and why you'd want one?

---

_@konstin reviewed on 2024-11-06 15:55_

---

_Review comment by @konstin on `docs/guides/publish.md`:65 on 2024-11-06 15:55_

i moved it up again and changed word to "internal", that's self-explanatory.

---

_Comment by @konstin on 2024-11-06 15:59_

> It might be worth leaving https://github.com/astral-sh/uv/issues/8214 open to explore the possibility of adding support for [tool.uv] private = true. While PEP 621 forbids appending to static parts of [project], I wonder if uv can make an exception only in this situation for better alignment with user's expectations and other package managers (e.g. cargo).

I think it's obvious but I'll spell it out explicitly: If `[project] private = true` or something similar gets added to https://packaging.python.org/en/latest/specifications/pyproject-toml/ and/or https://packaging.python.org/en/latest/specifications/core-metadata/, I'm in favor such a PEP getting accepted and we'll support that uv (and the less ergonomic `Private :: Do Not Upload` docs). Until that however, I think we've done as much as we can on uv's side, and I consider #8214 "finished" in the sense that we gave solid guidance for users (this PR) and it's not blocking stabilization anymore (https://github.com/astral-sh/uv/issues/7839). If there's other things you want to see discussed around publishing and avoiding that accidents that were not covered by #8214, please feel free to open a new issue!

---

_@zanieb reviewed on 2024-11-06 16:01_

---

_Review comment by @zanieb on `docs/guides/publish.md`:18 on 2024-11-06 16:01_

Sorry but I think this still needs an explanatory sentence to start since we're currently in the middle of telling someone how to setup their project _to_ be published. For example:

> If you have internal packages that you do not want to be published, you can mark them as private with a classifier:
>
> ...
>
> uv will still attempt to publish packages with the classifier, but PyPI will reject them.




---

_@zanieb reviewed on 2024-11-06 16:01_

---

_Review comment by @zanieb on `docs/guides/publish.md`:19 on 2024-11-06 16:01_

What happens with other package indexes?

---

_@konstin reviewed on 2024-11-06 16:09_

---

_Review comment by @konstin on `docs/guides/publish.md`:19 on 2024-11-06 16:09_

They should allow the upload, but in general this is undefined, as in the OP:

> Note that we cannot handle e.g. a GitLab repository that is set to public accidentally. It falls into the domain of registry vendors to have guardrails (e.g. private-by-default registries when the status of the source is unknown)

---

_@zanieb reviewed on 2024-11-06 16:10_

---

_Review comment by @zanieb on `docs/guides/publish.md`:19 on 2024-11-06 16:10_

We should probably say that too then.

---

_@konstin reviewed on 2024-11-06 16:20_

---

_Review comment by @konstin on `docs/guides/publish.md`:18 on 2024-11-06 16:20_

I don't think that level of phrasing matters here

---

_Comment by @charliermarsh on 2024-11-06 16:22_

I defer to @zanieb entirely on the docs. Feel free to ignore my comments on that front.

IMO it's okay to leave https://github.com/astral-sh/uv/issues/8214 open. I _can_ imagine other ways for us to solve this problem that don't require standards. For example, we _could_ error when `private = false` is set in a `pyproject.toml` and require publishing to take place in the context of a project by default or something like that. Or, if we have our own build backend, we can write this into the distribution somewhere, and check it on `uv publish`.

---

_@zanieb reviewed on 2024-11-06 16:23_

---

_Review comment by @zanieb on `docs/guides/publish.md`:18 on 2024-11-06 16:23_

Which part?

Is it not a significant security concern in the issue that we attempt to publish the package?

---

_@konstin reviewed on 2024-11-06 16:29_

---

_Review comment by @konstin on `docs/guides/publish.md`:18 on 2024-11-06 16:29_

Why would a guaranteed-to-fail misconfigured publish attempt be a security concern? If you're really that scared about external sites, then you need to block pypi on a network level as some orgs do, and before that users already shouldn't run `uv publish` locally if you care about such aspects.

---

_@konstin reviewed on 2024-11-06 16:40_

---

_Review comment by @konstin on `docs/guides/publish.md`:19 on 2024-11-06 16:40_

I mean, sure, but isn't saying that you need to read the manual to your registry to operate it securely both evident and outside of the scope of uv? I don't think anyone reads the section as keeping the package private when you switch your GitLab registry setting to public. (And the registry I checked start with private by default at least for private repos and have extra steps before switching it to public).

---

_@zanieb reviewed on 2024-11-06 17:34_

---

_Review comment by @zanieb on `docs/guides/publish.md`:19 on 2024-11-06 17:34_

I don't think we need to talk about other indexes in depth here, but if we're going to recommend something we need to be clear about the caveats Just asking for something like "When not publishing to PyPI, the classifier may or may not be respected." 

---

_Review comment by @zanieb on `docs/guides/publish.md`:18 on 2024-11-06 17:37_

I mean, the package is still being sent to the host which is contrary to the expectation of a "do not upload" declaration. Their are users in the issue that have brought this up repeatedly. I don't quite understand the pushback on documenting this behavior. If the behavior was more obviously aligned with the expectations, I'd feel differently.

---

_@zanieb reviewed on 2024-11-06 17:37_

---

_@konstin reviewed on 2024-11-06 18:01_

---

_Review comment by @konstin on `docs/guides/publish.md`:18 on 2024-11-06 18:01_

Because the code being revealed by some SSL proxy(?) is a fairly hypothetical concern, and I value concise documentation, not documentation that hints users towards problems that do not exist.

---

_@zanieb reviewed on 2024-11-06 18:29_

---

_Review comment by @zanieb on `docs/guides/publish.md`:28 on 2024-11-06 18:29_

I'm sorry to continue picking at this, but I'm earnestly confused by the language here. "is used against PyPI" doesn't quite make sense, the classifier is sent to all indexes and only PyPI is expected to act on it right? What does it mean to configure access to the uploaded packages on an alternative registry? I thought the goal here was to avoid upload and publish entirely — I'm confused that we're talking about access controls now.

---

_Review comment by @zanieb on `docs/guides/publish.md`:18 on 2024-11-06 18:32_

Aren't MITM attacks a valid concern? Another concern that comes to mind for me is that you accidentally upload the package to some other index. I just want to be clear about the behavior here since this is basically a "security feature".

---

_@zanieb reviewed on 2024-11-06 18:32_

---

_Comment by @konstin on 2024-11-06 20:05_

Summary of the latest changes: I've shortened the explanation of `classifiers = ["Private :: Do Not Upload"]`: I see this option as a defense-in-depth, not something that you can rely on for specific behavior. There's also a note that this only affects pypi: Security and privacy of the overall source code and package handling in an organization are outside of what uv can do unfortunately (we're only a client, and we for example can't even tell if the page from a registry we're uploading to is private or public), so we try not to give a wrong impression how far this classifier goes.

---

_@zanieb approved on 2024-11-06 20:18_

---

_Comment by @johnthagen on 2024-11-06 20:30_

@zanieb @konstin Any chance to leave #8214 open as @charliermarsh mentioned for future exploration of a nicer UX around this?

- https://github.com/astral-sh/uv/pull/8783#issuecomment-2460224991

As of now "Fixes #8214" will close the issue.

---

_Comment by @zanieb on 2024-11-06 21:04_

I am fine with further discussion on #8214, though I think we're in agreement that the clearest path forward is an improvement at the Python standards-level.

---

_Merged by @konstin on 2024-11-07 08:41_

---

_Closed by @konstin on 2024-11-07 08:41_

---

_Branch deleted on 2024-11-07 08:41_

---
