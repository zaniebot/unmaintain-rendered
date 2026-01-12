```yaml
number: 3310
title: "Remove old `define_violation!` (in favor of `#[violation]`)"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: proc-macro-experimenting
created_at: 2023-03-02T19:29:29Z
updated_at: 2023-03-06T17:26:19Z
url: https://github.com/astral-sh/ruff/pull/3310
synced_at: 2026-01-12T04:39:44Z
```

# Remove old `define_violation!` (in favor of `#[violation]`)

---

_Pull request opened by @konstin on 2023-03-02 19:29_

Though this would be good way to play around with the code a bit more, ended up changing every single rule.

Regex used:

```
define_violation!\(\n((    .*\n)*)(    pub struct .+(;|\{(\n    .*)*?\})\n)\);
```

```
$1#[violation]
$3
```

If you cherry pick only the first two commits, we avoid breaking everyone else's rule additions (and can merge commit 3 later)

---

_Converted to draft by @konstin on 2023-03-02 19:35_

---

_Renamed from "WIP: Replace `define_violation!` with `#[violation]`" to "Replace `define_violation!` with `#[violation]`" by @konstin on 2023-03-02 19:35_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/eradicate/rules.rs`:23 on 2023-03-02 20:23_

Nice! So we're using an attribute macro rather than a function-like macro. This does strike me as more intuitive. 

What would be the motivation to move from an attribute macro to a derive macro? How would that differ?

---

_@charliermarsh reviewed on 2023-03-02 20:23_

---

_Review comment by @konstin on `crates/ruff/src/rules/eradicate/rules.rs`:23 on 2023-03-03 09:42_

A derive macro can only define the contents of an impl block. So we would first define a trait for, either changing `Violation` or a new `ViolationExplanation` and then use it while also adding the other derives manually:

```rust
#[derive(Violation, Debug, PartialEq, Eq, serde::Serialize, serde::Deserialize)]
/// # Why is this bad
/// Has a rather stale taste
struct EatLaundry;
```

We could also go all the way refactoring and do a proper trait hierarchy. Note that we still need two proc macros and traits because we need to parse one method and create another.

```rust
trait ViolationMessage {
    fn message(&self) -> String;
    fn derived_messages(&self) -> Vec<&'static str>;
    fn autofix_title(&self) -> String {
        ""
    }
}

trait Violation: ViolationMessage + Debug + PartialEq + Eq + serde::Serialize + serde::Deserialize {
    fn explanation() -> String;
}

#[derive(Violation, Debug, PartialEq, Eq, serde::Serialize, serde::Deserialize)]
/// # Why is this bad
/// Has a rather stale taste
struct EatLaundry;

#[violation_messages] // Adds derived_messages()
impl ViolationMessage for EatLaundry {
    fn message(&self) -> String {
        format!("Found laundry-eating behaviour")
    }
    fn autofix_title(&self) -> String {
        "Makes the code munch on tokens instead".to_string()
    }
}
```

A more elegant alternative would be what [thiserror does](https://docs.rs/thiserror/latest/thiserror/), but that would be an awful lot of work for just an internal helper macro (unless [miette](https://docs.rs/miette/latest/miette/) or something gets us that for free, but i haven't checked and i'd be surprised if there was a perfect fit already existing):

```rust
use thiserror::Error;

#[derive(Error, Debug)]
pub enum DataStoreError {
    #[error("data store disconnected")]
    Disconnect(#[from] io::Error),
    #[error("the data for key `{0}` is not available")]
    Redaction(String),
    #[error("invalid header (expected {expected:?}, found {found:?})")]
    InvalidHeader {
        expected: String,
        found: String,
    },
    #[error("unknown data store error")]
    Unknown,
}
```


---

_@konstin reviewed on 2023-03-03 09:42_

---

_Marked ready for review by @konstin on 2023-03-03 17:34_

---

_@MichaReiser approved on 2023-03-03 18:11_

---

_@charliermarsh approved on 2023-03-03 18:28_

---

_Comment by @konstin on 2023-03-06 11:00_

i'll cherry-pick a60a0a2c83240404c57a586b4291fc81a73a3c27 in a couple of days

---

_Comment by @charliermarsh on 2023-03-06 13:09_

I think my preference would be to merge this sooner rather than later and just require that open PRs etc. rebase as appropriate. What do you think? I tend to prefer hard removals over soft deprecations for internal code, since a soft deprecation leaves the codebase in a somewhat inconsistent state _and_ runs the risk that users continue to add new code that relies on the deprecated functionality anyway.

---

_Comment by @konstin on 2023-03-06 15:09_

generally i agree, in this case it's just that this breaks every single rule addition started before this was merged, so i'd rather wait just a bit

---

_Comment by @charliermarsh on 2023-03-06 15:29_

Makes sense, although we have to fix them anyway, and we _don't_ want new rules to be added with the old macro, right? So is it just a question of whether we fix them now, or when we have time to do so?

---

_Renamed from "Replace `define_violation!` with `#[violation]`" to "Remove old `define_violation!` (in favor of `#[violation]`)" by @konstin on 2023-03-06 15:36_

---

_Comment by @konstin on 2023-03-06 15:36_

yeah, my plan was to see if there are any new contributors who still use the old syntax, merge them, run my regex migration again and then remove the old code so they don't get conflicts with main on their contributions. If you don't want to wait you can also merge this PR now

---

_Comment by @charliermarsh on 2023-03-06 15:52_

Yeah that makes sense. I guess my perspective is that it can be a little bit of a never-ending battle because even if we do that, then after we merge, people may submit PRs that they started prior to this merge, which still use the old syntax, and so on.

I'll fix conflicts and merge if that's cool with you.

---

_Comment by @konstin on 2023-03-06 15:55_

> I'll fix conflicts and merge if that's cool with you.

just wanted to start fixing them, thanks

---

_Merged by @charliermarsh on 2023-03-06 17:00_

---

_Closed by @charliermarsh on 2023-03-06 17:00_

---

_Branch deleted on 2023-03-06 17:26_

---
