```yaml
number: 2608
title: "globset: support Bash extended globbing"
type: issue
state: closed
author: calavera
labels:
  - wontfix
assignees: []
created_at: 2023-09-13T00:14:35Z
updated_at: 2023-09-13T10:12:37Z
url: https://github.com/BurntSushi/ripgrep/issues/2608
synced_at: 2026-01-12T16:13:24Z
```

# globset: support Bash extended globbing

---

_@calavera_

#### Describe your feature request

Bash supports a feature called ["Extended Globbing"](https://www.linuxjournal.com/content/bash-extended-globbing) that allows you to do group matching with a syntax similar to regular expressions.

I was wondering if you'd consider adding it to the Globset crate to be able to match expressions with that syntax. These are the patterns that Bash supports:

```
  ?(pattern-list)   Matches zero or one occurrence of the given patterns
  *(pattern-list)   Matches zero or more occurrences of the given patterns
  +(pattern-list)   Matches one or more occurrences of the given patterns
  @(pattern-list)   Matches one of the given patterns
  !(pattern-list)   Matches anything except one of the given patterns
```

I would expect that GlobSet matched expressions based on those patterns. This is an example on how I'd expect to work with this feature enabled:

```rs
const EXPR: &str = "main.@(rs|rb)";

fn main() {
    let glob = globset::GlobBuilder::new(EXPR)
	.extended_globbing(true)
        .build()
        .unwrap()
	.compile_matcher();
    assert!(glob.is_match("main.rs"));
    assert!(glob.is_match("main.rb"));
}
```

I can try to implement it myself with a little guidance on how to approach the codebase.


---

_Comment by @BurntSushi on 2023-09-13 00:19_

I don't think so. I'd rather stick to the standard globbing features. IMO, if you really benefit from this extended syntax, it would probably just be better to use regexes.

I'll also note that `!(pattern-list)` is likely pretty tricky to do. `globset` works by converting the glob to a regular expression, and seemingly the `!(pattern-list)` syntax would require a regex engine that supports complement. Such engines are quite rare, and I'm not aware of any that would be appropriate to include in ripgrep or globset. (Even if I was willing to include another regex engine in the first place, which I don't think I am.)

---

_Comment by @calavera on 2023-09-13 03:04_

That's fair, thanks for the input! 👍 

---

_Closed by @calavera on 2023-09-13 03:04_

---

_Label `wontfix` added by @BurntSushi on 2023-09-13 10:12_

---
