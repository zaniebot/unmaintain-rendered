```yaml
number: 6265
title: "Allow creating a `python` shim on `python install`"
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2024-08-20T18:44:36Z
updated_at: 2025-12-28T09:11:55Z
url: https://github.com/astral-sh/uv/issues/6265
synced_at: 2026-01-12T15:59:02Z
```

# Allow creating a `python` shim on `python install`

---

_@zanieb_

When installing Python, I should be able to opt-in to adding a `python` shim to my PATH.

---

_Label `enhancement` added by @zanieb on 2024-08-20 18:44_

---

_Comment by @ophiry on 2024-08-23 09:22_

not sure this was the original intention, but if the shim will work the same as uv run (in terms of environment resolution) it would be very useful 

this will allow simpler integration with IDEs 

(instead of explicitly providing a venv, they could be pointed to the uv shim that will do the environment resolution)

---

_Comment by @tux-type on 2024-08-25 09:35_

This would be very useful, and is also what is missing in uv compared to pyenv.

---

_Comment by @zanieb on 2024-08-26 16:42_

Please just 👍 the original post if you want this. If you have commentary on the implementation or behavior of the shim, that's totally welcome but let's keep the noise down for those subscribing to updates.

> not sure this was the original intention, but if the shim will work the same as uv run (in terms of environment resolution) it would be very useful

We don't think we can do this by _default_ because people have specific expectations about `python` behavior but I think an opt-in mode makes sense.

---

_Comment by @pietrodn on 2024-09-08 11:31_

This would be a great improvement for the entire Python ecosystem user-friendliness. Currently we manage Python projects like this where I work:
1. Install `pyenv`.
2. Install Python versions from source, _compiling_ them (this is a huge pain especially for nontechnical users which get stuck with environment issues and compilation errors).
3. Use Poetry which automatically detects the correct Python version from the constraint in `pyproject.toml`.

Having `uv` install _binary_ version of Python with pyenv-style shims would allow us to entirely drop pyenv and remove the burden of compiling Python from the users... even if we don't switch to uv for package management. 😄 

---

_Comment by @tats-u on 2024-09-23 12:34_

I want to migrate from rye to uv, but the migration is blocked by this issue.
This feature is especially wanted by Windows users because Windows doesn't bundle Python.
If you run `python` command without a global Python, an annoying Microsoft Store window is launched.

---

_Comment by @pietrodn on 2024-09-23 12:57_

I am currently using this workaround to expose uv's Pythons to my shell's `PATH`:

```shell
for dir in $(uv python dir)/*/bin; do PYTHON_PATHS="$dir:$PYTHON_PATHS"; done
export PATH="$PYTHON_PATHS:$PATH"
```

The problem is that there are multiple `python3.10`, `python3.11`, ... executables, only one version for each minor would be exposed. But I typically pick only one version for each Python minor version.

---

_Comment by @vivienm on 2024-09-23 13:42_

As a workaround, I wrote the following script `python3.x`:

```bash
#!/usr/bin/env bash
set -euo pipefail

SCRIPT_NAME="$(basename "${BASH_SOURCE[0]}")"

if [[ "${SCRIPT_NAME}" =~ ^python3\.[0-9]+$ ]]; then
  PYTHON_VERSION="${SCRIPT_NAME#python}"
else
  2>&1 echo "Error: Invalid script name: ${SCRIPT_NAME}"
  exit 1
fi

exec uv run --no-project --python "${PYTHON_VERSION}" --python-preference only-managed --no-config python "$@"
```

And I have symlinks `python3.x` pointing to this script:

```
~/.local/bin/python3.8  -> ~/.local/share/python3.x
~/.local/bin/python3.9  -> ~/.local/share/python3.x
~/.local/bin/python3.10 -> ~/.local/share/python3.x
~/.local/bin/python3.11 -> ~/.local/share/python3.x
~/.local/bin/python3.12 -> ~/.local/share/python3.x
```

It's not great, but it works.

---

_Comment by @chaush-server on 2024-10-31 11:02_

My like was 101, so this feature is in high demand. How can we communicate this to developers?

---

_Comment by @garyj on 2024-10-31 11:08_

Checkout #8458 - the functionality is nearly there


---

_Comment by @zanieb on 2024-10-31 13:30_

We agree this is important, we're working on it. There's also a prototype at #7677.

---

_Comment by @zanieb on 2024-11-07 01:01_

Note that there's a version of this available in preview mode (i.e. `uv python install --preview`) via #8458 and there's ongoing work in #8650 

---

_Comment by @weihenglim on 2024-11-07 02:08_

Are there any plans to add [PEP 514](https://peps.python.org/pep-0514/) support so that the Python executables are discoverable by the Windows `py` launcher?

---

_Comment by @zanieb on 2024-11-07 03:14_

@weihenglim yes there are plans for that

---

_Comment by @feoh on 2024-12-16 19:38_

This is so great!

I have use cases that make needing to type uv run every time I want something to use the uv installed Python problematic.

Putting a 'python' executable in $HOME/.local/bin works perfectly for my use cases. Super appreciate the hard work!

(In particular we use poetry for managing our python runtime environments and I couldn't get that working with uv run but it works great with the python installed using --preview --default).

---

_Comment by @postylem on 2025-02-26 15:36_


> We agree this is important, we're working on it. There's also a prototype at [#7677](https://github.com/astral-sh/uv/pull/7677).

I'm happy to see this is underway!

In the meantime, is there a reason why a simple alias would be a bad idea?
```zsh
echo 'alias python="uv run python"'  >> ~/.zshrc
```
I don't have a system python installation, I'll just use uv to manage it, like I did with `pyenv global` before switching to uv and uninstalling pyenv.

EDIT:

I think what I want would be better solved by doing e.g.
```zsh
uv python install 3.12 --preview --default
```
which installs the executables in my .local/bin directory  
```
~/.local/bin/
├── python -> ~/.local/share/uv/python/...bin/python3.12
├── python3 -> ~/.local/share/uv/python/.../bin/python3.12
└── python3.12 -> ~/.local/share/uv/python/.../bin/python3.12
```



---

_Comment by @zanieb on 2025-02-26 16:09_

Yeah aliases are tricky because they require a shell.


---

_Comment by @thanksshu on 2025-02-28 08:29_

> not sure this was the original intention, but if the shim will work the same as uv run (in terms of environment resolution) it would be very useful
> 
> this will allow simpler integration with IDEs
> 
> (instead of explicitly providing a venv, they could be pointed to the uv shim that will do the environment resolution)

I'm not a heavy user, but shims like rye as mentioned above may indeed benefit me. Here's my use case: I have some projects using pyo3 with rust-analyser in vscode. Now using uv, I have to try to reset the global python or start virtualenv manually every time I switch projects.

---

_Comment by @auxym on 2025-02-28 15:25_

I'd certainly like this feature! Would it be possible to install packages in this python install?

I often use interactive python sessions, or one-off scripts, where spinning up a new throw-away venv is a bit annoying. It's convenient to have a "general purpose" python install with a few packages for use cases like this. If it ever gets broken for whatever reason you throw it away and start a new one.

I assume it might not be possible since it wouldn't be linked to a venv, but I wanted to throw this use case out there.

---

_Comment by @Winand on 2025-02-28 16:02_

> I often use interactive python sessions, or one-off scripts, where spinning up a new throw-away venv is a bit annoying. It's convenient to have a "general purpose" python install with a few packages for use cases like this.

@auxym have you tried something like `uv tool run --with pandas ptpython` ? it creates a temporary venv with specified packages installed and start an interactive session in ptpython. Or you just don't want venv to be created each time?

---

_Comment by @zanieb on 2025-02-28 16:22_

👍 `uvx --with ... python` is the recommendation for that

---

_Comment by @clement-tourriere on 2025-03-03 09:48_

I was also a huge fan of the automatic shims with `rye` and being able to perform only a `python` in a project was great.
You can achieve almost the same with something like [mise](https://mise.jdx.dev/) and with the following `mise.toml` file in your project:

```
[env]
_.python.venv = ".venv" # relative to this file's directory
```

For global tools, like others said, I go with `uvx --with ...` 

---

_Comment by @auxym on 2025-03-03 12:38_

> 👍 `uvx --with ... python` is the recommendation for that

To be perfectly clear, it's perfectly fine if uv doesn't support my use case.  I understand it might not really fit with the philosophy.

But just for further explanation, `uvx --with` wouldn't really work for me. First, if I need, say, `matplotlib pandas numpy jupyter ipympl nptdms`, it's a bit of a hassle to type all that. Second, if I start an interactive ipython session or jupyter, then halfway through realize that I forgot `scipy`, I have to kill everything and start it back up with scipy. In a normal venv, I can just uv add or pip install scipy then import it without having to restart any process.

I'll probably just keep doing what I'm doing, which is a venv in .local which is added to my path.

But typing all this out... I might consider adding a shell alias that expands to `uvx --with ...` for all the packages I commonly use.

---

_Comment by @xM8WVqaG on 2025-03-24 21:41_

Is the intention to also install a `pip` shim as well? The current preview only creates a symlink for `python`.

```console
$ uv --version
uv 0.6.9
$ uv python install 3.12 --default --preview                                
Installed Python 3.12.9 in 6ms
 + cpython-3.12.9-linux-x86_64-gnu (python, python3, python3.12)
$ ls -1 ~/.local/bin | grep -E (pip|python)
python
python3
python3.12
```

I appreciate the intended workflow for projects is to use `venvs` where possible, but when using `uv` to install a "global" Python with no system Python it can be handy to be able to install things like `yt-dlp` or personal Python programs and have them always be just readily available in interactive shells and scripts.

---

_Comment by @mbeijen on 2025-03-24 21:47_

Therefore you can use `uv tool install yt-dlp`

---

_Comment by @fduxiao on 2025-05-06 20:38_

> Is the intention to also install a `pip` shim as well? The current preview only creates a symlink for `python`.
> 
> $ uv --version
> uv 0.6.9
> $ uv python install 3.12 --default --preview                                
> Installed Python 3.12.9 in 6ms
>  + cpython-3.12.9-linux-x86_64-gnu (python, python3, python3.12)
> $ ls -1 ~/.local/bin | grep -E (pip|python)
> python
> python3
> python3.12
> I appreciate the intended workflow for projects is to use `venvs` where possible, but when using `uv` to install a "global" Python with no system Python it can be handy to be able to install things like `yt-dlp` or personal Python programs and have them always be just readily available in interactive shells and scripts.

My case is that besides those python projects, I also have a lot of build scripts, crawlers, automation scripts scattering everywhere. They are all very simple and can be written in one file, and they all share the same dependencies like `numpy` or `requests`. Before the 3.11 version of python, I could just install dependencies using `pip`. After that, the system-wide python packages are managed by system package managers like `apt` or `brew`. So, if a dependency is not shipped with `apt` or `brew`, I have to turn to a virtual environment.

Because they belongs to non-python projects, e.g. build scripts, I don't want to put them in some `uv` project. They all share the same dependencies, so I don't want to prepare a virtual environment for each of them due to the unnecessary usage of disk spaces. (The size of `numpy` is around 50M, which is even larger than the data I want to manipulate for most of the cases.)

My solution is to make a `global` virtual environment by activating it at `.zshrc`. This method is certainly not good because when I activate another virtual environment and deactivate it, I will also deactivate the global one, and `uv` also complains about that if I forget to deactivate the global environment.

Previously, I was using `pyenv` to create `python shims`, with `pip` working as before. So, I can manage `system-wide` python packages by `pip`. But I also had to install `poetry` or `pipx` for project management  and python-shipped tools. Besides, `pyenv` sticks to compiling from source when installing a `python`, which I think is unnecessary. (This is why I also don't want to compile and install python manually from source.)

In a word, there does exist the need to maintain a `global` or `system-wide` python environment (e.g., `conda` is something like this). I really hope you can think of adding `pip` to shim and it is then the user's responsibility to resolve correct versions or create virtual environments.

The case for `cargo` or `npm` is different, because the outcome of such a project is something other than source code. For example, `cargo` makes an executable binary/shared-object that doesn't rely on the dependencies of source code. Thus, we don't need a `global` environment for them, which is not the case for `python` as we run a python script (source code) mostly.

---

_Comment by @leejoramo-d51 on 2025-05-06 22:29_

@fduxiao 

> My case is that besides those python projects, I also have a lot of build scripts, crawlers, automation scripts scattering everywhere.

This is major use case for me too. Using python for utilities, shell scripts and other CLI tools. 

---

_Comment by @auxym on 2025-05-07 21:47_

@fduxiao 

That's similar to the use case I describe in my previous comments, I believe. What currently do, and works well for me, is a "default" venv similar to what you describe, managed by uv, in my `$HOME/.local` dir. However, I don't activate it with the normal activate scripts, I just add its `bin` dir to my `PATH` in my shell init file. Packages in that venv are easily managed with `uv pip ...`. You can activate other venvs that override your "default" as you normally would with a "system" python install.

---

_Comment by @Andrej730 on 2025-05-23 18:18_

Is there a way to undo `--preview` or `--default` without completely reinstalling Python or just removing them from `.local/bin` should do and `uv` doesn't use preview and default python instances anywhere else?

---

_Comment by @TurtleOrangina on 2025-12-28 09:10_

> Because they belongs to non-python projects, e.g. build scripts, I don't want to put them in some `uv` project. They all share the same dependencies, so I don't want to prepare a virtual environment for each of them due to the unnecessary usage of disk spaces. (The size of `numpy` is around 50M, which is even larger than the data I want to manipulate for most of the cases.)
> 
> My solution is to make a `global` virtual environment by activating it at `.zshrc`. This method is certainly not good because when I activate another virtual environment and deactivate it, I will also deactivate the global one, and `uv` also complains about that if I forget to deactivate the global environment.

I think there is a better way to solve this.

- Use uv for all your python scripts. You can use the "inline syntax" for dependency definitions to avoid creating a separate folder and a separate pyproject.toml file for each script: E.g. with these commands: `uv init --script example.py` and `uv add --script example.py numpy` and `uv run example.py`
- When you run such a script uv will create a temporary virtual environment that is immediately removed after the script execution ends - uv is so fast that it can do that without slowing down noticably. It uses caching aggressively to make this work, and disk space utilization is minimal.
- Even when you use the "full blown" approach with a separate folder and a `pyproject.toml` file, the .venv directories use hard linking on Unix operating systems so the used disk storage will also not multiply if you have the same dependencies in multiple places, i think.

---
