---
number: 8432
title: "Python file autocomplete with `uv run` on zsh with ohmyzsh"
type: issue
state: closed
author: thethomp
labels:
  - bug
  - help wanted
  - cli
assignees: []
created_at: 2024-10-21T22:46:33Z
updated_at: 2025-12-28T10:24:22Z
url: https://github.com/astral-sh/uv/issues/8432
synced_at: 2026-01-07T13:12:17-06:00
---

# Python file autocomplete with `uv run` on zsh with ohmyzsh

---

_Issue opened by @thethomp on 2024-10-21 22:46_

Howdy - this is likely just a question and not a bug, but after googling and ChatGPT-ing with no luck for quite a while, I figured I'd post here. 

I have a standard install of ohmyzsh on my macbook and have installed `uv` and am using it a lot and love it.

Interestingly though, I am finding when I use `uv run file_i_want_to_run.py`, the Python file I want to run won't autocomplete with tab the way it normally does when running things just plainly with `python file_i_want_to_run.py`.

I've enabled all the autocomplete settings that are in the docs, but those seem to only apply to internal uv/uvx commands. Is this the intended behavior or am I missing a setting somewhere perhaps? Or is this a ohmyzsh issue?

Appreciate any input or feedback!

On an Apple M3 Max. Mac OS Sonoma 14.7.
`uv 0.4.9 (77d278f68 2024-09-10)`
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @zanieb on 2024-10-21 22:54_

I think we might need to setup something like https://docs.rs/clap_complete/latest/clap_complete/engine/struct.PathCompleter.html 

---

_Label `bug` added by @zanieb on 2024-10-21 22:55_

---

_Label `help wanted` added by @zanieb on 2024-10-21 22:55_

---

_Label `cli` added by @zanieb on 2024-10-21 22:55_

---

_Comment by @charliermarsh on 2024-10-22 01:46_

For what it's worth, a brief attempt to add `add = ArgValueCompleter::new(PathCompleter::file())` to `Cmd(Vec<OsString>)` had no effect.

---

_Comment by @zanieb on 2024-10-22 02:37_

Interesting, you turned on the unstable feature and reinstalled the completions?

---

_Comment by @charliermarsh on 2024-10-22 02:40_

Yeah. I had to add `clap-complete` as a dependency too. I may well have messed something up.

---

_Comment by @zanieb on 2024-10-22 02:46_

Oh, I just presumed that's what we were already using for completions. I guess we're using [`clap-complete-command`](https://github.com/nihaals/clap-complete-command) though.

---

_Comment by @charliermarsh on 2024-10-22 02:47_

I think that maybe just wraps completions in a command or something.

---

_Comment by @zanieb on 2024-10-22 02:49_

Yeah that calls through to `clap_complete`. Interesting... needs some more investigation I guess.

---

_Comment by @Ruhrozz on 2024-10-23 07:22_

Same problem with `uv pip install -e packa<tab>` and only for zsh. Bash has no problems with it.

---

_Comment by @jakehemmerle on 2024-10-24 15:30_

adding extra context, the issue is still present with the --script or -s arguments;
ie `uv run -s openai_bas` + TAB does not complete to `uv run -s openai_basics.py` as expected.
the argument itself completes fine, eg `uv run --sc` + TAB completes to `uv run --script `

---

_Comment by @simonwiles on 2024-10-28 22:05_

Just for avoidance of confusion, this issue doesn't seem to be specific to Oh My Zsh -- I get the same behaviour on ZSH and I've never used Oh My Zsh.

---

_Comment by @udiNur on 2024-10-31 12:39_

for what its worth, has patch i've just added into .zsh (or whatever you use), working as a charm
```
function py() {
    uv run $1
}
```

im use to `py` either way when im running py files.

---

_Referenced in [astral-sh/uv#7401](../../astral-sh/uv/issues/7401.md) on 2024-11-03 15:07_

---

_Comment by @shirayu on 2024-11-03 16:54_

I'm using this workaround in vanilla zsh. (I'm not a user of ohmyzsh)

```zsh
# License: CC0

eval "$(uv generate-shell-completion zsh)"

_uv_run_mod() {
    if [[ "$words[2]" == "run" && "$words[CURRENT]" != -* ]]; then
        _arguments '*:filename:_files'
    else
        _uv "$@"
    fi
}
compdef _uv_run_mod uv

```

---

_Referenced in [astral-sh/uv#9116](../../astral-sh/uv/issues/9116.md) on 2024-11-14 11:55_

---

_Comment by @emilioziniades on 2024-11-20 16:16_

Just wanted to chime in here, as I also noticed the lack of tab completion for `uv run` and spent thirty minutes investigating.

TLDR: with the way `uv run` is setup currently, it's not possible to do tab completion.

This is because `uv run` doesn't only accept filepaths, although that's the most common case. It actually accepts _any_ command. So the below works:
```
uv run echo "hello world"
```

Given that uv is using `clap_complete` for autocompletions, I reckon the correct [`ValueHint`](https://docs.rs/clap/latest/clap/enum.ValueHint.html) is probably `ValueHint::CommandWithArguments`. BUT there is currently no completion implemented for a `ValueHint::CommandWithArguments`: https://github.com/clap-rs/clap/blob/10c29ab75dfc15ae8ae7218d699de5a2b57afabe/clap_complete/src/engine/complete.rs#L349-L357

I think, but I stand to be corrected, that there is no good way to do tab completion for an arbitrary shell command.

So, in order for `uv run` to tab-complete a filename, you would have to limit the subcommand to `uv run [filename]`. Then it's pretty trivial to setup completions. Clearly this would be a breaking change. 

---

_Comment by @mattharrison on 2024-12-12 01:49_

Commenting to follow. (I'm activating bash right now to get completions)

---

_Comment by @TurtleOrangina on 2024-12-13 18:30_

Ideally `uv run` would auto-complete all commands from `.venv/bin` as well. Typically I do things like `uv run ruff check` or `uv run mypy .` or `uv run my_entrypoint` - where my_entrypoint is an entry from [project.scripts].

Always using `uv run` instead of activating the environment is a great habit for avoiding errors with inconsistent python versions or dependencies, but the handicap of having no auto-complete on these commands is the main down-side of doing it consistently.

---

_Comment by @shrektan on 2024-12-22 10:16_

> for what its worth, has patch i've just added into .zsh (or whatever you use), working as a charm
> 
> ```
> function py() {
>     uv run $1
> }
> ```
> 
> im use to `py` either way when im running py files.

I tweaked this into the below and pasted into `~/.zshrc`. It works perfect (support arbitrage arguments).

```bash
function uvrun() {
    uv run "$@"
}
```

---

_Comment by @TurtleOrangina on 2024-12-22 20:26_

> I tweaked this into the below and pasted into `~/.zshrc`. It works perfect (support arbitrage arguments).
> 
> function uvrun() {
>     uv run "$@"
> }

Sadly this does not work with binaries from `.venv/bin` that are currently not the $PATH yet. It works for auto-completing binaries that are already in $PATH, and directories and files.


---

_Comment by @phaabe on 2025-01-02 14:32_

same issue on [ghostty](https://ghostty.org/). 

Also: using bash it seems to me that it is autocompleting paths only. 
I cannot tab-autocomplete for `uv run python` .

I use: uv 0.5.8 on PopOS 22.04.

---

_Comment by @Galadirith on 2025-01-21 16:34_

Building on @shirayu great answer https://github.com/astral-sh/uv/issues/8432#issuecomment-2453494736, here is a snippet that I currently use in my vanilla zsh that also enables completion of binaries under `.venv/bin`. I think not perfect but I hope useful for anyone searching for a solution

```zsh
eval "$(uv generate-shell-completion zsh)"

_uv_run_mod() {
    if [[ "$words[2]" == "run" && "$words[CURRENT]" != -* ]]; then
        local venv_binaries
        if [[ -d .venv/bin ]]; then
            venv_binaries=( ${(@f)"$(_call_program files ls -1 .venv/bin 2>/dev/null)"} )
        fi
        
        _alternative \
            'files:filename:_files' \
            "binaries:venv binary:(($venv_binaries))"
    else
        _uv "$@"
    fi
}
compdef _uv_run_mod uv
```

---

_Comment by @PatrickAlphaC on 2025-01-25 17:45_

I'd be interested in sponsoring a small bounty on this if there is interest. This would also give me an excuse to donate to `uv` in some way.

---

_Comment by @baughmann on 2025-02-01 00:53_

In case it wasn't clear to everyone, [@vitawasalreadytaken](https://github.com/vitawasalreadytaken) has a [workaround](https://github.com/vitawasalreadytaken/dotfiles/commit/05b0baf1aec7867b2767a6184aee5ac5af480431) that you can plop into your `.zshrc`. This gives me `uv run` directory autocomplete using ohmyzsh

```sh
# uv
eval "$(uv generate-shell-completion zsh)" # you should already have these two lines
eval "$(uvx --generate-shell-completion zsh)"

# you will need to add the lines below
# https://github.com/astral-sh/uv/issues/8432#issuecomment-2453494736
_uv_run_mod() {
    if [[ "$words[2]" == "run" && "$words[CURRENT]" != -* ]]; then
        _arguments '*:filename:_files'
    else
        _uv "$@"
    fi
}
compdef _uv_run_mod uv
```

---

_Comment by @vitawasalreadytaken on 2025-02-01 07:52_

> In case it wasn't clear to everyone, [@vitawasalreadytaken](https://github.com/vitawasalreadytaken) has a [workaround](https://github.com/vitawasalreadytaken/dotfiles/commit/05b0baf1aec7867b2767a6184aee5ac5af480431) that you can plop into your `.zshrc`. This gives me `uv run` directory autocomplete using ohmyzsh

@baughmann  I literally just copied @shirayu's code posted in this thread [above](https://github.com/astral-sh/uv/issues/8432#issuecomment-2453494736) – all credit goes to them 🙂

---

_Comment by @phaabe on 2025-02-01 10:29_

The fix through .zshrc does not work for me. 

❓ Anyone else?

```
~ zsh
(eval):4345: command not found: compdef
(eval):131: command not found: compdef
```

Fixed this error by adding
```
autoload -Uz compinit
compinit
```

❌ still no autocomplete for files.

---

_Comment by @gandalfsaxe on 2025-02-13 11:10_

If you have the problem of `command not found: compdef`, add this whole thing to the `.zshrc`:

```
# Ensure OMZ loads before any manual `compinit` calls (for the fix for uv run autocompletion right below)
export ZSH="$HOME/.oh-my-zsh"
source $ZSH/oh-my-zsh.sh  # OMZ initializes compinit here

# Make `uv` work with autocomplete just like `python` - see https://github.com/astral-sh/uv/issues/8432
eval "$(uv generate-shell-completion zsh)"

_uv_run_mod() {
    if [[ "$words[2]" == "run" && "$words[CURRENT]" != -* ]]; then
        local venv_binaries
        if [[ -d .venv/bin ]]; then
            venv_binaries=( ${(@f)"$(_call_program files ls -1 .venv/bin 2>/dev/null)"} )
        fi

        _alternative \
            'files:filename:_files' \
            "binaries:venv binary:(($venv_binaries))"
    else
        _uv "$@"
    fi
}
compdef _uv_run_mod uv
```

---

_Comment by @nkitsaini on 2025-02-21 06:42_

I think the simplest way to fix this is to include pre-built completion files within uv binary similar to how [ripgrep does it](https://github.com/BurntSushi/ripgrep/blob/e2362d4d5185d02fa857bf381e7bd52e66fafc73/crates/core/flags/complete/rg.zsh#L7).

Another solution is to use dynamic completion by `clap_complete` but it is lacking a few things, most notably it can't do partial completions. So `s<tab>` will complete to `src/ ` (with space) instead of `src/`. So this will only partially solve the issue for zsh and make it worse for other shells. More details about this in the summary below.



<details>
<summary> <bold>Debugging summary</bold> </summary>
<br>

**Dynamic Completion by Clap**

> For what it's worth, a brief attempt to add add = ArgValueCompleter::new(PathCompleter::file()) to Cmd(Vec<OsString>) had no effect.

It seems there were two issues:
1. `ArgValueCompleter` doesn't apply to `external_subcommands`. Instead we need to use [SubcommandCandidates](https://docs.rs/clap_complete/latest/clap_complete/engine/struct.SubcommandCandidates.html).
2. since the above relies on dynamic completion we need a different source file which is generated by [CompleteEnv](https://docs.rs/clap_complete/latest/clap_complete/env/index.html).

After these changes, it does work to some extent but there are lot of gotchas since this is a completely different way of completion. Currently all the completion logic lives in the completion files generated by `uv generate-shell-completion`, but with dynamic-completion all the logic lives in uv itself and completion file works as a small wrapper which calls into uv. 

In little experience that I have had with `clap_complete` I don't think dynamic completions are ready for uv's usage yet. One can't complain as `clap_complete` documentation is pretty clear that this is unstable feature 😃. 

For example it does not support partial completions i.e. `s<tab>` will result in `src/ ` instead of `src/` (without space). So to complete `src/main.py` you have press `s<tab><backspace><tab>`.

I also came across a bug in `PathCompleter` where it doesn't respect the filter provided. So if you only want to complete files not folders using `PathCompleter::file()` it will still complete both. This one is easy fix though.

Similarly there might be more issues which I have not come across yet.


**Why is this an issue only on zsh**

This is mostly anecdotal, but in zsh you need to explicitly specify that you want files to included in completions, but in other shells (bash, fish) it is assumed that if no completions are present files will be used instead. So in fish it is opt-out but in zsh it is opt-in. A simple way to see this effect is to try `git diff <tab>` in zsh, it will not complete the files. But fish and bash will include the files.


</details>


---

_Comment by @detrin on 2025-03-01 23:02_

+1

I would appreciate this to be solved in future. Due to this fact, I just use `python script.py` instead.

---

_Comment by @nkitsaini on 2025-03-03 07:05_

I will look into implementing this. I am planning to go with bundling the zsh completion file at build time. Please let me know if anyone has a better solution.

---

_Comment by @hoechenberger on 2025-03-03 07:08_

How about creating a zsh plugin? Wouldn't that, in theory, simplify things?

---

_Comment by @nkitsaini on 2025-03-03 08:54_

I have not worked with zsh plugins, so please correct me if I am wrong. But if I understand correctly they can only help with registering the completion file, which is currently working as expected.

We need to make sure that zsh completion file that gets registered (either via plugin or a simpler `source`) specifies that `uv run ...` should also include filenames in completion.

Currently this file is generated by `clap-complete-command` at runtime, instead if we generate this file before-hand (with the help of `clap-complete-command`), we can fully control it with modifications on top.

---

_Comment by @hoechenberger on 2025-03-03 09:33_

> But if I understand correctly they can only help with registering the completion file, which is currently working as expected.

To my knowledge, plugins can do more than that, see e.g. https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/autopep8/_autopep8

However I'm not an expert on zsh plugins either; just wanted to point out that this **might** be a possible approach that was overlooked so far.

---

_Comment by @potiuk on 2025-03-21 22:36_

I also would love to +1 the idea of having generic auto-complete work with `uv run`.

I **love** the simplicity of workflows that `uv` came up with. @charliermarsh - > we know each other in person, we talked a lot,  I listened to your podcast https://www.youtube.com/watch?v=hGFb4mMMmkE - and I wholeheartedly agree with your "let's redefine the workflow for python devs out there". And Astral with uv mostly NAIL IT.  Even today after finally merging the final step of refactoring for Airflow I proposed to swich to `uv` as the ONLY supported tooling. https://lists.apache.org/thread/88yj3qxqdmc4ony7k8nvp292m28df31c


But... Autocomplete for `uv run` is BADLY missing, 

Currently, after activating the `.venv`, you could run (for airflow) `pytest airflow-core/tests/unit/a<TAB>` and get the test files auto-complete for you,. Which comes outside of `uv`.

But - I would **love** to be able to tell my users:

```bash
git clone <airflow repo>
cd airflow
uv sync
uv run pytest airflow-core/tests/unit/<TAB>
```

Even if it will require the `uv sync` and `uv run` will not get it out-of-the-box, that's fine. I think it would just be great to see the same auto-complete they would usually see after running their usual command with `uv run`.

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2025-03-26 04:07_

---

_Referenced in [astral-sh/uv#12509](../../astral-sh/uv/issues/12509.md) on 2025-03-27 13:58_

---

_Comment by @mattrossman on 2025-03-28 16:02_

Was just bit by this as a new `uv` user, though I don't use ohmyzsh, just zsh.

Weirdly in VSCode (which auto-activates the `.venv`), the completions work after I `deactivate` the virtual environment, and they continue to work if I `source .venv/bin/activate`. They just don't work with the default auto-activated venv, nor in a fresh iTerm 2 session which doesn't auto activate `.venv`.

---

_Referenced in [astral-sh/uv#12630](../../astral-sh/uv/issues/12630.md) on 2025-04-02 16:52_

---

_Comment by @xtream1101 on 2025-04-03 22:41_


> Weirdly in VSCode (which auto-activates the `.venv`), the completions work after I `deactivate` the virtual environment, and they continue to work if I `source .venv/bin/activate`. They just don't work with the default auto-activated venv, nor in a fresh iTerm 2 session which doesn't auto activate `.venv`.

I am seeing this exact pattern as well. At least I know how to have it work as expected in vscode when I need.

---

_Comment by @jerpint on 2025-05-02 15:47_

has anyone figured out a workaround for this in zsh? i also would love to be able to autocomplete filenames after `uv run python <TAB>/<TAB>/<TAB>`

---

_Comment by @pievalentin on 2025-05-03 17:26_

I would give 5$ to whoever fix this lol 

---

_Comment by @aidanmelen on 2025-05-09 03:30_

> I would give 5$ to whoever fix this lol

To enable shell autocompletion for uv commands, run one of the following:

```echo 'eval "$(uv generate-shell-completion zsh)"' >> ~/.zshrc```

To enable shell autocompletion for uvx, run one of the following:

```echo 'eval "$(uvx --generate-shell-completion zsh)"' >> ~/.zshrc```

Then restart the shell or source the shell config file.

source: https://docs.astral.sh/uv/getting-started/installation/#shell-autocompletion

cheers!

---

_Comment by @AMMullan on 2025-05-09 12:17_

@aidanmelen the problem people are pointing to isn't that shell completion doesn't work, it's that if you try to run **uv run [tab]** it doesn't autocomplete a file name. Sadly, today at least, you're not winning the $5 😛 

---

_Assigned to @Gankra by @zanieb on 2025-05-09 14:52_

---

_Comment by @marovira on 2025-05-09 17:07_

@pievalentin I've managed to "fix" this by adding the following code to my `zhsrc`:
```zsh
    # Fix completions for uv run.
    _uv_run_mod() {
        if [[ "$words[2]" == "run" && "$words[CURRENT]" != -* ]]; then
            _arguments '*:filename:_files'
        else
            _uv "$@"
        fi
    }
    compdef _uv_run_mod uv
```

I've been driving this for a bit over a month now on several platforms and has been working without issue.
I think this was mentioned somewhere in this thread, but I figured I'd mention it again.

---

_Comment by @tdt-amartin on 2025-05-13 01:29_

~~I don't know how many of you have tried this or if it's just a fluke on my end, but I do run Oh-My-Zsh, and I found that autocomplete works just fine if you use this:~~

`echo 'eval "$(uv --generate-shell-completion zsh)"' >> ~/.zshrc`

~~The docs are missing the leading --  for the generate command, and when I added those the completion works. Maybe it's a typo? Maybe it's Maybeline. I don't know, but I was looking for what was wrong, found this thread, and noticed that small change worked for me.~~

This was totally mistaken and should be ignored.

---

_Comment by @markonen on 2025-05-13 06:01_

@tdt-amartin that made no difference here on my plain vanilla zsh install.

---

_Comment by @AMMullan on 2025-05-14 13:15_

@marovira that's a brilliant workaround!!! You can even replace your _arguments with this to only return files with *.py (if you wanted):

```bash
_arguments '*:filename:_files -g "*.py"'```

---

_Comment by @poneding on 2025-05-28 06:08_

> [@pievalentin](https://github.com/pievalentin) I've managed to "fix" this by adding the following code to my `zhsrc`:我已设法通过向我的 `zhsrc` 添加以下代码来“修复”此问题：
> 
>     # Fix completions for uv run.
>     _uv_run_mod() {
>         if [[ "$words[2]" == "run" && "$words[CURRENT]" != -* ]]; then
>             _arguments '*:filename:_files'
>         else
>             _uv "$@"
>         fi
>     }
>     compdef _uv_run_mod uv
> I've been driving this for a bit over a month now on several platforms and has been working without issue.我已经在多个平台上运行它一个多月了，并且运行正常。 I think this was mentioned somewhere in this thread, but I figured I'd mention it again.我认为这个帖子中某处提到过这一点，但我想我会再次提及它。

You're my superman

---

_Comment by @DanielYang59 on 2025-06-05 12:40_

In my case (zsh, MacOS 15.5, no `oh my zsh`). I have `autoload -Uz compinit && compinit` in my `.zshrc` to allow autocomplete for `git`, and this seems to interfere with autocomplete `uv run`...

---

_Comment by @yoniLavi on 2025-06-12 08:47_

Just combining @marovira's solution with @AMMullan's filtering for `.py` files (to save future visitors 10 seconds doing so):

```shell
# Fix completions for uv run to autocomplete .py files
_uv_run_mod() {
    if [[ "$words[2]" == "run" && "$words[CURRENT]" != -* ]]; then
        _arguments '*:filename:_files -g "*.py"'
    else
        _uv "$@"
    fi
}
compdef _uv_run_mod uv
```

When added to .zshrc, the above works perfectly for me.
Thank you both!

---

_Comment by @Jazzinghen on 2025-06-18 08:19_

I wonder if this is an issue only on MacOS. I just encountered the same on MacOS, however on my Linux machine I never encountered it. Could this be a hint?

---

_Comment by @ICHx on 2025-06-19 22:37_

> Just combining [@marovira](https://github.com/marovira)'s solution with [@AMMullan](https://github.com/AMMullan)'s filtering for `.py` files (to save future visitors 10 seconds doing so):
> 
> # Fix completions for uv run to autocomplete .py files
> _uv_run_mod() {
>     if [[ "$words[2]" == "run" && "$words[CURRENT]" != -* ]]; then
>         _arguments '*:filename:_files -g "*.py"'
>     else
>         _uv "$@"
>     fi
> }
> compdef _uv_run_mod uv
> 
> When added to .zshrc, the above works perfectly for me. Thank you both!

This really helped.

I am on wsl omz zsh, and installed uv via homebrew

---

_Comment by @romanovzky on 2025-06-27 10:40_

> _uv_run_mod() {
>     if [[ "$words[2]" == "run" && "$words[CURRENT]" != -* ]]; then
>         _arguments '*:filename:_files -g "*.py"'
>     else
>         _uv "$@"
>     fi
> }
> compdef _uv_run_mod uv

This makes omz and uv work together as intended, thanks!

---

_Comment by @gergesh on 2025-07-15 08:17_

Bumping this issue with another nicety if anyone's interested:
```zsh
_uv_run_mod() {
    if [[ "$words[2]" == "run" && "$words[CURRENT]" != -* ]]; then
        # Check if any previous argument after 'run' ends with .py
        if [[ ${words[3,$((CURRENT-1))]} =~ ".*\.py" ]]; then
            # Already have a .py file, complete any files
            _arguments '*:filename:_files'
        else
            # No .py file yet, complete only .py files
            _arguments '*:filename:_files -g "*.py"'
        fi
    else
        _uv "$@"
    fi
}
compdef _uv_run_mod uv
```

Completes a Python file and then allows for arbitrary completion for the rest of the arguments.

---

_Comment by @itopaloglu83 on 2025-07-20 18:03_

Wow, uv is great but not being able to auto complete the `uv run <filename>` is quite the paper cut.

~I'm also surprised that the typo in the documentation is still not fixed either.~


---

_Comment by @charliermarsh on 2025-07-20 18:05_

I don't think there's a typo? `uv generate-shell-completion zsh` works just fine. The `--generate-shell-completion` variant is just an alias for backwards compatibility. They're exactly equivalent.

---

_Comment by @tdt-amartin on 2025-07-21 23:20_

@charliermarsh yeah, I guess you're write. I take it back. My bad. 

---

_Comment by @tarpas on 2025-07-23 10:15_

uvx on zsh/MacOS also breaks (the default) autocomplete

---

_Comment by @andrewaylett on 2025-08-15 22:21_

`jj` uses dynamic autocomplete -- so the entire fish completion script is:

```
complete -k --exclusive jj -a '(COMPLETE=fish jj -- (commandline --current-process --tokenize --cut-at-cursor) (commandline --current-token))'
```

This means jj can generate completions appropriate for the current operation and status, so if I type `jj log -r <tab>` then it'll give me a list of all current revisions, like so:

```
> COMPLETE=fish jj -- jj log -r ""
main    Update pre-commit hook renovatebot/pre-commit-hooks to v41.74.2 (#853)
push-xylttmtspktr       A smidge of tidying of remark stuff
<snip lots of other revisions>
```

It would be _really nice_ if `uv` could do something similar, enabling features along the lines of auto-completing `uv run <tab>` giving you a selection of the different commands available in the project, or even `uv run poe <tab>` giving you the completions you'd get from activating the venv and completing `poe <tab>`.

---

_Comment by @zanieb on 2025-08-15 22:36_

We'd love dynamic completions, but I'd recommend opening a new issue instead of posting here — this issue is about how zsh completions are broken.



---

_Referenced in [astral-sh/uv#15325](../../astral-sh/uv/issues/15325.md) on 2025-08-16 14:59_

---

_Unassigned @Gankra by @konstin on 2025-08-21 07:55_

---

_Comment by @adrianlzt on 2025-09-05 06:20_

Another workaround to autocomplete *.py files and commands (I use a lot "uv run ansible").
The command autocomplete is with commands already in the `PATH`, so it is dumb, and won't autocomplete commands installed by `uv`.

```zsh
eval "$(patch -i /path/uv-run-cmd-py-file.patch -o - =(uv --generate-shell-completion zsh) 2> /dev/null)"
```

[uv-run-cmd-py-file.patch](https://github.com/user-attachments/files/22166265/uv-run-cmd-py-file.patch)


---

_Referenced in [astral-sh/uv#17042](../../astral-sh/uv/issues/17042.md) on 2025-12-09 13:28_

---

_Assigned to @EliteTK by @EliteTK on 2025-12-10 00:22_

---

_Comment by @EliteTK on 2025-12-10 09:14_

Found a working solution. Setting `value_hint` in the right places does appear to work. Will push something today/this week.

---

_Comment by @EliteTK on 2025-12-10 16:19_

Did some more digging and noticed that it has actually been "working" since `uv` 0.8.4 because [`clap-complete` v4.5.53 fixed this](https://github.com/clap-rs/clap/commit/e2aa2f07d1cd50412de51b51a7cc897e80e0b92f) in the case we're using it in.

Both the `uv run` and `uv tool run` completions now complete with [`_default`](https://zsh.sourceforge.io/Doc/Release/Completion-System.html#index-_005fdefault).

`uv tool run` probably shouldn't be trying to complete anything local. That could be fixed.

For `uv run` we could change it to something more specific like [`_files`](https://zsh.sourceforge.io/Doc/Release/Completion-System.html#index-_005ffiles)[^1] to stop directories showing up. To complete things in the virtual environment bin directory we would need dynamic completions.

[^1]: There is also [`_cmdambivalent`](https://zsh.sourceforge.io/Doc/Release/Completion-System.html#index-_005fcmdambivalent). But it's probably not appropriate here. It would prioritise completing things in `$PATH`, then relative/absolute paths to executable files, and finally paths to any file. The `$PATH` wouldn't contain the `.venv/bin` unless you were in the virtual environment at which point, you probably aren't using `uv run`. To trigger local file completion you need to start a completion with an explicit `./`. Due to how this completion works the engine would refuse to complete anything non-executable if the current prefix matches an executable or directory which isn't the best UX.

---

_Closed by @EliteTK on 2025-12-10 17:23_

---

_Comment by @thedch on 2025-12-24 20:47_

> it has actually been "working" since uv 0.8.4

Unfortunately auto completions are still not working for me:

```
❯ uv --version
uv 0.9.18 (Homebrew 2025-12-16)

❯ echo $0
-zsh

❯ uv run ./ # pressing tab here does nothing

❯ ls ./ # pressing tab allows me to cycle through files in current directory
```

---

_Comment by @EliteTK on 2025-12-25 22:31_

Hey @thedch, thanks for the report, but I can't seem to replicate this:

```
$ $SHELL --version
zsh 5.9 (x86_64-apple-darwin23.0)
$ autoload -Uz compinit
$ compinit
$ typeset -f _uv | head
_uv () {
	# undefined
	builtin autoload -XUz
}
$ typeset -f _uv|grep -o 'external_command:\w\+'
$ source <(uv generate-shell-completion zsh)
$ typeset -f _uv|grep -o 'external_command:\w\+'
external_command:_default
external_command:_default
external_command:_default
$ mkdir test; cd test; uv init
Initialized project `test`
$ uv run <TAB>
README.md       main.py         pyproject.toml
```

You probably already have the completions loaded so you'll get a different output from the first typeset commands. It may be the case that you have the old completions loaded and this may fix them. In either case, let me know what output you do get.

---

_Comment by @TurtleOrangina on 2025-12-28 08:06_

For me, auto-completing local files (e.g. uv run foo/bar/script.py) works thanks to the "generate-shell-completion zsh" in my .zshrc. But, auto-completing scripts that I defined in my pyproject.toml does not. E.g. `uv run enable-warp-drive` doesn't auto-complete (but works, if enable-warp-drive is a script entry point, that exists in `.venv/bin`).

With pyproject.toml something like:

```
### snip
[project.scripts]
enable-warp-drive = 'spaceship.cli.enable_warp_drive:run'
### snip
```

It would be great if handling this was incorporated into what `uv generate-shell-completion` generates. For now, I work-around it by having this additionally in my .zshrc:

```
_uv_run_mod() {
    if [[ "$words[2]" == "run" && "$words[CURRENT]" != -* ]]; then
        local venv_binaries
        if [[ -d .venv/bin ]]; then
            venv_binaries=( ${(@f)"$(_call_program files ls -1 .venv/bin 2>/dev/null)"} )
        fi

        _alternative \
            'files:filename:_files' \
            "binaries:venv binary:(($venv_binaries))"
    else
        _uv "$@"
    fi
}
compdef _uv_run_mod uv
```

Note that this work-around is not ideal, since it will not auto-complete when the virtual environment is not yet set up.

I will put this in a new issue post instead, as this thread is a bit all-over-the-place (and closed).


---

_Referenced in [astral-sh/uv#17246](../../astral-sh/uv/issues/17246.md) on 2025-12-28 08:37_

---

_Comment by @EliteTK on 2025-12-28 10:24_

@TurtleOrangina This issue was just for path completion being unintentionally broken. Getting dynamic completions working is a bit more involved and is tracked here: #15325.

---
