```yaml
number: 2872
title: "Add `rome_formatter` fork as `ruff_formatter`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/formatter-i
created_at: 2023-02-13T23:30:33Z
updated_at: 2023-02-15T07:55:41Z
url: https://github.com/astral-sh/ruff/pull/2872
synced_at: 2026-01-12T04:52:01Z
```

# Add `rome_formatter` fork as `ruff_formatter`

---

_Pull request opened by @charliermarsh on 2023-02-13 23:30_

The Ruff autoformatter is going to be based on an intermediate representation (IR) formatted via [Wadler's algorithm](https://homepages.inf.ed.ac.uk/wadler/papers/prettier/prettier.pdf). This is architecturally similar to [Rome](https://github.com/rome/tools), Prettier, [Skip](https://github.com/skiplang/skip/blob/master/src/tools/printer/printer.sk), and others.

This PR adds a fork of the `rome_formatter` crate from [Rome](https://github.com/rome/tools), renamed here to `ruff_formatter`, which provides generic definitions for a formatter IR as well as a generic IR printer. (We've also pulled in `rome_rowan`, `rome_text_size`, and `rome_text_edit`, though some of these will be removed in future PRs.)

Why fork? `rome_formatter` contains code that's specific to Rome's AST representation (e.g., it relies on a fork of rust-analyzer's `rowan`), and we'll likely want to support different abstractions and formatting capabilities (there are already a few changes coming in future PRs). Once we've dropped `ruff_rowan` and trimmed down `ruff_formatter` to the code we currently need, it's also not a huge surface area to maintain and update.


---

_Label `autoformatter` added by @charliermarsh on 2023-02-13 23:30_

---

_Comment by @charliermarsh on 2023-02-13 23:31_

\cc @MichaReiser 

---

_Review comment by @MichaReiser on `crates/ruff_formatter/Cargo.toml`:5 on 2023-02-14 08:43_

You may want to change the authors to Ruff. 

---

_@MichaReiser reviewed on 2023-02-14 08:48_

LGTM

Some things to consider:

* Should we re-license the crates 
* Can we document the use of prior work in an acknowledgment section (pylint, Rome, Prettier...)
* It's probably worth unifying the logging infrastructure. Rome uses `tracing` whereas Ruff uses some other crates. 

---

_@charliermarsh reviewed on 2023-02-14 23:20_

---

_Review comment by @charliermarsh on `crates/ruff_formatter/Cargo.toml`:5 on 2023-02-14 23:20_

I wasn't certain what the "right" pattern was here. I noticed (e.g.) that `rome_rowan` has `authors = ["Aleksey Kladov <aleksey.kladov@gmail.com>"]`.

---

_Comment by @charliermarsh on 2023-02-14 23:24_

> Should we re-license the crates

Ah yeah. How should this work? We already include a bunch of licenses in our `LICENSE` file -- could we add the existing licenses there, and then cover these forks under the project's MIT license?

> Can we document the use of prior work in an acknowledgment section (pylint, Rome, Prettier...)

Good idea -- I'll do this in a separate PR.

> It's probably worth unifying the logging infrastructure. Rome uses tracing whereas Ruff uses some other crates.

Also a good idea -- I'll also do this in a separate PR.


---

_Merged by @charliermarsh on 2023-02-15 00:22_

---

_Closed by @charliermarsh on 2023-02-15 00:22_

---

_Branch deleted on 2023-02-15 00:22_

---

_Comment by @MichaReiser on 2023-02-15 07:55_

> Ah yeah. How should this work? We already include a bunch of licenses in our LICENSE file -- could we add the existing licenses there, and then cover these forks under the project's MIT license?

I'm not really familiar with re-licensing but adding another copyright seems to work (if that's even something we want)

https://gist.github.com/fbaierl/1d740a7925a6e0e608824eb27a429370

---
