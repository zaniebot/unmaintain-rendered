```yaml
number: 14856
title: "Add a command for `uv activate` for cross platform environment activation"
type: issue
state: open
author: kdheepak
labels:
  - enhancement
assignees: []
created_at: 2025-07-23T20:17:37Z
updated_at: 2025-07-24T11:09:50Z
url: https://github.com/astral-sh/uv/issues/14856
synced_at: 2026-01-12T16:01:57Z
```

# Add a command for `uv activate` for cross platform environment activation

---

_@kdheepak_


Thanks for developing and maintaining `uv`! It is becoming an integral part of my workflow!

I do see one source of friction when introducing `uv` to academics and researchers. Currently, activating a virtual environment requires different commands depending on the operating system and shell:

```bash
# Windows - Git Bash
source .venv/Scripts/activate

# Windows - CMD
.venv/Scripts/activate.bat

# Windows - PowerShell
.venv/Scripts/activate.ps1

# macOS / Linux
source .venv/bin/activate
```

I find users coming from a simpler `conda activate env-name` find the default `venv` activation steps not intuitive, since they need to remember platform-specific commands. Beginners unfamiliar with the command line even tend to mix up forward slash and backslash, which sometimes works on Windows but doesn't work on Unix machines.

I also find it is more work writing documentation and tutorials for what users should do. This is particularly true when using `uv run ...` is not possible (running in a different folder than where the environment is located, etc). Previously, I would be able to write cross-platform usage instructions:

```bash
conda activate my-cli-env
cd ~/projects/your-proj
my-cli execute ...
```

Currently, I believe this is the only point of friction in development workflow when moving from `conda` to `uv` for python only environment workflows.

This may also be the partially the impetus behind requests like `uv shell`: https://github.com/astral-sh/uv/issues/1910

Implementing `uv shell` may have technical challenges, however implementing an easier way to activate and deactivate environments would go a long way in addressing this use case. 

[jfcherng](https://github.com/jfcherng) has already posted a helpful function in https://github.com/astral-sh/uv/issues/1910#issuecomment-1963047776 that allows activating an environment in git-bash on Windows and Macos and Linux. It would be great if that functionality existed out of the box. 

I imagine it will require something like:

```bash
eval "$(uv shell-hook zsh)"
```

And then have `uv activate` call the appropriate hook function. 

`uv` already modifies the shell rc scripts during install, and the hook can be added then. `uv` also already supports generating shell completion using a similar eval script, so there is some precedence for this kind for feature.

```bash
eval "$(uv generate-shell-completion zsh)"
```



---

_Label `enhancement` added by @kdheepak on 2025-07-23 20:17_

---

_Comment by @zanieb on 2025-07-23 20:28_

>  This is particularly true when using uv run ... is not possible (running in a different folder than where the environment is located, etc).

You can use `uv run --directory ...` or `uv run --project ...` for that use case. Or `uv run --python <path-to-env>`.

I'm not sure what this is asking for beyond https://github.com/astral-sh/uv/issues/1910? It feels like a duplicate

---

_Comment by @kdheepak on 2025-07-24 02:43_

Thanks for the quick response! And thanks for sharing potential options.

It's still quite verbose to use `uv run --directory ...`. For example, instead of:

```bash
uv run --directory /path/to/venv my-cli setup
uv run --directory /path/to/venv my-cli validate
uv run --directory /path/to/venv my-cli preprocess
uv run --directory /path/to/venv my-cli execute
uv run --directory /path/to/venv my-cli postprocess
```

I would like to do:

```bash
uv activate /path/to/venv # in a manner that works cross platform

my-cli setup
my-cli validate
my-cli preprocess
my-cli execute
my-cli postprocess

uv deactivate
```

fwiw, `uv shell /path/to/venv` would also solve this overall problem for me, i.e. make it easier to run entrypoints without explicitly typing `uv run` every time. I assumed it is going to be more involved to get an implementation working well for `uv shell`, and I assumed this could be a simpler way of addressing the largely the same use case.

---

_Comment by @zanieb on 2025-07-24 04:36_

The challenge of `uv activate` is the same as `uv shell`. I'd recommend reading the discussion there. Unfortunately, there's not a great way for us to do activation without spawning a sub-shell.

---

_Comment by @kdheepak on 2025-07-24 09:14_

I think the difference between this and the other issue is that in this request you _don't_ have to spawn a sub-shell.

Imagine for a moment that `uv shell-hook zsh` existed, and when executed, printed out the following:

```bash
$ echo "$(uv shell-hook zsh)"
uv() {
    if [ "$1" = "activate" ]; then
        # Handle activation manually
        local venv_path="${2:-.venv}"
        local activate_script="${venv_path}/bin/activate"
        if [ -f "$activate_script" ]; then
            . "$activate_script"
        else
            echo "Error: Virtual environment not found at ${venv_path}"
            return 1
        fi
    else
        # For all other commands, run the original uv executable
        command uv "$@"
    fi
}
```

Then if a user added `eval $(uv shell-hook zsh)` to their `.zshrc`, they'd have a zsh function called `uv` that took precedence over the `uv` binary in their PATH. And the user would be able to run `uv activate` using the zsh function and it would do the equivalent of `source .venv/bin/activate`, all without spawning a sub-shell. This is effectively how `conda activate env-name` works.

I could certainly add this function in my own zshrc manually, but it would be nice if this came out of the box with `uv` in a way that worked for `bash`, `zsh`, `fish` and `powershell`. 

---
