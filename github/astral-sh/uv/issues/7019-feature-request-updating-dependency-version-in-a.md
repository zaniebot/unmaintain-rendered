---
number: 7019
title: "Feature request: updating dependency version in a lockfile"
type: issue
state: closed
author: Kobzol
labels:
  - documentation
assignees: []
created_at: 2024-09-04T13:49:12Z
updated_at: 2024-10-23T09:03:35Z
url: https://github.com/astral-sh/uv/issues/7019
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature request: updating dependency version in a lockfile

---

_Issue opened by @Kobzol on 2024-09-04 13:49_

Hi! While migrating an existing Python project to `uv`, I encountered a situation where a command similar to `cargo update --precise` would be quite useful. Essentially, I would like to have the option to tell `uv` to set a package version (usually of a transitive dependency, one that is not specified in `dependencies`) in a lockfile to a specific version (that is semver compatible with the other dependencies).

I have the following use-case (related to https://github.com/astral-sh/uv/issues/7018): I'm porting an existing Python project using a `requirements.txt` lockfile to `uv`. Let's say that I have a root dependency `a` that depends on a transitive dependency `b>2.1`, and the latest published version of `b` is `2.3`. However, in my existing Python environment, I'm using `b==2.2`, and I want to continue doing that. In theory, according to semver, `2.3` is compatible with `2.2`, but theory is not always practice (especially in the world of Python package management :) ).

If I want to move to `uv`, I'll generate a new project, set `dependencies = ["a==xyz"]` and generate a lockfile. However, `uv` will forcefully set the version of `b` to `2.3`, the newest compatible version. I haven't found any way to tell `uv` to set a specific version of the transitive dependency (aside from manually modifying the lockfile, but that's really not a good solution).

# Workaround
What I found to work is to do `uv add b==2.2`, which will update the lockfile, and then manually delete `"b==2.2"` from the `dependencies` array. This seems to work fine, but it would be nice if `uv` allowed me to do this in a more straightforward way, e.g. with something like `uv lock update b=2.2`.

# Related issue
A bit similar to https://github.com/astral-sh/uv/issues/6794, but I'm looking for a way to set the version of a specific package to a specific version, not just update everything.

---

_Comment by @zanieb on 2024-09-04 14:04_

I comment on some future upgrade options at https://github.com/astral-sh/uv/issues/6781#issuecomment-2318265988

However, it sounds like you're looking for constraints or overrides:

- https://docs.astral.sh/uv/concepts/resolution/#dependency-constraints
- https://docs.astral.sh/uv/concepts/resolution/#dependency-overrides
- https://docs.astral.sh/uv/reference/settings/#override-dependencies

---

_Comment by @Kobzol on 2024-09-04 14:15_

I saw the constraints and overrides in the documentation, but it looks like a too big hammer for this use-case, I think. Here I mostly just want to take a one-time snapshot of an existing package environment. Also, in general, `cargo update --precise` is a useful tool for other use-cases.

I'm not sure how to use the `--constraint` flag to affect the generation of the lockfile.

---

_Comment by @charliermarsh on 2024-09-04 23:34_

The workaround I generally recommend here is to snapshot the environment, add the versions as constraints, run `uv lock`, then remove the constraints (and we'll continue to respect those versions once set). But agree we should have a better workflow.

---

_Comment by @Kobzol on 2024-09-05 06:49_

Yeah, that's exactly the workaround I described in the issue. Thanks for confirming that there probably isn't a better way for now 👍

---

_Comment by @Kobzol on 2024-09-05 12:59_

Another related use-case that I encountered today is that dependabot told me to update a version of a transitive dependency in my lockfile. In Rust, I would do `cargo update <package>` or `cargo update <package> --precise <version that dependabot told me to upgrade to>` to achieve that.

---

_Comment by @charliermarsh on 2024-09-05 13:12_

I think this should be possible today with: `uv lock --upgrade-package "<package>==<version>"`.

---

_Comment by @Kobzol on 2024-09-05 14:12_

Ooh, that's it, thank you! I thought that I tried this before, not sure why I thought it doesn't work. This is essentially `cargo update`, and it solves this issue. Thanks :heart:

It would be nice to add some more visible information about this to the documentation, because I only found `uv lock --upgrade` to update everything. Do you think it would be good to add it e.g. [here](https://docs.astral.sh/uv/concepts/projects/#upgrading-locked-package-versions)? I can send a PR.

---

_Comment by @zanieb on 2024-09-05 14:13_

@Kobzol that'd be great, thanks!

---

_Label `documentation` added by @zanieb on 2024-09-05 14:14_

---

_Referenced in [astral-sh/uv#7083](../../astral-sh/uv/pulls/7083.md) on 2024-09-05 14:38_

---

_Comment by @Kobzol on 2024-09-05 14:38_

https://github.com/astral-sh/uv/pull/7083

---

_Closed by @zanieb on 2024-09-05 16:37_

---

_Closed by @zanieb on 2024-09-05 16:37_

---
