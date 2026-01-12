```yaml
number: 3242
title: "once-map: avoid hard-coding `Arc`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/once-map-arcless
created_at: 2024-04-24T14:54:46Z
updated_at: 2024-04-24T20:24:25Z
url: https://github.com/astral-sh/uv/pull/3242
synced_at: 2026-01-12T16:05:31Z
```

# once-map: avoid hard-coding `Arc`

---

_@BurntSushi_

The only thing a `OnceMap` really needs to be able to do with the value
is to clone it. All extant uses benefited from having this done for them
by automatically wrapping values in an `Arc`. But this isn't necessarily
true for all things. For example, a value might have an `Arc` internally
to making cloning cheap in other contexts, and it doesn't make sense to
re-wrap it in an `Arc` just to use it with a `OnceMap`. Or
alternatively, cloning might just be cheap enough on its own that an
`Arc` isn't worth it.


---

_Review requested from @zanieb by @BurntSushi on 2024-04-24 14:54_

---

_Comment by @zanieb on 2024-04-24 14:56_

Thank you!

---

_@charliermarsh approved on 2024-04-24 15:04_

---

_Label `internal` added by @charliermarsh on 2024-04-24 15:04_

---

_Merged by @BurntSushi on 2024-04-24 15:11_

---

_Closed by @BurntSushi on 2024-04-24 15:11_

---

_Branch deleted on 2024-04-24 15:11_

---

_Comment by @ibraheemdev on 2024-04-24 17:48_

We could also avoid the `Arc`s entirely by keeping a `DashMap<K, Value<usize>>` that indexes into a separate persistent vector once the value has been initialized.

---

_Comment by @BurntSushi on 2024-04-24 18:15_

@ibraheemdev Do you mean something like [this](https://docs.rs/im-rc/latest/im_rc/vector/struct.Vector.html)? I don't think I see a straight-forward way to make that work. You still need to synchronize updates to the persistent vector somehow. And that in turn usually makes returning a `&T` difficult. You could put a `Box<Vector>` into an `AtomicPtr`, but when you go to CAS it out with an updated `Vector` after a `done` call, I believe that could invalidate previous borrows (thus causing UB).

---

_Comment by @ibraheemdev on 2024-04-24 18:52_

@BurntSushi I was thinking of using [boxcar](https://docs.rs/boxcar/latest/boxcar/struct.Vec.html), which allows concurrent inserts and stable addresses. I don't know enough about how `OnceMap` is used to know if it's worth the dependency though.

---

_Comment by @BurntSushi on 2024-04-24 20:24_

Oh a concurrent vector, not a persistent vector. (A persistent vector has different properties. Like cheap clones, structural sharing and the ability to continue using older "versions.") But I see, yes, I think something like that could work. It's a nice data structure. But yeah, I don't think we're feeling the downsides of `Arc` here yet. I think it's typically the case that the relative work of cloning an `Arc` is dwarfed by other things (like I/O).

---
