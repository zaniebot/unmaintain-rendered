```yaml
number: 14644
title: "Add git commit and `--tag` option to `uv version --bump`"
type: issue
state: open
author: cthoyt
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-07-16T00:37:48Z
updated_at: 2025-07-16T12:08:03Z
url: https://github.com/astral-sh/uv/issues/14644
synced_at: 2026-01-10T03:32:45Z
```

# Add git commit and `--tag` option to `uv version --bump`

---

_Issue opened by @cthoyt on 2025-07-16 00:37_

### Summary

I'm very happy to see the movement forwards in improving uv's version bumping capabilities, especially with the inclusion of https://github.com/astral-sh/uv/pull/13578 in uv v0.7.21 earlier this week :)

This request is for uv's `uv version --bump` interface to better resemble other version bumping workflows, such as [`bump-my-version`](https://pypi.org/project/bump-my-version/). For context, `bump-my-version` is a fork of [`bump2version`](https://pypi.org/project/bump2version/), which itself was a fork of [`bumpversion`](https://pypi.org/project/bumpversion/).

This request has two parts:

1. to implement a way of making a git commit following a version bump
2. to implement a way of adding a tag to that git commit (ideally, using a `--tag` flag)

This would be valuable because it's currently a bit tricky to implement this in a portable way. The following shell commands can get the job done, but setting and reading variables is unreliable, e.g., in tools like `tox` where each command is executed in its own shell

```shell
$ version_str=`uv version --bump stable`
$ git commit -am "Released $(version_str)"
$ git tag -a "v$(version_str)" -m "Released version $(version_str)"
```

#### Related suggestions and implementations

- https://github.com/astral-sh/uv/issues/13827#issuecomment-2967771683 suggested this feature in a comment on the issue, which itself was about including configuration of uv to bump version strings appearing in other files
- https://github.com/astral-sh/uv/issues/13657 suggested this feature as a part of a more generic GitHub Actions workflow
- https://github.com/alltuner/uv-version-bumper

### Example

My current workflow uses `bump-my-version` in the following command:

```shell
$ bump-my-version bump release --tag
```

Part of this can already be translated to uv with


```shell
$ uv version --bump stable
```

However, `uv` does not make a commit like `bump-my-version`, an naturally there is therefore no possibility to create a tag. For a bit more context, I'm configuring `tox` like this:


```ini
# tox.ini

[testenv:bumpversion-release]
description = Remove the -dev tag from the version
commands = bump-my-version bump release --tag
skip_install = true
passenv = HOME
dependency_groups =
    bump
```

For more context, you can see this in one of my repositories at https://github.com/cthoyt/ssslm/blob/9181570a397d7c5ca3e4b4314497bef08e5b1703/tox.ini#L183-L197.

I would ultimately like to be able to replace this with the following, given that a uv can be extended to both make a commit and a tag when the `--tag` option is passed

```shell
$ uv version --bump stable --tag
```

## A few more considerations

- while `bump-my-version` can be configured to make a commit by default, it probably makes sense to have a flag for just making a commit in uv that doesn't require making a tag
- `bump-my-version` also enables configuring the commit message, but if uv picks a good one, then I'm sure most people would be happy with the default!

---

_Label `enhancement` added by @cthoyt on 2025-07-16 00:37_

---

_Renamed from "Add `--tag` option to `uv version --bump`" to "Add git commit and `--tag` option to `uv version --bump`" by @cthoyt on 2025-07-16 00:38_

---

_Label `wish` added by @zanieb on 2025-07-16 04:20_

---

_Comment by @zanieb on 2025-07-16 04:21_

I think this is out of scope in the short-term, but is an interesting idea.

---

_Comment by @cthoyt on 2025-07-16 12:08_

@zanieb thanks for the consideration! if you do come back to this, I am happy to elaborate and help with the design

---
