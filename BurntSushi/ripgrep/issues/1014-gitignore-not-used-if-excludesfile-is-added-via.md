```yaml
number: 1014
title: "gitignore not used if excludesfile is added via [include] block"
type: issue
state: closed
author: laur89
labels:
  - bug
assignees: []
created_at: 2018-08-14T10:58:02Z
updated_at: 2020-11-03T15:55:25Z
url: https://github.com/BurntSushi/ripgrep/issues/1014
synced_at: 2026-01-12T16:13:22Z
```

# gitignore not used if excludesfile is added via [include] block

---

_@laur89_

#### What version of ripgrep are you using?

```
ripgrep 0.9.0 (rev e86d3d95c2)
-SIMD -AVX
```

#### How did you install ripgrep?

deb release fetched from github.

#### What operating system are you using ripgrep on?

Debian testing.

#### Describe your question, feature request, or bug.

Global gitignore file is not picked up if `~/.gitconfig` doesn't include the reference to ignoresfile directly, but via
```
[include]
    path = /path/to/sub-git-config
```
and only `/path/to/sub-git-config` includes
```
[core]
    excludesfile = /home/user/.gitignore
```

#### If this is a bug, what are the steps to reproduce the behavior?
1. move all your ~/.gitconfig content to ~/.gitconfig_include
2. replace ~/.gitconfig contents with
```
[include]
    path = ~/.gitconfig_include
```
3. search for content/file that includes files matched by your ~/.gitignore
4. rg returns the file you expected to be ignored

#### If this is a bug, what is the actual behavior?

As can be seen running with `--debug` flag on, global gitignore file is not picked up.


---

_Label `bug` added by @BurntSushi on 2018-08-14 11:01_

---

_Label `icebox` added by @BurntSushi on 2018-08-14 11:01_

---

_Comment by @BurntSushi on 2018-08-14 11:01_

This is a bug, but I'm not sure when or if it will get fixed.

---

_Comment by @BurntSushi on 2020-05-08 11:42_

I'm not sure it's worth fixing this. ripgrep isn't ever going to bundle a full git configuration parser, so it's likely there will always be some kind of gap anyway.

With that said, [the code for loading your git config](https://github.com/BurntSushi/ripgrep/blob/a2e6aec7a4d9382941932245e8854f0ae5703a5e/crates/ignore/src/gitignore.rs#L589-L604) is fairly well isolated. So if an industrious individual can find a fix for this that's simple and safe, then I'd be willing to accept a PR.

---

_Closed by @BurntSushi on 2020-05-08 11:42_

---

_Label `icebox` removed by @BurntSushi on 2020-05-08 11:43_

---

_Comment by @piyo on 2020-05-11 14:50_

Thank you for the above clarification. I agree with closing this issue.

The ripgrep v12.1.0 documentation says "In some cases, ripgrep and git will not always be in sync in terms of which files are ignored". Currently this issue is one of those cases.

Can I understand why ripgrep does not ask the git-config executable for the location of the global excludes file? i.e. the one specified by [core.excludesFile](https://git-scm.com/docs/gitignore#_configuration) (`git config --get core.excludesFile`) ? Is it for execution performance reasons or safety reasons?

It seems `rg` already provides a workaround. Run the above and supply it to `rg` directly, ie.
```sh 
git_global_exclude_file_memoize="$(git config --get core.excludesFile)"
rg --ignore-file="$git_global_exclude_file_memoize" ...
```

---

_Comment by @BurntSushi on 2020-05-11 15:00_

That work-around is pretty good. It's not strictly equivalent since `--ignore-file` [has a lower precedence than global gitignores](https://docs.rs/ignore/0.4.15/ignore/struct.WalkBuilder.html#ignore-rules), although, the only thing with a lower precedence than global gitignores is `--ignore-file`, so I guess that works.

Otherwise, the reason for not using `git` itself is simply because 1) I'd rather `rg` didn't unconditionally spawn a sub-process for every execution and 2) it requires `git` to actually be installed and available for ripgrep to use it.

---

_Comment by @laur89 on 2020-05-12 16:16_

No idea why, but this issue is no longer reproducing for me.
Gitignore is still including additional config, yet the gitignore settings are now picked up.

v `12.0.1`

---

_Comment by @BurntSushi on 2020-05-12 16:55_

ripgrep doesn't follow includes in git config files, so something else must be going on. Run ripgrep with `--debug` to see where the ignore rules are coming from.

---

_Comment by @laur89 on 2020-11-03 14:40_

Given we only manage ignores via git, would it be reasonable (read: safe) to symlink `.rgignore` to wherever our global gitignore file is?

---

_Comment by @BurntSushi on 2020-11-03 15:03_

I think that should be okay. It won't have the same precedence as global gitignores (global has lower precedence than rgignore), but that isn't necessarily a problem in and of itself. Just the only difference I can think of.

---

_Comment by @laur89 on 2020-11-03 15:54_

Cheers.
For others with similar problem - if possible stop defining custom location for `core.excludesfile` and use the default `~/.config/git/ignore` as suggested in [this SO answer](https://stackoverflow.com/a/22885996).

Rg already picks that one up and all the problems go away with none of the workarounds or hacks.

---
