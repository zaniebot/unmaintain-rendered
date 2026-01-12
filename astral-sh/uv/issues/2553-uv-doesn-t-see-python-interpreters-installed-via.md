```yaml
number: 2553
title: "`uv` doesn't see python interpreters installed via `pyenv`"
type: issue
state: closed
author: theelderbeever
labels:
  - question
assignees: []
created_at: 2024-03-19T22:11:52Z
updated_at: 2024-04-02T02:57:58Z
url: https://github.com/astral-sh/uv/issues/2553
synced_at: 2026-01-12T15:58:38Z
```

# `uv` doesn't see python interpreters installed via `pyenv`

---

_@theelderbeever_

This isn't a huge deal since there is a simple workaround however, `uv` does not recognize python versions that have been installed via `pyenv`. It would be nice if this was seemless and didn't require activating the `pyenv shell` first.


```console
❯ uv --version
uv 0.1.22
```


```console
❯ pyenv versions
  system
  3.8.13
  3.9.5
* 3.9.10 (set by /Users/taylorbeever/.pyenv/version)
  3.9.13
  3.10.2
  3.10.6
  3.10.6/envs/venv
  3.10.10
  3.11.0
  3.11.4
  venv --> /Users/taylorbeever/.pyenv/versions/3.10.6/envs/venv
❯ uv venv -p 3.10.10
  × No Python 3.10.10 in `PATH`. Is Python 3.10.10 installed?
  ```
  
  Workaround
  
 ```console
pyenv shell 3.10.10 && uv venv
```


---

_Comment by @charliermarsh on 2024-03-19 22:16_

I don't think `virtualenv` is able to find these either, at least in my testing. Do you see that too?

---

_Comment by @theelderbeever on 2024-03-19 22:28_

Yeah `virtualenv` requires you to either activate the `pyenv` interpreter first since the default is the environment `virtualenv` is installed in or pass the exact interpreter path via `-p`.

If I remember correctly from a recent podcast you are planning on having `uv` be able to manage python installs as well so maybe it wouldn't be worth it but, maybe something like `UV_PYTHON_MANAGER=pyenv shell` so it knows how to find interpreters from other tools?


--


Also... I can drop this in a separate issue/request but a `uv venv --global/-G` or `uv venv --user/-u` could be interesting as well. It tells `uv` to install the new virtualenv in a `uv` managed directory. Sometimes its nice to not tie a python virtualenv directly to a project since I occasionally just want to hop into an interpreter or jupyter notebook to explore around some odd piece of data.

FWIW I come from a `pyenv` and `poetry` workflow.

---

_Comment by @zanieb on 2024-03-20 00:19_

I believe this will work if you do `pyenv global <version> ...` first? Otherwise these just aren't available in your path.

> maybe something like UV_PYTHON_MANAGER=pyenv shell so it knows how to find interpreters from other tools?

I don't think we'll do something like this, we'll design interpreter management from scratch and probably provide something  to register external interpreters like `rye` and `rustup` have.

---

_Label `question` added by @zanieb on 2024-03-20 00:19_

---

_Comment by @theelderbeever on 2024-03-20 00:41_

@zanieb The only problem with `pyenv global` is that it sets your version everywhere so it affects other tools in other/later shell sessions.

Totally understand and yeah building from scratch will create a better experience down the line.

Not to derail but do you think `uv` will eventually support something like `cargo`'s workspaces? A good solution to that is sorely missing in the python world.

---

_Comment by @zanieb on 2024-03-20 00:50_

You don't need to change your global version, you can still have the first `pyenv global` be your current Python version. `pyenv global` also allows you to add multiple versions to your path, e.g. I expose all of my versions globally

```
❯ pyenv global
3.7.17
3.8
3.9
3.10
3.11
3.12
```

And yeah workspaces are high on our list of priorities.

---

_Comment by @theelderbeever on 2024-03-20 01:18_

Wow... TIL. Thanks for that @zanieb and looking forward to workspaces!

---

_Closed by @theelderbeever on 2024-03-20 01:18_

---

_Comment by @eevmanu on 2024-04-02 02:56_

if anyone is interested, in the most recent version of **uv**

```sh
$ uv --version
uv 0.1.27
```

`PYENV_VERSION` env var works great 👌 (w/o using `pyenv global` neither `pyenv shell ...`)

system python interpreter

```sh
$ uv venv test1
Using Python 3.9.7 interpreter at: /usr/bin/python3
Creating virtualenv at: test1
Activate with: source test1/bin/activate
```

pyenv python interpreter

```sh
$ PYENV_VERSION=3.12.2 uv venv
Using Python 3.12.2 interpreter at: "< $HOME >"/.pyenv/versions/3.12.2/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

---
