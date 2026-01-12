```yaml
number: 11161
title: "Can't add package (pyqpbo)"
type: issue
state: closed
author: noamgot
labels:
  - error messages
assignees: []
created_at: 2025-02-02T09:39:24Z
updated_at: 2025-02-02T13:47:00Z
url: https://github.com/astral-sh/uv/issues/11161
synced_at: 2026-01-12T16:00:30Z
```

# Can't add package (pyqpbo)

---

_@noamgot_

### Summary

I tried adding the [pyqpbo](https://github.com/pystruct/pyqpbo) package to a fresh uv-initialized project
```
uv init --python 3.10
uv python pin 3.10
```

I got this error:
```
❯ uv add pyqpbo
  × Failed to build `pyqpbo==0.1.2`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File "$HOME/.cache/uv/builds-v0/.tmpOpYTfz/lib/python3.10/site-packages/setuptools/build_meta.py", line 334, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
        File "$HOME/.cache/uv/builds-v0/.tmpOpYTfz/lib/python3.10/site-packages/setuptools/build_meta.py", line 304, in _get_build_requires
          self.run_setup()
        File "$HOME/.cache/uv/builds-v0/.tmpOpYTfz/lib/python3.10/site-packages/setuptools/build_meta.py", line 522, in run_setup
          super().run_setup(setup_script=setup_script)
        File "$HOME/.cache/uv/builds-v0/.tmpOpYTfz/lib/python3.10/site-packages/setuptools/build_meta.py", line 320, in run_setup
          exec(code, locals())
        File "<string>", line 5, in <module>
      ModuleNotFoundError: No module named 'numpy'

      hint: This usually indicates a problem with the package or the build environment.
  help: `pyqpbo` (v0.1.2) was included because `pyqpbo-test` (v0.1.0) depends on `pyqpbo`
```

I know this is a very old package, but I must use it in my project. 

For reference, when I create a clean venv I can `pip install pyqpbo`  and use it:
```
python3.10 -m venv .venv
source .venv/bin/activate
pip install pyqpbo # works!
pip install numpy==1.25 # we must install it to make the next line work
python -c "from pyqpbo import alpha_expansion_general_graph"  # works!
```

Notes:
* I installed numpy in the venv example just to make the import work. pyqpbo's installation works without it. It doesn't work in the uv case either with or without numpy installed.
* The full context is that I tried to install it within a [pixi](https://pixi.sh) (conda) environment, but since they use uv as their pypi installation backend, I checked it in a fresh uv project and got the same error I got with pixi.

If you have a solution for this - I'd like to hear about it. thanks!

### Platform

Ubuntu 22.04, Linux 5.15.0-88-generic x86_64 GNU/Linux

### Version

uv 0.5.25

### Python version

3.10.12

---

_Label `bug` added by @noamgot on 2025-02-02 09:39_

---

_Comment by @konstin on 2025-02-02 12:07_

This is another instance of https://github.com/astral-sh/uv/issues/2252, `pyqpbo` fails to declare its build dependency on numpy. You can work around this by installing numpy manually (as you did) and then deactivating build isolation for the package (https://docs.astral.sh/uv/concepts/projects/config/#build-isolation).

We should add a better hint here for `ModuleNotFoundError: No module named '.*'`

---

_Label `bug` removed by @konstin on 2025-02-02 12:07_

---

_Label `error messages` added by @konstin on 2025-02-02 12:07_

---

_Comment by @noamgot on 2025-02-02 13:46_

thanks @konstin , looks like it's good enough for me (and works in pixi as well :) )


---

_Closed by @noamgot on 2025-02-02 13:46_

---
