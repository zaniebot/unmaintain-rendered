```yaml
number: 1761
title: "WalkBuilder: Improved API for adding multiple roots"
type: issue
state: open
author: ssokolow
labels:
  - enhancement
assignees: []
created_at: 2020-12-13T18:09:48Z
updated_at: 2026-01-09T02:58:47Z
url: https://github.com/BurntSushi/ripgrep/issues/1761
synced_at: 2026-01-12T16:13:24Z
```

# WalkBuilder: Improved API for adding multiple roots

---

_@ssokolow_

In the creations I'm porting from Python or writing anew, the most common use I have for recursive directory traversal is to turn a mixture of file and directory paths given at the command line into a list of file paths to be processed.

While `ignore` and `walkdir` one-up Python's `os.walk` in one respect (I don't need to call `os.path.isdir` and bypass `os.walk` for non-directories), `WalkBuilder` makes taking a `Vec` of roots surprisingly un-ergonomic because the first one must be special-cased.

If I'm willing to mutate StructOpt's output (which I'd prefer to avoid), the simplest solution I've found is:

```rust
    if let Some(path1) = opts.inpath.pop() {
        let mut builder = WalkBuilder::new(path1);
        for path in opts.inpath {
            builder.add(path);
        }
        for result in builder.build() {
            // TODO: Process each file
        }

    }
```

If I'm not willing to mutate it, then it's worse.

It reminds me of the awkwardness of using `Iterator::fold` when what's really needed is `Iterator::fold_first`.

I'd like to propose one of the following:

* Add a `WalkBuilder::empty()` constructor so I can call `add` for the first argument too.
* Add a `WalkBuilder::from_iter()` constructor so I can just feed my `Vec` of roots directly to it.

---

_Comment by @BurntSushi on 2020-12-13 18:47_

This sounds fine to me, as long as the semantics for an empty walker are clear and the implementation matches those semantics. (The existing code I imagine has never needed to handle that case, since a walker always has at least one root by construction.) I don't expect this to be a challenge or anything, but it's easy to overlook.

PRs are welcome. I'm unlikely to do this myself any time soon, but could do it next time I'm in the code.

---

_Label `enhancement` added by @BurntSushi on 2020-12-13 18:47_

---

_Comment by @ssokolow on 2020-12-13 19:07_

Fair enough. I certainly won't have time to work on someone else's codebase until at least the new year, but I'll leave myself a note in case I can find time.

Do you have a preference between `WalkBuilder::empty()` and `WalkBuilder::from_iter()` or should I use my own judgment on which one to implement if I get to it first?

---

_Comment by @BurntSushi on 2020-12-13 19:09_

I like from_iter, but having both doesn't aeem like a terrible idea.

---

_Comment by @SpyrosRoum on 2021-08-15 09:11_

Hello, I'd like to give this a try, I just have a couple of questions:
1.  What should happen if you use `WalkBuilder::from_iter()` with an empty iterator?
    I assume nothing, it would be the same as using `WalkBuilder::empty()`
2. What should happen if you try to run `WalkBuilder::build()` but you haven't added any paths? Should it panic? 

---

_Comment by @kokounet on 2022-03-24 23:25_

Hello, what's the status of this? I would be interested in this feature as well 😄.

My opinion @SpyrosRoum for building an empty `WalkBuilder` is that it should work fine, but the built `Walk` should just have no items during iteration, that saves us from having to panic.

---

_Comment by @jar-of-salt on 2026-01-09 02:58_

I am taking a stab at this, opening a PR shortly.

---
