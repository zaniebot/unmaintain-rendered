```yaml
number: 13417
title: Uv command to show the path of the current enviroment
type: issue
state: closed
author: infused-kim
labels:
  - enhancement
assignees: []
created_at: 2025-05-12T21:32:11Z
updated_at: 2025-12-04T14:59:35Z
url: https://github.com/astral-sh/uv/issues/13417
synced_at: 2026-01-12T16:01:28Z
```

# Uv command to show the path of the current enviroment

---

_@infused-kim_

### Summary

Hi, 

first of all, thank you so much for creating such an incredible piece of software.

I noticed that uv is very good at finding the right venv. I can execute `uv run xxx`  in sub-directories and it will correctly find the right venv.

Would it be possible to expose that path as a command or flag?

My use-case is that I would like to create a fish shell plugin that auto-activates and de-activates the venv on cd.

I am able to achieve it with: `env -u VIRTUAL_ENV uv run python -c "import sys; print(sys.prefix)"`

But it has the downside that it has the overhead of executing python and since I am using `uv run`, it sometimes syncs the project and creates a delay.

It would be better if I could use something like `uv venv --print-path` that only discovers the venv and prints it.

Of course, there are already several fish plugins that attempt to auto-activate the venv, but they all have different logic of how to find the venv dir. Some only look at the directory that was cded into, some traverse the path, some look for the git repo and check the root, etc.

But many don't consider all edge cases or are inconsistent to how uv discovers the venv.

It would be great to be able to use the same logic as uv.

Thank you for your consideration.

### Example

_No response_

---

_Label `enhancement` added by @infused-kim on 2025-05-12 21:32_

---

_Comment by @zanieb on 2025-05-12 22:17_

I think this would be covered by a JSON output format for `uv python find`. You _could_ infer it by just going up two directories and testing for the existence of a `pyvenv.cfg` for now?

---

_Comment by @zanieb on 2025-05-12 22:19_

e.g.

```
❯ test -f $(dirname $(dirname $(uv python find)))/pyvenv.cfg
❯ echo $?
1
❯ uv venv
Using CPython 3.13.3
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ test -f $(dirname $(dirname $(uv python find)))/pyvenv.cfg
❯ echo $?
0
```

---

_Comment by @infused-kim on 2025-05-13 02:48_

Thank you so much @zanieb. `uv python find` works perfectly and is much faster than my `uv run` method.

I'll test it for a few days and will then publish a fish plugin, but for now in case anyone wants the same...

```fish
# Activate the venv when PWD changes
function _uv_auto_env_event_on_cd --on-variable PWD -d "uv-auto-env function to activate the venv when PWD changes"
    _uv_auto_env_activate_with_checks
end

# Activate the venv when the shell loads
function _uv_auto_env_event_on_prompt --on-event fish_prompt -d "uv-auto-env function to activate the venv when the shell loads"
    if not set -q _uv_auto_env_initialized
        set -g _uv_auto_env_initialized
        _uv_auto_env_activate_with_checks
    end
end

function _uv_auto_env_activate_with_checks -d "uv-auto-env function to activate the venv if the shell is interactive and uv is installed"
    if status --is-interactive && type -q uv
        uv_auto_env_activate
    end
end

function uv_auto_env_activate -d "uv-auto-env function to activate the venv"
    if not type -q uv
        echo "ERROR:Could not activate uv .venv because the uv command was not found"
        return
    end

    # Use uv itself to find the venv path because it correctly handles
    # sub-directories, etc.
    set -l venv_path (dirname (dirname (env -u VIRTUAL_ENV uv python find)))
    if test $status -ne 0
        echo "ERROR: Could not find Python executable using uv"
        return 1
    end

    # if venv ends with .venv, activate it
    if string match -q "*/.venv" $venv_path
        if test "$VIRTUAL_ENV" != "$venv_path"
            # echo "Activating $venv_path"
            source $venv_path/bin/activate.fish
        end
    # If we cd into a dir without a venv and a venv is currently active,
    # deactivate it
    else if test -n "$VIRTUAL_ENV"
        # echo "Deactivating $VIRTUAL_ENV"
        deactivate
    end
end
```

---

_Closed by @infused-kim on 2025-05-13 02:48_

---

_Comment by @lucaspar on 2025-12-04 14:59_

Is there an updated (and more reliable) way to do this? This method wouldn't work when using a system/managed interpreter.


---
