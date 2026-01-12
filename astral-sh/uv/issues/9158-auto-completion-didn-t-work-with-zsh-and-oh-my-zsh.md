```yaml
number: 9158
title: "auto completion didn't work with zsh and oh my zsh"
type: issue
state: closed
author: amirreza8002
labels:
  - needs-mre
assignees: []
created_at: 2024-11-16T01:17:53Z
updated_at: 2024-11-21T02:50:51Z
url: https://github.com/astral-sh/uv/issues/9158
synced_at: 2026-01-12T15:59:43Z
```

# auto completion didn't work with zsh and oh my zsh

---

_@amirreza8002_

running 
`echo 'eval "$(uv generate-shell-completion zsh)"' >> ~/.zshrc`
did not start auto-completion
this was done weeks ago and never once worked

what i have done now, is 
```
cd $ZSH_CUSTOM/plugins
touch uv.plugin.zsh
uv generate-shell-completion zsh >> uv.plugin.zsh
```
and then adding `uv` to `plugin` section of `.zshrc`


but i think something like [poetry](https://python-poetry.org/docs/#oh-my-zsh)'s way should be implemented

let me know what you think

---

_Comment by @toyourheart163 on 2024-11-16 04:49_

update ohmyzsh or make a uv folder

step 1

```bash
cd $ZSH_CUSTOM/plugins
mkdir uv && cd uv  # add this line
uv generate-shell-completion zsh > uv.plugin.zsh
```

step 2

```bash
exec zsh
```

---

_Comment by @amirreza8002 on 2024-11-16 05:45_

> mkdir uv && cd uv  # add this line

well actualy it's not necessery
i'm not even sure if it should be there, haven't tested, i went with the way that omz maintainer had written

also you need to add `uv` to `.zshrc`'s `plugin` section


---

_Comment by @toyourheart163 on 2024-11-16 12:43_

you are right. don't need to `mkdir uv`

but `echo 'eval "$(uv generate-shell-completion zsh)"' >> ~/.zshrc` work for me.

### Maybe you should show `your ~/.zshrc` or upgrade `zsh`

If you update your ohmyzsh. there's a plugin uv in `~/.oh-my-zsh/plugins/uv` you can test by

```bash
cd ~/.oh-my-zsh/plugins
mv uv{,.bak}
```

then

```bash
echo 'eval "$(uv generate-shell-completion zsh)"' >> ~/.zshrc
exec zsh
```

### my zsh version

```bash
zsh --version
# zsh 5.9 (aarch64-unknown-linux-android) 
```

### my `~/.zshrc`

```bash
export ZSH="$HOME/.oh-my-zsh"
ZSH_THEME="ys"
source $ZSH/oh-my-zsh.sh
eval "$(uv generate-shell-completion zsh)"
```

if you update ohmyzsh. you can add uv to plugins

```bash
# ~/.zshrc
plugins=(git uv)
```


---

_Comment by @amirreza8002 on 2024-11-16 23:02_

zsh is installed via pacman and is at latest version
OMZ auto updates and is at latest version

running `echo 'eval "$(uv generate-shell-completion zsh)"' >> ~/.zshrc` did not work for me

`uv generate-shell-completion zsh >> ~/.zshrc` did work, but it added too many lines to my file
so i did what i described

---

_Comment by @samypr100 on 2024-11-20 23:57_

To clarify, is the issue with stock zsh or only with zsh + omz enabled? It sounds like it's omz related only, but I might be wrong.

Does stock zsh not work with the [documented](https://docs.astral.sh/uv/getting-started/installation/#shell-autocompletion) steps? 

Can you provide a reproducible configuration steps (e.g. zsh + omz install + plugins + uv command that doesn't auto-complete)?

---

_Label `needs-mre` added by @samypr100 on 2024-11-20 23:58_

---

_Comment by @amirreza8002 on 2024-11-21 01:20_

```
sudo pacman -S zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
echo 'eval "$(uv generate-shell-completion zsh)"' >> ~/.zshrc
```

```
plugins=(
    git
    poetry
    asdf
    zsh-autosuggestions
  )
```

nothing i used worked with auto-complation

---

_Comment by @samypr100 on 2024-11-21 02:21_

@amirreza8002 Thanks.

Using `uv 0.5.4` and `zsh 5.9`, I tried `echo 'eval "$(uv generate-shell-completion zsh)"' >> ~/.zshrc` and reloaded the shell.

I tried on stock zsh and it works as expected.
I also tried with omz using above and it also seems to work as expected. Note, only tried with `git` and `zsh-autosuggestions` plugins.

```zsh
➜  ~ uv <TAB or ^I>
add                        -- Add dependencies to the project
build                      -- Build Python packages into source distributions and wheels
build-backend              -- The implementation of the build backend
cache                      -- Manage uv's cache
clean                      -- Clear the cache, removing all entries or those linked to specific packages
export                     -- Export the project's lockfile to an alternate format
generate-shell-completion  -- Generate shell completion
help                       -- Display documentation for a command
init                       -- Create a new project
lock                       -- Update the project's lockfile
pip                        -- Manage Python packages with a pip-compatible interface
publish                    -- Upload distributions to an index
python                     -- Manage Python versions and installations
remove                     -- Remove dependencies from the project
run                        -- Run a command or script
self                       -- Manage the uv executable
sync                       -- Update the project's environment
tool                       -- Run and install commands provided by Python packages
tree                       -- Display the project's dependency tree
venv                       -- Create a virtual environment
version                    -- Display uv's version
```


---

_Comment by @amirreza8002 on 2024-11-21 02:37_

hi
seems like i had older uv version, it got fixed with an update.
tho the docs at the time were the same, perhaps it's been fixed in later versions

sorry for the false alarm 🙏

---

_Comment by @samypr100 on 2024-11-21 02:50_

Thanks for checking and following up. I will proceed to close the issue 😄

---

_Closed by @samypr100 on 2024-11-21 02:50_

---
