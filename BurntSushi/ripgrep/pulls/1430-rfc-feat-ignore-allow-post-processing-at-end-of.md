```yaml
number: 1430
title: "RFC feat(ignore): Allow post-processing at end-of-thread"
type: pull_request
state: closed
author: epage
labels:
  - rollup
assignees: []
base: master
head: visitor
created_at: 2019-11-16T22:07:50Z
updated_at: 2020-02-17T22:16:38Z
url: https://github.com/BurntSushi/ripgrep/pull/1430
synced_at: 2026-01-12T18:23:13Z
```

# RFC feat(ignore): Allow post-processing at end-of-thread

---

_@epage_

On top of the parallel-walk's closures, this provides a Visitor API.
- Clarifies the role of the two different closures.
- Allows implementing of `Drop` for post-processing at thread
  completion.

The closure API is maintained not just for compatibility but also
convinience for simple cases.

Fixes #469

---

_Comment by @epage on 2019-11-16 22:09_

I labeled this "RFC" because I have an open question on the API over at the original issue, https://github.com/BurntSushi/ripgrep/issues/469#issuecomment-546652231 .

---

_@BurntSushi reviewed on 2020-02-17 17:54_

Awesome! I ended up merging this with your other PR. It makes the trait definitions a bit more gnarly, but the idea still works. I also removed the trait object from the signature of `visit` and instead just used normal parametric polymorphism. Although, now that I say that, I wonder if a trait object is preferrable. There's unlikely any performance benefit, and a trait object would avoid copying all of the traversal code for each implementation of `ParallelVisitorBuilder`.

I saw your ideas with respect to reduce APIs, but I think I'd rather stick with the smallest addition that unblocks folks. As I've said a few other things, IMO, the API of `ignore` is an unmitigated disaster and needs to be completely rethought anyway. So until then, I'm somewhat okay with continuing to make the API worse as long as it serves a real need while also minimizing churn. So I really appreciated that this change is backwards compatible. :-)

---

_Label `rollup` added by @BurntSushi on 2020-02-17 17:54_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
