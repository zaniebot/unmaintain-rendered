```yaml
number: 2603
title: Return which regex pattern was successful
type: issue
state: closed
author: Akash-Mair
labels: []
assignees: []
created_at: 2023-08-30T14:57:42Z
updated_at: 2023-08-30T15:10:28Z
url: https://github.com/BurntSushi/ripgrep/issues/2603
synced_at: 2026-01-12T16:13:24Z
```

# Return which regex pattern was successful

---

_@Akash-Mair_

Hey! 

I have a lot of regex patterns I'd like to apply on a file, this leds to something like:

```
rg --json --regexp <pattern> --regexp <pattern> .... <path>
```
This works great returning a bunch of matches but in the match json there is no way to identify which regex pattern/s led to the match.

If there is a way to do this already could you let me know and I'll close this issue.

Thanks so much :) 


---

_Comment by @BurntSushi on 2023-08-30 15:10_

Duplicate of #2471.

The good news is that the changes to the `regex` crate I talked about in #2471 have landed and you can now write a Rust program to search for multiple regexes simultaneously in a way that will tell you which matched. I should caution you though that it isn't magic and it can't scale to arbitrarily many regexes. But I have no plans to add this functionality to ripgrep at present.

Another technique is to use a crate like [`aho-corasick`](https://docs.rs/aho-corasick) to do some part of the matching for you because it scales better.

But I'll stop here. If you want to talk more about the regex engine, we should move it to here: https://github.com/rust-lang/regex/discussions

---

_Closed by @BurntSushi on 2023-08-30 15:10_

---
