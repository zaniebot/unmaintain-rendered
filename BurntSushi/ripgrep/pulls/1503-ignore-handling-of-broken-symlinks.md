```yaml
number: 1503
title: "ignore: handling of broken symlinks"
type: pull_request
state: closed
author: sharkdp
labels: []
assignees: []
base: master
head: broken-symlinks-with-follow_links-on
created_at: 2020-02-28T13:22:10Z
updated_at: 2022-03-07T23:15:16Z
url: https://github.com/BurntSushi/ripgrep/pull/1503
synced_at: 2026-01-12T18:23:14Z
```

# ignore: handling of broken symlinks

---

_@sharkdp_

This is maybe more of a question than a bug report.

Currently, the `ignore` crate has the following behavior:

* If `.follow_links(false)`: both broken and normal symlinks are reported as directory entries. This is as expected.
* If `.follow_links(true)`: normal symlinks are reported as directory entries (and traversed, if they point to a directory), broken symlinks are not reported (an `ignore::Error::WithPath` is returned from the iterator).

Question: in the `follow_links(true)` case, do we expect to see broken symlinks as normal directory entries?

I would argue yes, they should be reported. Broken symlinks are file-like directory entries which should show up when "walking a directory" with `ignore`. This is backed by `find`s behavior, which does report broken symlinks, even when `-follow` is present.


This is a PR which adds a (currently failing) unit test, which can be used as a basis to fix this, if desired. I tried to implement a fix myself, but didn't get very far:
``` diff
diff --git a/crates/ignore/src/walk.rs b/crates/ignore/src/walk.rs
index 6fd7087..c7bf039 100644
--- a/crates/ignore/src/walk.rs
+++ b/crates/ignore/src/walk.rs
@@ -958,6 +958,13 @@ impl Iterator for Walk {
             };
             match ev {
                 Err(err) => {
+                    if err.io_error().map_or(false, |io_error| {
+                        io_error.kind() == io::ErrorKind::NotFound
+                    }) {
+                        // TODO: make sure this is actually caused by a broken symlink
+                        // TODO: how to create a `DirEntry` from `io_error`? We only have
+                        // io_error.path().
+                    }
                     return Some(Err(Error::from_walkdir(err)));
                 }
                 Ok(WalkEvent::Exit) => {
```

---

_Comment by @BurntSushi on 2020-02-28 13:36_

Interesting question. It kind of feels to me like the current API is probably right though? The user is asking for symlinks to be followed, so `ignore` will do just that. If an error occurs while attempting that, then that really should be reported as an error. This is also consistent with how the rest of the API is designed and is probably why you're having trouble creating a `DirEntry`. Namely, methods like `file_type()` and `metadata()` return the relevant value for the _target_ of the link when `follow_links` is enabled. (Technically this isn't documented behavior in `ignore`, but it should be, and is [documented behavior in walkdir](https://docs.rs/walkdir/2.3.1/walkdir/struct.DirEntry.html).)

So if the caller asked to follow symlinks, and you yield a dir entry with a symlink that could not be followed, then how are methods like `metadata` and `file_type` implemented? The only things you can really do here are return an error or _not_ follow the symlink. But this could lead to surprising behavior. If you asked the traversal to follow symlinks, then I think it's reasonable to expect that every dir entry is _not_ a symlink. If the symlink couldn't be followed, then that really seems like a proper error to me.

Moreover, if we decided to return a `DirEntry` instead, and a symlink couldn't be followed because an error occurred, then how is that error reported to the caller? It sounds like you'd either have to squash it or otherwise attach the error to the `DirEntry`. Which seems not great.

Your point about `find` is well taken, but it seems to me like you have all the tools with the current API to implement that behavior, no? That is, if your application wants to emit broken symlinks even when the user asked to follow them, then it should be possible to inspect the error. Maybe the problem here is that you don't have a way to detect whether the error is due to being unable to follow the symlink, then I'd be in favor of adding a method or something to detect that case specifically on the error type. Would that work?

---

_Comment by @sharkdp on 2020-02-28 16:40_

Thank you so much for your detailed response!

> This is also consistent with how the rest of the API is designed [...]. Namely, methods like `file_type()` and `metadata()` return the relevant value for the _target_ of the link when `follow_links` is enabled.

Right. I hadn't thought about that.

> So if the caller asked to follow symlinks, and you yield a dir entry with a symlink that could not be followed, then how are methods like `metadata` and `file_type` implemented?

According to `find`, a broken symlink still has a "file type" of "symlink":
```
❯ tree
.
├── broken_link -> non-existing
├── dir
└── normal_link -> dir

❯ find -type l        
./normal_link
./broken_link

❯ find -follow -type l
./broken_link
```
So in this sense, I would expect a `DirEntry` with a `std::fs::FileType` that returns `true` for `.is_symlink()`.

Since `.metadata()` returns a `Result<MetaData>`, I would expect an `Err(…)` in this case. This would be consistent with what `ls` does (prints a warning, but shows the entry with unknown metadata):
```
❯ ls -l   
total 0
lrwxrwxrwx 1 shark lxd 12 Feb 28 17:14 broken_link -> non-existing
drwxr-xr-x 2 shark lxd 40 Feb 28 17:14 dir
lrwxrwxrwx 1 shark lxd  3 Feb 28 17:13 normal_link -> dir

/tmp/fooo                                                                        
❯ ls -l -L
ls: cannot access 'broken_link': No such file or directory
total 0
l????????? ? ?     ?    ?            ? broken_link
drwxr-xr-x 2 shark lxd 40 Feb 28 17:14 dir
drwxr-xr-x 2 shark lxd 40 Feb 28 17:14 normal_link
```

> If you asked the traversal to follow symlinks, then I think it's reasonable to expect that every dir entry is _not_ a symlink. If the symlink couldn't be followed, then that really seems like a proper error to me.

> Moreover, if we decided to return a `DirEntry` instead, and a symlink couldn't be followed because an error occurred, then how is that error reported to the caller? It sounds like you'd either have to squash it or otherwise attach the error to the `DirEntry`. Which seems not great.

Agreed, I see your point.

> Your point about `find` is well taken, but it seems to me like you have all the tools with the current API to implement that behavior, no? That is, if your application wants to emit broken symlinks even when the user asked to follow them, then it should be possible to inspect the error.

We had two failed attempts at fixing this bug (https://github.com/sharkdp/fd/issues/357) in `fd` (https://github.com/sharkdp/fd/pull/366, https://github.com/sharkdp/fd/pull/497), but that certainly doesn't mean it's not possible. I will try once again myself.

> Maybe the problem here is that you don't have a way to detect whether the error is due to being unable to follow the symlink, then I'd be in favor of adding a method or something to detect that case specifically on the error type. Would that work?

We currently get a `ignore::Error::WithPath` with an internal `io::Error` of kind `io::ErrorKind::NotFound` ("An entity was not found, often a file."). Subsequently, we can check if the path inside the error is a symlink (with `std::fs::symlink_metadata`, i.e. without following the link). This way, I think we should actually be able to implement this.

---

_Comment by @sharkdp on 2020-02-28 19:56_

I found a way to make this work: https://github.com/sharkdp/fd/compare/cc08493...9fe73baa

I think we can close this(?). Maybe the only thing to note is that users of `ignore` which want to implement a `ls` or `find`-like tool will maybe not have the optimal API to handle broken symlinks. But I don't think this is enough of a reason to make a breaking change to `ignore`s current behavior. This is a rather weird use case anyway (combination of `follow` and broken symlinks).

---

_Closed by @sharkdp on 2020-02-28 19:56_

---

_Comment by @lzm0 on 2022-03-07 23:15_

As you probably know, VS Code depends on ripgrep to find files by name: https://github.com/microsoft/vscode/blob/ab49f4f310a014c9e03839f37c1ca3c48196aeca/src/vs/workbench/services/search/node/fileSearch.ts#L200
Therefore, VS Code cannot find broken symbolic links in a workspace due to this issue.
I just filed a bug to them: https://github.com/microsoft/vscode/issues/144610
I looked into this issue on my own and traced it down to here.

---
