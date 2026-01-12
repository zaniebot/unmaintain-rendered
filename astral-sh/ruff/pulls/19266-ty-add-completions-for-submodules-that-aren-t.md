```yaml
number: 19266
title: "[ty] Add completions for submodules that aren't attributes of their parent"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/submodule-completions
created_at: 2025-07-10T17:10:23Z
updated_at: 2025-07-11T14:06:37Z
url: https://github.com/astral-sh/ruff/pull/19266
synced_at: 2026-01-12T15:56:35Z
```

# [ty] Add completions for submodules that aren't attributes of their parent

---

_@BurntSushi_

While we did previously support submodule completions via our
`all_members` API, that only works when submodules are attributes of
their parent module. For example, `os.path`. But that didn't work when
the submodule was not an attribute of its parent. For example,
`http.client`. To make the latter work, we read the directory of the
parent module to discover its submodules.

Kudos to @AlexWaygood who started this patch. :-)

Note that this doesn't currently do any caching.


---

_Review requested from @carljm by @BurntSushi on 2025-07-10 17:10_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-07-10 17:10_

---

_Review requested from @sharkdp by @BurntSushi on 2025-07-10 17:10_

---

_Review requested from @dcreager by @BurntSushi on 2025-07-10 17:10_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-07-10 17:10_

---

_Review request for @dcreager removed by @BurntSushi on 2025-07-10 17:10_

---

_Review request for @carljm removed by @BurntSushi on 2025-07-10 17:10_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-07-10 17:10_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-07-10 17:10_

---

_Comment by @BurntSushi on 2025-07-10 17:10_

Demo:

https://github.com/user-attachments/assets/a21b1ba0-15f0-4eb8-86f0-0ef653c6caaf



---

_Label `server` added by @MichaReiser on 2025-07-10 17:12_

---

_Label `ty` added by @MichaReiser on 2025-07-10 17:12_

---

_Comment by @github-actions[bot] on 2025-07-10 17:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:169 on 2025-07-10 19:24_

hmm... this comment seems semi-relevant here:

https://github.com/astral-sh/ruff/blob/965f415212f4f9f3ef855b647d53e892e6913828/crates/ty_vendored/build.rs#L93-L99

I think the assumptions we make elsewhere are probably okay, given that we in practice only care about the zip file that we create ourselves at build time in `crates/ty_vendored/build.rs`. But I can't really remember the details, and it's quite possible that this assumption should at least be documented better!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/module.rs`:148 on 2025-07-10 19:30_

is it worth emitting a log message saying that we couldn't provide completions because of error blah that meant we couldn't read the directory?

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:334 on 2025-07-10 19:36_

heh, not sure how I feel about this crate having three different `DirectoryEntry` structs 😆 but I also don't have a suggestion for how to avoid that

---

_@AlexWaygood approved on 2025-07-10 19:38_

Nice!

---

_@AlexWaygood reviewed on 2025-07-11 11:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:87 on 2025-07-11 11:07_

I guess it does feel a bit silly that we have to do all this work to construct the absolute `ModuleName` and then resolve it. The only reason we need to resolve the module here is so that we can get a `Type` for the `Completion` -- but the only reason we need a `Type` for the `Completion` is so that we can figure out the `CompletionKind` later on. In this specific situation, we know what the `CompletionKind` will be here without having to resolve the module and then use that to construct a `Type`: it will always be a `CompletionKind::Module`. So it does feel like this is doing a lot more work than is really necessary. Should we do the `Type -> CompletionKind` conversion at an earlier stage, so that `Completion` can just store the `CompletionKind` directly rather than storing a `Type`?

Similarly, we don't really need to call `literal.resolve_submodule` here either, which could also be pretty expensive in some situations -- we know that the kind of the all the completions in this `.extend()` call will always be `CompletionKind::Module`:

https://github.com/astral-sh/ruff/blob/25c429556421ddd6f715f5aaf906610e0c564606/crates/ty_python_semantic/src/types/ide_support.rs#L204-L209

---

_@AlexWaygood reviewed on 2025-07-11 11:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:87 on 2025-07-11 11:26_

Oh, unless the plan is to use the `Type` for more than just the `CompletionKind` at some point in the future?

---

_@BurntSushi reviewed on 2025-07-11 11:50_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/semantic_model.rs`:87 on 2025-07-11 11:50_

That occurred to me too. I do suspect that we'll want to head in this direction, but I'd rather leave that as an improvement for the future. I do expect we'll want to use the `Type` for other things like showing the type signature, but even for cases like that, we might want to elide the type signatue for certain things like modules where its kind is already descriptive enough. Or just write `module` as its type.

---

_@BurntSushi reviewed on 2025-07-11 11:52_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/vendored.rs`:334 on 2025-07-11 11:52_

I noticed that too. I wondered why there wasn't an abstraction layer over both `db.system()` and `db.vendored()`. But there were already distinct types for `FileType` and `Metadata`. So instead of rethinking the whole setup here, I just followed the existing pattern in deference to Chesterton's fence. :-)

---

_@AlexWaygood reviewed on 2025-07-11 11:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:87 on 2025-07-11 11:53_

Right -- in diagnostic messages in ty, we'd just display the type of a module-literal as `<module 'json'>` or similar. We should be able to render that just fine in autocomplete suggestions without having an actual `Type` to hand 😄

---

_@BurntSushi reviewed on 2025-07-11 13:26_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/vendored.rs`:169 on 2025-07-11 13:26_

Aye. I've doubled down on `/` and added a comment on `VendoredFileSystem` noting this.

---

_Comment by @BurntSushi on 2025-07-11 13:36_

I'm going to work on caching this in follow-up work.

---

_Merged by @BurntSushi on 2025-07-11 14:06_

---

_Closed by @BurntSushi on 2025-07-11 14:06_

---

_Branch deleted on 2025-07-11 14:06_

---
