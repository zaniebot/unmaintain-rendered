```yaml
number: 2973
title: Overrides are order sensitive
type: issue
state: closed
author: alpaylan
labels: []
assignees: []
created_at: 2025-01-15T07:18:40Z
updated_at: 2025-01-15T12:06:13Z
url: https://github.com/BurntSushi/ripgrep/issues/2973
synced_at: 2026-01-12T16:13:25Z
```

# Overrides are order sensitive

---

_@alpaylan_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

I'm using `ignore = "0.4.23"` installed by Cargo as a library.

### How did you install ripgrep?

Cargo

### What operating system are you using ripgrep on?

macOS 13.2.1

### Describe your bug.

Overrides in the ignore crate are order sensitive, so if a later pattern includes a previously ignored pattern, the pattern will not be ignored.

```
!src/lib.rs
**/*.rs
```

will have a different behavior than,

```
**/*.rs
!src/lib.rs
```

as the first does not ignore `src/lib.rs` but the second does.

### What are the steps to reproduce the behavior?

```
fn main() {
    let mut overrides = OverrideBuilder::new(".");
    overrides.add("!a.txt")?;
    overrides.add("**/*.txt")?;

    overrides.build()?.matched("!a.txt", false) 
    // this returns 
    // Whitelist(Glob(Matched(Glob { from: None, original: "**/*.rs", actual: "**/*.rs", is_whitelist: false, is_only_dir: false })))

    let mut overrides = OverrideBuilder::new(".");
    overrides.add("**/*.txt")?;
    overrides.add("!a.txt")?;

    overrides.build()?.matched("!a.txt", false) 
    // this returns 
    // Ignore(Glob(Matched(Glob { from: None, original: "!src/lib.rs", actual: "src/lib.rs", is_whitelist: true, is_only_dir: false })))
}

### What is the actual behavior?

-

### What is the expected behavior?

I think this should either be documented(I assume that's this is the actual behavior of gitignore too), or if not, my previous expectation was that the strictest pattern would win in ignore/match cases.

---

_Comment by @BurntSushi on 2025-01-15 12:06_

This is absolutely intended. Overrides use the same semantics as gitignore.

I also think this is by far the most sensible behavior. Otherwise you have to come up with a notion of "strictness." It might be obvious in this case, but I have no idea how to do that in general.

---

_Closed by @BurntSushi on 2025-01-15 12:06_

---
