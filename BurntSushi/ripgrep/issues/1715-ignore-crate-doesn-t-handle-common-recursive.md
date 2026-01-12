```yaml
number: 1715
title: "ignore crate doesn't handle common recursive directory ignore pattern"
type: issue
state: closed
author: jazzdan
labels: []
assignees: []
created_at: 2020-10-22T17:34:48Z
updated_at: 2020-10-23T14:31:58Z
url: https://github.com/BurntSushi/ripgrep/issues/1715
synced_at: 2026-01-12T16:13:24Z
```

# ignore crate doesn't handle common recursive directory ignore pattern

---

_@jazzdan_

#### What version of ripgrep are you using?

master branch @ `c777e2cd5766128e11f7fd5dffd79e1ba8a753fb`

#### How did you install ripgrep?

n/a, I was using the ignore crate as a library.

#### What operating system are you using ripgrep on?

Tried on both macOS catalina and Windows Subsystem for Linux v2 running Ubuntu:

```
$ uname -a
Linux MYPC 4.19.104-microsoft-standard #1 SMP Wed Feb 19 06:37:35 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```

#### Describe your bug.

There's a common way to ignore top level directories in `.gitignore` files: simply put the name of the directory in the `.gitignore` followed by a `/`. For example, if I wanted to ignore a directory called `target` and all of the files in it I would put `target/` in my `.gitignore` file. You can see an example of this in [GitHub's Rust `.gitignore`](https://github.com/github/gitignore/blob/master/Rust.gitignore#L1-L4).

Unfortunately this doesn't work in the ignore crate's handling of `.gitignore`. 

I understand that ripgrep doesn't want to exactly handle all of `.gitignore`'s edgecases (I've written this code in Go and ... it's really hard) but this is a pretty common case and I didn't see it documented in issues here. That said I completely understand if this is a WONTFIX issue. :)

#### What are the steps to reproduce the behavior?

I wrote [a test case for the ignore crate demonstrating this](https://github.com/jazzdan/ripgrep/blob/b1ed7e01a9b004f8ecae540830d4050ecdb5575d/crates/ignore/src/gitignore.rs#L707).

#### What is the actual behavior?

With this `.gitignore` file:

```gitignore
target/
```

A change to a path like `./target/tcr.d` is not marked as ignored.

#### What is the expected behavior?
With this `.gitignore` file:

```gitignore
target/
```

A change to a path like `./target/tcr.d` should be ignored.

Thank you for making ripgrep and all of these great libraries!

---

_Comment by @BurntSushi on 2020-10-22 17:41_

Looks correct to me. The pattern `target/` will ignore the path `target`. So when using ignore for recursive traversal, it should ignore the `target` directory before descending into it. If you aren't doing recursive search, then you may need to match each parent path up to the level at which the gitignore file resides.

---

_Comment by @jazzdan on 2020-10-22 17:52_

Ah, I might be misunderstanding how to use the library then. [Here's the code I wrote](https://github.com/jazzdan/tcr/blob/master/src/ignore.rs#L32), here's the part where I use the ignore crate:

```rust
match &self.gitignore {
  Some(gi) => {
    let is_dir = event.is_dir;
    return paths.iter().all(|p| gi.matched(p, is_dir).is_ignore());
  }
  None => {}
 }
```

Where `p` is a path: I would expect like `matched('target/tcr.d', false).is_ignore() == true`. But from what you're saying it sounds like it won't check that `target/tcr.d` is a child of `target/` and so it will return false. Whereas if I just put `target/**` in my `.gitignore` then it will do what I expect.

Is there a way for the ignore crate to do the kind of parent matching that I'm describing?

By the way, I'm guessing that `target/` works with `git` because of some intricacy for how `git` handles files? Anyway, not important, feel free to close this issue as it seems I have a misunderstanding of the API.

Thanks for the quick response and for all this great code!

---

_Comment by @BurntSushi on 2020-10-22 18:21_

The only way to do that with the ignore crate is to iterate over parent paths.

I think people probably don't realize just how limited the ignore crate is. If you aren't doing filtered recursive directory traversal, then _maybe_ parts of the crate will be useful to you, but you'll likely need to work hard to make it come together.

> By the way, I'm guessing that target/ works with git because of some intricacy for how git handles files?

Yeah git handles ignore rules pretty differently than ripgrep. Obviously I did my best to make them match, but I did that in the context of recursive directory traversal because that was the problem I needed to solve. But for example, if a file matches a gitignore pattern but is committed, then git won't ignore but ripgrep will.

---

_Comment by @jazzdan on 2020-10-22 19:10_

Gotcha, thanks @BurntSushi! I think I will implement the recursive directory traversal myself, that sounds like a fun project.

---

_Closed by @jazzdan on 2020-10-22 19:10_

---

_Comment by @BurntSushi on 2020-10-22 19:20_

Errm, to clarify, the ignore crate provides recursive directory traversal.

---

_Comment by @jazzdan on 2020-10-23 14:28_

Ah! Yeah you just need to use `matched_path_or_any_parents` it seems. Thanks again!

---

_Comment by @BurntSushi on 2020-10-23 14:31_

Oh cool. I forgot about that API!

---
