---
number: 1896
title: Alias short arguments
type: issue
state: closed
author: connorskees
labels: []
assignees: []
created_at: 2020-05-02T20:17:00Z
updated_at: 2020-05-11T16:37:18Z
url: https://github.com/clap-rs/clap/issues/1896
synced_at: 2026-01-07T13:12:19-06:00
---

# Alias short arguments

---

_Issue opened by @connorskees on 2020-05-02 20:17_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

I have a project that needs to offer 2 shorthand flags (`-s` and `-t`) for the same argument in order to conform to a reference implementation from another language. The latter of the two is hidden and exists only for backwards compatibility; however, some old tools only use the hidden form of the flag. 

### Describe the solution you'd like

I'd be looking for something similar to [`Arg::alias`](https://docs.rs/clap/2.33.0/clap/struct.Arg.html#method.alias) but for shorthand arguments. It would probably make sense to have the signature look like
```rust
pub fn alias_short<S: Into<&'help str>>(mut self, name: S) -> Self;
```
that adds a hidden shorthand alias.

The relevant code https://github.com/clap-rs/clap/blob/70287eae82e25ada84ed38920fbd3598e5b19039/src/build/arg/mod.rs#L358

### Alternatives, if applicable

It shouldn't be too hard to just add a hidden flag and then check if either are set, but it seems unintuitive to only allow aliasing of long arguments.


---

_Label `T: new feature` added by @connorskees on 2020-05-02 20:17_

---

_Comment by @kbknapp on 2020-05-02 23:47_

I agree it's a gap in what we currently offer. I can't say it'll make the cut for the 3.0 release, but it should be added shortly after.

Thanks for issue.

---

_Label `C: alias` added by @kbknapp on 2020-05-02 23:47_

---

_Label `D: easy` added by @kbknapp on 2020-05-02 23:47_

---

_Label `P3: want to have` added by @kbknapp on 2020-05-02 23:47_

---

_Added to milestone `3.1` by @kbknapp on 2020-05-02 23:47_

---

_Referenced in [clap-rs/clap#1901](../../clap-rs/clap/pulls/1901.md) on 2020-05-03 06:33_

---

_Comment by @pksunkara on 2020-05-03 06:51_

Does that mean we need to rename `alias` to `long_alias` and other methods similarly?

---

_Comment by @kbknapp on 2020-05-04 17:18_

> Does that mean we need to rename alias to long_alias and other methods similarly?

I hadn't thought that far yet.... ugh. :worried: 

... [ thinking intensifies ] ...

I'd say no. So long as the docs are explicit that `alias` is for longs, and if they wish to find something similar but for shorts, look for `short_alias` I think it's fine.

---

_Closed by @bors[bot] on 2020-05-11 16:37_

---
