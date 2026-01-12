```yaml
number: 1555
title: "Ignore: Skip files on MacOS hidden by attribute"
type: issue
state: closed
author: casey
labels:
  - enhancement
assignees: []
created_at: 2020-04-20T01:24:05Z
updated_at: 2020-05-09T03:24:44Z
url: https://github.com/BurntSushi/ripgrep/issues/1555
synced_at: 2026-01-12T16:13:23Z
```

# Ignore: Skip files on MacOS hidden by attribute

---

_@casey_

@Celeo is working on integrating `ignore` into a project, when before it was using `walkdir`, in order to benefit from `.gitignore` and `.ignore` processing. (Thanks for both of these crates!)

There's one piece of functionality that isn't easy to replicate with `ignore`, which is skipping files and directories on MacOS if they're hidden by attribute. I had written my own function to check if files were hidden, and then applied it with the `filter_entry` iterator combinator from `walkdir`.

It looks like `ignore` checks for the hidden attribute on Windows, but not MacOS, and it doesn't expose `filter_entry` from walkdir.

What do you think about adding support for checking the macos hidden attribute? I'd be happy to contribute this as a PR. Or possibly is there something in the API that I've missed that allows filtering entries?

Unfortunately, my implementation of checking the hidden attribute involves unsafe code, and I don't think there's a safe alternative. Here's [the code](https://github.com/casey/intermodal/blob/b67a2f1885c9445c08411457809f8893ebfa2045/src/platform.rs#L17) for reference.

---

_Comment by @BurntSushi on 2020-04-20 14:22_

There is currently no "filter entry" API in the `ignore` crate.

My understanding is that using a `.` prefix on macOS is fairly standard for indicating hidden files. I didn't even know of the existence of a separate attribute.

I'm not inclined to support this in `ignore` directly. Windows support for this exists principally because the `.` prefix convention isn't really used there, _and_ it's cheap. That is, it requires no additional stat calls, but it looks like macOS would. That's a pretty expensive proposition.

I think the right path forward here is probably to add some kind of filtering functionality to the walker, possibly by adding a new method on `WalkBuilder` (instead of how the `walkdir` API works). Then callers could add their own filtering like this if they wanted.

I'd be okay accepting a PR for that if it's a relatively simple change.

---

_Label `enhancement` added by @BurntSushi on 2020-04-20 14:22_

---

_Comment by @casey on 2020-04-20 22:00_

Whoops, I totally missed something, it looks like `st_flags` is [actually available](https://github.com/rust-lang/rust/blob/d8bdb3fdcbd88eb16e1a6669236122c41ed2aed3/src/libstd/os/macos/fs.rs#L66) via `MetadataExt` on MacOS.

I read the [comment here](https://github.com/BurntSushi/ripgrep/blob/73103df6d982da1a0dc775e9b7a03f5bb8cfd7fb/crates/ignore/src/pathutil.rs#L33) about the call to `metadata` returning cached data, and thus being free. Is that only true on Windows? If it's also true on MacOS, then we could do the same thing.

---

_Comment by @BurntSushi on 2020-04-20 22:15_

Yes, it's only true on Windows.

---

_Comment by @casey on 2020-04-20 22:39_

Ah, that's too bad.

For the API of `WalkBuilder::filter_entry`, taking a generic predicate would require parametrizing `WalkBuilder` on the type of that predicate. Some options:

1. Make `WalkBuilder` generic over the type of that predicate, i.e. `WalkBuilder<T>`.
2. Make WalkBuilder::filter_entry` take the concrete type `fn(&DirEntry) -> bool`. (This would work for my purposes, but wouldn't work for others.)
3. Box the predicate.

I'm leaning towards 2., since it meets my needs and doesn't complicate the types. What do you think?

---

_Comment by @BurntSushi on 2020-04-20 22:44_

Definitely (3). In particular, you can put it in an Arc. (You'll have to for the parallel traverser.) We already do something similar for the sort function: https://github.com/BurntSushi/ripgrep/blob/73103df6d982da1a0dc775e9b7a03f5bb8cfd7fb/crates/ignore/src/walk.rs#L489

---

_Comment by @casey on 2020-04-27 01:54_

I opened #1557 with an implementation.

---

_Closed by @BurntSushi on 2020-05-09 03:24_

---
