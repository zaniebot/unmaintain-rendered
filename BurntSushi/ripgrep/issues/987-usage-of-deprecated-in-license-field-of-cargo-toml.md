```yaml
number: 987
title: "Usage of deprecated `/` in `license` field of Cargo.toml"
type: issue
state: closed
author: awaitlink
labels:
  - enhancement
assignees: []
created_at: 2018-07-20T15:53:43Z
updated_at: 2018-07-21T17:25:56Z
url: https://github.com/BurntSushi/ripgrep/issues/987
synced_at: 2026-01-12T16:13:22Z
```

# Usage of deprecated `/` in `license` field of Cargo.toml

---

_@awaitlink_

In [`Cargo.toml`](https://github.com/BurntSushi/ripgrep/blob/master/Cargo.toml#L16):

```toml
license = "Unlicense/MIT"
```

But [The Manifest Format](https://doc.rust-lang.org/cargo/reference/manifest.html#package-metadata) docs say that:
```toml
# Multiple licenses can be separated with a `/`, although that usage
# is deprecated.  Instead, use a license expression with AND and OR
# operators to get more explicit semantics.
license = "..."
```

---

_Comment by @BurntSushi on 2018-07-20 16:27_

Cargo doesn't emit any deprecation warning, so as far as I'm concerned, there is either a bug in the Cargo implementation or the manifest format documentation.

But I don't really care what format we use, as long as we pick one and stick with it. If someone cares enough to submit a PR, then great, but otherwise this isn't worth tracking IMO.

---

_Closed by @BurntSushi on 2018-07-20 16:27_

---

_Comment by @awaitlink on 2018-07-21 08:01_

Well, although Cargo does not produce a warning, this is not a bug. Usage of `/` is deprecated in rust-lang/cargo#4898:

> ...deprecated the `/` syntax, since it was not clear whether that was conjunctive (like `AND`) or disjunctive (like `OR`).

In that pull request, there is also a note about warnings:

> It's possible that crates.io will want to warn authors about their use of deprecated identifiers or syntax (e.g. the `/` I've deprecated here) so they can upgrade before the deprecated element is dropped (probably years after the initial deprecation).


---

_Label `enhancement` added by @BurntSushi on 2018-07-21 17:25_

---

_Reopened by @BurntSushi on 2018-07-21 17:25_

---

_Closed by @BurntSushi on 2018-07-21 17:25_

---
