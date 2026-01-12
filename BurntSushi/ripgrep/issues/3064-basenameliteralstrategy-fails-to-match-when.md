```yaml
number: 3064
title: "`BasenameLiteralStrategy` fails to match when basename ends with `.`"
type: issue
state: closed
author: tjjfvi
labels: []
assignees: []
created_at: 2025-06-06T17:52:39Z
updated_at: 2025-06-06T17:54:55Z
url: https://github.com/BurntSushi/ripgrep/issues/3064
synced_at: 2026-01-12T16:13:25Z
```

# `BasenameLiteralStrategy` fails to match when basename ends with `.`

---

_@tjjfvi_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

globset 0.4.16

### How did you install ripgrep?

Cargo

### What operating system are you using ripgrep on?

N/A

### Describe your bug.

`globset` fails to match the path `foo.` for the glob `**/foo.`

I believe the offending code is here: https://github.com/BurntSushi/ripgrep/blob/master/crates/globset/src/pathutil.rs#L10-L12
```rs
    if path.last_byte().map_or(true, |b| b == b'.') {
        return None;
    }
```
`file_name` reports any path ending in the character `.` as having no file name; not just paths with a final component of `.` or `..`.

### What are the steps to reproduce the behavior?

```rs
fn main() {
    assert!(
        dbg!(globset::GlobSetBuilder::new()
            .add(globset::Glob::new("**/foo.").unwrap())
            .build())
        .unwrap()
        .matches("foo.")
        .len()
            == 1
    );
}
```

### What is the actual behavior?

The assertion fails, as there were zero matches.

### What is the expected behavior?

The assertion should succeed.

---

_Comment by @tjjfvi on 2025-06-06 17:54_

🤦 having a really hard time reading today. sorry for all the noise.

---

_Closed by @tjjfvi on 2025-06-06 17:54_

---
