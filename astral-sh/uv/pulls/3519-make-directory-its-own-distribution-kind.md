```yaml
number: 3519
title: "Make `Directory` its own distribution kind"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/source-tree
created_at: 2024-05-10T21:19:07Z
updated_at: 2024-05-13T14:14:37Z
url: https://github.com/astral-sh/uv/pull/3519
synced_at: 2026-01-10T14:37:54Z
```

# Make `Directory` its own distribution kind

---

_Pull request opened by @charliermarsh on 2024-05-10 21:19_

## Summary

I think this is overall good change because it explicitly encodes (in the type system) something that was previously implicit. I'm not a huge fan of the names here, open to input.

It covers some of https://github.com/astral-sh/uv/issues/3506 but I don't think it _closes_ it.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-10 21:19_

---

_Review requested from @konstin by @charliermarsh on 2024-05-10 21:19_

---

_Label `internal` added by @charliermarsh on 2024-05-10 21:19_

---

_@charliermarsh reviewed on 2024-05-10 21:19_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/buildable.rs`:75 on 2024-05-10 21:19_

I kind of wish the names were like... `Directory` and `Archive`? But `Archive` is a bit strange.

---

_@charliermarsh reviewed on 2024-05-10 21:20_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/source/mod.rs`:323 on 2024-05-10 21:20_

This is good IMO, we removed this branch in favor of types that encode the difference.

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-05-10 21:22_

---

_Converted to draft by @charliermarsh on 2024-05-11 17:20_

---

_Comment by @charliermarsh on 2024-05-11 17:21_

Need to fix test failures.

---

_Marked ready for review by @charliermarsh on 2024-05-11 17:33_

---

_Converted to draft by @charliermarsh on 2024-05-11 17:40_

---

_Marked ready for review by @charliermarsh on 2024-05-11 21:10_

---

_Comment by @charliermarsh on 2024-05-11 21:10_

Ok, tests passing...

---

_@konstin reviewed on 2024-05-13 08:19_

---

_Review comment by @konstin on `crates/distribution-types/src/buildable.rs`:75 on 2024-05-13 08:19_

I like `Archive`, pip uses that terminology too

---

_Review comment by @konstin on `crates/uv-cache/src/lib.rs`:794 on 2024-05-13 08:23_

Should we raise those if they aren't file not found errors?

---

_@konstin approved on 2024-05-13 08:28_

---

_@charliermarsh reviewed on 2024-05-13 14:03_

---

_Review comment by @charliermarsh on `crates/uv-cache/src/lib.rs`:794 on 2024-05-13 14:03_

Yes, I'll do it separately though since this is just a move from above.

---

_Merged by @charliermarsh on 2024-05-13 14:03_

---

_Closed by @charliermarsh on 2024-05-13 14:03_

---

_Branch deleted on 2024-05-13 14:03_

---

_Review comment by @BurntSushi on `crates/distribution-types/src/buildable.rs`:75 on 2024-05-13 14:04_

Is `Archive` in this context referring to a specific file like a `.tar.gz`? If that's always the case, then I like the name `Archive`.

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/source/mod.rs`:323 on 2024-05-13 14:08_

Yeah. I like.

---

_@BurntSushi reviewed on 2024-05-13 14:10_

This LGTM. Contrary to what I said in the DM, in context, I think `Archive` is a good name if it always refers to a single archive-like file (i.e., `.tar.gz`).

---

_@charliermarsh reviewed on 2024-05-13 14:11_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/buildable.rs`:75 on 2024-05-13 14:11_

Yeah it needs to be a `.tar.gz` or `.zip` or `.tar.bz2` or a few other extensions that we support.

---

_@charliermarsh reviewed on 2024-05-13 14:11_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/buildable.rs`:75 on 2024-05-13 14:11_

I don't know that it's a perfect name because (e.g.) when you provide `Direct` (i.e., a direct URL), that has to be a direct URL to... an archive. So they're both archives?

---

_@charliermarsh reviewed on 2024-05-13 14:12_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/buildable.rs`:75 on 2024-05-13 14:12_

Honestly `Self::File` could be the right name.

---

_@BurntSushi reviewed on 2024-05-13 14:14_

---

_Review comment by @BurntSushi on `crates/distribution-types/src/buildable.rs`:75 on 2024-05-13 14:14_

Yeah that's a good point. I suppose `PathArchive` would also work and is maybe a bit more descriptive. But I agree that `File` also works. And it's more succinct.

---
