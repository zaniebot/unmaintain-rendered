```yaml
number: 2447
title: Disable specific ignore files (specifically $HOME/.gitignore)
type: issue
state: closed
author: munro
labels:
  - wontfix
assignees: []
created_at: 2023-03-09T18:58:44Z
updated_at: 2023-03-10T00:03:04Z
url: https://github.com/BurntSushi/ripgrep/issues/2447
synced_at: 2026-01-12T16:13:24Z
```

# Disable specific ignore files (specifically $HOME/.gitignore)

---

_@munro_

Hello!

I manage my `~/` directory dotfiles with git.  And thus, I have a special `~/.gitignore` that excludes everything but dotfiles [1].

Sooooo what happens is when I use `rg`, it never finds anything unless I add `--no-ignore-vcs`.  Problem with that is I still want it to use every other `.gitignore` — because it's really helpful!

I also tried `--no-ignore-parent`, which seems to work well, except it doesn't work at all when I'm in `~/` which is annoying.

__It would be really nice to have something like `rg --no-ignore-exclude=~/.gitignore`__

[1] https://github.com/munro/dotfiles
```gitignore
# ignore everything
*
# except dotfiles!!!!!
!.*
!.oh-my-zsh


# don't commit these dot files to the repo
.DS_Store
.Xauthority
.bash_history
.brew_packages
.cargo/
# ...
```

---

_Comment by @BurntSushi on 2023-03-09 19:05_

Your specific feature request is definitely never going to happen. There are already a bazillion flags for ignoring various flavors of ignore files, and I'm sorry, but there is just no way I could justify adding more flags to override individual ignore files. Both the user facing and internal complexity of that is mind boggling to me.

However, thank you for sharing the actual problem you have. Because there are solutions to it.

The first it to add a `.ignore` file right next to your `.gitignore` that whitelists everything, and then blacklists whatever you don't want to search. For example, `echo '!*' > ~/.ignore`.

Second is the approach I use now. I used to do the first approach of, but other than it being annoying for ripgrep, I actually didn't like it because it was annoying to add new files since `git` ignored everything by default. It happened too often that I'd add something, forget about it, later run `git status` and see that there were no changes. And only then realize that I had never actually committed the new thing I added. So now I [maintain an exhaustive list of ignored files](https://github.com/BurntSushi/dotfiles/blob/23488b723aaab4c08fc857e55454938efdc3ac6e/.gitignore#L1-L3) in my home directory.

---

_Closed by @BurntSushi on 2023-03-09 19:05_

---

_Label `wontfix` added by @BurntSushi on 2023-03-09 19:05_

---

_Comment by @munro on 2023-03-10 00:02_

@BurntSushi yesss thank you `echo '!*' > ~/.ignore` worked perfectly!  it wasn't totally obvious how I could exclude that file, thank you!

![love-you-brown-bear-1 2](https://user-images.githubusercontent.com/500774/224188726-687b3a43-42f9-4c31-ab9d-4d7a51c805cd.gif)


---
