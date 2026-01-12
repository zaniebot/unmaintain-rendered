```yaml
number: 376
title: "regex behaviour modified from regex::Regex ?"
type: issue
state: closed
author: jedahan
labels: []
assignees: []
created_at: 2017-02-22T16:56:39Z
updated_at: 2017-03-12T21:02:15Z
url: https://github.com/BurntSushi/ripgrep/issues/376
synced_at: 2026-01-12T16:13:21Z
```

# regex behaviour modified from regex::Regex ?

---

_@jedahan_

I tried this in play.integer32.com :
```rust
extern crate regex;
use regex::Regex;

fn main() {
    let re = Regex::new(r"CurrentCapacity.*?([0-9]+)").unwrap();
    let text = "      \"CurrentCapacity\" = 420";
    let mat = re.find(text).unwrap();
    for cap in re.captures_iter(text) {
        println!("{}", &cap[1]);
    }
}
```

and it seemed to work (printed **420**)

Then I tried in my shell:

```sh
echo '      "CurrentCapacity" = 420' | rg -N 'CurrentCapacity.*?(\d+)' --replace '$1'
```
and it returned

```sh
     "420
```

---

_Comment by @jedahan on 2017-02-22 16:59_

I searched in older issues, and found https://github.com/BurntSushi/ripgrep/issues/320 , which does what I wanted. I wonder if theres a spot to add this to the documentation, or somewhere about an FAQ and --only-matching, or if this is just a dumb mistake.

```sh
rg -N '.*?CurrentCapacity.*?(\d+)' --replace '$1'
```

If theres no need to document, feel free to close.

---

_Comment by @BurntSushi on 2017-03-12 21:02_

The issue here is that the `-r/--replace` flag replaces *each* occurrence of a match with the given substitution string. This provides the most flexibility, but is annoying to deal with for simpler use cases such as yours. I think the `-o/--only-matching` flag should solve this.

---

_Closed by @BurntSushi on 2017-03-12 21:02_

---
