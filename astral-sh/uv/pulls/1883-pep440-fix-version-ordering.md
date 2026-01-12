```yaml
number: 1883
title: "pep440: fix version ordering"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/fix-version-order
created_at: 2024-02-22T20:56:49Z
updated_at: 2024-02-23T12:30:07Z
url: https://github.com/astral-sh/uv/pull/1883
synced_at: 2026-01-12T16:04:46Z
```

# pep440: fix version ordering

---

_@BurntSushi_

A couple moons ago, I introduced an optimization for version comparisons
by devising a format where *most* versions would be represented by a
single `u64`. This in turn meant most comparisons (of which many are
done during resolution) would be extremely cheap.

Unfortunately, when I did that, I screwed up the preservation of
ordering as defined by the [Version Specifiers spec]. I think I messed
it up because I had originally devised the representation so that we
could pack things like `1.2.3.dev1.post5`, but later realized it would
be better to limit ourselves to a single suffix. However, I never
updated the binary encoding to better match "up to 4 release versions
and up to precisely 1 suffix." Because of that, there were cases where
versions weren't ordered correctly. For example, this fixes a bug where
`1.0a2 < 1.0dev2`, even though all dev releases should order before
pre-releases.

We also update a test so that it catches these kinds of bugs in the
future. (By testing all pairs of versions in a sequence instead of just
the adjacent versions.)

[Version Specifiers spec]: https://packaging.python.org/en/latest/specifications/version-specifiers/#summary-of-permitted-suffixes-and-relative-ordering


---

_Review requested from @charliermarsh by @BurntSushi on 2024-02-22 20:57_

---

_Review request for @charliermarsh removed by @BurntSushi on 2024-02-22 20:57_

---

_Review requested from @konstin by @BurntSushi on 2024-02-22 20:57_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-02-22 20:57_

---

_Comment by @MichaReiser on 2024-02-22 21:06_

Nice find. I'm too tired to review this today. Does this have any implications on perf?

---

_Comment by @BurntSushi on 2024-02-22 21:12_

I don't think. I can check later. If anything, it will help, because it expands the set of versions that can be represented in a packed format.

But also, **do not merge** this yet. We need a way to invalidate the cache before we can ship this. Otherwise existing versions in the cache will be interpreted incorrectly. (Which might lead to panics or even silent logic bugs.)

---

_Comment by @konstin on 2024-02-22 21:17_

> We need a way to invalidate the cache before we can ship this. 

We can bump the cache bucket suffixes (`-v1` -> `-v2`). (I'll review tomorrow)


---

_Comment by @BurntSushi on 2024-02-22 21:31_

> > We need a way to invalidate the cache before we can ship this.
> 
> We can bump the cache bucket suffixes (`-v1` -> `-v2`). (I'll review tomorrow)

Done!

---

_@charliermarsh approved on 2024-02-22 21:57_

Awesome, thanks! I read it closely.

---

_@charliermarsh reviewed on 2024-02-22 21:58_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:928 on 2024-02-22 21:58_

Should `number` be 0 here? (Should we assert?)

---

_@BurntSushi reviewed on 2024-02-22 22:10_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:928 on 2024-02-22 22:10_

Nope! If the kind isn't a pre-release, then it could be something else with a non-zero number.

---

_Merged by @charliermarsh on 2024-02-22 23:01_

---

_Closed by @charliermarsh on 2024-02-22 23:01_

---

_Branch deleted on 2024-02-22 23:01_

---

_Label `bug` added by @BurntSushi on 2024-02-23 12:30_

---
