```yaml
number: 1909
title: "`ignore`: gitignore relative paths are not treated relatively"
type: issue
state: open
author: SergioBenitez
labels: []
assignees: []
created_at: 2021-06-23T19:14:54Z
updated_at: 2022-04-29T12:42:56Z
url: https://github.com/BurntSushi/ripgrep/issues/1909
synced_at: 2026-01-12T16:13:24Z
```

# `ignore`: gitignore relative paths are not treated relatively

---

_@SergioBenitez_

In short, gitignore rules that should be relative to a specific gitgnore path are instead reinterpreted as `**/rule`. Here's the failing test case:

```rust
use std::path::PathBuf;
use ignore::gitignore::GitignoreBuilder;

fn main() {
    let mut builder = GitignoreBuilder::new("/root");
    builder.add_line(Some(PathBuf::from("/root/scratch/.gitignore")), "!foo.html").unwrap();
    let matcher = dbg!(builder.build().unwrap());

    assert!(matcher.matched("/root/scratch/foo.html", false).is_whitelist());
    assert!(dbg!(matcher.matched("/root/unknown/foo.html", false)).is_none());
}
```

The relevant output:

```
[src/main.rs:7] builder.build().unwrap() = Gitignore {
    root: "/root",
    globs: [
        Glob {
            from: Some(
                "/root/scratch/.gitignore",
            ),
            original: "!foo.html",
            actual: "**/foo.html",
            is_whitelist: true,
            is_only_dir: false,
        },
    ],
    num_ignores: 0,
    num_whitelists: 1,
}
[src/main.rs:10] matcher.matched("/root/unknown/foo.html", false) = Whitelist(
    Glob {
        from: Some(
            "/root/scratch/.gitignore",
        ),
        original: "!foo.html",
        actual: "**/foo.html",
        is_whitelist: true,
        is_only_dir: false,
    },
)
thread 'main' panicked at 'assertion failed: dbg!(matcher.matched(\"/root/unknown/foo.html\", false)).is_none()', src/main.rs:10:5
```

---

_Comment by @BurntSushi on 2021-06-23 22:34_

I believe these are the relevant gitignore docs:

> If there is a separator at the beginning or middle (or both) of the pattern, then the pattern is relative to the directory level of the particular .gitignore file itself. Otherwise the pattern may also match at any level below the .gitignore level.

In particular, the "Otherwise ..." language applies here since there is no `/` in your pattern. The issue here is that it should _only_ apply to any level _below_ that `.gitignore` file, but here, it is also being applied (incorrectly) to a sibling directory. However, the pattern `foo.html` absolutely should match at any directory beneath the point in which it was defined. So the `**/foo.html` construction is correct... to a point.

This is very likely an API-level bug. In particular, when building the crate, I likely assumed that a gitignore matcher would only be used on paths at or below the directory in which it was created. And generally speaking, that's how the `ignore` file traversal works. Fixing the gitignore matcher to handle this case correctly will likely incur an unavoidable additional cost (I think) to file traversal, and thus, some new API mechanism needs to exist.

I don't see this bug getting fixed any time soon unfortunately. The `ignore` crate desperately needs a very very careful rewrite. (And has needed it for a long time.)

---

_Comment by @SergioBenitez on 2021-06-24 04:08_

> I believe these are the relevant gitignore docs:
> 
> > If there is a separator at the beginning or middle (or both) of the pattern, then the pattern is relative to the directory level of the particular .gitignore file itself. Otherwise the pattern may also match at any level below the .gitignore level.
> 
> In particular, the "Otherwise ..." language applies here since there is no `/` in your pattern. The issue here is that it should _only_ apply to any level _below_ that `.gitignore` file, but here, it is also being applied (incorrectly) to a sibling directory. However, the pattern `foo.html` absolutely should match at any directory beneath the point in which it was defined. So the `**/foo.html` construction is correct... to a point.

I suppose it could depend on what `**/foo.html` means to `ignore`. Unfortunately it doesn't appear to mean anything with correct semantics as the following _also_ doesn't work:

```rust
use std::path::PathBuf;
use ignore::gitignore::GitignoreBuilder;

fn main() {
    let mut builder = GitignoreBuilder::new("/root/scratch");
    builder.add_line(Some(PathBuf::from("/root/scratch/.gitignore")), "!/**/*.html").unwrap();
    // builder.add_line(None, "!/**/*.html").unwrap(); // Using `None` doesn't help, unfortunately.

    let matcher = dbg!(builder.build().unwrap());

    assert!(matcher.matched("/root/scratch/foo.html", false).is_whitelist());
    assert!(dbg!(matcher.matched("/root/unknown/foo.html", false)).is_none());
}
```

Here, we see:

```
[src/main.rs:7] builder.build().unwrap() = Gitignore {
    set: GlobSet {
    root: "/root/scratch",
    globs: [
        Glob {
            from: Some(
                "/root/scratch/.gitignore",
            ),
            original: "!/**/*.html",
            actual: "**/*.html",
            is_whitelist: true,
            is_only_dir: false,
        },
    ],
}
[src/main.rs:10] matcher.matched("/root/unknown/foo.html", false) = Whitelist(
    Glob {
        from: Some(
            "/root/scratch/.gitignore",
        ),
        original: "!/**/*.html",
        actual: "**/*.html",
        is_whitelist: true,
        is_only_dir: false,
    },
)
thread 'main' panicked at 'assertion failed: dbg!(matcher.matched(\"/root/unknown/foo.html\", false)).is_none()', src/main.rs:10:5
```

> I don't see this bug getting fixed any time soon unfortunately. The `ignore` crate desperately needs a very very careful rewrite. (And has needed it for a long time.)

I understand. I'm working on a replacement for the core gitignore pattern matching part of `ignore` using `GlobSet` directly. Once it's complete, I imagine it wouldn't be too difficult to swap in this new library in `ignore`.

---

_Comment by @BurntSushi on 2021-06-24 11:52_

@SergioBenitez Yeah, that looks like the same variant of the bug that is "the gitignore matcher has an undocumented and in some cases inconvenient assumption that you only use it with paths at or below the directory in which the gitignore rules were defined."

---

_Comment by @LeGEC on 2021-10-22 06:44_

@BurntSushi : is there some way to group the issues related to the `ignore` crate ? or perhaps an issue, which describes the various points that need to be clarified ?

I am aware of #278 for example, but searching the issue tracker for `"ignore"` brings along a whole lot of unrelated issues.

---

_Comment by @BurntSushi on 2021-10-22 12:19_

@LeGEC No, such a grouping doesn't exist. You're right that it should, but it will take work and effort to do.

---

_Comment by @iesahin on 2022-04-29 12:42_

I hit to this bug when I was developing something with the ignore crate.

`mydir/.gitignore` has the entry `mydir/myfile.txt`. `matches` returns `Match::Ignore` on `mydir/myfile.txt`,  I believe the correct ignore should be `mydir/mydir/myfile.txt`, and `mydir/myfile.txt` should return a `None`. 

I can submit a PR to add directory to the pattern. Currently lines in `mydir/.gitignore` are interpreted as if they are in `./.gitignore`, and if a `/` is in the pattern, `mydir/myfile.txt` becomes `**/mydir/myfile.txt` which matches all parent dirs. Instead, it should add `mydir/mydir/myfile.txt` when `ignore::add_file("mydir/.gitignore")` is used in `GitignoreBuilder`. 

---
