```yaml
number: 13974
title: "Add support for requesting indexes with `--index <name>`"
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-06-11T19:12:08Z
updated_at: 2026-01-16T16:32:17Z
url: https://github.com/astral-sh/uv/issues/13974
synced_at: 2026-01-16T16:59:49Z
```

# Add support for requesting indexes with `--index <name>`

---

_@zanieb_

### Summary

We currently don't support requesting a previously defined index by name in various operations, but we should.

### Example

e.g., where an index is defined in a `pyproject.toml`, I should be able to use

```
$ uv add --index <name>
```

or when defined in a `uv.toml`

```
$ uv tool install --index <name>
```

---

_Label `enhancement` added by @zanieb on 2025-06-11 19:12_

---

_Comment by @zanieb on 2025-06-11 19:12_

I thought this was discussed elsewhere, but can't find it.

Related

- #13874 
- https://github.com/astral-sh/uv/issues/13973
- https://github.com/astral-sh/uv/issues/13872

---

_Comment by @andrader on 2025-09-05 18:42_

I support the need for this feature!

---

_Comment by @EliteTK on 2026-01-05 23:12_

So at least in `uv add`'s case. If only one index is provided, the requirements are pinned to that index. And then regardless of how many indexes are provided, the specified indexes are added/re-added to the pyproject.toml (such that any pre-existing entries appear at the top in the order they are passed on the command line (or from other sources)).

Regarding `uv tool install`, currently what you pass via `--index` seems to get checked after whatever is already in your `uv.toml`.

It seems like maybe we should be prioritising the index passed there?

As for how to achieve this, I've considered a few different ideas but I think what makes the most sense to me is something like: Add a new type for an `IndexOrName` which is what we would take on the CLI instead of an `Index`. The direct consumers of the arguments would then attempt resolve this from the pyproject.toml (or the embedded script toml) before treating it like a normal index.

Alternatively we could add some logic to `Index::from_str` which attempts to find the right `pyproject.toml` / script toml / `uv.toml` but this seemed a bit too dirty.

Happy to hear other suggestions.

---

_Assigned to @EliteTK by @EliteTK on 2026-01-09 20:53_

---

_Comment by @EliteTK on 2026-01-13 17:31_

Two interesting quirks came up:

1. We could technically allow `pyproject.toml` to reference `uv.toml` indices by name.
2. We could include also include the index name in `uv-receipt.toml` for a tool, instead of the index URL, if it is installed.

#1 seems like it could lead to surprises. I think #2 might be an okay idea. But for now, I'm going to not do/allow either of those... Just wanted to document this decision in case there were interesting objections to this choice.

---

_Comment by @EliteTK on 2026-01-16 16:28_

As written, my PR makes passing `--index name` equivalent to passing `--index name=uri` which has the impact that, for a workspace child package, it will copy the index definition into it.

Additionally, this means that if you're in the directory of a workspace child or using --project in the workspace root, and the child has its own index defined with a conflicting name, the definition will get overwritten by the workspace's. This again matches the behaviour of doing `--index name=url` and the resolution logic matches our normal resolution logic (which honestly seems a bit unusual to me).

While it would in some sense deviate from the behaviour of `--index name=uri`, I think we shouldn't write new index definitions when picking up an `--index` by name. Not quite sure how to make that happen yet though...

---
