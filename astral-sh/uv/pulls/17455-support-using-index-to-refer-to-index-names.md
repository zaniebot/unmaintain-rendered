```yaml
number: 17455
title: "Support using `--index` to refer to index names"
type: pull_request
state: open
author: EliteTK
labels:
  - breaking
assignees: []
base: main
head: tk/index-by-name
created_at: 2026-01-13T23:35:35Z
updated_at: 2026-01-21T17:54:45Z
url: https://github.com/astral-sh/uv/pull/17455
synced_at: 2026-01-21T18:05:41Z
```

# Support using `--index` to refer to index names

---

_@EliteTK_

## Summary

Implement #13974.

Some of the code was inspired by existing code from `uv_distribution::metadata::lowering` which handles resolving index name references within `pyproject.toml`.

This PR adds a new `IndexArg` type to `uv_distribution_types` which holds information about an index as passed on the CLI. Specifically the current `Index` type has a non-optional `url` field. `IndexArg` solves this by including two variants, one for the `Index` type and one for an `UnresolvedIndex` which holds the `name` and whether the index was passed using `--default-index`. This means that all the CLI handling can use `IndexArg` and then the standard `resolve` functions can transform these `IndexArg`s into `Index`es. This allows you to reference indexes by name if they are already defined somewhere else.

### Error handling

The initial code made use of `Result` to propagate errors regarding resolution (i.e. no index found for that name). But I added a commit which replaces this with an early `exit(2)` as that seems to be the style used by other `resolve` style code.

### The new `Resolve` trait

`PipOptions` required a bunch of changes as it was using the `From` trait initially but index resolution requires additional metadata for the names which could have been passed as a tuple except for the fact that it's ugly and also breaks the orphan rule so it wasn't actually an option. Another alternative would be to do the resolution earlier but that seemed like an excessive amount of boilerplate, so instead a new trait was used.

### Argument cross-references

You can't resolve an index which you passed on the CLI (e.g. `--index foo=bar --index foo`) as I felt that didn't make sense as a feature even if it could have been supported. Maybe there is a use for it though? Not sure.

### Index writeback

The code for handling writing the index definition back into the `pyproject.toml` has remained unchanged. This means that if you reference a workspace index in a package add command, it will get copied to the package. This behaviour matches what would happen if you pass the full index including a name. I wasn't completely sure what to do here, but I feel as if it might be confusing to have them behave differently.

I think if this is undesirable, probably the best option here would be to make a decision on [Index priority] and then make the change in the writeback code itself, either to update the workspace index, or complain about modifications if the index is part of the workspace.

### Index priority

One area of ambiguity is which indexes to prioritise when trying to resolve a name.

When resolving the name of an index within a `pyproject.toml`, the lowering code resolves it by looking at the current file first, and then the workspace file.

On the other hand, when resolving which index to use for a package which has no specified index, the resolution takes the workspace index first and then the package's indexes next.

You can't have conflicting index names in a `pyproject.toml`, maybe the simple solution here is to forbid this within a workspace?

But, for the time being, the code follows the second strategy. It would be a bit of a change to make it follow the lowering strategy, but it seems possible toI believe that o. Since I realised this problem after having already written all of the code to implement the current strategy, I decided to leave it be for now.

### `uv tool`

Passing `--index` to `uv tool` even if it's the only index doesn't guarantee that it will use only that index for the tool. Moreover, due to the fact that `--index <name>` works identically to `--index <name>=...`, the side effect is that the index will appear twice in the `uv-receipt.toml`. I think that any changes in this area are probably outside of the scope of this change.

## Test Plan

A suite of new tests specifically for `uv add` in various situations.

The tests also help demonstrate how the current design decisions affect the results.

## Outstanding

A few things are outstanding because I am hoping to get some feedback on the design decisions before I go ahead and implement them (to avoid unnecessary churn).

* Additional tests for other parts of uv which can now take `--index <name>`
* Relevant documentation changes.
* Additional tests for index definitions in `uv.toml` (I had tested this manually but simply forgot to add a test case yet).

---

_@EliteTK reviewed on 2026-01-15 19:38_

---

_Review comment by @EliteTK on `crates/uv/tests/it/edit.rs`:11501 on 2026-01-15 19:38_

Need to investigate what happens if you don't pass `--index`.

---

_@EliteTK reviewed on 2026-01-15 19:39_

---

_Review comment by @EliteTK on `crates/uv/tests/it/edit.rs`:11283 on 2026-01-15 19:39_

I am not sure if we want this. But also, need to check what happens normally.

---

_@EliteTK reviewed on 2026-01-19 14:30_

---

_Review comment by @EliteTK on `crates/uv/tests/it/edit.rs`:11501 on 2026-01-19 14:30_

So, I am going to leave this as is for now.

Although I think there's an argument to be made that if you're adding an index and it matches identically an index in the workspace (regardless of if you used the name, or spelled it out) that maybe we shouldn't mirror it in the child.

But the way things like #17610 work seems like maybe justification for leaving it as is.

---

_@EliteTK reviewed on 2026-01-19 14:50_

---

_Review comment by @EliteTK on `crates/uv/tests/it/edit.rs`:11283 on 2026-01-19 14:50_

Okay, so index name references in `[tool.uv.sources]` are resolved in "project" then "workspace" order.

But when it comes to picking an index to use for a package without a specified source, we pick "workspace" then "project".

I'm not really sure what the correct answer is here. I'm going to leave it as is for now...

---

_Marked ready for review by @EliteTK on 2026-01-19 15:16_

---

_Review requested from @konstin by @EliteTK on 2026-01-19 15:16_

---

_Review comment by @konstin on `crates/uv/tests/it/edit.rs`:11283 on 2026-01-20 09:20_

I'd prefer workspace root only, to avoid the problem of workspace root and member declaration getting out of sync.

---

_@konstin reviewed on 2026-01-20 09:20_

---

_Review comment by @konstin on `crates/uv/tests/it/edit.rs`:11456 on 2026-01-20 09:22_

nit: `regex::escape` the mock server URL - though that filter doesn't seem to be used?

---

_Review comment by @konstin on `crates/uv/tests/it/edit.rs`:11501 on 2026-01-20 09:29_

Can you remove the commented out code, and/or leave a TODO comment?

---

_@konstin approved on 2026-01-20 09:30_

---

_Review requested from @zanieb by @konstin on 2026-01-20 09:30_

---

_Label `breaking` added by @zanieb on 2026-01-21 15:22_

---

_Added to milestone `v0.10.0` by @zanieb on 2026-01-21 15:22_

---

_Comment by @zanieb on 2026-01-21 16:25_

> Moreover, due to the fact that --index <name> works identically to --index <name>=..., the side effect is that the index will appear twice in the uv-receipt.toml

This seems problematic?

---

_Comment by @zanieb on 2026-01-21 16:25_

> This means that if you reference a workspace index in a package add command, it will get copied to the package. This behaviour matches what would happen if you pass the full index including a name. I wasn't completely sure what to do here, but I feel as if it might be confusing to have them behave differently.

Is it necessary for it to be copied into the child package? Or if you added it to the child package without doing so would the parent index still be used properly?

---

_Comment by @zanieb on 2026-01-21 16:27_

> When resolving the name of an index within a pyproject.toml, the lowering code resolves it by looking at the current file first, and then the workspace file.
>
> On the other hand, when resolving which index to use for a package which has no specified index, the resolution takes the workspace index first and then the package's indexes next.

So `--name` resolves to an index in the opposite order in which index priority is applied during resolution?

---

_Comment by @EliteTK on 2026-01-21 17:09_

> > Moreover, due to the fact that --index  works identically to --index =..., the side effect is that the index will appear twice in the uv-receipt.toml
> 
> This seems problematic?

This doesn't cause any issues at the moment but we could de-duplicate things such that only the first index for each unique URL is kept in the order they would have been kept before. I don't think the name is itself used for anything? That being said, this seems like an issue with how `uv tool install` works as you can also replicate the same receipt (with a duplicate entry) in other ways.

I'm happy to fix it though, do you want me to do it as part of this PR?

> > This means that if you reference a workspace index in a package add command, it will get copied to the package. This behaviour matches what would happen if you pass the full index including a name. I wasn't completely sure what to do here, but I feel as if it might be confusing to have them behave differently.
> 
> Is it necessary for it to be copied into the child package? Or if you added it to the child package without doing so would the parent index still be used properly?

It's definitely an option not to add it to the child package. The right index will be picked up as the name resolution order is project then workspace, so if the project doesn't have an index with that name, the workspace one will be picked.

But there is this issue: #17610, which is somewhat related.

I feel that if we were to fix this, we should fix this in a way which is agnostic to whether the index was mentioned by name, or fully written out with a conflicting name. Although we can also just mark the index as having originated by name and then not write it back in those situations. But we have to be careful as you can specify a `--default-index` by name but the original index may have been a non-default one, in which case it would be wrong to not write it back.

> > When resolving the name of an index within a pyproject.toml, the lowering code resolves it by looking at the current file first, and then the workspace file.
> > On the other hand, when resolving which index to use for a package which has no specified index, the resolution takes the workspace index first and then the package's indexes next.
> 
> So `--name` resolves to an index in the opposite order in which index priority is applied during resolution?

No, `--index <name>` resolves in the order of index priority during resolution, which I felt made sense initially, but the approach used by the lowering code (still uses priority, but it puts package indexes in front of the workspace indexes, which is the reverse) also makes sense.

---

_Comment by @zanieb on 2026-01-21 17:39_

@EliteTK can we ship this under a preview flag so we don't need to wait until 0.10 to merge?

---

_Comment by @zanieb on 2026-01-21 17:40_

> No, --index <name> resolves in the order of index priority during resolution, which I felt made sense initially, but the approach used by the lowering code (still uses priority, but it puts package indexes in front of the workspace indexes, which is the reverse) also makes sense.

I don't understand how to reconcile this comment with your original one?

---

_@zanieb reviewed on 2026-01-21 17:44_

---

_Review comment by @zanieb on `crates/uv-cli/src/options.rs`:189 on 2026-01-21 17:44_

I think this could be a bit less descriptive of the code and more of the intent? LIke

```suggestion
/// Resolve index options into a prioritized list of [`Index`].
/// 
/// Index name references are resolved to index URLs using values from the file system.
///
/// The default index is placed after all other indexes.
```

---

_@zanieb reviewed on 2026-01-21 17:45_

---

_Review comment by @zanieb on `crates/uv-cli/src/options.rs`:208 on 2026-01-21 17:45_

Do you know _why_ we do this in the other code? It's pretty janky. It doesn't respect our `Printer`, it doesn't print the error chain, etc.

---

_Comment by @EliteTK on 2026-01-21 17:54_

> @EliteTK can we ship this under a preview flag so we don't need to wait until 0.10 to merge?

Yeah, no objections here.

> > No, --index  resolves in the order of index priority during resolution, which I felt made sense initially, but the approach used by the lowering code (still uses priority, but it puts package indexes in front of the workspace indexes, which is the reverse) also makes sense.
> 
> I don't understand how to reconcile this comment with your original one?

I will adjust it for clarity, but the section you referenced mentions that this PR uses the "second strategy" which refers to:

"On the other hand, when resolving which index to use for a package which has no specified index, the resolution takes the workspace index first and then the package's indexes next."



---
