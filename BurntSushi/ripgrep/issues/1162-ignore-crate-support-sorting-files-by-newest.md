```yaml
number: 1162
title: "ignore crate: Support sorting files by newest/oldest"
type: issue
state: closed
author: ebkalderon
labels:
  - question
assignees: []
created_at: 2019-01-14T13:24:09Z
updated_at: 2019-01-15T04:46:51Z
url: https://github.com/BurntSushi/ripgrep/issues/1162
synced_at: 2026-01-12T16:13:23Z
```

# ignore crate: Support sorting files by newest/oldest

---

_@ebkalderon_

#### What version of ripgrep are you using?

`ignore-0.4.4`

#### How did you install ripgrep?

Installed `ignore` via crates.io.

#### What operating system are you using ripgrep on?

Arch Linux (4.20.1.arch1-1), macOS 10.14.2

#### Describe your question, feature request, or bug.

Would it be reasonable to add to `WalkBuilder` a method which allows files to be sorted by their last modified time? This method could be called `sort_by_mtime()`, or something similar, in the same vein of the other existing `sort_by` methods. This would be quite handy for attempting to identify the newest or oldest files in a directory, among other things.

Since both [`Path::metadata()`](https://doc.rust-lang.org/std/path/struct.PathBuf.html#method.metadata) and [`Path::symlink_metadata()`](https://doc.rust-lang.org/std/path/struct.PathBuf.html#method.symlink_metadata) both return a `Result`, I imagine that this method would accept a closure like:

```rust
Fn(&io::Result<SystemTime>, &io::Result<SystemTime>) -> Ordering + Send + Sync + 'static
```

It is worth noting that this method is quite different from the existing `sort_by` methods in that the values being compared are `Result` which adds a bit of complexity to the API (what `Ordering` should the closure return if one of the arguments is `Err`?), and I'm not sure what can be done about that. In my own specific use case, the user is guaranteed to have permissions to all the files being sorted, but obviously that is not the case for everyone.

If anyone has suggestions on how to move forward from here, it would be appreciated. I am also willing to prepare a PR for this, if you would like.

---

_Comment by @BurntSushi on 2019-01-14 13:28_

You can already do this today. See how ripgrep does it, for example, [here](https://github.com/BurntSushi/ripgrep/blob/dd396ff34e36fbd5aff9487745842410686e578c/src/args.rs#L434-L460) and [here](https://github.com/BurntSushi/ripgrep/blob/dd396ff34e36fbd5aff9487745842410686e578c/src/args.rs#L1582-L1608).

I'm not sure what the ideal API looks like, but I'd prefer not to add new `sort_by` methods for each possible time.

---

_Label `question` added by @BurntSushi on 2019-01-14 13:28_

---

_Comment by @ebkalderon on 2019-01-15 04:46_

Interesting! Thanks for the helpful links. I think I'll pursue an approach similar to how `ripgrep` does it with `sort_by_path()` and an external comparator.

Fair enough. I figured it might be too much of a stretch to incorporate as a dedicated `sort_by` method, but I'm glad to see the operation can still be done without it. Thanks for the help!

---

_Closed by @ebkalderon on 2019-01-15 04:46_

---
