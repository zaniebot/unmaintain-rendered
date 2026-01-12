```yaml
number: 2842
title: Support .jj as well as .git
type: pull_request
state: closed
author: fowles
labels:
  - rollup
assignees: []
base: master
head: main
created_at: 2024-06-22T10:35:30Z
updated_at: 2025-09-20T01:08:26Z
url: https://github.com/BurntSushi/ripgrep/pull/2842
synced_at: 2026-01-12T18:23:14Z
```

# Support .jj as well as .git

---

_@fowles_

- Allow `.jj` dirs to count as vcs directories.
- Simplify the control flow around resolving info/exclude

---

_Comment by @fowles on 2024-06-22 10:39_

I totally understand if you don't want to expand things to support every oddball VCS, but so many tools I like (cargo watch, rg, fzf, fd) are all built on this common library that adding a bit of support at the core gets a lot of things to just work.

---

_Comment by @fowles on 2024-07-02 21:03_

any chance I can get this considered?

---

_Comment by @BurntSushi on 2024-07-02 21:15_

I think my main concerns here are:

1. I don't even know what `.jj` is referring to. You didn't give a link or explain anything.
2. This patch seems to assume that `.jj` and `.git` have identical semantics. Do they? There are literally no differences?
3. This doubles the number of stat calls for every directory ripgrep searches. This is a rather annoying hurdle because I don't want this to be used as a bludgeon to forever fix ourselves to `git`. But at the same time, there is a cost here that needs to be weighed with the benefit.

---

_Comment by @fowles on 2024-07-02 21:19_

Sorry about the lack of link, that is my bad.  https://martinvonz.github.io/jj/latest/ is the best spot to read.  It is a version control system like git, so `.jj` is a directory that contains metadata for it (just like `.git`).

As an alternate suggestion, I would recommend respecting `.gitignore` files even when there is no `.git` directory.  Then you can cut the stat calls down from 1 to zero.

---

_Comment by @BurntSushi on 2024-07-02 21:52_

> As an alternate suggestion, I would recommend respecting `.gitignore` files even when there is no `.git` directory. Then you can cut the stat calls down from 1 to zero.

That already exists today with `--no-require-git`. But vcs directory detection is in general required for correctness so that you don't let gitignore files cross repository boundaries.

---

_Comment by @fowles on 2024-07-02 21:54_

The problem that I run into with `--no-require-git` is that every tool built on the library exposes it differently (and some don't).  Is there a way to make that controlled via env variable or something?

---

_Comment by @BurntSushi on 2024-07-02 22:07_

No. I personally think that would be wildly inappropriate. `ignore` isn't a "standard," and trying to reach out into the environment in a library to do an end-runaround on the CLI interface just seems really bad to me. Like, there would, by construction, be no coupling between the environment variable and the CLI, which means there would be no way for `ignore` to know when to respect the environment variable versus some other option (which might override the environment variable). The only way to solve that would be to introduce more API machinery to deal with it. Sounds awful.

I'm not saying No to this PR, but it's going to require that I or someone goes and understands `jj` to ensure this is implemented correctly. At minimum. And I'll also need to make a judgment of whether the cost is worth it.

---

_Comment by @fowles on 2024-07-03 16:54_

I checked with folks on the `jj` discord to make sure that I was getting all the corners.  I have updated the PR with that and also moved things around a tiny bit to minimize the number of stat calls as well as the number of allocations.

I absolutely understand that this is a judgement call on your end about complexity versus general utility.  If there is any data I can provide to make that case, I would be happy to try, but I recognize that `jj` is a niche VCS.  (I have hopes it will grow, but realistically I am not sure it will.)

---

_Comment by @fowles on 2024-07-16 17:11_

Did you have a chance to come to a conclusion here?

---

_Comment by @fowles on 2024-09-04 18:26_

Following up, I have verified this PR with `jj`'s owner (and I am also a committer on it).  So I think this is ready for review (pending your actual acceptance of the direction).

Thanks!

---

_Comment by @jaybosamiya on 2025-03-07 03:02_

Some musings: 

Jujutsu is a neat VCS, and is [quite `git` compatible](https://jj-vcs.github.io/jj/latest/git-compatibility/). It has a lot of positives for it, and the biggest negative for it I've found in the past ~2 months I've been using it come from the (understandable, given it being relatively newer and more niche) lack of recognition of it by some tools, so I was considering making a PR to ripgrep to add support, and lo and behold, this PR already exists ❤️ 

As a quick note: `jj`'s only way (currently?) to ignore files is to use a `.gitignore` ([it even looks at `$GIT_DIR/info/exclude` as git would](https://jj-vcs.github.io/jj/latest/working-copy/#ignored-files)), and while the `ignore` format is _itself_ not a standard, I would be hard-pressed to accept that any tool that uses `.gitignore` (i.e., _with_ the `git` in the file name) with a departure in semantics from what `git` already uses to not be considered a bug in the tool. Thus, I would expect `jj` to maintain the same semantics as `git` for their usage of `.gitignore`.

Personally, I was using ripgrep suboptimally for some time, because I wasn't aware of `--no-require-git`, since at least as of ripgrep 14.1.1, it is not easy to realize that `.gitignore`s are ignored _unless_ a `.git` exists. Obviously, once I found `--no-require-git` (via this PR, funnily), I took a gander through `rg --help`, and the only mention of the `.git` directory being necessary is in the `--no-require-git` option's docs (which does mention the defaults). However, I wonder if people might be happier with the opposite default (I am somewhat biased here, so maybe I am not accounting for some other common use case where a `.gitignore` file should be ignored). I will note that literally the second line of `--help` states:
> By default, ripgrep will respect gitignore rules and automatically skip hidden files/directories and binary files.

So I don't think people would be annoyed by a default that respects gitignore rules (even if `.git` is absent).

If the flipped version becomes the default, then we would not need to double the number of `stat` calls (indeed, number of stat calls would _drop_ for most people), at which point, we might actually be able to afford having some `--require-vcs git,jj,...`-style flag (again, this is just me musing) where the user is opting in for which VCSs they want the requirements checked, allowing for more stat calls (obviously, `--require-git` is already a flag right now, so more thought would be needed into such a future flag), but nonetheless, a flipped default would probably fix the issue, and not even need a `.jj/` check.

Sidenote: I now have `--no-require-git` as part of my CLI default for `rg` now; happy to report back if it'd help with (one person) anecdotal data if you find it useful.

Another alternative to consider: if a `.gitignore` file is found, but a `.git` is not, telling the user that `--no-require-git` exists could be nice. Clearly I am not the only one who missed that this option exists :) 

---

With all the above, I also want to add: I don't want to increase maintainer burden, and I understand things like defaults are a hard decision. The existence of `--no-require-git` is great, and I am very glad that @BurntSushi you place such a laser focus on having a fast and awesome tool! :heart:

---

_Comment by @devnoname120 on 2025-03-17 23:57_

> So I don't think people would be annoyed by a default that respects gitignore rules (even if .git is absent).

That's a problem for globally-defined `.gitignore`s that people tend to place in `~`.

---

_Comment by @strega-nil on 2025-03-29 13:11_

Ping on this, it would be awesome to get this in, although for now `--no-require-git` is a fine temporary solution :)

---

_Review comment by @BurntSushi on `crates/ignore/src/dir.rs`:285 on 2025-03-29 13:29_

This is introducing an allocation in the common case where previously none existed. Maybe a `Cow<Path>` would help here?

---

_Review comment by @BurntSushi on `crates/ignore/src/dir.rs`:290 on 2025-03-29 13:36_

I think this is my biggest concern by far here. I know that y'all are just looking for tooling to work, but from my perspective, I need to worry about the more expansive view. My understanding is that `jj` is still evolving, which makes me worry about whether this particular path is stable or not. If it gets changed, then all of a sudden every ripgrep version shipped out to users with this baked in is going to no longer work. Thankfully this would, I think, just impact `jj` users. But still, it's non-ideal.

ripgrep does bake in similar sorts of assumptions about `git`, but that's because `git` is "stable" software. If ripgrep would break because `git` changed something, then the _world_ breaks. But that same sort of pressure isn't there for `jj` yet.

I'm not really sure what to do here. Does the `jj` project have a commitment to stability for these kinds of things yet?

---

_@BurntSushi requested changes on 2025-03-29 13:36_

---

_Comment by @BurntSushi on 2025-03-29 13:38_

And yes, there's no way that `--no-require-git` is going to become a ripgrep default. That would allow gitignore files to cross pollinate into other repos. And nesting git repos inside other git repos is a very common pattern.

---

_Review comment by @fowles on `crates/ignore/src/dir.rs`:290 on 2025-03-29 19:37_

Speaking as a jj developer, this path has been stable for many years and I don't think there is any reason we would change it.

---

_Review comment by @fowles on `crates/ignore/src/dir.rs`:285 on 2025-03-29 20:11_

reworked to avoid the allocation (and also to cache it if it happens so the net result of this change reduces allocations).

---

_@fowles reviewed on 2025-03-29 20:11_

---

_Review requested from @BurntSushi by @fowles on 2025-03-29 20:21_

---

_@martinvonz reviewed on 2025-03-30 06:26_

---

_Review comment by @martinvonz on `crates/ignore/src/dir.rs`:290 on 2025-03-30 06:26_

I'm not aware of any plans to change the that path, but I'm still a bit reluctant to promise that that path won't change. I wonder if just looking for `.jj/` is sufficient. Note that `.gitignore` is respected even if the jj repo is not using the Git backend. I think we would cover most cases by looking only for `.jj/`. The possible cases are:

1. Colocated repos. This is when `.jj/` and `.git/` are siblings and `.jj/repo/store/git_target` points to that `.git/` directory.
2. Internal Git repo. This is when the Git repo lives in side `.jj/`, specifically at `.jj/repo/store/git`.
3. External Git repo. This is when the Git repo lives lives outside `.jj/` but not as its sibling. For example, you can have the ripgrep repo at `~/ripgrep` and a jj repo pointing to it from `~/ripgrep-jj`, so `~/ripgrep-jj/.jj/repo/store/git_target` points to `~/ripgrep/.git`.
4. Non-Git repo. This is when the there is no Git repo involved at all. Currently that only really happens at Google (Google has its own custom backends).

Case 1 is covered by ripgrep's current detection of the `.git/` directory. In all the other cases, there would normally still be a `.gitignore` in the working copy. The only thing we'd lose is that the `.git/info/exclude` would not be respected. I suspect it's quite unusual that users add ignore patterns there, especially when they're using a non-colocated setup. Note that jj respects your global gitignores too in all of the above cases, i.e. regardless of commit backend. (It depends on the `WorkingCopy` backend. We have an implementation at Google that doesn't respect .gitignores at all. rigrep is largely impractical in that environment anyway.)

---

_@martinvonz reviewed on 2025-04-13 17:39_

---

_Review comment by @martinvonz on `crates/ignore/src/dir.rs`:290 on 2025-04-13 17:39_

So, to clarify, I would recommend just looking for `.jj/`. Let me know if you see a problem with that solution.

---

_Review comment by @BurntSushi on `crates/ignore/src/dir.rs`:290 on 2025-04-13 17:51_

I think that SGTM.

---

_@BurntSushi reviewed on 2025-04-13 17:51_

---

_Comment by @BurntSushi on 2025-04-13 17:52_

The next step here is for me to try this out and do some ad hoc perf testing to ensure the extra stat call isn't too big of a perf hit. If it is, we can brainstorm how to mitigate it (or just accept it). My guess is that it will be acceptable.

---

_@fowles reviewed on 2025-04-13 20:47_

---

_Review comment by @fowles on `crates/ignore/src/dir.rs`:290 on 2025-04-13 20:47_

Awesome, I have simplified things to reflect that.

---

_Review comment by @BurntSushi on `crates/ignore/src/dir.rs`:848 on 2025-07-12 16:16_

Are these changes necessary?

---

_Review comment by @BurntSushi on `crates/ignore/src/dir.rs`:297 on 2025-07-12 16:16_

Why don't we need to also do `dir.join(".jj")` here?

---

_@BurntSushi reviewed on 2025-07-12 16:17_

I'm unsure of how this patch is supposed to work. And it looks like there are unrelated changes here?

---

_@BurntSushi reviewed on 2025-07-12 16:19_

---

_Review comment by @BurntSushi on `crates/ignore/src/dir.rs`:297 on 2025-07-12 16:19_

Oh I see, as @martinvonz said above, it looks like we are punting on handling clone-specific excludes.

---

_@BurntSushi approved on 2025-07-12 16:26_

---

_Label `rollup` added by @BurntSushi on 2025-07-12 16:31_

---

_Review comment by @fowles on `crates/ignore/src/dir.rs`:848 on 2025-07-12 17:21_

You had expressed concern about number of allocations and these changes eliminate some extra allocations.  Happy to separate them to a different PR if you prefer

---

_@fowles reviewed on 2025-07-12 17:21_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
