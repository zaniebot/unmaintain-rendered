```yaml
number: 6236
title: Add dependabot and renovate documentation page
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/dependency-bots
created_at: 2024-08-19T23:04:27Z
updated_at: 2024-08-28T12:41:26Z
url: https://github.com/astral-sh/uv/pull/6236
synced_at: 2026-01-10T12:53:33Z
```

# Add dependabot and renovate documentation page

---

_Pull request opened by @zanieb on 2024-08-19 23:04_

cc @mkniewallner

---

_Label `documentation` added by @zanieb on 2024-08-19 23:04_

---

_Label `preview` added by @zanieb on 2024-08-19 23:04_

---

_Review comment by @mkniewallner on `docs/guides/integration/dependency-bots.md`:17 on 2024-08-19 23:26_

`lockFileMaintenance` is not required for uv support, it mostly allows refreshing transitive dependencies (or also direct dependencies if using range of versions for dependencies in `pyproject.toml`) in the lock file on a regular basis, so this should probably be a dedicated paragraph.

I'd probably go with something like:

```suggestion
Renovate will detect the presence of `uv.lock` file to determine that uv is used for managing
dependencies, and will suggest updates to
both [project dependencies](../../concepts/dependencies.md#project-dependencies), [optional dependencies](../../concepts/dependencies.md#optional-dependencies)
and [development dependencies](../../concepts/dependencies.md#development-dependencies), updating
`uv.lock` file along the way.

It is also possible to refresh `uv.lock` file on a regular basis (for instance to update transitive
dependencies) by
enabling [`lockFileMaintenance`](https://docs.renovatebot.com/configuration-options/#lockfilemaintenance)
option:

```json5 title="renovate.json5"
{
  $schema: "https://docs.renovatebot.com/renovate-schema.json",
  lockFileMaintenance: {
    enabled: true,
  },
}
```
```

---

_@mkniewallner reviewed on 2024-08-19 23:34_

Since support for Renovate was only recently added, and Renovate GitHub app does not yet use a version that added support for uv, do you mind waiting a bit before merging this one? I'd ideally prefer to test that it works correctly once the new version is used by Renovate GitHub app (which I'll make sure to do as soon as it's available, and report status here), to make sure we don't advertise for something that does not work :smile: 

---

_@mkniewallner reviewed on 2024-08-19 23:44_

---

_Review comment by @mkniewallner on `docs/guides/integration/dependency-bots.md`:30 on 2024-08-19 23:44_

Created a dedicated GitHub discussion for that (https://github.com/renovatebot/renovate/discussions/30899) that will hopefully be converted to an issue, to better track the implementation. Will report back if it ends up being converted to an issue.

---

_@mkniewallner reviewed on 2024-08-20 11:33_

---

_Review comment by @mkniewallner on `docs/guides/integration/dependency-bots.md`:30 on 2024-08-20 11:33_

An issue now exists: https://github.com/renovatebot/renovate/issues/30909.

---

_@zanieb reviewed on 2024-08-20 12:31_

---

_Review comment by @zanieb on `docs/guides/integration/dependency-bots.md`:17 on 2024-08-20 12:31_

Thanks!

---

_@zanieb reviewed on 2024-08-20 12:31_

---

_Review comment by @zanieb on `docs/guides/integration/dependency-bots.md`:30 on 2024-08-20 12:31_

Awesome, I'll link to that.

---

_Label `preview` removed by @zanieb on 2024-08-20 19:05_

---

_@charliermarsh approved on 2024-08-22 22:54_

---

_Comment by @zanieb on 2024-08-22 23:19_

@mkniewallner just let me know when it's ready.

I made some edits as you suggested — let me know if there's anything incorrect.

---

_Review comment by @mkniewallner on `docs/guides/integration/dependency-bots.md`:14 on 2024-08-22 23:38_

```suggestion
The lockfile can also be refreshed on a regular basis (for instance to update transitive
```

---

_Review comment by @mkniewallner on `docs/guides/integration/dependency-bots.md`:35 on 2024-08-22 23:38_

```suggestion
Support for uv is not yet available. Progress can be tracked at
```

---

_@mkniewallner reviewed on 2024-08-22 23:39_

---

_Comment by @mkniewallner on 2024-08-22 23:45_

> @mkniewallner just let me know when it's ready.
> 
> I made some edits as you suggested — let me know if there's anything incorrect.

Unfortunately, Renovate GitHub app still uses an old version that doesn't have the changes made to support uv for now, so I wasn't able to test that it works as expected on projects I've switched to uv. But I'm still regularly checking :slightly_smiling_face: 

---

_Label `do-not-merge` added by @zanieb on 2024-08-23 00:01_

---

_Comment by @mkniewallner on 2024-08-28 08:20_

Renovate GitHub app has been updated, and it seems to work fine (https://github.com/mkniewallner/showcase-uv-renovate/pull/13, https://github.com/mkniewallner/showcase-uv-renovate/pull/15), so this PR should be ready to merge.

---

_Label `do-not-merge` removed by @zanieb on 2024-08-28 12:41_

---

_Merged by @zanieb on 2024-08-28 12:41_

---

_Closed by @zanieb on 2024-08-28 12:41_

---

_Branch deleted on 2024-08-28 12:41_

---
