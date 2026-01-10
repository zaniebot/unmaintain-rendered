```yaml
number: 1910
title: "Add a command to activate the virtual environment, e.g., `uv shell`"
type: issue
state: open
author: twiddles
labels:
  - virtualenv
  - cli
assignees: []
created_at: 2024-02-23T12:26:49Z
updated_at: 2026-01-01T22:54:31Z
url: https://github.com/astral-sh/uv/issues/1910
synced_at: 2026-01-10T03:11:31Z
```

# Add a command to activate the virtual environment, e.g., `uv shell`

---

_Issue opened by @twiddles on 2024-02-23 12:26_

**Summary**
I propose the introduction of a new command, `uv shell`. This command would provide users with a quick and straightforward way to enter the associated Python virtual environment (venv) for their projects.

**Motivation**
The inspiration for `uv shell` comes from the convenience and simplicity offered by the `poetry shell` command in Poetry. Having worked with poetry on multiple OSs over the last year, I miss this intuitive approach to environment management. I propose introducing a similar capability into uv.

**Proposed Solution**
The `uv shell` command would activate the project's Python virtual environment (basically a shorthand for `.venv/bin/activate`), allowing users to immediately start working within the context of that environment **OR** return an error message if the venv is missing. This feature would abstract away the need to manually source the venv activation script, thus providing a more seamless development workflow across operating systems.



---

_Label `project-management` added by @zanieb on 2024-02-23 15:12_

---

_Label `cli` added by @zanieb on 2024-02-23 15:12_

---

_Label `virtualenv` added by @zanieb on 2024-02-23 15:13_

---

_Label `project-management` removed by @zanieb on 2024-02-23 15:13_

---

_Comment by @mgaitan on 2024-02-24 03:27_

This is somewhat similar to the `workon <venv>` command of [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/) (a tool I have been using for many years), except that here each virtualenv is referrer by a name and not its path (its not stored inside the project directory but in as a subidir of a command path defined by $WORKON_HOME . like `~/.venvs`)

Also, a functionality that I find useful of virtualenvwrapper are the hooks (pre/post hooks on venv activation and venv creation)

If you can include some of these features in `uv` many of us would be grateful! . 


---

_Comment by @rgilton on 2024-02-24 10:30_

I frequently use `pipenv shell`, and the killer feature of this is that it starts _a new_ shell.  Whereas activating a virtual environment modifies the currently running shell.  This means that one can exit the shell without exiting the terminal window one is running the shell in.  It would be great if `uv shell` could replicate this behaviour.

---

_Comment by @jfcherng on 2024-02-25 20:11_

Would like to have an even shorter alias at the same time such as `uv sh` (just like `uv v` for `uv venv`).

---

Fwiw, this is what I have for my Bash shell on both Windows (git-bash) and Linux right now.

## Bash Script

You can add this into your `.bashrc` / `.bash_aliases` or whatever auto sourced bash file.

```bash
uvsh() {
    local venv_name=${1:-'.venv'}
    venv_name=${venv_name//\//} # remove trailing slashes (Linux)
    venv_name=${venv_name//\\/} # remove trailing slashes (Windows)

    local venv_bin=
    if [[ -d ${WINDIR} ]]; then
        venv_bin='Scripts/activate'
    else
        venv_bin='bin/activate'
    fi

    local activator="${venv_name}/${venv_bin}"
    echo "[INFO] Activate Python venv: ${venv_name} (via ${activator})"

    if [[ ! -f ${activator} ]]; then
        echo "[ERROR] Python venv not found: ${venv_name}"
        return
    fi

    # shellcheck disable=SC1090
    . "${activator}"
}
```

## Usage

- `$ uvsh`: this will activate `.venv` by default
- `$ uvsh .my_venv`: this will activate `.my_venv`
- `$ deactivate`: exit the current venv

---

_Comment by @mitsuhiko on 2024-02-26 07:53_

I had this in rye and backed it out because there is just no non hacky way to make this work. I think the desire is real, but the experience just will always have holes. You basically need to allocate a pty, hook it up with the parent pty and then send key presses into it to manipulate the shell environment. It's pretty hacky and breaks stuff in subtle ways :(

---

_Comment by @rgilton on 2024-02-26 21:12_

> You basically need to allocate a pty, hook it up with the parent pty and then send key presses into it to manipulate the shell environment. It's pretty hacky and breaks stuff in subtle ways :(

Is it not just a case of exec'ing `$SHELL`, after modifying the environment as required?  (Or exec'ing `$SHELL` with an argument to make it source the venv's `activate` script on start-up?)


---

_Comment by @NMertsch on 2024-02-27 06:52_

For work I'm switching between Windows and Linux a lot. Having one command for activating would be nice to have, so I don't have to deal with `.venv\Scripts\activate.bat` vs `. .venv/bin/activate`.

---

_Comment by @rgilton on 2024-02-28 10:15_

[Here's the script](https://gist.github.com/rgilton/8bbc1aa0810fed454bd47d37eb32051f) that I am currently using as a stand-in for `uv shell`.  Currently specific to bash, but that fits my use-case.  Making it work on other platforms/shells looks reasonably tractable as [demonstrated in pipenv](https://github.com/pypa/pipenv/blob/main/pipenv/shells.py).

Quick demo of it in operation:
```
[rob@zem foo-project(main)]$ uv venv
Using Python 3.12.1 interpreter at /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
[rob@zem foo-project(main)]$ cd src
[rob@zem src(main)]$ uvs
Entering virtualenv /home/rob/tmp/foo-project/.venv
(foo-project) [rob@zem src(main)]$ 
(foo-project) [rob@zem src(main)]$ uvs
Error: Already within virtualenv
(foo-project) [rob@zem src(main)]$ 
exit
[rob@zem src(main)]$ 
```


---

_Comment by @tiagofassoni on 2024-04-08 04:11_

What about if, instead of hooking up pty's, we just do the bare minimum? We detect the shell (whether zsh, bash, nushell or powershell), the OS, and just call the source file.

Although I'm afraid @mitsuhiko thought of this and decided not to do it, so perhaps it just isn't feasible.

---

_Comment by @zanieb on 2024-04-08 04:25_

Unfortunately you can't `source` things _for_ someone from another process.

---

_Comment by @rgilton on 2024-04-08 09:14_

I think there are two separate feature requests that are being combined in this issue:

 1. The ability to spawn a _new shell_ as a new process with the venv activated within it.  This is what `pipenv shell`, and `poetry shell` do.
 2. The ability to activate the venv within the _current shell_.  As running `source .venv/bin/activate` would do.

I think the talk of injecting keypresses into a shell via a pty is about the second item, whereas the original request in this issue was about the first.  My preference is for the first, as it is straightforward to 'exit' the spawned shell without exiting the terminal emulator/window, whereas if the current shell is modified then it is not easy to 'unmodify' it.

---

_Comment by @samypr100 on 2024-04-10 04:50_

I think the first option somewhat reflects what pixi does, https://github.com/prefix-dev/pixi/blob/main/src/cli/shell.rs

I gave their impl a quick whirl on uv to test, and seems to works pretty well

<img width="375" alt="ps_venv_shell" src="https://github.com/astral-sh/uv/assets/3933065/635887b6-c2d0-40e1-817e-5bcfcd76d966">



---

_Comment by @patrick-kidger on 2024-04-17 08:40_

FWIW I've always considered `poetry shell` a misfeature from a UX perspective. It's too easy to drop into a shell, then change directory, and lose track of which venv you're actually working in.

So for my team I prefer to recommend always using `poetry run` (often plus some shell aliases to minimise typing). I think that would make better sense as a first-class citizen for `uv`. And then if someone still really *really* wants a shell then this is still accomplishable via `uv run [bash|fish|etc.]`.

---

_Comment by @zefr0x on 2024-05-16 21:59_

> Unfortunately you can't `source` things _for_ someone from another process.

What about providing a wrapper shell script to do the sourcing part?

Just like [`virtualfish`](https://github.com/justinmayer/virtualfish), but simple and using `uv`.

We have two options:
1. `uv` will manage every thing from creating to deleting venvs in a global directory. But for the sourcing part the shell script will do the task.
2. The script will be a wrapper for all related venv management tasks using `uv` behind-the-scenes. _(**best**, since we will not need to toggle between multiple commands for venv related tasks)_

---

_Comment by @lmanc on 2024-05-18 10:03_

I don't know if this is relevant here, but sometimes I have difficulties understanding which venv I'm actually using and what's happening under the hood (and why). I'm not sure if there's already a way to figure out these things and I'm just missing it.

Consider this scenario: I have a project in VSCode without a venv. I open the terminal and run `uv pip list`, which outputs:
```
Package    Version
---------- -------
pip        23.0.1
setuptools 65.5.0
```
This output reflects my clean, system environment.

Next, I run `uv venv`. According to the documentation:
```
By default, `uv` lists packages in the currently activated virtual environment, or a virtual environment (`.venv`) located in the current working directory or any parent directory, falling back to the system Python if no virtual environment is found.
```
So, I'm expecting `uv` to pick the local `.venv` without activation, and it does, even if the venv is not activated.

Now, if I close VSCode and reopen it, VSCode continues with its "implicit" activation of `.venv` in every terminal I open from now on.

If, for some reason, I now delete `.venv` and run `uv pip list`, I get this error:
```
$ uv pip list
error: failed to canonicalize path `<path>`
  Caused by: The system cannot find the file specified. (os error 2)
```
While I expected the same output (the system environment) as before.

I think it's VSCode's fault because I guess it somehow implicitly runs something like `source .venv/bin/activate` if I open it with a venv already created. However, since the venv is now activated, I think this "overrides" uv's standard venv detection flow when none is activated.

So my point is: it would be nice to have some mechanism inside of `uv` to check which venv it is currently selecting and why (e.g., "explicit activation of `<path>`", "implicit use since no other venvs are activated and there's a `.venv` in the current directory", "implicit system environment use because no venv is activated and no `.venv` in the current directory," etc.).

---

_Comment by @zanieb on 2024-05-18 13:25_

Hi @lmanc — thanks for the write up!

I'm actually adding tracking of all of these sources at #3266 so I think we'll have better logging of that in the near future.

---

_Comment by @NikosAlexandris on 2024-06-11 08:15_

Just a kind reminder of the existence of https://github.com/direnv/direnv. It would be actually nice to learn from it and explore integration with it ?

---

_Comment by @zefr0x on 2024-07-29 14:19_

> > Unfortunately you can't `source` things _for_ someone from another process.
> 
> What about providing a wrapper shell script to do the sourcing part?
> 
> Just like [`virtualfish`](https://github.com/justinmayer/virtualfish), but simple and using `uv`.
> 
> We have two options:
> 
>    1. `uv` will manage every thing from creating to deleting venvs in a global directory. But for the sourcing part the shell script will do the task.
> 
>    2. The script will be a wrapper for all related venv management tasks using `uv` behind-the-scenes. _(**best**, since we will not need to toggle between multiple commands for venv related tasks)_


I wrote a simple function for the Fish shell: https://github.com/zefr0x/dotfiles/blob/f3f20a4ab91c198674211c76ad431ef8260cfe10/utils/fish/functions/vf.fish

It does store all venvs in a global dir, and provides some commands to manage them.

It address the problem for my workflow, and can be used as a reference to implement a general solution that gets packaged with `uv`.

---

_Comment by @mitsuhiko on 2024-07-29 16:40_

I think it's worth looking at this from the lense of what the world might look like in two years.  A lot of the reasons people want to "activate" virtualenvs is that this is how we used to do things, not necessarily how we would do things in a slightly changed world.

Even with rye today the need to actually activate a virtualenv is largely gone as the python executable always does the right thing, and `rye run` does too.  So it primarily are things created in a world that assumed that virtualenvs are for activating that need this.

npm got away without any equivalent of "activation" and I think a future Python ecosystem will also no longer find much use in virtualenv activation.

---

_Comment by @anddam on 2024-08-06 20:44_

> What about providing a wrapper shell script to do the sourcing part?

I just threw this in my `~/.bashrc` (MSYS2 on win) 

    uv () {
      [ -n "$2" ] && d="$2" || d=".venv"
      [ "$1" = "venv" ] && [ -d "$d" ] \
        && source "./${d}/Scripts/activate" \
        || command uv "$@"
    }

this mimics the same `venv` function I have had for years for creating/listing/activating venvs.

Albeit feeling the need for a quicker way to activate the env while using uv I agree with the sentiment that it's not the called process role to alter the parent's environment.

---

_Comment by @chrisrodrigue on 2024-08-08 21:50_

It's tricky enough that other projects like `pdm` have avoided adding this feature. Too many broken corner cases.

See the green note here: [Looking for `pdm shell`?](https://pdm-project.org/en/latest/usage/venv/#activate-a-virtualenv)

---

_Comment by @thernstig on 2024-09-11 05:27_

> Just a kind reminder of the existence of https://github.com/direnv/direnv. It would be actually nice to learn from it and explore integration with it ?

There is a thread about integration that might go official sometime, see https://github.com/direnv/direnv/issues/1250.

---

_Comment by @pkucmus on 2024-09-12 19:38_

`uv shell` (I do like it in Poetry) or not, I would like to have a `uv venv --activate` or even `uv activate` something that would set the project's Python interpreter for my current shell.

---

_Comment by @tanloong on 2024-09-12 20:19_

And it would be great if `uv activate` could activate the environment from any **subfolder**, not just the root. Here is my function to do the subfolder activation in fish:

```fish
function activate
    set --function cwd (pwd)
    set --function home (dirname (realpath $HOME))
    set --function venv_path ""
    # Recursive search upward for .venv directory
    while test -n $cwd -a $home != $cwd
        if test -e $cwd/.venv
            set --function venv_path (realpath $cwd/.venv)
            break
        end
        set --function cwd (dirname $cwd)
    end
    if test -n $venv_path
        if test -d $venv_path
            source $venv_path/bin/activate.fish
        else
            echo "Found .venv at $venv_path, but it is not a valid directory"
        end
    else
        echo "Could not find .venv directory"
    end
end
```

---

_Comment by @wlnirvana on 2024-09-16 21:00_

Instead of activating the environment, I personally find it helpful instead to

1. either `uv run <script>` to run a script directly
2. or `uv run python` to start the repl backed by the virtual environment

---

_Comment by @seapagan on 2024-09-17 08:38_

I'll be honest, I originally saw the lack of a 'shell' command as a deal-breaker since I am so used to Poetry. When I stepped back for a sec I realise I can get by with a couple of shell aliases (Linux or Mac):

```bash
# uv aliases
alias va='source .venv/bin/activate'
alias vd='deactivate'
```

This is from my `zsh` config but all shells offer similar. Windows users can use a `Doskey` or powershell alias.

Now, I just enter my project folder and type `va` and ready to go, actually faster that the Poetry equivalent.

I've got my GitHub actions working again, and changed to `Renovate` from `Dependabot` so I think I'm ready to go full on `uv` :grin:

EDIT: `direnv` now has a working `uv` script I've just tried and it works perfectly: <https://github.com/direnv/direnv/wiki/Python#uv>

---

_Comment by @luxedo on 2024-09-23 17:57_

(Grabs 🍿)

I like `uv shell` concept because of consistency. No need to run anything else other than `uv` tooling. Newcomers won't even have to worry about what is a `venv`.

Although I like very much using `uv run`, `uv pip`, ... I think that `uv shell` could make it easier for them to migrate to `uv` .


---

_Comment by @MrHamel on 2024-10-05 03:08_

What I do to get around this for now, is use the `python` plugin in `oh-my-zsh` to load/unload the environment based on the directory I'm in, by setting this in my `.zshrc` file:

```shell
plugins=(... python)

export PYTHON_VENV_NAME=".venv"
export PYTHON_AUTO_VRUN=true
```

---

_Comment by @seapagan on 2024-10-05 09:29_

> What I do to get around this for now, is use the `python` plugin in `oh-my-zsh` to load/unload the environment based on the directory I'm in, by setting this in my `.zshrc` file:
> 
> ```shell
> plugins=(... python)
> 
> export PYTHON_VENV_NAME=".venv"
> export PYTHON_AUTO_VRUN=true
> ```

OMG, I really need to RTFM more often, i've been using omz for years :rofl:  - that works just prefectly and utterly transparent :+1: 

Doesn't seem to interfere with folders where i'm using the `direnv` method too.

---

_Comment by @richin13 on 2024-10-09 18:09_

I've had this in my `.zshrc` for ages and never have to worry about activating a virtualenv

```zsh
export DEFAULT_PYTHON_VENV_DIR=.venv

function _py_venv_activation_hook() {
  local ret=$?

  if [ -n "$VIRTUAL_ENV" ]; then
    source $DEFAULT_PYTHON_VENV_DIR/bin/activate 2>/dev/null || deactivate || true
  else
    source $DEFAULT_PYTHON_VENV_DIR/bin/activate 2>/dev/null || true
  fi

  return $ret
}

typeset -g -a precmd_functions
if [[ -z $precmd_functions[(r)_py_venv_activation_hook] ]]; then
  precmd_functions=(_py_venv_activation_hook $precmd_functions);
fi
```

---

_Comment by @ultrabear on 2024-10-21 23:35_

Just wanted to leave my two cents, as I see this is a more complicated issue than I first thought (started learning uv, missed `poetry shell`, yknow the deal)

fish has a flag to run a command before starting interactivity (`-C`) which I will use going forwards, hope that helps anyone who is (like me) too conservative to use a direnv type functionality:
```fish
function uvenv
    fish -C "source .venv/bin/activate.fish"
end
```

---

_Comment by @Ravencentric on 2024-11-07 21:33_

Poetry's `poetry shell` has been brought up several times in this thread but poetry 2.0 has removed `poetry shell` in https://github.com/python-poetry/poetry/pull/9763 and replaced it with a command that simply prints the activate command (similar to pdm's solution).

---

_Comment by @cosmincatalin on 2024-11-09 15:17_

Not sure why they did this, the `shell` command is easy, intuitive and does the expected job.

---

_Comment by @thernstig on 2024-11-09 15:25_

Note that `poetry shell` still exists here https://github.com/python-poetry/poetry-plugin-shell.

---

_Comment by @Ravencentric on 2024-11-09 15:34_

> Not sure why they did this, the `shell` command is easy, intuitive and does the expected job.

The issues with `poetry shell` are extensively discussed in:

https://github.com/python-poetry/poetry/issues/9136
https://github.com/python-poetry/poetry-plugin-shell/issues/18

---

_Comment by @dpprdan on 2024-11-13 21:03_

It's been [mentioned before](https://github.com/astral-sh/uv/issues/1910#issuecomment-2276729062), but seriously, wouldn't an implementation [similar to pdm's ](https://pdm-project.org/en/latest/usage/venv/#activate-a-virtualenv) be a good compromise here? 
It generates a platform independent activation command, allows for a custom `uv shell` command if one prefers, and a `uv venv activate` would fit nicely in the uv pip/venv interface.

---

_Comment by @alberto-gomezcasado-AP on 2024-11-20 13:19_

Writing from windows, I have just tried `uv run cmd.exe` and `uv run powershell` from the project folder root and they seem to do exactly what I would expect. Am I missing something here?

---

_Comment by @anddam on 2024-11-20 16:49_

> Writing from windows, I have just tried `uv run cmd.exe` and `uv run powershell` from the project folder root and they seem to do exactly what I would expect. Am I missing something here?

Well, the request was about activating the environment, thus modifying the shell process the user was already in.  
In your case you are spawning a new process.

---

_Comment by @DonDebonair on 2024-11-23 11:53_

> > Writing from windows, I have just tried `uv run cmd.exe` and `uv run powershell` from the project folder root and they seem to do exactly what I would expect. Am I missing something here?
> 
> Well, the request was about activating the environment, thus modifying the shell process the user was already in. In your case you are spawning a new process.

That's the thing: this issue is about mimicking `poetry shell`, which does in fact spawn a new process. What you're referring to (modifying the shell process the user is already in), is basically activating the venv.

---

_Comment by @anddam on 2024-11-23 21:48_

I see, I am unfamiliar with poetry and was only following OP's "Proposed Solution" bit.

I am personally not a fan of spawning a new shell just in order to have the environment automatically activated, at that point I can setup a quick, unobtrusive way to activate the shell and use that in plave of `uv shell`, say an function called `uva` (uv activate).

---

_Comment by @astrojuanlu on 2024-11-24 12:03_

FYI, you don't need to activate environments anymore. `uv run`, `uv add`, `uv pip` etc pick up the appropriate env based on cwd.

---

_Comment by @bluss on 2024-11-24 13:24_

I think easy virtual environment activation is great to have. IMO uv has not made it obsolete, uv has based all its functionality on virtual environments, so they continue to be relevant.

Virtual environment activation needs shell integration. I understand if `uv` doesn't want to build this, but users need a place to go where they can install good existing solutions for this kind of shell integration. Most(?) experienced python developers already have some solution for this, and they vary: simple shell aliases, shell functions, direnv, etc.

* We need visibility of existing or future solutions - users need a place to go to get these shell integrations, so that not every developer writes their own shell functions

---

_Comment by @Ravencentric on 2024-11-24 13:37_

venvs made by uv have the activation scripts, so `source .venv/bin/activate` (maybe an alias if you find that too verbose). Poetry did the `poetry shell` and had to get rid of it. PDM decided from the get-to that `pdm shell` would be too problematic for how little of benefit it brings. Rye decided against it too. At one point I thought `poetry shell` (or an equivalent) is important but after a while of using just `poetry run` and `uv run`, I've come to enjoy them. `uv run` also has the benefit of guaranteeing a proper up-to-date environment on every run.

---

_Comment by @Ravencentric on 2024-11-24 14:37_

After realising that `$tool shell` is obviously problematic, I did think about proposing the alternative of a command that simply prints that activate command, after all the commands are different on Windows  (`.venv/scripts/activate`) and Linux (`.venv/bin/activate`):
```
$ uv shell
source .venv/bin/activate
$ eval $(uv shell)
```
This sounds nice on paper (cross platform command!) but it's not actually cross platform. On Windows you would need to `iex (uv shell)` while on Linux you would need to `eval $(uv shell)`. So in the end, the benefits are negligible. You save a few characters while still needing to remember some minor differences. I haven't really needed a reason to activate the venv either, after all, `uv run/add/pip/etc` do the right thing and are cross platform while also avoiding the foot gun of "oops I forgot to activate the venv and installed something in my global environment". VS Code and Pycharm also pick the in project venv without any activation. On the rare occasion where I do need to activate the venv `source .venv/bin/activate` works fine.

---

_Comment by @luxedo on 2024-11-24 16:28_

`hatch shell` also spawns a new process and I really like it. https://hatch.pypa.io/1.9/environment/#entering-environments

Also, quitting the `venv` with `Ctrl+D` feels better than using `deactivate` for me.


---

_Comment by @mitsuhiko on 2024-11-25 19:21_

@luxedo that is exactly what everybody else is doing. You don't spawn the shell, you pexpect into a shell with all the complexities and bugs that this causes.

https://github.com/pypa/hatch/blob/6bc4bbeff1be8226958874e43698f5bf3b8ea339/src/hatch/utils/shells.py#L109-L141

There is no way to do this with shells today without having to remote control it and send key presses into it.

I really believe the best one can achieve here is that someone runs `uv shell` and it prints out an informational text with something you can copy paste to source it.  I think the goal should be to just stop activating virtualenvs and do everything to kill all uses cases that currently require virtualenv activation in other ways.

---

_Comment by @chrisrodrigue on 2024-11-26 19:33_

Opinions ahead…

I kind of agree with @mitsuhiko.

virtualenv activation adds even more state to an already stateful shell, in the name of saving a few keystrokes. An activated virtual environment makes things available on the PATH that otherwise shouldn’t be there. The fact that a different activation script is needed for each platform is yet another code smell.

Maybe `uvr` could be the final answer to the virtual environment activation problem?

---

_Renamed from "Feature Request: `uv shell`" to "A command to activate the virtual environment, e.g., `uv shell`" by @zanieb on 2024-11-26 23:46_

---

_Renamed from "A command to activate the virtual environment, e.g., `uv shell`" to "Add a command to activate the virtual environment, e.g., `uv shell`" by @zanieb on 2024-11-27 02:07_

---

_Comment by @sebastian-correa on 2024-11-27 13:31_

The [`uv`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/uv) OhMyZSH plugin has the `uvr` alias for `uv run`.

Disclaimer: I built the plugin.

---

_Comment by @anddam on 2024-11-28 10:35_

> I think the goal should be to just stop activating virtualenvs and do everything to kill all uses cases that currently require virtualenv activation in other ways.

I might be missing something, what's the beef with virtualenv activation here?

My use case for venv usage is you activate one, start exploring what you need and add package interactively along the way, then at a certain point you solidify (i.e. "freeze") the requirements. It's kinda the REPL concept.

---

_Comment by @rgilton on 2024-11-28 13:47_

> I might be missing something, what's the beef with virtualenv activation here?

That was my thought too.  When I went back and read through what @mitsuhiko [said](https://github.com/astral-sh/uv/issues/1910#issuecomment-2256407301), I _think_ the general aim here is to remove the need to "active" a virtualenv.  If all the tooling just does the "right thing" based on the working directory (and, perhaps some other environment variables, for some rare cases, I assume), then there is no longer any need to 'activate'.

> My use case for venv usage is you activate one, start exploring what you need and add package interactively along the way, then at a certain point you solidify (i.e. "freeze") the requirements

Don't `uv pip install` and `uv pip freeze` fulfil your needs here?



---

_Comment by @rumpelsepp on 2024-11-28 15:18_

Fish has a useful flag for this (`-C`:  Evaluate  specified  commands after reading the configuration but before executing command specified by -c or reading interactive input.)

I have a qick 'n' dirty script `uv-shell` which looks like this:

``` bash
#!/usr/bin/bash

set -eu

activation_script=".venv/bin/activate.fish"

if [[ ! -r "$activation_script" ]]; then
    activation_script="$(git rev-parse --show-toplevel)/.venv/bin/activate.fish"
fi

fish -C "source $activation_script"
```

I can imagine, that a potential `uv shell` requires such functionality from the shell to do it without problematic pty code.

---

_Comment by @anddam on 2024-11-28 18:29_

> Don't `uv pip install` and `uv pip freeze` fulfil your needs here?

Likely yes, but my digital (in the finger meaning) habits come from before uv.

---

_Comment by @rgilton on 2024-11-29 09:57_

> I can imagine, that a potential `uv shell` requires such functionality from the shell to do it without problematic pty code.

Agreed -- I don't think there's really _any_ shell that requires such problematic code.  Any shell that supports having an init file you can spec via command-line (or env-var, I guess!) can do similar.

---

_Comment by @krystofbe on 2024-12-22 19:39_

The lack of a uv shell command is currently the only reason preventing me from switching away from Poetry. Having a built-in way to activate the virtual environment is such a convenience in my workflow.

---

_Comment by @glukicov on 2024-12-30 18:40_

Hey @krystofbe and others, don't wait too long to switch to `uv` 🙌  The migration has been great for me: dev groups, container building with dependencies, pulling packages from our private registry all works as expected, and the project build is so much faster. The workaround to activate the `venv` that I use is: 
```python
from subprocess import run

def activate():
    """Activate uv virtual environment 🐍."""
    cmd = f"source .venv/bin/activate"
    run(args=["/bin/bash", "-c", f"{cmd}; exec $SHELL"], check=True)
```
This can be wrapped in a `typer` / `click` CLI command and incorporated with other `uv` commands like `sync` or `uv python pin` to have one single command that installs and activates a Python project. 

---

_Comment by @kaine-bruce-dmt on 2025-01-02 09:03_

As a previous user of pipenv, I thought I would miss `pipenv shell`... But I've just been hitting `.` and then autocompleting `.venv/scripts/activate`. It doesn't slow me down.





---

_Comment by @tringenbach on 2025-01-05 02:55_

I scrolled through the comments and didn't see this suggested yet (but maybe I missed it, or maybe it's just a terrible idea):

uv super environment

- `source` it once in your `.zshrc` / `.bash_profile` / etc
- Replace `python` with a a shell script that sets `VIRTUAL_ENV` and runs the real `python`
  - Or it could be a symlink to `uv` itself, which would check argv[0], set the env var and exec python (if it's faster to implement in Rust than in shell)
- Add a "super environment"/bin directory to PATH, with files for all entrypoints in all uv managed environments
  - they would be shims or symlinks that check the dynamic virtual environment and forward to the right command
  - commands that don't exist in that environment would fail

Your virtualenv would be dynamic based on the CWD.

One down side, unless there's a clever way around this, is that you would get tab completion for commands not installed in the active virtualenv. You'd also want to try to detect if a virtual environment was manually activated and disable "super environment" mode, which might get tricky. 

I could also see it being frowned upon to add a layer in front of python like this.

---

_Comment by @mortenb-buypass on 2025-01-24 16:16_

Another scenario:

Us coming from pyenv with the `uv install python <pythonver> --default --preview` would really like to have this feature. From pyenv we have a standard `~/.bash_profile` that works like this: (scenario is a pod running a not root service that need a dedicated python environment):

```
uv venv 
source ~/.venv/bin/activate
uv pip install -e .

# And I append this file:
cat ~/.bash_profile
export UV_ROOT=~/.local/bin
export PATH="$UV_ROOT:$PATH"
if [ -f ~/.venv/bin/activate ]; then
        source ~/.venv/bin/activate
fi
```
I use this in all my containers. UV has removed more than 200MB from the container compared to pyenv which needs to build from source. We never use `sudo apt-get install python3` because we like to control everything ourselves and patch independently of ubuntu. and once pyenv had a new python ready (usually max 5-10hours after python.org release) Our CICD system had build and tested.

Add this source file based on what shell you use to give you a default python on login.

`uv shell ~/.venv --shell=bash`

---

_Comment by @krystofbe on 2025-01-24 16:20_

Turns out I’ve been overreacting about the lack of uv shell. Poetry removed their poetry shell command in 2.0.0, so I guess manual source .venv/bin/activate is the hot new lifestyle. 

---

_Comment by @Noobzik on 2025-02-13 10:01_

I came from conda and I am transitionning to uv.

I have a project that I needed to share with someone. So I zipped my project, and then I noticed that I forgot the .venv is here. This is something I didn't need to worry when I was in miniconda since the environnements are all located at the same path. I think this is an additional argument to back up this feature request.

Right now, i need to go inside my project, select all needed files to be zipped. Or delete the .venv folder before I go back to one level, file zip a single folder containing my project.

It's an additional step compared to conda since it doesn't store the venv inside the project folder... 

---

_Comment by @alberto-gomezcasado-AP on 2025-02-13 10:42_

> I came from conda and I am transitionning to uv.
> 
> I have a project that I needed to share with someone. So I zipped my project, and then I noticed that I forgot the .venv is here. This is something I didn't need to worry when I was in miniconda since the environnements are all located at the same path. I think this is an additional argument to back up this feature request.
> 
> Right now, i need to go inside my project, select all needed files to be zipped. Or delete the .venv folder before I go back to one level, file zip a single folder containing my project.
> 
> It's an additional step compared to conda since it doesn't store the venv inside the project folder...

FYI: UV stores the venv wherever you tell it to do so, https://docs.astral.sh/uv/pip/environments/#using-arbitrary-python-environments and will search for it, even will try automatically one folder up https://docs.astral.sh/uv/pip/environments/#discovery-of-python-environments.

But I don't see how any of this is relevant to the activation, and the existence of a command to do so, so maybe you are in the wrong issue?

---

_Comment by @elisiariocouto on 2025-02-13 10:45_

@Noobzik there's an issue tracking the problem you're describing: #1495 

---

_Comment by @anddam on 2025-02-14 13:49_

> I have a project that I needed to share with someone. So I zipped my project, and then I noticed that I forgot the .venv is here. This is something I didn't need to worry when I was in miniconda

Or if you use `git-archive` or equivalent, with the additional bonus that you don't carry build products along.

---

_Comment by @detrin on 2025-03-15 23:20_

+1

---

_Comment by @MycrofD on 2025-03-17 06:24_

Referring [this article](https://hynek.me/articles/python-virtualenv-redux/), I use the following in my `.envrc` and my set-up of `uv` and `direnv` works amazingly well.
```
test -d .venv || uv venv --python 3.11
source .venv/bin/activate
```
But I needed to integrate this with my `.env`.
I referred to a [SlackOverflow answer](https://stackoverflow.com/a/30969768) later, and updated my `.envrc` as the following.
```
test -d .venv || uv venv --python 3.11
source .venv/bin/activate
set -o allexport
source .env
set +o allexport
```


---

_Comment by @benbenbang on 2025-03-22 17:56_

As an old `poetry` user, I was also looking for this option.
I eventually wrote my function + completion,
It might be a bit hacky, but before `uv` officially supports it (or never), I'll use this setup.

---

### A complete replica and adjustment of `poetry`
Sharing the configs here:
```bash
# function to create, activate, or deactivate .venv
function uv() {
    if [[ "$1" == "shell" ]]; then
        local venv_path=".venv"
        local project_name=${PWD##*/}  # Extract current directory name

        # Check if .venv directory exists
        if [[ ! -d "$venv_path" ]]; then
            # Create new virtual environment if it doesn't exist
            command uv venv "$venv_path"
        fi

        # Check if .venv directory exists
        if [[ -d "$venv_path" ]]; then
            # Activate existing virtual environment using bash emulation
            echo "Activate existing virtual environment at $venv_path"
            # Print deactivation instructions
            echo "To deactivate, hit ctrl + d"

            # Get Python version
            local py_version=$($venv_path/bin/python -c "import sys; print(f'{sys.version_info.major}.{sys.version_info.minor}')")

            # Export these variables to the new shell
            export VIRTUAL_ENV="$PWD/$venv_path"
            export VIRTUAL_ENV_PROMPT="$project_name-py$py_version"

            # Activate and spawn new shell
            $SHELL -c "emulate bash -c '. $venv_path/bin/activate'; exec $SHELL"

            # After hit ctrl + d, clean up the prompt
            if [[ -n "$VIRTUAL_ENV" ]]; then unset VIRTUAL_ENV; unset VIRTUAL_ENV_PROMPT; fi
        else
            echo "Failed to create virtual environment at $venv_path"
        fi
    else
        # Forward all other commands to the actual uv command
        command uv "$@"
    fi
}
```

```bash
# uv (python package manager)
if type uv &>/dev/null; then
    eval "$(uv generate-shell-completion zsh)"
fi

_uv_shell() {
    # call the original _uv completion function
    _uv "$@"

    if [[ $CURRENT -eq 2 ]]; then
        # Add 'shell' to the completion options
        _values 'command' 'shell[activate or create and activate a virtual environment]'
    fi
}
compdef _uv_shell uv
```

### activate or deactivate

This works fine for me    

<img src="https://github.com/user-attachments/assets/aefca822-e7d5-4117-82bd-92b03c51ac69" width=50%>

(Just testing `uv` not working on `main` 😅)

### completion

> [!NOTE]
> The indentation won't work beautifully. But at least it works fine
> Don't try to use space or tab to fix it. It will just break your completion.

<img src="https://github.com/user-attachments/assets/40cfe790-c83e-4ff4-845e-b2551acfbd47" width=40%>

---

Adjust to your needs. I hope it helps.

---

_Comment by @shapiromatron on 2025-03-26 13:31_

Having a nice solution for this would be incredible. I build python apps for scientists that only dabble in using python. Figuring out how to activate the environment after creation is the hardest part and I get questions every time. Documentation is confusing and problematic because activation is OS-specific. Having this being a solved problem with uv would be incredible, even if it's a hacky solution, it solves a real problem.

Solving this unlocks a whole new class of python users.


---

_Comment by @benbenbang on 2025-03-29 11:43_

> Because activation is OS-specific

Thanks for the inspiration, I have made a more robust way to handle the prompt. 

Sharing the repo: [benbenbang/uv-shell](https://github.com/benbenbang/uv-shell)


---

_Comment by @dan-alvares on 2025-04-05 21:12_

A simple way to achieve such thing you need to setup an alias for the command.
I just added the following line to the .zshrc file

1. nano ~/.zshrc
2. add the following line by the end of the file: **alias uvshell="source .venv/bin/activate"**
3. after saving, apply the changes using the following command: source ~/.zshrc

After that I just need to access my project folder and use the command **uvshell**

---

_Comment by @asadmoosvi on 2025-04-05 21:30_

I am currently using [direnv](https://github.com/direnv/direnv) to automatically activate/deactivate virtualenvs whenever I move in and out of different projects using `cd`. I think it's way more efficient than having to manually activate them each time.

### direnv config for uv
```shell
layout_uv() {
    if [[ -d ".venv" ]]; then
        VIRTUAL_ENV="$(pwd)/.venv"
    fi

    if [[ -z $VIRTUAL_ENV || ! -d $VIRTUAL_ENV ]]; then
        log_status "No uv project exists. Executing \`uv init\` to create one."
        uv init
        uv venv
        VIRTUAL_ENV="$(pwd)/.venv"
    fi

    PATH_add "$VIRTUAL_ENV/bin"
    export UV_ACTIVE=1  # or VENV_ACTIVE=1
    export VIRTUAL_ENV
}
```

---

_Comment by @foundinblank on 2025-04-18 17:02_

Adding to @asadmoosvi's [comment above](https://github.com/astral-sh/uv/issues/1910#issuecomment-2781101207) in case anyone else gets stuck. This assumes you've already installed [direnv](https://github.com/direnv/direnv) and hooked it into your shell. 

1. The **direnv config for uv** code snippet goes into `~/.config/direnv/direnv`
2. In your project folder's `.envrc`:
    ```
    layout_uv .
    ```
3. Run `direnv allow .`

---

_Comment by @schlich on 2025-04-24 21:08_

Just adding my late $.02, but the "venv-less" world being advocated for by @mitsuhiko and others upthread is not really plausible for test-driven development or test-heavy workflows where you may be running "uv run pytest" dozens or hundreds of times per session.  Rye avoided this by providing a `rye test` command, but one can imagine other frequently-invoked commands as well.  It's a bit separate from the question of whether or not uv should provide a built-in "shell" or "activate" command, but i agree with the commenter who said that as long as uv's implementation is tied to virtual environments, it will be necessary to activate them.

---

_Comment by @rgilton on 2025-04-25 08:32_

> workflows where you may be running "uv run pytest" dozens or hundreds of times per session

I'm not sure, but I _think_ the vague idea of the direction I've picked up from this thread is that ultimately you wouldn't need to have the `uv run` bit, and the python executable would just sort it all out.  Maybe I've misunderstood though.

(edit)  But also, I completely agree: I have similar workflows that would involve continuously typing out `uv run`.  So I use the `uvs` script I posted above.

---

_Comment by @JalinWang on 2025-05-15 07:14_

- `uv shell` will spawn a shell with the virtualenv activated and use `exit` to exit the env, e.g., `poetry shell` and `pipenv shell`.
- `uv activate` will activate the local env in current shell and use `deactivate`/`uv deactivate` to exit, e.g., `conda activate` and  `conda deactivate`

---

_Comment by @trevyn on 2025-05-16 05:09_

i propose that in the meantime, `uv shell` (or `uv activate`) exists and just tells me the command to activate

---

_Comment by @mwdiers on 2025-05-23 17:15_

Here is my solution. This is a zsh function, with an alias to activate. It will search from the current directory on up for a .venv environment and activate it. Handy also in that it is not uv-specific:

```
activate_venv() {
  local current_dir=”$PWD”
  local venv_path=”"

  # Loop upwards until we find a .venv with an activate script
  while [[ "$current_dir" != "/" ]]; do
        if [[ -d "$current_dir/.venv" ]]; then
          if [[ -f "$current_dir/.venv/bin/activate" ]]; then
                venv_path="$current_dir/.venv/bin/activate"
                # venv found
                break
          else
                 echo "Found .venv directory at '$current_dir/.venv', but 'bin/activate' script is missing or not a file." >&2
          fi
        fi
        # Move up one directory level
        current_dir=$(dirname “$current_dir”)
  done

  # Check if we found a valid activation script
  if [[ -n "$venv_path" ]]; then
        echo "Activating virtual environment: $venv_path"
        source "$venv_path"
  else
        echo "Error: No .venv directory with a 'bin/activate' script found in the current directory or any parent directories." >&2
        return 1
  fi
}

alias venvup=’activate_venv’
```

---

_Comment by @sdelahaies on 2025-06-15 11:29_

hello, I just build a GUI, [guv](https://github.com/sdelahaies/guv), to launch uv envs via an UI, under the hood the UI runs the command

```bash
gnome-terminal --tab -- bash -c "cd '{env_path}' && exec bash --rcfile ~/.bashrc_uv"
```

where `.bashrc_uv` is a duplicate of my `.bashrc` with `source .venv/bin/activate` added at the end

I think it may be a solution to some of the comments above

---

_Comment by @tetv on 2025-06-16 18:21_

Workaround for .zshrc (or similar) - Auto-activate python .venv environment when entering a folder using `cd`:
```bash
cd() {
  builtin cd "$@" || return
  local VENV_PATH=$(pwd)
  while :; do
    if [ -d "$VENV_PATH/.venv" ]; then
      source "$VENV_PATH/.venv/bin/activate"
      export PATH="$(realpath ~/bin):$(realpath ~/bin/gnu):$(realpath ~/.local/bin):$PATH"
      return
    fi
    [ "$VENV_PATH" = "/" ] && break
    VENV_PATH=$(dirname "$VENV_PATH")  
  done
  [ -n "$VIRTUAL_ENV" ] && deactivate
} 
cd .
```

If you just create a new environment do a `cd .` to activate, example:
```bash
uv init
uv python pin 3.12
uv sync # creates .venv
cd . # auto-activate environment
cd .. # auto-deactivates environment
cd - # auto-activate environment - reenter folder
```

---

_Comment by @maistrotoad on 2025-06-23 06:19_

> Workaround for .zshrc (or similar) - Auto-activate python .venv environment when entering a folder using `cd`:
> 
> cd() {
>   builtin cd "$@" || return
>   local VENV_PATH=$(pwd)
>   while :; do
>     if [ -d "$VENV_PATH/.venv" ]; then
>       source "$VENV_PATH/.venv/bin/activate"
>       export PATH="$(realpath ~/bin):$(realpath ~/bin/gnu):$(realpath ~/.local/bin):$PATH"
>       return
>     fi
>     [ "$VENV_PATH" = "/" ] && break
>     VENV_PATH=$(dirname "$VENV_PATH")  
>   done
>   [ -n "$VIRTUAL_ENV" ] && deactivate
> } 
> cd .
> If you just create a new environment do a `cd .` to activate, example:
> 
> uv init
> uv python pin 3.12
> uv sync # creates .venv
> cd . # auto-activate environment
> cd .. # auto-deactivates environment
> cd - # auto-activate environment - reenter folder

this was exactly what I needed, thanks!

---

_Comment by @yomajo on 2025-07-09 09:01_

For those wanting to mess around interactively in python shell using project's dependencies (virtual env), apparently using interpreter from .venv is sufficient.

`pip list | grep flask` - to showcase, there's no globally installed `flask`

```
$:/tmp/test$ pip list | grep flask
$:/tmp/test$ ls
main.py  pyproject.toml  uv.lock
$:/tmp/test$ grep flask pyproject.toml 
    "flask>=3.1.1",
$:/tmp/test$ .venv/bin/python
Python 3.12.3 (main, Jun 18 2025, 17:59:45) [GCC 13.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import flask
>>> print('working within virtualenv')
working within virtualenv
>>> exit()
```

---

_Comment by @ChaitanyaYeole02 on 2025-07-16 03:51_

@charliermarsh Using cd . or cd .. would still confuse new people. My suggstion was to create a new command
uv .venv activate,

You enter your virtual env which is .venv or whatever the name is in your project.

---

_Comment by @Kilo59 on 2025-07-16 15:38_

The ambivalence about this feature being expressed here is entirely reasonable from the perspective of experienced python developers. However that's not who this feature is primarily for.

`uv` has a number of game changing features that make bootstrapping a working python environment from scratch (including installing python itself) incredibly simple.

A `uv shell` command (or something equivalent) solves the ["last mile"](https://en.wikipedia.org/wiki/Last_mile_(transportation)) problem of python environment setup for many newer or infrequent python developers.

Solutions that involve modifying `.zschrc` profiles are missing the point of this entirely.

---

_Comment by @schlich on 2025-07-16 15:49_

for what it's worth i've been using `pixi shell` for quite a while now and it seems to be pretty seamless.  The only quirk is that running `pixi add` in a separate shell outside of an active one still requires you to run `pixi install` inside that shell, or open a new shell, for the update to take effect.  But that's really a minor enough problem that I don't really understand what all the fuss is about.

I also use nushell as my daily driver, which usually means i can expect a bunch of quirks, but this has been absolutely fine.

*edit* to include the [pixi shell](https://pixi.sh/latest/advanced/pixi_shell/) documentation which outlines their approach and anticipated quirks

---

_Comment by @ChaitanyaYeole02 on 2025-07-18 04:47_

@charliermarsh Can I work on adding this command?
If any other work is not going on this from your side.

I agree with @Kilo59, a `uv shell` or `uv .venv activate` command would help a lot.

---

_Comment by @vlcinsky on 2025-07-18 06:34_

@ChaitanyaYeole02 why do you use the dot in `uv .venv activate`? Do you propose to add new `.venv` subcommand (we already have `uv venv` for virtual env creation.


---

_Comment by @DonDebonair on 2025-07-18 09:36_

> The ambivalence about this feature being expressed here is entirely reasonable from the perspective of experienced python developers. However that's not who this feature is primarily for.
> 
> `uv` has a number of game changing features that make bootstrapping a working python environment from scratch (including installing python itself) incredibly simple.
> 
> A `uv shell` command (or something equivalent) solves the ["last mile"](https://en.wikipedia.org/wiki/Last_mile_(transportation)) problem of python environment setup for many newer or infrequent python developers.
> 
> Solutions that involve modifying `.zschrc` profiles are missing the point of this entirely.

Why not just teach newer developers how to use `uv run`?

I think that last thing we should do, is teach new Python developers bad "black magic" habits like what is being proposed here with `uv shell`. As explained by @mitsuhiko and many others, there are very good reasons why Poetry got rid of `poetry shell` in its original implementation.

As for the person lamenting that `uv run` doesn't work for TDD, I truly don't understand why running `uv run pytest` or something like it many times is a problem. It's not like it's gonna be slower performance-wise than having `uv shell`.

---

_Comment by @yomajo on 2025-07-18 09:59_

Humble addition (~6yr python dev, `venv`, `pip`, `pipenv` toolset reliance before, attempted to use `uv` twice)

`uv run` works well for running [entry] scripts. I would assume most devs advocating for `uv shell` here want to work interactively with python within uv installed dependencies and imports from project scope.

---

_Comment by @kaine-bruce-dmt on 2025-07-18 10:14_

> > The ambivalence about this feature being expressed here is entirely reasonable from the perspective of experienced python developers. However that's not who this feature is primarily for.
> > `uv` has a number of game changing features that make bootstrapping a working python environment from scratch (including installing python itself) incredibly simple.
> > A `uv shell` command (or something equivalent) solves the ["last mile"](https://en.wikipedia.org/wiki/Last_mile_(transportation)) problem of python environment setup for many newer or infrequent python developers.
> > Solutions that involve modifying `.zschrc` profiles are missing the point of this entirely.
> 
> Why not just teach newer developers how to use `uv run`?
> 
> I think that last thing we should do, is teach new Python developers bad "black magic" habits like what is being proposed here with `uv shell`. As explained by [@mitsuhiko](https://github.com/mitsuhiko) and many others, there are very good reasons why Poetry got rid of `poetry shell` in its original implementation.
> 
> As for the person lamenting that `uv run` doesn't work for TDD, I truly don't understand why running `uv run pytest` or something like it many times is a problem. It's not like it's gonna be slower performance-wise than having `uv shell`.

Yeah, this is my take too.  `.venv\Scripts\activate` does what most people want right?

Propose closing the issue with a final statement to guide people.. any Astral people have any input on keeping this open?


---

_Comment by @DonDebonair on 2025-07-18 10:14_

> Humble addition (~6yr python dev, `venv`, `pip`, `pipenv` toolset reliance before, attempted to use `uv` twice)
> 
> `uv run` works well for running [entry] scripts. I would assume most devs advocating for `uv shell` here want to work interactively with python within uv installed dependencies and imports from project scope.

Those people can just use `uv run python` or `uv run jupyter notebook` or something similar.

---

_Comment by @turbotimon on 2025-07-18 14:41_

> Why not just teach newer developers how to use uv run?

1. People sometimes want an IDE shell that doesn't require them to write 'uv run' before every command. It's not that the dont know about `uv run`
2. `uv run` does a lot more than just run something, e.g. automatic locking and syncing. Preventing this makes the command even longer.
3. Why not give people the choice, rather than forcing them into a workflow that may not fit their needs?

The popularity of this issue, as well as the fact that it is available in many similar tools, shows that there is a need for such a command. It's a simple feature that wouldn't hurt anyone. Comments such as "those people can just use..." because they don't need it in their personal workflow seem rather ignorant to me.

Addition: Btw, why does `uv venv` actively recommend in its output `Creating virtual environment at: .venv \n Activate with: source .venv/bin/activate` when "people should just use uv run"? (hypothetical question, I don't need an answer)

---

_Comment by @kaine-bruce-dmt on 2025-07-18 14:54_

@turbotimon Is it a simple feature though?

The inital proposal was straight forward. Alias `.venv\Scripts\activate` to `uv shell` but apparently that isn't that straightforward...

I defer to Armin's experience here: https://github.com/astral-sh/uv/issues/1910#issuecomment-1963507697



---

_Comment by @rgilton on 2025-07-19 06:47_

It's incorrect that one has to do pty allocation to create a shell. It's currently as simple as running `uv run $SHELL`. No ptys involved.

---

_Comment by @DonDebonair on 2025-07-19 14:03_

> > Why not just teach newer developers how to use uv run?
> 
>     1. People sometimes want an IDE shell that doesn't require them to write 'uv run' before every command. It's not that the dont know about `uv run`
> 
>     2. `uv run` does a lot more than just run something, e.g. automatic locking and syncing. Preventing this makes the command even longer.
> 
>     3. Why not give people the choice, rather than forcing them into a workflow that may not fit their needs?
> 
> 
> The popularity of this issue, as well as the fact that it is available in many similar tools, shows that there is a need for such a command. It's a simple feature that wouldn't hurt anyone. Comments such as "those people can just use..." because they don't need it in their personal workflow seem rather ignorant to me.
> 
> Addition: Btw, why does `uv venv` actively recommend in its output `Creating virtual environment at: .venv \n Activate with: source .venv/bin/activate` when "people should just use uv run"? (hypothetical question, I don't need an answer)

As has been argued quite a bit in this issue, it's **not a simple feature**. There are subtleties that make this feature more complex than it seems. There are good reasons why Poetry actually dropped this feature. 

You have to realize that adding a new feature also means adding extra code that needs to be maintained. The many people asking for this feature, will also be the ones who are going to complain when there are bugs in this new feature.

People **do have a choice**. Many examples are given in the comments on how to recreate this feature for specific shells etc. People can use that.

I really wish for the maintainers to make a choice here though. Either cave to the people asking for this, which means building and maintaining an allegedly complex feature, or lock this issue and be done with it.

---

_Comment by @CHC383 on 2025-07-19 17:45_

If we end up with not having this feature, maybe a doc can be added to summarize some workarounds, for example:
- To shorter the command, use alias like `uvr="uv run"`
- To automatically activate the virtual environment, use tools like `direnv`, `mise`.
- To create a new shell with the virtual environment, run `uv run <shell>`
- To help the beginners, explain how to activate the environment in the doc or through a new command (like some of the other tools)

This is not a comprehensive list, and there might be some other good workarounds in the comments above that could go into the doc.

---

_Comment by @chrisrodrigue on 2025-07-20 15:21_

Some uv run commands below. Feel free to alias them for shorthand.

## Linux
```bash
$ uv run sh -c “. .venv/Scripts/activate; sh”
(venv) 
```

## Windows
```dos
> uv run cmd /k .venv\Scripts\activate
(venv) 
```


---

_Comment by @turbotimon on 2025-07-24 14:02_

After thinking about it, the `uv run <shell>` from @CHC383 is actually a really neat solution! Anybody seeing a problem with this (as an alternative for `. .venv/bin/activate` and the windows equivalent)?

Maybe we should recommend this in the docs, e.g. in the projects guide (I have an open PR in which I could include it #13542) and maybe also in the `uv venv` [output](https://github.com/astral-sh/uv/blob/23ed31b94da8ca2ba23866cbfd13f0998e3b04f3/docs/index.md?plain=1#L182)? `uv activate` could still be an alias for that, however, as there is a uv-solution, maybe this could end this discussion and make everybody happy☀️.

(@chrisrodrigue I think it's not necessary to explicit activate the venv because `uv run` does it already)

---

_Comment by @bersbersbers on 2025-07-24 15:43_

> Anybody seeing a problem with this (as an alternative for . .venv/bin/activate and the windows equivalent)?

Yes: `call activate.bat` can be used in a script, followed by other commands. `uv run cmd`, for instance, cannot.

---

_Comment by @turbotimon on 2025-07-24 18:51_

> > Anybody seeing a problem with this (as an alternative for . .venv/bin/activate and the windows equivalent)?
> 
> Yes: `call activate.bat` can be used in a script, followed by other commands. `uv run cmd`, for instance, cannot.

I ment in the context of this issue, which is not about scripts but getting a developer shell (see Proposed Solution). And anyway, you could use `uv run` before the script, e.g. `uv run cmd myscript.bat`. That also would keep your script uv agnostic. Also, it's not about not using `activate` at all if someone prefer it.

---

_Comment by @chrisrodrigue on 2025-07-25 01:05_

> ## Linux
> 
> $ uv run sh -c “. .venv/Scripts/activate; sh”
> (venv) 
> ## Windows
> 
> ```
> > uv run cmd /k .venv\Scripts\activate
> (venv) 
> ```

These keep the activated venv shell alive instead of immediately returning to a normal shell. 

I guess there’s no point to use `uv run` for this until we have uv run aliases in `pyproject.toml`.

---

_Comment by @turbotimon on 2025-07-25 08:35_

> These keep the activated venv shell alive instead of immediately returning to a normal shell.

If the shell executed by `uv run`, it keeps its context and therefore venv alive. You can try it out:

```sh
$ which python
/usr/bin/python
$ echo $VIRTUAL_ENV

$ uv run bash
$ which python
/home/project/.venv/bin/python
$ echo $VIRTUAL_ENV
/home/project/.venv
```

---

_Comment by @jakob1379 on 2025-07-31 11:17_

for people who use `direnv` to automatically setup the environment just add

```bash
# file: .envrc
VIRTUAL_ENV=".venv"
layout python
```
to automatically enable the python venv, this is basically the auto enable `pyenv` uses.

---

_Comment by @KarthikRajashekaran on 2025-08-22 03:04_

Any scope of feature `uv shell` to activate the virtual env just like pipenv shell .. This is super easy feature

---

_Comment by @gsornsen on 2025-08-23 21:38_

Thanks all — I read through the full thread and want to check my understanding before proposing anything concrete. I’m interested in taking this on, but I’d like to validate the pain points and desired behavior first so we can converge on something that’s useful, maintainable, and consistent with uv’s goals.

Working assumptions from this thread
- Main problems users are trying to solve
  - A cross‑platform “one command” to work inside the project environment, without remembering platform/shell specifics.
  - A developer workflow that supports interactive loops (REPL, pytest, linters, ad‑hoc commands) without constantly prefixing uv run.
  - Activating from subdirectories (upward search) and understanding which env uv chose and why.
- Two conflated asks
  1) Spawn a new shell that’s inside the env and exits cleanly with exit/Ctrl‑D (pipenv/poetry/pixi/hatch style).
  2) Activate the env in the current shell (true activation), which requires user cooperation (eval/source/iex) or shell integration.
- Current friction points
  - Activation is OS- and shell-specific; it’s the “last mile” that confuses newcomers.
  - Nested activations and directory changes can be foot‑guns.
  - Users want clear introspection (“which env is active/selected and why?”).
  - Some want hooks or centralized env locations (virtualenvwrapper‑like) and upward discovery.
- Philosophical split
  - “Don’t activate; just use uv run and let uv select the right env.”
  - “Give me a developer shell; typing uv run repeatedly is clunky for interactive workflows.”

Prior art and constraints
- Practical constraint: a child process can’t safely mutate the parent shell’s environment. That rules out “activating your current shell” from uv without user cooperation or shell integration.
- What other tools do
  - pipenv: pipenv shell spawns a subshell.
  - pixi: pixi shell spawns a subshell and is widely reported as workable.
  - poetry: poetry shell existed; Poetry 2.0 removed it; there’s a plugin; modern approach leans toward printing activation commands.
  - pdm: avoids a shell command; prints activation commands.
  - hatch: shells via pexpect/PTY control (works but brittle and high maintenance).
  - conda/mamba: rely on shell integration (activate/deactivate functions).
  - direnv, oh‑my‑zsh plugins, mise: shell‑side solutions many users already use with uv.
- uv today
  - uv run/add/pip select the appropriate env based on CWD and discovery rules (and editors like VS Code/PyCharm often pick .venv automatically).
  - Users can already approximate a developer shell with uv run $SHELL.
  - There’s interest in better introspection/logging of how an env was selected.

Desired behaviors I’m hearing
- A first‑class way to:
  - Spawn a subshell “inside” the project env (clean exit returns you to your original shell unchanged).
  - Activate in the current shell safely via printed commands (eval/source/iex) rather than trying to mutate the parent process.
- Respect uv’s environment discovery (CWD, parent dirs, configured/global env locations).
- Clear handling when no env exists (helpful error by default; optional create).
- Sensible defaults for nested activations (warn/refuse by default), minimal/opt‑in prompt changes, and Windows/fish support.
- Good docs and quick paths for newcomers, plus recipes for direnv/oh‑my‑zsh/mise.

Tentative direction to validate (not a final proposal)
- Two complementary commands plus introspection, avoiding PTY/expect hacks:
  - uv shell: spawn an interactive subshell with the env prepared; exit to leave.
    - If the env is missing: error by default; allow --create.
    - Respect discovery, support fish/pwsh/cmd, warn on nested activations.
  - uv activate: print shell‑appropriate activation snippets to stdout.
    - Users opt‑in via eval/iex/source.
    - If missing: error by default; allow --create.
    - Optional --with-prompt, and a --json mode for tooling.
  - uv env explain: show which env uv selected and why (and provide --json for editors/IDEs).
- Documentation
  - Make the tradeoffs explicit; show uv run $SHELL, uv shell, and uv activate variants for bash/zsh/fish/pwsh/cmd.
  - Link to curated recipes (direnv, oh‑my‑zsh plugin, fish examples), so users don’t reinvent shell functions.
  - Update messaging after uv venv to include “developer shell” and “activate‑in‑place” options alongside source .venv/bin/activate.

Questions for maintainers and contributors
- Scope and defaults
  - Missing env: should uv shell and uv activate error by default and offer --create, or auto‑create by default with a config to opt‑out?
  - Prompt: minimal by default, or only on explicit request? Any strong preferences?
  - Nested activation: warn and refuse by default for uv shell? Allow with a flag?
- UX and naming
  - Would uv activate (top‑level) be acceptable, with uv venv activate as an alias for discoverability?
  - Is uv shell as a “spawned subshell” aligned with uv’s philosophy, or should we only recommend uv run $SHELL in docs?
- Platform specifics
  - Any gotchas you want to avoid for fish/pwsh/cmd specifically?
  - Preference order on Windows (pwsh vs Windows PowerShell vs cmd)?
- Introspection
  - Is a uv env explain (text and JSON) desirable to address “which env and why?” Or should this live behind a --verbose mode of existing commands?
- Out of scope for v1
  - Hooks on activation/creation, centralized env management UX, and shell integration installers — okay to defer and focus on the core primitives?

If this framing sounds right, I’m happy to draft a concise UX/CLI spec (flags, behaviors, examples) and a technical plan that reuses uv’s existing env discovery/creation paths, with explicit notes on testing and platform nuances. I’d really appreciate reactions or corrections on the assumptions above, plus guidance on the defaults and naming so the first draft aligns with project direction.

---

_Comment by @vlcinsky on 2025-08-24 10:22_

@gsornsen great and thorough summary.

## Main motivation
My main motivation was to have simple, unified instruction for python project README.md for activation of python virtual env.

`uv run $SHELL` seem to be missing good MS Windows equivalent.

## explicit or implicit `--create`?
I would lean more towards auto-creation. This is in line with current uv philosophy with `uv run <command>` which simply syncs the environment whenever run.

---

_Comment by @Deshdeepak1 on 2025-08-31 20:35_

I am currently using this.  Seems to work fine on linux. Not sure if it works on windows.

`alias uvsh='eval $(uv venv --allow-existing 2>&1 | tail -n 1 | sed "s/Activate with: //")'`

Activate: `uvsh`

Deactivate: `deactivate`

---

_Comment by @klmr on 2025-09-01 09:45_

@gsornsen Unfortunately **`uv run $SHELL` does not work** (even if we ignore Windows for now), otherwise there would be no impediment to adding a `uv shell` implementation that does just that internally (and this thread would probably already be closed).

But with `uv run $SHELL` the user’s shell configuration is executed *after* the venv configuration, and that often negates its effect wholly or partly (most trivially if the user configuration overwrites `PATH`, and causes another Python to be given precedence over the venv’s). We could say “well that’s on the users, they need to fix their shell configuration” but that would be very unhelpful, and would lead to this repo being swamped with [arguably] spurious bug reports.

And there’s no portable way to solve this. — In particular, of spawning a new shell, automatically executing a command inside it, and then handing control back to the user (this is what pexpect is trying to solve, but that solution has the aforementioned caveats). A hacky workaround is to create a shell script that first sources the default user configuration and afterwards the `.venv`’s (i.e. something like Bash’s `bash --rcfile <(printf '. ~/.bashrc; . .venv/bin/activate\n')`). But this is not portable across shells.

(I assume that many people involved in this discussion thread — in particular those who have previously used pexpect — know all this, but I couldn’t find it spelled out anywhere above, and I am convinced that this led to some of the misunderstandings and back and forth.)

---

_Comment by @rgilton on 2025-09-01 10:06_

> And there’s no portable way to solve this. — In particular of spawning a new shell, automatically executing a command inside it, and then handing control back to the user

pipenv [did this](https://github.com/pypa/pipenv/blob/main/pipenv/shells.py) -- no pexpect required.  Just detect what shell is in use and do the shell-appropriate thing.

---

_Comment by @klmr on 2025-09-01 19:24_

@rgilton As far as I can tell from just reading the source code, it only fully supports a fairly small number of shells and [falls back on pexpect](https://pipenv.pypa.io/en/stable/shell.html#shell-activation-modes) for others (including extremely widely-used ones, e.g. zsh). This might be good enough but it’s not truly portable/cross-shell, and [it’s not perfect](https://pipenv.pypa.io/en/stable/shell.html#troubleshooting-shell-integration).

---

_Comment by @rgilton on 2025-09-03 09:12_

> [@rgilton](https://github.com/rgilton) As far as I can tell from just reading the source code, it only fully supports a fairly small number of shells and [falls back on pexpect](https://pipenv.pypa.io/en/stable/shell.html#shell-activation-modes) for others (including extremely widely-used ones, e.g. zsh). This might be good enough but it’s not truly portable/cross-shell, and [it’s not perfect](https://pipenv.pypa.io/en/stable/shell.html#troubleshooting-shell-integration).

You're quite right -- I hadn't spotted that it falls back to pexpect.

However, I think the argument that this shouldn't be a feature because it can't possibly support _all_ shells, is not reasonable.  Just because something will not work in literally every imaginable situation is not a reason to not have the feature.  The reason to have the feature in this case is that it will make the lives of a significant number of users easier.  There will be situations it doesn't work in, but that's OK, and it also seems like they are solvable anyway.

If the "it must work in all situations or never become a feature" argument were to be applied to other uv subcommands, then many of those would not exist.  For example I could easily feed a script to `uv run` that botches up the python execution environment, but it wouldn't make sense to say that this meant that `uv run` shouldn't exist.

Seems to me that just avoiding the pexpect implementation would suffice, and grow support for the most common shells.

Also, a side-note: it should be pretty easy for uv to examine whether the shell configuration has been botched-up by the user by just running a few commands within in it. 

---

_Comment by @schlich on 2025-09-03 21:14_

> **snip**
> (I assume that many people involved in this discussion thread — in particular those who have previously used pexpect — know all this, but I couldn’t find it spelled out anywhere above, and I am convinced that this led to some of the misunderstandings and back and forth.)

Thank you, I think this is a helpful addition to the thread even though I'm still sorting out the implications.  Can you comment on how this assessment fits with how the `pixi shell` command operates?  Ive found it suitable for most of my needs.

EDIT: it seems like what you are saying tracks with the [documentation for `pixi shell`](https://pixi.sh/latest/advanced/pixi_shell/)

> On Unix systems the shell command works by creating a "fake" PTY session that will start the shell, and then send a string like source /tmp/activation-env-12345.sh to the stdin in order to activate the environment. If you would peek under the hood of the the shell command, then you would see that this is the first thing executed in the new shell session.
> # Issues With Pixi Shell
> As explained, pixi shell only works well if we execute the activation script after launching shell. Certain commands that are run in the ~/.bashrc might swallow the activation command, and the environment won't be activated.
>
> For example, if your ~/.bashrc contains code like the following, pixi shell has little chance to succeed:
>
> ```bash
> # on WSL - the `wsl.exe` somehow takes over `stdin` and prevents `pixi shell` from succeeding
> wsl.exe -d wsl-vpnkit --cd /app service wsl-vpnkit start
> 
> # on macOS or Linux, some users start fish or nushell from their `bashrc`
> # If you wish to start an alternative shell from bash, it's better to do so
> # from `~/.bash_profile` or `~/.profile`
> if [[ $- = *i* ]]; then
>   exec ~/.pixi/bin/fish
> fi
> ```
> In order to fix this, we would advise you to follow the steps below to use pixi shell-hook instead.


So it sounds like they roll with the solution being discussed here and additionally say "if you want to customize your shell init, we also have a command for the config code"

That seems like a reasonable compromise to me.

You also claim this wouldn't be portable across shells, but it seems like pixi does something under the hood to run the right command.  I use Nushell as my daily driver and the `pixi shell-hook` command sets some environment variables (using correct nushell syntax) and finishes with:

```nu
let old_prompt = $env.PROMPT_COMMAND
$env.PROMPT_COMMAND = {|| echo $"\(sample-dir\) (do $old_prompt)"}
```

---

_Comment by @vlcinsky on 2025-09-09 08:33_

I see the a lot of energy put into this thread which goes in repeated cycles from eager engagement of skilled programmer willing to resolve it which later turns into a lesson, other already learnt before that each solution is tricky and could possibly harm `uv`.

For my key requirement: having simple method to instruct user in README.md on how to activate a python environment, I would consider one of:
- find or create dedicated page with proper instructions and link to it. The page would have to be really dedicated just to brief instruction on activation of the environment (deactivation would be fine too there) but without too much text which is not directly focused on env activation / deactivation
- create a gist with given instructions and read it into my README.md (I do not like including obvious things in my README.md as it may become obsolete and spoils brevity of the text)
- create a gist and link to it from my README.md (as in the first point).

I read through uv docs and all pages, which deal with activation, are not good enough as they often cover too much and would be confusing.

Any idea, where such a page already exists? Or shall uv docs add dedicated page https://docs.astral.sh/uv/howto/activate/ ?


---

_Comment by @turbotimon on 2025-09-09 11:08_

@vlcinsky Gread Idea. It's not exactly about activate, but I'm working on chapter "[Working on an existing project](https://github.com/astral-sh/uv/pull/13542)", and it may also would fit in the same context. (Sadly, I haven't received any responses anymore for that PR for a while from the uv team..)

---

_Comment by @vlcinsky on 2025-09-10 07:40_

Nice documentation effort @turbotimon . Without good docs tools become more difficult to adopt and use.

Anyway, my idea was very focused page, which would only describe how to activate python environment. The concept is to keep it as short as possible to prevent distraction and overwhelming with details, which are not essential during reading project README.md.

I think, I will try to draft some gist for this purpose and note it here.

---

_Comment by @mihaitodor on 2026-01-01 22:54_

> I am currently using this. Seems to work fine on linux. Not sure if it works on windows.
> 
> `alias uvsh='eval $(uv venv --allow-existing 2>&1 | tail -n 1 | sed "s/Activate with: //")'`
> 
> Activate: `uvsh`
> 
> Deactivate: `deactivate`

Looks like `uv venv --allow-existing` overwrites the Python symlinks in the venv. I guess this is a bug?

I settled for `alias uvsh='source .venv/bin/activate'`

---
