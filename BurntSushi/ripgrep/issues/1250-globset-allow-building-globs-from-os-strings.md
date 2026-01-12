```yaml
number: 1250
title: "globset: allow building globs from OS strings"
type: issue
state: open
author: BurntSushi
labels:
  - enhancement
assignees: []
created_at: 2019-04-16T16:47:49Z
updated_at: 2019-04-16T23:53:52Z
url: https://github.com/BurntSushi/ripgrep/issues/1250
synced_at: 2026-01-12T16:13:23Z
```

# globset: allow building globs from OS strings

---

_@BurntSushi_

Initially reported here: https://github.com/rust-lang-nursery/glob/issues/78

While globset can _match_ on arbitrary OS strings, it cannot accept an arbitrary OS string as a glob. The link above gives an example. Here it is in code form (which currently prints `false`):

```rust
use std::error::Error;
use std::ffi::OsStr;
use std::os::unix::ffi::OsStrExt;

use globset::Glob;

fn main() -> Result<(), Box<dyn Error>> {
    let glob = Glob::new(r"d\xE9cembre 2018 - *.jpg")?;
    let matcher = glob.compile_matcher();
    let filename = OsStr::from_bytes(b"d\xE9cembre 2018 - foo.jpg");
    println!("{:?}", matcher.is_match(filename));
    Ok(())
}
```

To be clear, this _particular_ program shouldn't succeed because a UTF-8 string is given to `Glob::new`. But there should be a way to, for example, call `Glob::new(some_os_str)`. On Unix, that OS string can be arbitrary bytes.

(The globset crate is due for an API refresh and likely a rewrite of its internal parser anyway. This should be fairly easy to support.)

---

_Label `enhancement` added by @BurntSushi on 2019-04-16 23:53_

---
