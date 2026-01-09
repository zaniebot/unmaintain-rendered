---
number: 13827
title: "Add support for updating the version of arbitrary files in `uv version —bump`"
type: issue
state: open
author: HenriBlacksmith
labels:
  - enhancement
assignees: []
created_at: 2025-06-03T22:37:36Z
updated_at: 2025-11-03T22:47:20Z
url: https://github.com/astral-sh/uv/issues/13827
synced_at: 2026-01-07T13:12:18-06:00
---

# Add support for updating the version of arbitrary files in `uv version —bump`

---

_Issue opened by @HenriBlacksmith on 2025-06-03 22:37_

### Summary

`uv version —bump`  currently supports updating project version in `pyproject.toml` and `uv.lock`.

In many projects other files need to be bumped (like a `Chart.yaml` file).

Currently my team uses tools like bump-my-version or changie. Those allow to configure search/replace patterns to update the version string in any file.

They also provide configuration parameters to add a git commit and/or a git tag with the version bump changes.

It would be interesting for uv to support that. To avoid introducing other tools and wrapper scripts around uv in release workflows.

### Example

TBA

---

_Label `enhancement` added by @HenriBlacksmith on 2025-06-03 22:37_

---

_Comment by @nrlulz on 2025-06-10 18:05_

I would also be interested in the git commit/tag functionality. We've used [bump2version](https://github.com/c4urself/bump2version) for many years in our python (and other) projects, but I have just learned that this tool is no longer maintained, and I am having a hard time getting it to work with the uv lockfile.

I don't know if this makes a good argument for adding this responsibility to uv or not, but JS projects get commits/tags (but not search/replace) natively using [`npm version`](https://docs.npmjs.com/cli/v11/commands/npm-version#description).

Another (less opinionated) idea would be some kind of hook that gets called with the current and new versions, then people could write their own scripts to search/replace, commit, tag, whatever.

---

_Comment by @HenriBlacksmith on 2025-06-11 19:10_

You can reverse the wrapping to by  `uv version` wrapped in a script which also uses git and bump-my-version (which is still maintained and replaces bump2version). 

But having uv doing it all would greatly help standardizing stuffs and IMO that's the role of a project management tool.

It can probably be done step by step, by first allowing simple patterns (like files just containing the version) and allowing more complex implementations in later versions.

---

_Comment by @konstin on 2025-06-11 19:50_

CC @Gankra 

---

_Comment by @saroad2 on 2025-06-12 18:10_

Some implementation suggestions, at least for my own use-case. Feel free to take from it the things that make sense to you 😄:

# `uv version` needed capabilities

When using `uv version --bump <something>`, there should be a possibility to:
- Commit the changed files (currently this is only `pyproject.toml` and `uv.lock`)
- Create a new git tag with the new version in the format "vX.X.X". When choosing to use tagging, it automatically means to commit the updated
- Support changing the version in other files as well (`.version`, `Chart.yaml`, etc.)

All capabilities should be optional and support opt-out.

# Setting via `pyproject.toml`
One should be able to opt in/out for the above capabilities using the following configuration in `pyproject.toml`

```toml
[tool.uv.versioning]
commit = true/false
tag = true/false
version_files = [
    ".version",
    "Chart.yaml",
    "src/my_package/__version__.py",
    ...
]
```

As states above, when `tag = true` it means that `commit = true` automatically, even if not stated in the config.
If we set both `tag = true` and `commit = false`, an error message should be shown.

# Support via command line
In addition to the setting of these flags via the config file, we should also be able to override the config definitions by adding flags to the command line:
- `uv version --bump <something> --commit/--no-commit`
- `uv version --bump <something> --tag/--no-tag`
- `uv version --bump <something> "--version-files=src/my_package/__init__.py,Chart.yaml"`

-------
Please let me know if this all make sense.
I can try to implement some of it, if approved by maintainers 😃 

---

_Comment by @HenriBlacksmith on 2025-06-12 19:05_

I will probably develop later on but looks like integrating the whole bump-my-version logic into uv including similar configuration formats would be my expectation.

I am not sure wether there are legal issues with that but uv already replaces a few Python ecosystem products, hence I assume it is fine and expected.

The goals is to allow search/replace patterns (not just listing version files) and also to provide patterns for the tag and commit messages (and opt-in/out for the tag/commit)

It could even go a bit farther with changelog and GitLab/Hub etc.  releases but looks a bit too much for uv scope though.

---

_Comment by @Gankra on 2025-06-12 19:10_

Additional prior art can be found in [cargo-release's config](https://github.com/crate-ci/cargo-release/blob/master/docs/reference.md#pre-release-replacements)
  * [as used in cargo-dist](https://github.com/astral-sh/cargo-dist/blob/2b950a01358adce549fc77222d77ecb7f7279a6f/cargo-dist/Cargo.toml#L86-L89)
  * [example of it going off](https://github.com/astral-sh/cargo-dist/commit/33d6179af319d1a87c2b2c841c8267a0ae0c08d7)


---

_Comment by @Gankra on 2025-06-12 19:11_

In general I think this is a good idea and we should eventually support it but it's a *big* project in terms of config design, so I'm not sure we can prioritize it right now.

---

_Referenced in [astral-sh/uv#14644](../../astral-sh/uv/issues/14644.md) on 2025-07-16 00:37_

---

_Referenced in [astral-sh/uv#14707](../../astral-sh/uv/issues/14707.md) on 2025-07-18 11:37_

---

_Comment by @leandrodamascena on 2025-07-18 11:39_

> In general I think this is a good idea and we should eventually support it but it's a _big_ project in terms of config design, so I'm not sure we can prioritize it right now.

+1

---

_Comment by @HenriBlacksmith on 2025-08-06 11:40_

> In general I think this is a good idea and we should eventually support it but it's a *big* project in terms of config design, so I'm not sure we can prioritize it right now.

Are they any improvements planned for the version commands?

A small path forward could be to support updating a file (or files) containing just the version string like a VERSION file.

This allows easy reuse for other CI steps which do not embed uv

`$(cat VERSION)` becomes them a handy way to get the version 

This is really small as it just saves:
`uv version --short > VERSION`

---

_Comment by @HenriBlacksmith on 2025-11-03 22:47_

@Gankra is there any chance that this feature could be considered?

Alternatively, having a way to hook some script or command on version bumps could be helpful.

---
