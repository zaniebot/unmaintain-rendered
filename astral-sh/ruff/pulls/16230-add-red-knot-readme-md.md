```yaml
number: 16230
title: "Add `red_knot/README.md`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: rk-readme
created_at: 2025-02-18T13:02:15Z
updated_at: 2025-02-19T08:12:11Z
url: https://github.com/astral-sh/ruff/pull/16230
synced_at: 2026-01-10T19:57:23Z
```

# Add `red_knot/README.md`

---

_Pull request opened by @InSyncWithFoo on 2025-02-18 13:02_

## Summary

Resolves #15979.

The file explains what Red Knot is (a type checker), what state it is in (not yet ready for user testing), what its goals ("extremely fast") and non-goals (not a drop-in replacement for other type checkers) are as well as what the crates contain.

## Test Plan

None.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-02-18 13:02_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-02-18 13:02_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-02-18 13:02_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-02-18 13:02_

---

_Label `documentation` added by @AlexWaygood on 2025-02-18 13:02_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-18 13:02_

---

_Comment by @InSyncWithFoo on 2025-02-18 13:03_

The "Contributing" section is rather crude. I'm not sure what to add other than a link to open issues.

---

Also, what does `red_knot_wasm` do?

---

_Review comment by @AlexWaygood on `crates/red_knot/README.md`:3 on 2025-02-18 13:06_

well... we _hope_ it will be. I'm not sure I'm comfortable saying that it is "extremely fast" relative to other type checkers _yet_, because there's still so much that they do that we don't.

---

_Review comment by @AlexWaygood on `crates/red_knot/README.md`:8 on 2025-02-18 13:07_

While this is true, I worry that this might be the wrong emphasis. We certainly won't be a drop-in replacement for either tool in every respect, but it's important to us that it's not too onerous for users to switch from one of those tools to red-knot

---

_Review comment by @AlexWaygood on `crates/red_knot/README.md`:20 on 2025-02-18 13:08_

I worry that this might get easily out of date. We're still at the stage where we're often doing quite sweeping reorganisation of code. Also, you're missing `ruff_db` from the list :-)

---

_Review comment by @AlexWaygood on `crates/red_knot/README.md`:21 on 2025-02-18 13:08_

I think this is a great idea! Doesn't seem that crude to me

---

_@AlexWaygood reviewed on 2025-02-18 13:08_

Thank you!

---

_@InSyncWithFoo reviewed on 2025-02-18 13:13_

---

_Review comment by @InSyncWithFoo on `crates/red_knot/README.md`:3 on 2025-02-18 13:13_

And yet this tagline is already so prominently displayed:

```shell
$ red_knot --help
An extremely fast Python type checker.

Usage: red_knot.exe <COMMAND>

Commands:
  check    Check a project for type errors
  server   Start the language server
  version  Display Red Knot's version
  help     Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```

Might as well go all the way, I suppose.

---

_@InSyncWithFoo reviewed on 2025-02-18 13:21_

---

_Review comment by @InSyncWithFoo on `crates/red_knot/README.md`:8 on 2025-02-18 13:21_

I can't think of a better phrasing. What do you suggest?

---

_@InSyncWithFoo reviewed on 2025-02-18 13:21_

---

_Review comment by @InSyncWithFoo on `crates/red_knot/README.md`:20 on 2025-02-18 13:21_

Added. Updating should be easy enough.

---

_@AlexWaygood reviewed on 2025-02-18 13:48_

---

_Review comment by @AlexWaygood on `crates/red_knot/README.md`:3 on 2025-02-18 13:48_

Heh, fair enough, I suppose. I guess we're not displaying comparative benchmarks yet, anyway. I'd like to check other members of the team are also okay with this, however.

---

_@AlexWaygood reviewed on 2025-02-18 13:48_

---

_Review comment by @AlexWaygood on `crates/red_knot/README.md`:20 on 2025-02-18 13:48_

Updating it is easy, sure. Remembering to update it is hard, however. I think it's pretty likely we'll forget to update it.

---

_@AlexWaygood reviewed on 2025-02-18 13:54_

---

_Review comment by @AlexWaygood on `crates/red_knot/README.md`:8 on 2025-02-18 13:54_

Probably something like

```
While red-knot will produce similar results to mypy and pyright on many codebases,
100% compatibility with these tools is a non-goal. On some codebases, red-knot's design decisions
lead to different outcomes than you would get from running one of these more established tools.
```

---

_@InSyncWithFoo reviewed on 2025-02-18 14:05_

---

_Review comment by @InSyncWithFoo on `crates/red_knot/README.md`:20 on 2025-02-18 14:05_

It's not like anyone other than Red Knot contributors will read this, either, so a rough/slightly outdated guideline shouldn't be too much of a disaster (updating it could be a good first issue, even).

---

_@MichaReiser reviewed on 2025-02-18 17:09_

---

_Review comment by @MichaReiser on `crates/red_knot/README.md`:3 on 2025-02-18 17:09_

I'm fine with it and it's easy to change if it turns out that red knot is slow (I think it's about 30x faster on my machine when comparing black)

---

_@MichaReiser reviewed on 2025-02-18 17:10_

---

_Review comment by @MichaReiser on `crates/red_knot/README.md`:20 on 2025-02-18 17:10_

I'd prefer to remove it as well. Most of it is either obvious from the name or documented in the project READMEs. 

---

_Comment by @carljm on 2025-02-19 07:30_

> what does `red_knot_wasm` do?

It was the start of an effort to support compiling red-knot under WASM, so we could have an online red-knot playground, like the ruff playground. It's on hold because we need to remove Salsa's reliance on catch-unwind before it can be WASM compatible. My work on fixpoint iteration support in Salsa does this.

---

_Comment by @carljm on 2025-02-19 07:30_

Looks good to me! Merging.

---

_Merged by @carljm on 2025-02-19 07:31_

---

_Closed by @carljm on 2025-02-19 07:31_

---

_Branch deleted on 2025-02-19 08:12_

---
