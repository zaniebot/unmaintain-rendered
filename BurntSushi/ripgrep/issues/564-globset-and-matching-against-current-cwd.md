```yaml
number: 564
title: globset and matching against current cwd
type: issue
state: closed
author: mqudsi
labels:
  - question
assignees: []
created_at: 2017-07-24T18:05:41Z
updated_at: 2017-07-24T21:25:21Z
url: https://github.com/BurntSushi/ripgrep/issues/564
synced_at: 2026-01-12T16:13:22Z
```

# globset and matching against current cwd

---

_@mqudsi_

It seems that `globset` requires pathed filters, i.e. `*.jpg` does not match `./something.jpg`, instead, the filter `./*.jpg` must be used.

Currently, I pass all expressions to a function called `fix_filters` first before passing them to the glob builder, then I pass only "pathed" values to globset.is_match (i.e. either a path starting with `/` (absolute path) or `.` for relative paths like ../something or ./something).

_If_ globset is intended to be used for paths and not general strings, then you might consider doing that automatically? Or else modifying the regexp to match `^(|./)` for unpathed expressions?

---

_Comment by @BurntSushi on 2017-07-24 20:53_

Could you please show some code the reproduces the issue? I cannot reproduce the issue. The following program

```rust
extern crate globset;

use globset::Glob;

fn main() {
    let g = Glob::new("*.jpg").unwrap().compile_matcher();
    println!("{:?}", g.is_match("./something.jpg"));
    println!("{:?}", g.is_match("something.jpg"));
}
```

outputs

```
$ cargo run
true
true
```

Are you perhaps enabling the [`literal_separator`](https://docs.rs/globset/0.2.0/globset/struct.GlobBuilder.html#method.literal_separator) option? (It's disabled by default.) Because if so, then the behavior your described is expected.

---

_Label `question` added by @BurntSushi on 2017-07-24 20:53_

---

_Comment by @mqudsi on 2017-07-24 21:23_

Ah, I have `literal_separator` set because I only want to match files in cwd.
I guess that explains it.

---

_Closed by @BurntSushi on 2017-07-24 21:25_

---
