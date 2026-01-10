```yaml
number: 13438
title: Follow links when cache-key is a glob
type: pull_request
state: merged
author: aldanor
labels:
  - bug
assignees: []
merged: true
base: main
head: feat/cache-key-glob-symlink
created_at: 2025-05-14T01:43:13Z
updated_at: 2025-07-14T15:35:34Z
url: https://github.com/astral-sh/uv/pull/13438
synced_at: 2026-01-10T06:53:01Z
```

# Follow links when cache-key is a glob

---

_Pull request opened by @aldanor on 2025-05-14 01:43_

## Summary

There's some inconsistent behaviour in handling symlinks when `cache-key` is a glob or a file path. This PR attempts to address that.

- When cache-key is a path, [`Path::metadata()`](https://doc.rust-lang.org/std/path/struct.Path.html#method.metadata) is used to check if it's a file or not. According to the docs:
  > This function will traverse symbolic links to query information about the destination file.

  So, if the target file is a symlink, it will be resolved and the metadata will be queried for the underlying file.

- When cache-key is a glob, `globwalk` is used, specifically allowing for symlinks:

  ```rust
  .file_type(globwalk::FileType::FILE | globwalk::FileType::SYMLINK)
  ```

- However, without enabling link following, `DirEntry::metadata()` will return an equivalent of `Path::symlink_metadata()` (and not `Path::metadata()`), which will have a file type that looks like

   ```rust
   FileType {
       is_file: false,
       is_dir: false,
       is_symlink: true,
      ..
    }
    ```

- Then, there's a check for `metadata.is_file()` which fails and complains that the target entry "is a directory when file was expected".

- TLDR: glob cache-keys don't work with symlinks.

## Solutions

Option 1 (current PR): follow symlinks.

Option 2 (also doable): don't follow symlinks, but resolve the resulting target entry manually in case its file type is a symlink. However, this would be a little weird and unobvious in that we resolve files but not directories for some reason. Also, symlinking directories is pretty useful if you want to symlink directories of local dependencies that are not under the project's path.

## Test Plan

This has been tested manually:

```rust
fn main() {
    for follow_links in [false, true] {
        let walker = globwalk::GlobWalkerBuilder::from_patterns(".", &["a/*"])
            .file_type(globwalk::FileType::FILE | globwalk::FileType::SYMLINK)
            .follow_links(follow_links)
            .build()
            .unwrap();
        let entry = walker.into_iter().next().unwrap().unwrap();
        dbg!(&entry);
        dbg!(entry.file_type());
        dbg!(entry.path_is_symlink());
        dbg!(entry.path());
        let meta = entry.metadata().unwrap();
        dbg!(meta.is_file());
    }

    let path = std::path::PathBuf::from("./a/b");
    dbg!(path.metadata().unwrap().file_type());
    dbg!(path.symlink_metadata().unwrap().file_type());
}
```

Current behaviour (glob cache-key, don't follow links):
```
[src/main.rs:9:9] &entry = DirEntry("./a/b")
[src/main.rs:10:9] entry.file_type() = FileType {
    is_file: false,
    is_dir: false,
    is_symlink: true,
    ..
}
[src/main.rs:11:9] entry.path_is_symlink() = true
[src/main.rs:12:9] entry.path() = "./a/b"
[src/main.rs:14:9] meta.is_file() = false
```

Glob cache-key, follow links:
```
[src/main.rs:9:9] &entry = DirEntry("./a/b")
[src/main.rs:10:9] entry.file_type() = FileType {
    is_file: true,
    is_dir: false,
    is_symlink: false,
    ..
}
[src/main.rs:11:9] entry.path_is_symlink() = true
[src/main.rs:12:9] entry.path() = "./a/b"
[src/main.rs:14:9] meta.is_file() = true
```

Using `path.metadata()` for a non-glob cache key:
```
[src/main.rs:18:5] path.metadata().unwrap().file_type() = FileType {
    is_file: true,
    is_dir: false,
    is_symlink: false,
    ..
}
[src/main.rs:19:5] path.symlink_metadata().unwrap().file_type() = FileType {
    is_file: false,
    is_dir: false,
    is_symlink: true,
    ..
}
```

---

_Review requested from @charliermarsh by @konstin on 2025-05-14 07:20_

---

_Label `bug` added by @konstin on 2025-05-14 07:20_

---

_Review request for @charliermarsh removed by @konstin on 2025-05-15 14:09_

---

_Review requested from @BurntSushi by @konstin on 2025-05-15 14:09_

---

_@BurntSushi reviewed on 2025-05-15 16:39_

There are two things that worry me about this change:

1. Following symlinks means directory traversal can "escape" out of the CWD and into anywhere else on the file system.
2. Following symlinks could potentially be much more expensive, which I think could be a problem specifically here since I think this a performance sensitive part of uv.

Maybe option (2) is the right way to go here for now? Although I do agree it is perhaps a little odd.

---

_Comment by @aldanor on 2025-05-15 16:55_

@BurntSushi thanks for the reply!

Both points are valid and I had pretty much the same thoughts... a few notes though:
1. In the current uv, you can essentially already escape the current CWD by specifying a non-glob symlink which would then follow a different code path. It's quite odd that `file = "a/b"` would work but `file = "a/*"` won't.
2. As it was written, the globwalk filter being used specifically allows for symlinks, so it seemed like the original intention was to support symlinks.

> Maybe option (2) is the right way to go here for now? Although I do agree it is perhaps a little odd.

Yea, it feels a bit inconsistent and may be even harder to explain/document - like, then we allow `a/b` when a is a symlink and/or b is a symlink, we allow `a/b/*` when `*` is a symlink and `a/*/b` when b is a symlink, but we don't allow `a/*/b` when `*` is a symlink... I think it might be easier to just say 'non-globs already follow symlinks; globs behave the same way'?

If you have a strong preference towards option 2 though, that should be easy to implement as well.

---

_Comment by @BurntSushi on 2025-05-15 17:15_

Yeah I agree it's non-ideal. Hmmmm.

Another option that comes to mind that is found in other glob implementations is `***`, as in, `foo/***/something.toml`. The triple star behaves like `**`, but follows symlinks. However, I don't think any of the Rust glob crates support this. It would be plausibly ideal here because it would let folks writing globs specifically opt into symlink traversal.

Yet another option is to add an opt-in configuration controlling whether symlinks are followed for glob walking. That feels not great though.

The reason why I am being super cautious here is that, in my experience, it's _very easy_ for symlinks to blow up the size of a directory tree. I see this all the time with users of VS Code somehow finding that ripgrep is constantly re-traversing `node_modules` directories. Which can easily get to hundreds of thousands of directory entries when symlinks are followed. Now I don't think that's as common in Python ecosystem, but it's still something to be cautious about. Basically, I worry about merging this and it causing massive perf regressions for people using cache key globs.

cc @zanieb here for an opinion about UX.

---

_Comment by @zanieb on 2025-05-28 18:39_

I'm not sure I agree with the premise that

> It's quite odd that file = "a/b" would work but file = "a/*" won't.

In the former, you've explicitly requested we use "a/b" as a cache key whereas in the latter it's implicit.

Are there cases where you really need to follow a globbed symlink instead of specifying it explicitly?

---

_Assigned to @zanieb by @zanieb on 2025-05-28 18:39_

---

_Comment by @aldanor on 2025-05-28 21:46_

@zanieb a real-world example: there's a proto/ folder with tons of *.proto symlinks to various proto schemas around the codebase. The package itself reruns protobuf codegen. If any of those symlinked schemas changes, we'd like to rebuild the package automatically.

It is possible to specify it all manually but then we're doing it twice.

One middle-ground solution here, I guess, is to not follow links in the walker, however always resolve the final DirEntry (i.e. via fs::metadata and not entry.metadata()). It's trivial to add if there's consensus.

---

_Comment by @zanieb on 2025-06-02 16:09_

I think I'm in favor of the opt-in `***` syntax. I think it'll overlap with the conversation at https://github.com/astral-sh/uv/pull/13469#issuecomment-2913017907 though.

---

_Comment by @aldanor on 2025-06-02 23:03_

@zanieb Thanks for replying. Do you think extra globbing syntax like `***` would be less intrusive than just resolving symlinks on the leaves of the walk? (i.e. not descend into symlinked dirs for walking but just query `fs::metadata()` instead of the current `fs::symlink_metadata()` for the entries yielded)

---

_Comment by @zanieb on 2025-06-02 23:06_

I'm not sure, honestly. Is that equivalent to saying "we don't follow symlinks for directories" or would you still follow to a directory if it was a leaf? I worry that will be harder to teach than "we only follow symlinks with `***`".

I'm not sure if that assuages @BurntSushi's performance concerns.

---

_Comment by @aldanor on 2025-06-02 23:17_

> or would you still follow to a directory if it was a leaf?

No you won't follow if it was a leaf (because that's it, it's a leaf). `walkdir` itself keeps its internal state where it doesn't follow symlinked dirs because we don't enable `follow_links` option. It's just that you can resolve those symlinks yourself post-factum without altering the walking behaviour, that's what I meant.

As for `***` - if there's consensus on adding it, if need be I can also add it to the crate proposed in https://github.com/astral-sh/uv/pull/13469.

---

_Comment by @BurntSushi on 2025-06-03 12:36_

I think you're suggesting your option 2?

> Option 2 (also doable): don't follow symlinks, but resolve the resulting target entry manually in case its file type is a symlink. However, this would be a little weird and unobvious in that we resolve files but not directories for some reason. Also, symlinking directories is pretty useful if you want to symlink directories of local dependencies that are not under the project's path.

That downside seems kinda unfortunate. I feel like `***` is the best solution here, but I don't know how willing we will be to switch to a brand new globbing crate. And yeah, adding `***` support otherwise is pretty tricky because it requires coupling between the glob syntax and directory traversal (which `globset` does not have yet).

---

_Comment by @charliermarsh on 2025-07-10 23:17_

> One middle-ground solution here, I guess, is to not follow links in the walker, however always resolve the final DirEntry (i.e. via fs::metadata and not entry.metadata()). It's trivial to add if there's consensus.

I don't mind this, personally, since it seems unintrusive and resolves a real-world use-case.

---

_Comment by @aldanor on 2025-07-11 02:21_

@charliermarsh thanks, I'll update the PR shortly

---

_Comment by @aldanor on 2025-07-11 18:25_

@BurntSushi I think this should do it? I've added a quick unix test as well.

Please lmk if I missed something.

---

_Comment by @aldanor on 2025-07-11 18:30_

I'm not entirely sure though how to make the tests pass clippy since entire std::fs seems to be banned 🤔 

---

_Comment by @charliermarsh on 2025-07-11 18:35_

(Use `fs_err` instead of `std::fs`.)

---

_Comment by @aldanor on 2025-07-11 18:44_

> (Use `fs_err` instead of `std::fs`.)

@charliermarsh I see, thanks! Should be fixed now.

(btw I think you can make clippy bans even a bit tighter - e.g. there's methods in `Path` that are essentially wrappers around `fs` functions like `Path::metadata()`)

---

_@BurntSushi approved on 2025-07-14 15:35_

LGTM. Thanks for sticking with it on this one!

---

_Merged by @BurntSushi on 2025-07-14 15:35_

---

_Closed by @BurntSushi on 2025-07-14 15:35_

---
