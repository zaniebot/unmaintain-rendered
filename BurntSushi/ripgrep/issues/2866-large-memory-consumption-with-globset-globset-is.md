```yaml
number: 2866
title: "Large memory consumption with `globset::GlobSet.is_match`"
type: issue
state: closed
author: iesahin
labels: []
assignees: []
created_at: 2024-08-06T09:10:56Z
updated_at: 2024-08-27T17:42:24Z
url: https://github.com/BurntSushi/ripgrep/issues/2866
synced_at: 2026-01-12T16:13:25Z
```

# Large memory consumption with `globset::GlobSet.is_match`

---

_@iesahin_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

globset = "0.4"

### How did you install ripgrep?

cargo add globset

### What operating system are you using ripgrep on?

macOS 14.5 (reproduced this with Linux as well)

### Describe your bug.

This is about globset crate. Sorry if I'm reporting it at the wrong place. 

I'm building [a project](https://github.com/iesahin/xvc/) where you can track binary files on top of Git. 

I'm using globset in [Xvc](https://github.com/iesahin/xvc) for `check-ignore` functionality, mostly around: https://github.com/iesahin/xvc/blob/develop/walker/src/ignore_rules.rs

Xvc uses `.gitignore` to hide files it tracks from Git. I have a large repository with ~3000 lines in its `.gitignore`. 

When I run a command which uses ignore files, I noticed memory consumption jumps to 10+GB from ~50MB. I traced the memory consumption line by line.

```rust
    watch!(PEAK_ALLOC.current_usage_as_mb());
    let result = if ignore_rules.whitelist_set.read().unwrap().is_match(&path) {
        MatchResult::Whitelist
    } else if ignore_rules.ignore_set.read().unwrap().is_match(&path) {
        MatchResult::Ignore
    } else {
        MatchResult::NoMatch
    };
    watch!(PEAK_ALLOC.current_usage_as_mb());
```

In this code, `ignore_set` and `whitelist_set` are `Arc<RwLock<GlobSet>>`. The first `watch!` and second `watch!` results are:

```
[TRACE][walker/src/lib.rs::798] PEAK_ALLOC.current_usage_as_mb(): 59.06844
[TRACE][walker/src/lib.rs::806] PEAK_ALLOC.current_usage_as_mb(): 10988.586
```

Two `GlobSet`s are built before these lines and only `is_match` is called. 

This is likely something on my end, but I wanted to ask whether you have observed this or 3000 ignore rules are something unexpected. I'll continue to debug this and may go inside the crate as well, but I'd like to know if this is a known issue.

Thanks a lot. 

### What are the steps to reproduce the behavior?

I can submit the file I'm testing and other detailed instructions if you think this could be something with globset

### What is the actual behavior?

The memory consumption from `is_match` method is +10GB. 

### What is the expected behavior?

The memory consumption shouldn't change much between those lines

---

_Comment by @BurntSushi on 2024-08-06 10:50_

Please provide an MRE.

---

_Comment by @iesahin on 2024-08-27 17:42_

Replaced dependency with another lib. I'll provide an MRE if my time permits. Closing this for now. Thanks. 

---

_Closed by @iesahin on 2024-08-27 17:42_

---
