```yaml
number: 12372
title: "Turn on --document-private-items in CI `cargo doc` run?"
type: issue
state: open
author: carljm
labels:
  - internal
  - ci
assignees: []
created_at: 2024-07-17T18:55:01Z
updated_at: 2024-07-18T00:15:15Z
url: https://github.com/astral-sh/ruff/issues/12372
synced_at: 2026-01-10T11:09:54Z
```

# Turn on --document-private-items in CI `cargo doc` run?

---

_Issue opened by @carljm on 2024-07-17 18:55_

If you don't use `--document-private-items`, then `cargo doc` won't do anything with doc comments in private modules or on private items; they can have all kinds of issues (e.g. link to nonexistent things) and CI won't catch it.

Given that our public docs use mkdocs, and our rustdoc comments are for our own internal maintenance use, not for documenting a public API, it might make more sense to have our CI use `--document-private-items`?

This will require a bit of work to clean up a bunch of existing problems in non-public doc comments that were never caught because we don't use `--document-private-items`.

One thing worth noting: even with `--document-private-items`, it's still a warning (which for our CI run means an error, since we set `RUSTDOCFLAGS='-D warnings'`) for a public item's docs to link to a private item. I think this is a good thing.

---

_Renamed from "Turn on --document-private-items and deny warnings in CI `cargo doc` run?" to "Turn on --document-private-items in CI `cargo doc` run?" by @carljm on 2024-07-17 18:55_

---

_Label `internal` added by @carljm on 2024-07-17 18:55_

---

_Label `ci` added by @carljm on 2024-07-17 18:55_

---

_Comment by @carljm on 2024-07-17 19:18_

cc @MichaReiser @BurntSushi for opinions here

---

_Comment by @BurntSushi on 2024-07-18 00:15_

Just to make sure I understand here, this is just about a CI run that does a check that `cargo doc --document-private-items` actually works? I don't think I have a strong opinion on that personally, and your reasoning seems good to me. I very rarely use that flag though, so I don't have a ton of experience with it.

---
