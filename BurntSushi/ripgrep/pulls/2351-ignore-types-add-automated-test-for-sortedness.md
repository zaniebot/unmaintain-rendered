```yaml
number: 2351
title: "ignore/types: add automated test for sortedness"
type: pull_request
state: merged
author: arbrauns
labels: []
assignees: []
merged: true
base: master
head: sorted-types-test
created_at: 2022-11-14T12:44:00Z
updated_at: 2022-11-14T13:31:08Z
url: https://github.com/BurntSushi/ripgrep/pull/2351
synced_at: 2026-01-12T18:23:14Z
```

# ignore/types: add automated test for sortedness

---

_@arbrauns_

After I accidentally mixed up the sorting in #2349 and didn't notice, I thought I'd add a test so hopefully the next time it happens to someone it can be caught before it's sent in as a PR.

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:294 on 2022-11-14 12:49_

The test should be in a `#[cfg(test)] mod tests { ... }` block. It doesn't have to be, but this is the pattern followed in this repo and clearly delineates tests from non-tests.

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:298 on 2022-11-14 13:00_

I would write this as:

```rust
let first = match names.next() {
    None => return,
    Some(first) => first,
};
```

Or perhaps even better, using the [newly stabilized `let else`](https://github.com/rust-lang/rust/pull/93628/):

```rust
let Some(first) = names.next() else { return };
```

Although in order to use `let else` we need to bump MSRV to Rust 1.65. Since ripgrep tracks the latest stable release of Rust, I have no problem with that. Indeed, [I just pushed a commit to do so](https://github.com/BurntSushi/ripgrep/commit/8905d54a9f25f4c1e4e3ca8331f517473e174d87), so all you need to do to use `let else` is to rebase on master.

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:298 on 2022-11-14 13:01_

The higher level point here is to keep code linear via early returns. That way, you avoid nesting the main body here.

---

_@BurntSushi requested changes on 2022-11-14 13:01_

Great idea, thank you!

---

_Review comment by @arbrauns on `crates/ignore/src/default_types.rs`:298 on 2022-11-14 13:18_

That's much neater. Of course the best solution would be rust-lang/rust#53485, but that seems to still be quite a while away.

If ripgrep uses latest stable anyway, what's the opinion on `edition = "2021"`? It would allow for things like implicit arguments in panic/assert format strings: https://doc.rust-lang.org/edition-guide/rust-2021/panic-macro-consistency.html.

---

_@arbrauns reviewed on 2022-11-14 13:18_

---

_@BurntSushi reviewed on 2022-11-14 13:29_

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:298 on 2022-11-14 13:29_

I'll very likely do the edition migration myself before the next release, whenever that is. I'm in no rush to be honest. I'll likely only do things that `cargo fix` does automatically. Converting explicit to implicit arguments by hand is a fair bit of work.

With respect to `is_sorted`, I actually think your approach here is better. It gives a better failure mode by providing a witness. `is_sorted` wouldn't do that. :-)

---

_@BurntSushi approved on 2022-11-14 13:30_

---

_Merged by @BurntSushi on 2022-11-14 13:31_

---

_Closed by @BurntSushi on 2022-11-14 13:31_

---
