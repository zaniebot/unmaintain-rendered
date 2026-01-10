```yaml
number: 1820
title: "Splitting up the `ruff_linter` crate for faster compile times"
type: issue
state: open
author: not-my-profile
labels:
  - internal
assignees: []
created_at: 2023-01-12T16:13:27Z
updated_at: 2023-11-08T10:54:15Z
url: https://github.com/astral-sh/ruff/issues/1820
synced_at: 2026-01-10T11:09:44Z
```

# Splitting up the `ruff_linter` crate for faster compile times

---

_Issue opened by @not-my-profile on 2023-01-12 16:13_

@squiddy commented on #1547 with:

> Slightly related: Any thoughts on splitting up the code into multiple crates? From my limited understanding this is one way to go for faster compiles. Multiple crates can be compiled in parallel. I did some profiling the other day and a release build takes quite some time because everything is one crate currently.

Crates can only be compiled in parallel if they don't depend on each other. Matklad has a [nice blog post "Fast Rust Builds"](https://matklad.github.io/2021/09/04/fast-rust-builds.html).

I just opened the PR #1816 to split off the CLI into a separate `ruff_cli` crate, since `ruff_cli` depends on `ruff` this doesn't really change the total build time (especially because `ruff_cli` can be compiled very quickly ... except the bin generation).

![image](https://user-images.githubusercontent.com/73739153/212118689-c8bd1b16-eb29-4b5f-ba5d-c53325b922f1.png)

![image](https://user-images.githubusercontent.com/73739153/212109241-4102b2a3-9334-4c0b-9d56-81c82a8f9c1c.png)

These graph were generated via `cargo build --release --timings`, see the [timing before the split](https://share.push-f.com/ruff-0.0.219-timing-release.html) and the [timing after the split](https://share.push-f.com/ruff-0.0.219-timing-release-post-split.html).

I think the question is if/how we could further split up the `ruff` library. Currently the rule implementations depend on `ast::Checker` and `ast::Checker` depends on the rule implementations. I am not sure what we can do about that.

With my PR #1816 `cargo build -p ruff` for the first time now builds ~100 dependencies fewer than `cargo build` for the first time (since it doesn't build the CLI), so this might still be neat for people who just want to contribute a lint and test that it works.

---

_Comment by @colin99d on 2023-01-12 16:27_

I think another benefit of this could be if we want to have certain features not always included for the pip install. For example,  #1628 wants to add spell checking which adds the typos dependency. Since we are adding a dependency and code that will only be used in one place, I feel like this is a great feature to keep in a separate crate.

---

_Comment by @not-my-profile on 2023-01-12 16:38_

We can easily introduce an optional dependency on `typos` via a [feature flag](https://doc.rust-lang.org/cargo/reference/features.html). This doesn't require a separate crate and I don't see any benefit to using a separate crate as opposed to a feature flag for something like that.

---

_Label `internal` added by @charliermarsh on 2023-01-12 18:16_

---

_Comment by @charliermarsh on 2023-01-20 23:48_

Dev compile times are really getting to me 😅 Have they regressed at all lately? Perhaps we haven't been tracking closely enough to answer that.

---

_Comment by @MichaReiser on 2023-01-21 13:06_

One benefit of separate crates is that it avoids re-compiling unchanged code because Rust skips crates where neither its dependencies nor modules have changed. 



---

_Comment by @charliermarsh on 2023-02-03 21:51_

As an initial proposal to tear down, would something like this work?

- One crate per linter (like `eradicate`).
- A crate for core, linter-agnostic definitions (like `Violation`, `Diagnostic`, etc.). This could include a trait to outline the `Checker` API, which the linter crates could depend on (but not its implementation).
- A crate that depends on all of those, and includes `Checker` -- so it ties together the linters, and the linter-agnostic definitions.
- A couple one-off crates that can exist as standalone libraries, like the current `source` crate that contains `Generator`, `Locator`, etc.
- (Not sure what to do with `Settings` and the per-linter settings structs.)


---

_Comment by @cnpryer on 2023-02-04 01:36_

> We can easily introduce an optional dependency on `typos` via a [feature flag](https://doc.rust-lang.org/cargo/reference/features.html). This doesn't require a separate crate and I don't see any benefit to using a separate crate as opposed to a feature flag for something like that.

This was my first thought. Has anyone started profiling some of these ideas? I don't think splitting ruff into more crates and introducing features are mutually exclusive, and it's probably a much bigger undertaking to do the former.

---

_Comment by @not-my-profile on 2023-02-04 06:07_

> Dev compile times are really getting to me

`cargo test -p ruff` is much faster than `cargo test` since it doesn't compile the `ruff_cli`. We could consider dropping `ruff_cli` from `default-members` in `Cargo.toml` to speed up `cargo test` when adding rule implementations ... but then `cargo run` wouldn't work anymore and you'd have to use `cargo run -p ruff_cli` (and the benefit would only apply when you just run `cargo test` but not `cargo run` but I still think it could be worth it).

> One crate per linter

I don't think we want to structure our code by linter at all. I think we rather want to group our AST rules by which AST node they target.

In general I think we should firstly improve the structure of our code within the `ruff` crate before thinking about how we can split it up.

>  Has anyone started profiling some of these ideas?

Reducing such dependencies won't do much for our dev compile time since these dependencies only need to be compiled once.

---

_Comment by @charliermarsh on 2023-02-04 13:16_

> I think we rather want to group our AST rules by which AST node they target.

I can think of reasons we wouldn't want to do this, but it feels unproductive to debate it right now. Most important is getting the codebase into a state at which we can start breaking it down :) which requires at least (1) changing the project structure (e.g., #2088 or similar) and (2) disentangling the mutual dependencies between rule implementations and `Checker` et al, plus other work I'm sure.


---

_Comment by @colin99d on 2023-02-04 13:19_

Will how we build this also determine how we will let people "hook" into ruff and build their own modules into our linter, like flake8 has? Also, once you guys decide on a plan feel free to assign me some refactoring work!

---

_Comment by @MichaReiser on 2023-02-22 15:00_

One thing that seems somewhat straightforward to achieve is to split out the `ast`, `source_code`, `cst` and `docstring` modules into a `ruff_python_syntax` crate (assuming I understood their purpose correctly and it make sense to group them in such crates). 

## Open questions

* `source_code` depends on `vendor`: Move `vendor` to `ruff_python_syntax` or introduce new `ruff_vendor` crate?
* `stylist` depends on `leading_quote` helper -> Move/extract
* A few `ast` helpers depend on `Checker`. Move to `ruff` and/or implement as extenion traits on `Checker`. 

I've probably overlooked a few dependencies but it seems less hard than others (e.g. extracting settings is.... challenging)

---

_Comment by @charliermarsh on 2023-03-02 04:54_

I am starting to hack on a few of these problems in https://github.com/charliermarsh/ruff/pull/3298, mostly to see what breaks and what problems we run into. (Not ready for review but feedback welcome, etc.)

---

_Renamed from "Splitting up the `ruff` crate for faster compile times" to "Splitting up the `ruff_linter` crate for faster compile times" by @konstin on 2023-11-08 10:54_

---
