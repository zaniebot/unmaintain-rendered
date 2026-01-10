```yaml
number: 16691
title: "Add `uv workspace list` to list workspace members"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/workspace-list
created_at: 2025-11-11T20:19:01Z
updated_at: 2025-11-17T15:35:53Z
url: https://github.com/astral-sh/uv/pull/16691
synced_at: 2026-01-10T05:58:11Z
```

# Add `uv workspace list` to list workspace members

---

_Pull request opened by @zanieb on 2025-11-11 20:19_

I'm a little wary here, in the sense that it might be silly to have a command that does something so simple that's covered by `uv workspace metadata`? but I think this could be stabilized much faster than `uv workspace metadata` and makes it easier to write scripts against workspace members.

---

_Marked ready for review by @zanieb on 2025-11-11 20:56_

---

_@terror reviewed on 2025-11-11 22:37_

---

_Review comment by @terror on `crates/uv/src/commands/workspace/list.rs`:20 on 2025-11-11 22:37_

Looks like this is supposed to be:

```suggestion
    if !preview.is_enabled(PreviewFeatures::WORKSPACE_LIST) {
```

---

_Comment by @terror on 2025-11-11 22:43_

Also, should we add an entry to the [preview docs](https://github.com/astral-sh/uv/blob/main/docs/concepts/preview.md)?

---

_Comment by @zanieb on 2025-11-11 22:47_

Thanks!

---

_Label `preview` added by @zanieb on 2025-11-11 22:47_

---

_Comment by @konstin on 2025-11-12 13:16_

I'd go for a single API for machine-readable workspace metadata, I'd have expected this to print something more like a `uv pip list` table with names and paths.

If we're worried about stabilizing `uv workspace metadata`, we can stabilize a v1.0 with only workspace member information, and have a v1.1 that populates more fields behind `--preview`.

---

_Comment by @zanieb on 2025-11-12 14:46_

>  I'd have expected this to print something more like a uv pip list table with names and paths.

I'm not really into displaying tables. I think I'd expect you to pass items from here to `uv workspace dir --package <name>` if you want to do this simple composition.

---

_Comment by @zanieb on 2025-11-12 14:47_

Here I'm mostly thinking of the trivial case where you want to loop over workspace members. I don't want to make people use `jq` for that?

---

_Comment by @konstin on 2025-11-12 14:48_

When do you need to iterate over workspace members?

---

_Comment by @zanieb on 2025-11-12 15:12_

e.g., I need to run pytest / pyright / deptry in each workspace member

---

_Comment by @konstin on 2025-11-13 11:50_

Does that mean `uv workspace` will be entirely a primarily machine-readable interface, and human readable output will go into `uv tree`(?) or another interface?

---

_Comment by @zanieb on 2025-11-13 13:55_

I'm not sure yet. I just don't think showing the paths in this specific command is worth the added complexity, since they're _usually_ just a bunch of directories next to each other and it breaks composability. 

---

_Comment by @konstin on 2025-11-13 13:59_

We should pin down whether we want this interface to be specifically machine readable (with a specific format) or not, otherwise it will be hard to change it later. If we want it to be shell-iterable, we should commit to printing a single name per line and document that format.

---

_Comment by @zanieb on 2025-11-13 16:03_

>  If we want it to be shell-iterable, we should commit to printing a single name per line and document that format.

That's what I want to do here, I'm not sure why we need to document that explicitly? but I can do so. The command is just in preview though, I'm looking for concrete use-cases and feedback to guide the interface of this and the entire `workspace` namespace.

---

_Comment by @konstin on 2025-11-14 10:06_

I really mean just something simple in the docs:

> The output is one package name per line.

And maybe an even more explicit:

> This interface is machine readable.

As long as it's in preview we can change it again anyway.

---

_@konstin approved on 2025-11-17 15:33_

---

_Merged by @zanieb on 2025-11-17 15:35_

---

_Closed by @zanieb on 2025-11-17 15:35_

---

_Branch deleted on 2025-11-17 15:35_

---
