---
number: 6204
title: uv conflicts with pyenv-virtualenv auto-activation
type: issue
state: closed
author: kozlek
labels: []
assignees: []
created_at: 2024-08-19T13:21:08Z
updated_at: 2025-05-12T19:23:46Z
url: https://github.com/astral-sh/uv/issues/6204
synced_at: 2026-01-07T13:12:17-06:00
---

# uv conflicts with pyenv-virtualenv auto-activation

---

_Issue opened by @kozlek on 2024-08-19 13:21_

Hello,

As a python developer, I'm currently using:
- `pyenv` to install specific python version
- `pyenv-virtualenv` (official `pyenv` plugin) to manage my virtualenvs, including auto-activation
- `poetry` for managing dependencies for bigger projects (mainly using groups)
- `pip-tools` for managing dependencies for smaller projects
- `pipx` for installing CLI tools

I'm very excited about `uv` and I started to integrate it into my workflow.
Both `poetry` and `pip-tools` can be replaced quite easily as `uv` supports most of my needs.
`pipx` is replaced by `uv tool`.

Now enter python version + virtualenvs management.
`uv` can do a lot, but currently it lacks virtualenv auto-activation and virtualenv delocalisation.
It's been discussed #1495, and I totally understand you cannot implement every behaviours a few months after the first release ! 😉

However, I should be able to keep my existing python + virtualenv setup with `pyenv` + `pyenv-virtualenv` (which works well), while enjoying the speed of `uv` for dependencies management, as `pyenv-virtualenv` set a `VIRTUAL_ENV` variable properly.
In reality, it's not working due to `uv` expectation from `.python-version`.
`pyenv-virtualenv` set the name of the virtualenv in `.python-version`, while `uv` seems to expect a python version.

To allow a progressive adoption of `uv`, it would great to be able to continue using `pyenv-virtualenv` auto-activation feature.


## Full workflow to reproduce the bug

`pyenv`, `pyenv-virtualenv` and `uv` are expected to be properly setup according their official documentation.

```sh
# install expected python version
pyenv install 3.12.5

# create a new project
mkdir test
cd test
pyenv virtualenv 3.12.5 test
pyenv local test

# at that point, a .python-version has been created and the shell should have auto activate the new virtualenv
cat .python-version  # test 
echo $VIRTUALENV  # ~/.pyenv/versions/3.12.5/envs/test

# now if we try to use uv to declare package...
uv init  # OK
uv add ruff  # [Error: ](error: No interpreter found for executable name `test` in managed installations or system path)
```

Again, my point is not about implementing a new behaviour in `uv` (yet), but instead to allow it to co-exist with existing tools like `pyenv-virtualenv`, which offer interesting features that `uv` lacks for the moment 🙂

I'll be happy to help if I can !  

---

_Comment by @cmin764 on 2024-10-30 12:53_

I have exactly the same problem and I think this would be a big leap in adoption if the author vouches for it. Hope for the best from the developers! 🤞🏼 

---

_Comment by @dannysteenman on 2024-10-30 19:08_

+1 for this!

I have a temporary solution for anyone that has this issue. I made a fork from the zsh-autoswitch-virtualenv repo and made it work with uv venv so that it automatically switches environments once you get into a working directory containing uv virtual environment: https://github.com/dannysteenman/zsh-autoswitch-virtualenv

Just load this script in your .zshrc and you're good to go!

---

_Comment by @albireox on 2024-10-30 19:17_

Another +1 from me.

So far this has been my main headache with my transition to `uv` since my project uses `pyenv` quite extensively and it will take time to transition out of it.

My solution has been using [direnv](https://direnv.net) and using this layout in the project `.envrc`

```bash
layout_uv_pyenv() {
    if [ -e ".python-version" ]; then
        VENV=`cat .python-version`
        VENV_PATH="$PYENV_ROOT/versions/$VENV"
        export UV_PYTHON=$VENV_PATH/bin/python
        export UV_PROJECT_ENVIRONMENT=$VENV_PATH
        export UV_ACTIVE=1
    fi
}
```

which works reasonably well, but it would be nice if `uv` could just default (or be configured) to using the environment loaded in `$VIRTUAL_ENV`.

---

_Referenced in [modelcontextprotocol/create-python-server#5](../../modelcontextprotocol/create-python-server/issues/5.md) on 2024-11-25 23:42_

---

_Comment by @fredrikaverpil on 2024-12-25 20:31_

In case it helps anyone, I've got this in my `.zshrc` which seems to work pretty ok (activate/deactivate .venv on `cd`, `z` or `zi`):

```bash
function virtual_env_activate() {
  # check the current folder belong to earlier VIRTUAL_ENV folder, potentially deactivate.
  if [[ -n "$VIRTUAL_ENV" ]]; then
    parentdir="$(dirname "$VIRTUAL_ENV")"
    if [[ "$PWD"/ != "$parentdir"/* ]]; then
      deactivate
    fi
  fi

  # if .python-version is found, create/show .venv
  if [ -f .python-version ] && [ ! -d ./.venv ]; then
    uv venv
  fi

  # if .venv is found and not activated, activate it
  if [[ -z "$VIRTUAL_ENV" ]]; then
    # if .venv folder is found then activate the vitualenv
    if [ -d ./.venv ] && [ -f ./.venv/bin/activate ]; then
      source ./.venv/bin/activate
    fi
  fi
}


function cd() {
  builtin cd "$@" || return
  virtual_env_activate
  # node_version_manager  # TODO: with pkgx, maybe nvm is no longer needed?
}
cd . # trigger cd overrides when shell starts

function z() {
  __zoxide_z "$@" && cd . || return
}

function zi() {
  __zoxide_zi "$@" && cd . || return

}
```

Full source: https://github.com/fredrikaverpil/dotfiles/blob/main/shell/sourcing.sh

---

_Comment by @EricPobot on 2025-01-02 11:34_

+1 here too. I'm too in the process of assessing if I'll switch from pyenv (which I'm 100% satisfied with) to uv and have to manage the coexistence with projects using pyenv and that won't be modified. 

uv using the same .python-version file as pyenv is not a good choice because this raises the probabilities of conflicts. 

This doesn't help in favoring uv because the resulting hiccups tend to be annoying and prevent from using the tool in the best possible conditions.

---

_Referenced in [astral-sh/uv#10372](../../astral-sh/uv/issues/10372.md) on 2025-01-07 18:34_

---

_Referenced in [astral-sh/uv#11273](../../astral-sh/uv/issues/11273.md) on 2025-02-06 04:51_

---

_Referenced in [astral-sh/uv#11544](../../astral-sh/uv/issues/11544.md) on 2025-02-16 18:53_

---

_Comment by @erny on 2025-03-09 19:30_

> Another +1 from me.
Another +1 from me too.
 
> So far this has been my main headache with my transition to `uv` since my project uses `pyenv` quite extensively and it will take time to transition out of it.
> 
> My solution has been using [direnv](https://direnv.net) and using this layout in the project `.envrc`
> 
> layout_uv_pyenv() {
>     if [ -e ".python-version" ]; then
>         VENV=`cat .python-version`
>         VENV_PATH="$PYENV_ROOT/versions/$VENV"
>         export UV_PYTHON=$VENV_PATH/bin/python
>         export UV_PROJECT_ENVIRONMENT=$VENV_PATH
>         export UV_ACTIVE=1
>     fi
> }
> which works reasonably well, but it would be nice if `uv` could just default (or be configured) to using the environment loaded in `$VIRTUAL_ENV`.

I had problems invoking `uv sync` with the same error, so I changed the pyenv-virtualenv plugin to define UV variables on auto-activating virtuaenvs: In `$HOME/.pyenv/plugins/pyenv-virtualenv/bin/pyenv-virtualenv-init` near line 132, just after activating the virtualenv with `pyenv sh-activate` I added:

    export UV_PYTHON=$VIRTUAL_ENV/bin/python
    export UV_PROJECT_ENVIRONMENT=$VIRTUAL_ENV
    export UV_ACTIVE=1


---

_Referenced in [kreuzberg-dev/kreuzberg#36](../../kreuzberg-dev/kreuzberg/issues/36.md) on 2025-04-08 20:11_

---

_Referenced in [beeware/briefcase#2036](../../beeware/briefcase/issues/2036.md) on 2025-05-04 03:14_

---

_Comment by @ThomAub on 2025-05-12 17:35_

Hello @kozlek after our deep discussion at the bar I think you should just use uv for your project 

---

_Comment by @zanieb on 2025-05-12 17:44_

We now ignore `pyvenv-virtualenv` `.python-version` files instead of failing, at least. See #12909 

---

_Comment by @brycedrennan on 2025-05-12 18:02_

Just to be clear, does this mean `pyenv-virtualenv` and `uv` now play nice together?  

This would be great news as I could not push switching to `uv` since it would trigger a massive single-time cost of moving all the repos over at once.

---

_Comment by @kozlek on 2025-05-12 18:38_

> We now ignore `pyvenv-virtualenv` `.python-version` files instead of failing, at least. See [#12909](https://github.com/astral-sh/uv/pull/12909)

Indeed, thanks to the `--active` flag, we're able to add dependencies to the active venv.
Except the warning regarding the `.python-version` parsing error, everything seems to work well ! 

Thanks for your help, feel free to close the issue ! 🙂

---

_Closed by @konstin on 2025-05-12 19:23_

---

_Referenced in [astral-sh/uv#15603](../../astral-sh/uv/issues/15603.md) on 2025-08-31 08:56_

---
