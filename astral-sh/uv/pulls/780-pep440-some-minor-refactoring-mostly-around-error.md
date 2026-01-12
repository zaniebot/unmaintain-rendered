```yaml
number: 780
title: "pep440: some minor refactoring, mostly around error types"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/version-refactor
created_at: 2024-01-04T14:56:57Z
updated_at: 2024-01-04T17:28:37Z
url: https://github.com/astral-sh/uv/pull/780
synced_at: 2026-01-12T16:04:11Z
```

# pep440: some minor refactoring, mostly around error types

---

_@BurntSushi_

This PR does a bit of refactoring to the pep440 crate, and in
particular around the erorr types. This PR is meant to be a precursor
to another PR that does some surgery (both in parsing and in `Version`
representation) that benefits somewhat from this refactoring.

As usual, please review commit-by-commit.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-04 14:57_

---

_Review request for @charliermarsh removed by @BurntSushi on 2024-01-04 14:57_

---

_Review requested from @konstin by @BurntSushi on 2024-01-04 14:57_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-04 14:57_

---

_Review comment by @konstin on `crates/pep440-rs/src/version.rs`:200 on 2024-01-04 15:21_

Is that the number of segments in a release?

---

_Review comment by @konstin on `crates/pep440-rs/src/version_specifier.rs`:585 on 2024-01-04 15:36_

nit: Avoid a leading sigil which make it unclear if it's part of the error or coming from somewhere else.

```suggestion
                    "The ~= operator requires at least two segments in the release version"
```

---

_Review comment by @konstin on `crates/pep440-rs/src/version_specifier.rs`:631 on 2024-01-04 15:37_

What's the motivation for boxing?

---

_Review comment by @konstin on `crates/pep440-rs/src/version_specifier.rs`:1309 on 2024-01-04 15:38_

I had these as strings so you can see the error displayed to the user

---

_Review comment by @konstin on `crates/pep440-rs/src/version_specifier.rs`:214 on 2024-01-04 15:39_

Are these `ref` required, i thought rustc could infer them nowadays?

---

_@konstin approved on 2024-01-04 15:40_

---

_@BurntSushi reviewed on 2024-01-04 15:42_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:200 on 2024-01-04 15:42_

Yup!

---

_@BurntSushi reviewed on 2024-01-04 15:46_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version_specifier.rs`:631 on 2024-01-04 15:46_

I added this comment:

```
    // We box to shrink the error type's size. This in turn keeps Result<T, E>
    // smaller and should lead to overall better codegen.
    kind: Box<ParseErrorKind>,
```

Clippy actually warns about this if the error type gets big enough (there's a commit about that). In this case, the error is a little beefy with `String`s in it and what not.

Usually this is overall justified because 1) error cases are usually rare and so the extra alloc is immaterial and 2) it shrinks the size of `Result<T, E>` which is in the common path.

---

_@konstin reviewed on 2024-01-04 15:47_

---

_Review comment by @konstin on `crates/pep440-rs/src/version_specifier.rs`:631 on 2024-01-04 15:47_

It's that clippy lint, makes sense!

---

_@BurntSushi reviewed on 2024-01-04 15:50_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version_specifier.rs`:1309 on 2024-01-04 15:50_

Hmmm. I personally find it rather tedious to update the user-visible error messages for tests like this. I also find the assertion failures clearer, since it prints the `Debug` representation of the type and thus gives the full details of what error value is sitting in memory. The display strings are somewhat divorced from that. Moreover, the `Display` impl of an error doesn't always show the full error message. (It does in this crate since I intentionally did not implement the [`Error::source`](https://doc.rust-lang.org/std/error/trait.Error.html#method.source) method to match the status quo.)

I could add some additional tests for each error variant on the human readable part, but leave the tests checking for the error cases themselves as-is. Would you be okay with that?

---

_@konstin reviewed on 2024-01-04 15:53_

---

_Review comment by @konstin on `crates/pep440-rs/src/version_specifier.rs`:1309 on 2024-01-04 15:53_

Yeah the updating is annoying, insta has been a great improvement for that. I'm fine with keeping the structs here as long as we keep the user facing error messages test in other tests.

---

_@BurntSushi reviewed on 2024-01-04 15:53_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version_specifier.rs`:214 on 2024-01-04 15:53_

Right, rustc can. I write them for the human. https://github.com/astral-sh/ruff/pull/8524#discussion_r1383902834

I wouldn't mind dropping them, but I do sincerely believe it leads to code that's easier to read. Specifically, it makes it clearer whether a particular name binding is a borrow or not.

---

_@BurntSushi reviewed on 2024-01-04 16:45_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version_specifier.rs`:1309 on 2024-01-04 16:45_

OK, this is done!

---

_Merged by @BurntSushi on 2024-01-04 17:28_

---

_Closed by @BurntSushi on 2024-01-04 17:28_

---

_Branch deleted on 2024-01-04 17:28_

---
