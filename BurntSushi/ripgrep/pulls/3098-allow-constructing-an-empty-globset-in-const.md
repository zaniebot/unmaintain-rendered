```yaml
number: 3098
title: Allow constructing an empty GlobSet in const
type: pull_request
state: closed
author: dtolnay
labels:
  - rollup
assignees: []
base: master
head: constglobset
created_at: 2025-07-08T07:44:07Z
updated_at: 2025-09-20T01:08:44Z
url: https://github.com/BurntSushi/ripgrep/pull/3098
synced_at: 2026-01-12T18:23:15Z
```

# Allow constructing an empty GlobSet in const

---

_@dtolnay_

My use case: I have a struct that contains a GlobSet, among other information:

```rust
pub struct Thing {
    pub extra_srcs: GlobSet,
    pub flags: Vec<String>,
    ...
}
```

and some instances of this struct held in a different data structure &mdash; imagine this is akin to Cargo.toml's `[dependencies]` and `[target.'cfg(…)'.dependencies]`:

```rust
pub struct ConfigFile {
    common: Option<Thing>,
    platform_specific: BTreeMap<Platform, Thing>,
}
```

and a function where it would be really convenient to be able to use a const to get a sufficiently long-lived reference to a static Thing containing an empty globset:

```rust
impl ConfigFile {
    pub fn things(&self) -> impl Iterator<Item = &Thing> {
        self.common
            .iter()
            .chain(self.platform_specific.values())
            .chain([
                const {
                    &Thing {
                        extra_srcs: GlobSet::empty(),  // <---
                        flags: Vec::new(),
                        ...
                    }
                },
                ...
            ])
    }
}
```

Without being able to call `GlobSet::empty()` in const, I would need to do one of the following workarounds instead:

- Store `Option<GlobSet>` instead of just `GlobSet`, where `None` means the same thing as `GlobSet::empty()`.

- Find a place to put my constant Thing at runtime, either inside `ConfigFile` (awkward) or some other storage that is passed in to `things` (awkward).

- Switch to `Cow<'_, Thing>` and sprinkle the implementation of `things` with `Cow::Owned` and `map(Cow::Borrowed)`.

- Clone everything every time we iterate this data structure.

- Something with a `static` `OnceLock`.

---

_Label `rollup` added by @BurntSushi on 2025-08-20 00:01_

---

_@BurntSushi approved on 2025-08-20 00:01_

Makes sense to me! Thanks!

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
