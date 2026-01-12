```yaml
number: 562
title: " Globset directory iteration hint?"
type: issue
state: closed
author: mqudsi
labels:
  - duplicate
assignees: []
created_at: 2017-07-20T23:28:50Z
updated_at: 2017-10-22T01:26:19Z
url: https://github.com/BurntSushi/ripgrep/issues/562
synced_at: 2026-01-12T16:13:22Z
```

#  Globset directory iteration hint?

---

_@mqudsi_

I've been working with the original `glob` for a while and have realized it has some severe shortcomings. I was hoping to contribute some changes to `globset` that would allow it to fill in for `glob` entirely.

A question I've had pertains to performance; currently it does not seem that there's any way to find out via an api call whether or not there is a point in entering a directory to attempt to match against its children. Is this correct?

To illustrate: given a glob `**/*.rs` and a cwd of `./`, one call `read_dir` on `./src` to see if it has any children that match `**/*.rs`.

But if the glob is `*.rs`, it's obviously not necessary to enter any subdirectories, this directory can't match.
Similarly, if `require_literal_separator` is true, there's no need to enter a subdirectory of `src/` to search for `*.rs` files - that glob won't match.

Does globset expose this functionality so that it can be consumed from another crate/library?

---

_Comment by @BurntSushi on 2017-07-21 00:00_

Is this a duplicate of #546? (If it isn't, then I don't think I understand what you're asking for.)

What problem are you trying to solve?

---

_Comment by @mqudsi on 2017-07-21 00:01_

I think it is. Although I use ripgrep, this was a general question about the `globset` crate and not about `ripgrep`.

Thanks.

---

_Comment by @BurntSushi on 2017-07-21 00:07_

Sure ya, but fixing that issue probably means solving it at the glob level I'm guessing.

I'm still curious to hear the problem you're trying to solve though. Does the glob crate have this functionality?

---

_Comment by @mqudsi on 2017-07-21 00:15_

The glob crate does directory walking for you (`glob::glob_with`), so the dev doesn't need to think about whether or not the result is optimized.

I imagine it'll be tough to find all the corner cases, but something as simple as 

```rust
fn max_depth(&self) -> Option<int> {
    if !self.require_literal_separator() {
        return None();
    }
    return Some(self.max_slash_count());
}
```

where `max_slash_count()` is the maximum number of `/` instances in a single glob within the globset.

(I'm sure I'm missing a corner case here, but anyway) would suffice as a basic performance improvement for an attempt at walking a tree while matching against a globset.

I'm basically trying to avoid needlessly recursing into directories and matching futilely against the globset if we can know in advance that the contents of subdirectory x or any file past depth y will not match.

---

_Comment by @BurntSushi on 2017-10-22 01:10_

I think I'm going to let #546 track this issue. I think the problem of "don't descend into directories when you know there will be no matches" is worth solving, but it does seem like a quite complicated optimization that requires a lot of testing. It's not something I plan on working on any time soon.

---

_Closed by @BurntSushi on 2017-10-22 01:10_

---

_Label `duplicate` added by @BurntSushi on 2017-10-22 01:10_

---

_Comment by @mqudsi on 2017-10-22 01:26_

I almost feel like the optimal mapping between whitelist/blacklist and a list of globs is an np problem once you introduce `**` into the mix.

---
