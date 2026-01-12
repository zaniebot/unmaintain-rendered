```yaml
number: 2396
title: "ignore: respect local .git/config excludes"
type: pull_request
state: closed
author: zaidhaan
labels: []
assignees: []
draft: true
base: master
head: use-local-git-conf
created_at: 2023-01-12T12:14:37Z
updated_at: 2023-07-07T18:35:54Z
url: https://github.com/BurntSushi/ripgrep/pull/2396
synced_at: 2026-01-12T18:23:14Z
```

# ignore: respect local .git/config excludes

---

_@zaidhaan_

Currently, ripgrep respects `$HOME/.gitconfig` and `$XDG_CONFIG_HOME/git/config`, but not `.git/config` (the "local" or "project" config). Git makes it such that `.git/config` overrides the other two options, while still respecting the `./.gitignore`. This commit brings this functionality to ripgrep (while also cleaning a few repetitive segments of code).

To demonstrate with a reproducible example (of course, be careful with overriding any files):
```sh
$ mkdir test && cd test
$ touch file file.customexclude file.globalignore file.pwdignore
$ echo '*.pwdignore' > .gitignore
$ mkdir -p /tmp/tempdir && echo '*.customexclude' > /tmp/tempdir/.gitignore
$ git config core.excludesfile /tmp/tempdir/.gitignore
$ echo '*.globalignore' > ~/.gitignore
$ git config --global core.excludesfile ~/.gitignore
$ git status
...
	.gitignore
	file
	file.globalignore
...
$ rg --files # without this patch
file.customexclude
file
$ rg --files # with the patch
file
file.globalignore
```

Note that the global ignore file is [intended to be ignored](https://stackoverflow.com/questions/8801729/is-it-possible-to-have-different-git-configuration-for-different-projects) when a `.git/config` exists (as demonstrated by the output of git).

Fixes #2392

---

_Comment by @zaidhaan on 2023-02-08 15:20_

Ping @BurntSushi. Had any time to review this?

---

_Comment by @BurntSushi on 2023-02-08 15:40_

After a quick look I don't think this is right? It appears to assume that `.git` must be in the CWD. But whether `.git/config` is used or not shouldn't depend on whether ripgrep is run at the root of the repo. It might be run above the root or even below, but in both cases, `.git/config` should be respected. This will unfortunately probably require quite a gnarly refactor that I don't think I have the bandwidth for right now.

---

_Comment by @zaidhaan on 2023-02-10 04:18_

> It appears to assume that .git must be in the CWD

Whoops, you're right.
```sh
$ mkdir subdir && cd subdir
$ touch subfile subfile.customexclude subfile.globalignore subfile.pwdignore
$ # only subfile and subfile.globalignore should be seen
$ rg --files
subfile
subfile.customexclude
$ rg-patched --files
subfile
subfile.customexclude
```

I'm not too familiar with the code but it would appear that in dirs.rs there's going to have to be a matcher specifically made for `.git/config`'s `excludesfile` then? Which I suppose would bring in a bit of logic (`parse_excludes_file`) into dirs.rs. After such a matcher is made that would produce a `Gitignore`, then some logic could be used to decide whether to use the `global_matcher` if the config (which would override it) already exists.

Is this somewhere in the general right direction (at least with the way the code is now)? I've been working a bit on an approach like what I just described, might be able to make another commit in a bit.

(This admittedly isn't that elegant since anything to do with the git dir, git config and excludesfile is done in gitignore.rs, with the apparent exception of .git/info/exclude logic which is handled in dir.rs)

There's still a lot of cases I haven't explored (would global ignore be used if ~/.gitconfig exists with an excludes file, etc.), so I'll mark this PR as a draft. I'll try to see how git behaves in all cases this weekend and document it here, that way if this PR doesn't land at the very least it might help whoever ends up implementing this.

---

_Converted to draft by @zaidhaan on 2023-02-10 04:35_

---

_Comment by @zaidhaan on 2023-02-10 04:45_

The above commit removes the incorrect CWD parsing and moves the `.git/config` excludesfile ignoring logic to `add_child_path` in dirs.rs (which looks at all the options in `IgnoreOptions`), I didn't create another `IgnoreOptions` option, but instead made it such that it respects the `git_exclude` option (which prior to this only looked at `.git/info/exclude`, but not would also look at `.git/config`s excludesfile).

Haven't tested this that thoroughly, still a WIP, but so far it does work in subdirectories since it correctly identifies the git path.

```sh
$ # all the setup stuff
$ mkdir subdir && cd subdir
$ touch subfile subfile.customexclude subfile.globalignore subfile.pwdignore
$ git add -A

$ git ls-files
subfile
subfile.globalignore

$ rg --files
subfile
subfile.customexclude

$ rg-patched --files
subfile
subfile.globalignore
```

---

_Comment by @BurntSushi on 2023-07-07 18:35_

Thanks for the work on this, but I'm going to close it. The `ignore` crate badly needs an overhaul, and hopefully I can address this use case when I do that. But I'm not sure when it's going to happen. As of now, I can't really stomach adding more stuff like this because I can't be confident that it won't break other stuff.

I've opened #2553 to track this particular use case so that it's factored into the overhaul.

---

_Closed by @BurntSushi on 2023-07-07 18:35_

---
