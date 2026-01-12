```yaml
number: 2366
title: "[ignore]: Support absolute paths in global ignore files (via `add_ignore`). "
type: issue
state: closed
author: tmccombs
labels:
  - wontfix
assignees: []
created_at: 2022-12-04T07:19:57Z
updated_at: 2025-05-08T18:51:46Z
url: https://github.com/BurntSushi/ripgrep/issues/2366
synced_at: 2026-01-12T16:13:24Z
```

# [ignore]: Support absolute paths in global ignore files (via `add_ignore`). 

---

_@tmccombs_

#### Describe your feature request

It is sometimes desirable to add a global ignore rule using an absolute path. Say for example I always wanted to ignore everything in /home/myuser/.cache. 

.gitignore doesn't have a mechanism to do this, because git doesn't really care about anything outside of the git repository (at least for the context of .gitignore). However, for more general tools such as  ripgrep (and `fd`) it does make sense to want to ignore something based on an absolute path, at least for global ignore files (such as ~/.config/fd/ignore). 


I see a couple ways of addressing this:

1. Add some new syntax which is only supported in "global" ignore files (or at least not git ignore files) to indicate that a pattern should be matched against the full absolute path of the file. Maybe if the pattern starts with "//"? 
2. If the pattern starts with a "/" in a global ignore file, then treat it as an absolute path pattern. This would be backwards compatible, unless there was new API for adding an ignore file that had this behavior (and maybe ripgrep wouldn't use this, or if it did used a new flag for it?). 


See https://github.com/sharkdp/fd/issues/1150#issuecomment-1336156334


---

_Comment by @BurntSushi on 2022-12-04 12:22_

> Say for example I always wanted to ignore everything in /home/myuser/.cache.

Could you please say why you can't use a `.ignore` file? e.g.,

```
$ echo '/.cache' > /home/myuser/.ignore
```

Also, ripgrep will ignore `.cache` by default because it is a hidden directory.

> If the pattern starts with a "/" in a global ignore file, then treat it as an absolute path pattern. This would be backwards compatible

It is not backwards compatible. Starting a glob pattern in a gitignore file with `/` anchors the pattern to the current working directory of the search. That's true in global gitignore files just as much as any other.

Overall, I don't see a compelling use case here. And even if there were a good use case, it would have to be _very_ compelling because this is a rather complex feature to implement in an already complex part of ripgrep. Namely, if you have an absolute glob pattern, then you also need to turn _every file path_ you're searching into an absolute path. That has a non-trivial cost, which means ripgrep would want to avoid turning everything into an absolute path if it didn't have to. Which means it would have to scan gitignore files for absolute patterns before starting the search. Which is just not great.

---

_Closed by @BurntSushi on 2022-12-04 12:22_

---

_Label `wontfix` added by @BurntSushi on 2022-12-04 12:22_

---

_Comment by @tmccombs on 2022-12-05 08:10_

> Could you please say why you can't use a .ignore file? e.g.,

I can think of a few reasons:

1. You don't have write access to the necessary directory. Say you wanted to ignore /var/log/noisy-app, but didn't have root access on the system.
2. You want your ignore rules in a more centralized location, possibly to make it easier to check it in to a git repo or similar.
3. You don't want to clutter the parent folder with a hidden file.

But these are just hypotheses. See below. 

> It is not backwards compatible

Sorry, that was a typo. I meant to say that it is backwards incompatible. 

>  Starting a glob pattern in a gitignore file with / anchors the pattern to the current working directory of the search. That's true in global gitignore files just as much as any other.

First of all, "/" in gitignore usually anchors to the location of the .gitignore file. But for global gitignores it anchors to the the root of the current git repository.  The current working directory doesn't matter beyond dtermining the current git repo. Anchoring to the current directory doesn't actually seem very useful to me. If I have a rule to ignore "/foo/bar", in my global ignore file, and there is a file at "/a/b/foo/bar/c/d", then the current behavior is if I run `rg` in "/a/b" it will be ignored, but if I run it from "/a" or "/a/b/foo/", then it won't be ignored. Maybe there are use cases for that, but that seems less useful than being able to say that I want to ignore "/a/b/foo/bar" regardless of where I run `rg` from. Or even have a pattern like "/**/foo/bar".

Also, of the two, I would prefer having a distinct syntax for absolute patterns (maybe some sigil at the beginning that indicates the remander should be matched against the absolute path, I suggested "//" becuase that seems unlikely to appear at the beginning of an existing pattern).  I included the option of a backwards incompatible change to how pattersn starting wtih "/" are matched mainly for completeness. 

>  Namely, if you have an absolute glob pattern, then you also need to turn every file path you're searching into an absolute path. That has a non-trivial cost, which means ripgrep would want to avoid turning everything into an absolute path if it didn't have to. Which means it would have to scan gitignore files for absolute patterns before starting the search. Which is just not great.

That's a legitimate concern.  And I don't really see a way out of increasing the complexity, and if it would significantly add to the complexity maybe it isn't worth it. But I would like to point out a few things:

- I am not proposing that this would be used for gitignore files. This would only be for non-git ignore files that are treated as global, such as `~/.config/fd/ignore` for `fd`, or maybe a file passed to ripgrep with `--ignore-file` (or possibly via a new flag). 
- This is primarily a request to have an API that would allow doing this with the ignore crate used as a library, even if ripgrep itself doesn't make use of it.
- Given that it would be limited in scope mentioned above, the ignore files would need to parsed before starting the search anyway. 


Finally, I haven't personally run into a need for this. However, this has come up a few times for [`fd`](https://github.com/sharkdp/fd). [Here](https://github.com/sharkdp/fd/issues/1150) and [here](https://github.com/sharkdp/fd/issues/851) (actually that one would impact the `overrides` API, but similar idea) and [here](https://github.com/sharkdp/fd/issues/763). And since `fd` uses the `ignore` crate, it would be rather difficult to support the feature there without something changing in the `ignore` crate. 

That last one actually points out something interesting. If the starting path passed in to `WalkBuilder::new` is an absolute path, then patterns that match absolute paths _are_ ignored.  And IMO at least, the fact that ignore matches ignore patterns differently depending on whether the path is passed in as "." or "/home/myuser" is rather unexpected. 





---

_Comment by @BurntSushi on 2022-12-05 12:15_

>  And IMO at least, the fact that ignore matches ignore patterns differently depending on whether the path is passed in as "." or "/home/myuser" is rather unexpected.

It is consistent with how greps behave. Notice, for example, that if you use an absolute file path as an input, then ripgrep will emit absolute file paths in the search result output. Indeed, the way paths are dealt with is almost purely symbolically. This is also simultaneously the reason why absolute glob patterns don't work when one provides a relative file path.

Bottom line here is that I've been saying that the `ignore` crate needs to be completely rewritten for a long time now. Its current state is basically not much better than a proof of concept. I just haven't had the time or bandwidth to come back around to the project. When I do, I'll consider issues like this one, but I would say it's unlikely to happen. It's a somewhat niche concern, although I confess it would be nice if all possible sensible things would "just work."

---

_Comment by @omentic on 2025-05-08 01:33_

> Starting a glob pattern in a gitignore file with / anchors the pattern to the current working directory of the search.

Why is this the case, and how does it differ from leaving off the leading `/`?

I'm interested in this being fixed. I find it to be my biggest pain point when using both ripgrep and fd: I have a *lot* of directories I never want either to search (`~/.icons`, `/usr/share/icons`, etc) and especially when I'm searching through config files (and so searching hidden files) both tools always have to dump a bunch of garbage from these directories into stdout. Relative paths just don't cut it, because I commonly search from places other than `~`.

If there is no difference between an entry `foo` and `/foo` in an ignore file, I think changing this would give rise to enough of a UX improvement to be worth the breaking changes.

> Which means it would have to scan gitignore files for absolute patterns before starting the search.

This seems very reasonable. Are there non-degenerate cases in which this would cause serious performance hits?

---

_Comment by @BurntSushi on 2025-05-08 01:40_

> Why is this the case

Because that's how gitignores work. See `man gitignore`.

---

_Comment by @tmccombs on 2025-05-08 15:57_

For gitignore it makes sense, because the git repository is its own container. Git operations don't do anything outside of the git repo, and  using absolute paths in a gitignore file wouldn't be portable.

For ripgrep or fd it makes more sense to want to use an absolute path, especially on the command line. 

What if a pattern that started with "//" was treated as an absolute path? But only in command line patterns and maybe .ignore and .rgignore files. In fd we would probably want to support it in the global ignore file.

---

_Comment by @BurntSushi on 2025-05-08 16:03_

I understand that. I'm just explaining why `/` is interpreted the way it is.

The main problem I have here is that gitignore is the fundamental interface through which ripgrep deals with ignore rules. ripgrep _meets git where it is_, and there's value in that. I agree that there is an impedance mismatch here because git and ripgrep are two different tools. Moreover, we _could_ at minimum extend the format of `.ignore` and `.rgignore` since those aren't read by git. But still, there is a cost that comes from diverging from what git supports. Maybe we need a v2 `.ignore`/`.rgignore` format or something that has different semantics from `.gitignore`.

---

_Comment by @omentic on 2025-05-08 18:42_

> See `man gitignore`.

Ah, I see. That's unfortunate.

Another approach might be to have a separate file / command line option for global ignores? This would be a little ugly but would be clear to end users if it's mentioned next to the docs for `--ignore-file`.

---

_Comment by @BurntSushi on 2025-05-08 18:51_

There's a lot of different ways to do it. See my comments above for `ignore` plans.

---
