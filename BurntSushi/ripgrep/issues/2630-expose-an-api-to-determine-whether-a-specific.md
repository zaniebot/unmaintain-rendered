```yaml
number: 2630
title: Expose an API to determine whether a specific path is ignored
type: issue
state: closed
author: yonip23
labels:
  - wontfix
assignees: []
created_at: 2023-10-15T11:08:03Z
updated_at: 2023-10-15T11:35:37Z
url: https://github.com/BurntSushi/ripgrep/issues/2630
synced_at: 2026-01-12T16:13:24Z
```

# Expose an API to determine whether a specific path is ignored

---

_@yonip23_

Hi all,
I have a use case where I want to determine whether a specific path is ignored, for example - I have a file watcher, and I want to determine whether or not to handle the incoming event with respect to the user's ignore configurations.
What are you guys recommending to do in my use case?

I started playing around with the `ignore` crate, and I think what I'm looking for is something along the lines of this function:
```rust
fn is_ignored(path: &Path) -> bool {
  let ig_root = IgnoreBuilder::new().build();
  let (ig, _err) = ig_root.add_parents(path);
  ig.matched(path, path.is_dir()).is_ignore()
}
```
I'm quite certain the code above is not 100% correct, so if you can provide some guidelines on how to implement it that'd be awsome.
I'll gladly open a PR with this API if you guys agree, and I'll be even happier if there is a way to handle my use case without changing any code :)

Kind regards and thanks in advance!

---

_Comment by @BurntSushi on 2023-10-15 11:13_

The ignore crate isn't really designed for this use case unfortunately. It's designed for directory traversal as opposed to ad hoc querying.

It might be better to build a list of all non-ignored files in memory and maintain it somehow.

---

_Closed by @BurntSushi on 2023-10-15 11:13_

---

_Label `wontfix` added by @BurntSushi on 2023-10-15 11:13_

---

_Comment by @yonip23 on 2023-10-15 11:24_

@BurntSushi Thank you for the quick reply!
Unfortunately, maintaining an in-memory cache is a less preferable option for my use case - I'd rather maintain a private fork of your crate with the additional API necessary or find an alternative.
Can I ask how would you recommend to implement this API using the `IgnoreBuilder` APIs? how to use the `add_child` or `add_parents` in the right order?
Or if you know of any alternatives, that'd help greatly as well.

Thanks again!

---

_Comment by @BurntSushi on 2023-10-15 11:34_

> Can I ask how would you recommend to implement this API using the IgnoreBuilder APIs? 

I truly and honestly don't know. This isn't a problem I have thought about much and I just don't have the bandwidth to do so at present.

I do have plans to (at some point) overhaul the `ignore` crate, and I'll keep this use case in mind when I do. But it might be years before that's done.

---

_Comment by @yonip23 on 2023-10-15 11:35_

I see,
Thank you for the replies and for this crate 🙌

---
