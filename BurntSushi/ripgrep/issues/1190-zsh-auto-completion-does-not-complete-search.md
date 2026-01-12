```yaml
number: 1190
title: ZSH auto-completion does not complete search options
type: issue
state: closed
author: sluongng
labels: []
assignees: []
created_at: 2019-02-08T02:04:43Z
updated_at: 2020-09-04T19:10:27Z
url: https://github.com/BurntSushi/ripgrep/issues/1190
synced_at: 2026-01-12T16:13:23Z
```

# ZSH auto-completion does not complete search options

---

_@sluongng_

#### What version of ripgrep are you using?

```
rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Cargo update

#### What operating system are you using ripgrep on?

MacOS 10.14.2 (18C54)

#### Describe your question, feature request, or bug.

it seems like ZSH tab auto completion broke, no longer getting auto-completion after typing `rg "abc" --file` (expect for it to suggest options)

#### If this is a bug, what are the steps to reproduce the behavior?

`rg "abc" --file<Tab><Tab>`

#### If this is a bug, what is the actual behavior?

autocompleting with ALL files name

#### If this is a bug, what is the expected behavior?

autocompleting with the search options


---

_Comment by @BurntSushi on 2019-02-08 02:12_

Your observed behavior seems like the correct behavior to me. The `--file` flag accepts a single file path as an argument, so it should be competing file paths. Could you please explain why you're expecting it do otherwise?

---

_Comment by @sluongng on 2019-02-08 02:18_

Hi @BurntSushi , I was expecting it to provide me with auto-complete to "--files-with-matches" and "--files-without-match" and I am pretty sure it did in the past.

I use these 2 options pretty regularly and often type `rg <PATTERN> --file<tab><tab>` to select the correct option. Latest update seem to have broke most <Tab> behaviors?

---

_Comment by @okdana on 2019-02-08 02:27_

>Your observed behavior seems like the correct behavior to me. The `--file` flag accepts a single file path as an argument, so it should be competing file paths.

It shouldn't complete further options for the reason you mentioned, but also `--file` isn't offered at all (on versions of zsh that support arg-spec grouping) after `rg abc` because `abc` is taken as a pattern, which is mutually exclusive with `--file`.

On balance, it wouldn't be unreasonable for us to remove that restriction, since the function can't actually assume that `abc` was meant to be a pattern rather than a file.

>I was expecting it to provide me with auto-complete to "--files-with-matches" and "--files-without-match" and I am pretty sure it did in the past. ... Latest update seem to have broke most behaviors?

I've just tested on zsh 5.3 and zsh 5.7 and it seems to work as expected for me. Are those options offered if you try to complete *before* typing the first operand? Can you run `echo $ZSH_VERSION` and `whence -v _rg` and report the results?

---

_Comment by @sluongng on 2019-02-08 02:29_

> > Your observed behavior seems like the correct behavior to me. The `--file` flag accepts a single file path as an argument, so it should be competing file paths.
> 
> It shouldn't complete further options for the reason you mentioned, but also `--file` isn't offered at all (on versions of zsh that support arg-spec grouping) after `rg abc` because `abc` is taken as a pattern, which is mutually exclusive with `--file`.
> 
> On balance, it wouldn't be unreasonable for us to remove that restriction, since the function can't actually assume that `abc` was meant to be a pattern rather than a file.
> 
> > I was expecting it to provide me with auto-complete to "--files-with-matches" and "--files-without-match" and I am pretty sure it did in the past. ... Latest update seem to have broke most behaviors?
> 
> I've just tested on zsh 5.3 and zsh 5.7 and it seems to work as expected for me. Are those options offered if you try to complete _before_ typing the first operand? Can you run `echo $ZSH_VERSION` and `whence -v _rg` and report the results?

```
~> echo $ZSH_VERSION
5.3
~> whence -v _rg
_rg not found
exit 1
~> which rg
/Users/son.luongngoc/.cargo/bin/rg
```

here you go

---

_Comment by @okdana on 2019-02-08 02:36_

>_rg not found

It sounds like the function isn't loaded at all then.

Does Cargo even provide the completion function? I've never used it to install ripgrep before, but i tried just now and i don't see it anywhere. If you had it before, maybe you got it from somewhere else? Homebrew maybe?

---

_Comment by @sluongng on 2019-02-08 04:14_

> > _rg not found
> 
> It sounds like the function isn't loaded at all then.
> 
> Does Cargo even provide the completion function? I've never used it to install ripgrep before, but i tried just now and i don't see it anywhere. If you had it before, maybe you got it from somewhere else? Homebrew maybe?

well I definitely have it since `rg` still run, just the autocompletion does not.
I think `whence` and `which` does the same thing. 

```
whence -v rg
rg is /Users/son.luongngoc/.cargo/bin/rg
```

this works without underscore i think we were just using different system :D

---

_Comment by @sluongng on 2019-02-08 04:18_

FYI i ran cargo uninstall and brew install ripgrep

Still could not get autocompletion to work T_T

---

_Comment by @okdana on 2019-02-08 04:28_

>this works without underscore i think we were just using different system :D

`_rg` is the name of the completion function for `rg`. It's included with ripgrep as a separate file, also named `_rg`. (But, again, i'm not sure if Cargo gives it to you)

>FYI i ran cargo uninstall and brew install ripgrep
>
>Still could not get autocompletion to work T_T

If you install it through Homebrew you should get the `_rg` file under `/usr/local/share/zsh/site-functions`. We're getting outside the scope of ripgrep now, but, assuming that directory is in your `fpath` (and i think it's there by default in the zsh that macOS ships with), it should be loaded by `compinit`. You should have something about that in your `.zshrc`. You'll probably need to restart the shell after installing it, too.

The [zsh documentation](http://zsh.sourceforge.net/Doc/Release/Completion-System.html) goes into more detail about how to set up completion

---

_Comment by @sluongng on 2019-02-08 04:31_

hmm i took a dive into it and manually removed my `_rg` https://github.com/robbyrussell/oh-my-zsh/issues/4127 then uninstall using brew and reinstalled using brew and it worked.

Possibly that not installing via brew, e.g. using Cargo, would break the autocompletion?

I suggest we include `rg --auto-complete` flag to generate autocompletion for ZSH and BASH shells by default?

This issue can be closed

---

_Closed by @sluongng on 2019-02-08 04:31_

---

_Comment by @BurntSushi on 2019-02-08 11:32_

To clarify, `cargo install ripgrep` only installs the binary. It installs nothing else. No man page. No completions. No license. No docs. Nothing. This is why it's the last installation option in the README, and is specifically prefixed with "If you're a Rust programmer ..." If I were more ideologically driven, I probably wouldn't allow ripgrep to be installed with `cargo install` at all, precisely because of confusing issues like this. Nevertheless, people seem to love having this option available to them.

---

_Comment by @Norfeldt on 2020-08-21 06:58_

I had similar issue with **oh my zsh** but and was using `brew`. Added this to the end of my `.zshrc` file: 

```
# https://github.com/zsh-users/zsh-completions/issues/277#issuecomment-414139674
fpath=(
  /usr/local/share/zsh/site-functions
  $fpath
)
rm -f "$HOME/.zcompdump"; compinit 
```

At first it takes a long time to source. But quitting the terminal completely made it speedy again.

---

_Comment by @okdana on 2020-08-21 09:02_

Deleting the `zcompdump` file can fix issues with completion after installing/upgrading something, but doing it in `zshrc` like that will disable `compinit`'s caching mechanism, forcing it to scan through all of your completion files every single time the shell starts. It's similar to calling `compinit` with the `-D` option. There are some situations where that's what you'd want, but it doesn't seem like the right fix if you're just missing a completion function.

Also, OMZ already runs `compinit` for you, so adding that to the end of your `zshrc` is probably making it run twice, slowing it down even more.

Lastly, i'm not sure that putting `/usr/local/share/zsh/site-functions` in your `fpath` there is really doing anything, since it's already at the head of the default `fpath` in both the zsh shipped with macOS and the one in Homebrew.

If you're using `rg` from Homebrew and you're still having problems after deleting your `zcompdump` (once, by hand), there's probably something else going on in your `zshrc` that needs fixed

---

_Comment by @Norfeldt on 2020-08-31 06:25_

@okdana 

> Deleting the `zcompdump` file can fix issues with completion after installing/upgrading something, but doing it in `zshrc` like that will disable `compinit`'s caching mechanism, forcing it to scan through all of your completion files every single time the shell starts. It's similar to calling `compinit` with the `-D` option. There are some situations where that's what you'd want, but it doesn't seem like the right fix if you're just missing a completion function.
> 
> Also, OMZ already runs `compinit` for you, so adding that to the end of your `zshrc` is probably making it run twice, slowing it down even more.
> 
> Lastly, i'm not sure that putting `/usr/local/share/zsh/site-functions` in your `fpath` there is really doing anything, since it's already at the head of the default `fpath` in both the zsh shipped with macOS and the one in Homebrew.
> 
> If you're using `rg` from Homebrew and you're still having problems after deleting your `zcompdump` (once, by hand), there's probably something else going on in your `zshrc` that needs fixed

Thank you very much for taking the time to reply to my latest post. I don't mind sharing my `.zshrc` setup - if you find anything that is causing the autocomplete to not work (and allow me to drop the `compinit` part) then I'm all ears 👂 


```
# THEME
# PowerLevel10k - https://github.com/romkatv/powerlevel10k#oh-my-zsh
# To customize prompt, run `p10k configure` or edit ~/.p10k.zsh.
[[ -f ~/.p10k.zsh ]] && source ~/.p10k.zsh
source ~/powerlevel10k/powerlevel10k.zsh-theme

# function prompt_git_user() {
#     p10k segment -f 003 -i "$(git config user.name)"
#   }

# POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(
#   newline
#   # git_user
# )

# Set default editor
export EDITOR="/usr/bin/nano"

# PATH & ALIAS STUFF
setopt complete_aliases

alias reload="source ~/.zshrc"
alias r="source ~/.zshrc"

# Node & NPM
#PATH="/usr/local/bin:$PATH"
PATH="$PATH:$HOME/.npm-global/bin/"
PATH="$PATH:$HOME/.npm-global/lib/node_modules"

# Git & Git Flow
alias master="git checkout master"
alias dev="git checkout develop"
alias hotfix="git flow hotfix"
alias feature="git flow feature"
alias tags="git push --tags"
alias log="git log --compact-summary --graph --date=relative"
alias git-cleanup="sh ~/Dropbox/Code/Git/git-cleanup.sh"

# Pip - https://gist.github.com/haircut/14705555d58432a5f01f9188006a04ed
PATH="$PATH:~/Library/Python/2.7/bin"
PATH="$PATH:~/Library/Python/3.8.5/bin"

# Android
export ANDROID_HOME=/Users/norfeldt/Library/Android/sdk
PATH="$PATH:${ANDROID_HOME}/platform-tools"
PATH="$PATH:${ANDROID_HOME}/tools"
PATH="$PATH:${ANDROID_HOME}/tools/bin"

alias emu="pushd ${ANDROID_HOME}/tools;emulator -avd Pixel_2; popd"


# SOURCE & OTHER STUFF

# Path to your oh-my-zsh installation.
export ZSH=/Users/norfeldt/.oh-my-zsh


# Autojump
[[ -s `brew --prefix`/etc/autojump.sh ]] && . `brew --prefix`/etc/autojump.sh


# Load zsh-autosuggestions.
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh

# zsh-syntax-highlighting
source /Users/norfeldt/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# NODE VERSION MANAGER
export NVM_DIR=~/.nvm
source ~/.nvm/nvm.sh

# fuzzy search
[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh

alias code="code ."

# ripgrep
export RIPGREP_CONFIG_PATH=$HOME/.ripgreprc

# shell startup.
export PATH

plugins=(git github)
source $ZSH/oh-my-zsh.sh

# https://github.com/zsh-users/zsh-completions/issues/277#issuecomment-414139674
fpath=(
  /usr/local/share/zsh/site-functions
  $fpath
)
rm -f "$HOME/.zcompdump"; compinit 
```

---

_Comment by @okdana on 2020-08-31 06:56_

I don't see any problems with it, at least nothing that would break ripgrep completion.

One thing i should have said before is that when you delete the cache by hand you should use `rm -f ~/.zcompdump*` (with the asterisk), since sometimes there are extra bits at the end of the file name. I glanced at OMZ and it looks like it creates the files that way by default, so that probably applies here. You can see if doing that lets you remove the extra stuff you added.

If it doesn't, the only other thing i can suggest off the top of my head is removing the `compinit` line and putting the `fpath` addition above where you source OMZ. Again, i'd be surprised if that's actually doing anything for you, but that's how i'd expect OMZ to want `fpath` additions to be done.

---

_Comment by @Norfeldt on 2020-09-04 19:10_

I tried your suggestion with `rm -f ~/.zcompdump*` and removed code at the end of my `.zshrc`. It seems to do the trick.

Thanks @okdana 

---
