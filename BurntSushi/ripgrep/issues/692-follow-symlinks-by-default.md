```yaml
number: 692
title: Follow symlinks by default
type: issue
state: closed
author: ChengCat
labels: []
assignees: []
created_at: 2017-11-27T02:27:21Z
updated_at: 2026-01-02T23:47:23Z
url: https://github.com/BurntSushi/ripgrep/issues/692
synced_at: 2026-01-12T16:13:22Z
```

# Follow symlinks by default

---

_@ChengCat_

It is a very surprising behavior to not follow symlinks by default. It could lead to unnoticed errors when the correctness is depended upon. I read through the issues, and there are other people also encountering issues related to this.

I understand that following symlinks can impact performance, but I don't think the default trade-off should be sacrificing reliability for performance. We could make not following symlinks an optional flag when extra performance is needed.

---

_Comment by @okdana on 2017-11-27 02:31_

Not following symlinks is the default behaviour of almost all traditional UNIX CLI tools that work on files, including notably `find`, `grep`, and `ag`, so i think one could make a strong case that that is the least surprising behaviour.

---

_Comment by @BurntSushi on 2017-11-27 02:40_

@okdana is right. Not following symlinks is the default behavior for Unix tools. If you want to follow symlinks by default, then add the -L switch to an rg alias.

---

_Closed by @BurntSushi on 2017-11-27 02:40_

---

_Comment by @ChengCat on 2017-11-27 02:54_

As the traditional alternative of ripgrep, `grep` doens't search recursively by default, but provides two flags `-R` and `-r` for searching recursively with symlinks followed and not followed. I'd say `-R` is 'more default', since recursion is usually indicated by '-R'. I don't even know about `-r` before, and have always used `-R`.

Moreover, I don't think the default behavior for Unix tools matters here. I expect `ripgrep` to be a modern alternative to `grep`, and this gives us the opportunity to rethink about this. Those related issues reported by others indicate that this behavior is actually surprising.

---

_Comment by @BurntSushi on 2017-11-28 02:14_

I'm finally back from vacation and have a bit more time to respond to this.

First and foremost, in my own personal experience, I very rarely see anyone use `grep -R`, but rather `grep -r`. Before I wrote ripgrep, I used plain old grep for searching code repositories. I had a script named `grepfr` in my `$HOME/bin` for many many years:

```bash
#!/bin/bash

first=$1
shift
grep -nrHIF "$first" $@
```

And this is pretty consistent with similar shortcuts that I've seen other people use.

Now with that said, I very much believe that reasonable people can disagree on this matter. I could easily argue both sides of this particular issue myself. In one sense, symlinks are just an implementation detail that otherwise should be treated as a normal part of a directory tree. In another sense, symlinks are typically not followed by standard tools by default, which in turn creates an expectation that other tools will behave the same. Therefore, it's quite difficult to make the "surprising" argument here, because either side of this is surprising depending on your prior.

As a more personal anecdote, I'd like to note that if ripgrep did follow symlinks by default, then I personally would actually wind up disabling that behavior behind an alias because I typically don't want to follow symlinks based on my use of them. My use of symlinks tend to be as shortcuts to various parts of my home directory or nas, and therefore if ripgrep searched them, then it would wind up showing duplicate results or worse, irrelevant results. I don't know if others have a similar setup as me, but it wouldn't surprise me, and in particular, the duplicate result problem is pretty annoying which is another reason for symbolic link following to be behind a flag. (You might see this as an invitation to in turn solve the duplicate result problem, but that's just another type of behavior that standard tooling doesn't do and would likely have performance implications along the lines of "store data proportional to the directory tree that is being searched.")

My own tendencies are to be conservative in the presence of a choice that has no obvious answer because the conservative path is typically quite obvious, and indeed, in this case, the obvious conservative path is to do what standard Unix tooling has done for eons: don't follow symbolic links by default, but provide a flag that does it for you.

---

_Comment by @ChengCat on 2017-11-28 02:32_

Thanks for the reply. This is pretty reasonable. I actually do prefer solving the duplication problem by sacrificing performance, and this is from the idea that implementation details should be hidden from users at all cost.

I would show my use case, as that may help. I am using NixOS, which in turn uses symlinks extensively across the whole system. Many config files in /etc are symlinks. Not including symlinks is probably not what I want when I am dealing with NixOS files.

---

_Comment by @ChengCat on 2017-11-28 06:19_

After reconsidering about the duplication problem, IMO a separate option to enable symlink deduplication would be better. A simple way to understand a file system with symlinks is: there are no symlinks, and all original symlinks are expanded.

---

_Comment by @snan on 2026-01-02 23:47_

Just FWIW, `grep stuff*` does find stuff in symlinked files. As does `rg stuff *`, of course, but normally I don't provide file arguments to ripgrep the way I normally do with grep where I'm way more likely to grep in `*` than in either `-R .` or `-r .`. Standard anecdata disclaimer and I'm not trying to relitigate or reopen this issue; it's just one more data point or perspective in case this comes up again down the road.

I will say that I was very baffled why rg wouldn't find certain strings (it turned out that those strings were in files that were symlinks, which was kinda tricky to figure out. Thanks for `--debug`, that helped) and I immediatedly added `--follow` to my deadgrep settings for `rg` arguments.

---
