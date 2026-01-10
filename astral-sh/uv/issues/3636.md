```yaml
number: 3636
title: Add support for config_settings in requirements.txt / requirements.in
type: issue
state: open
author: tiptenbrink
labels:
  - enhancement
  - compatibility
  - great writeup
  - needs-design
assignees: []
created_at: 2024-05-17T11:41:15Z
updated_at: 2025-06-17T04:47:11Z
url: https://github.com/astral-sh/uv/issues/3636
synced_at: 2026-01-10T03:32:44Z
```

# Add support for config_settings in requirements.txt / requirements.in

---

_Issue opened by @tiptenbrink on 2024-05-17 11:41_

In #1460, the CLI gained support for passing config_settings to the build backend. However, sometimes you want to pass these settings only for specific requirements (or simply you don't want to have to write it in the CLI each time). 

If you use `pip install -r requirements.txt`, this is a valid line inside a requirements.txt:

```
--config-settings editable_mode=strict -e ../lib 
```

However, `uv` will fail with:

```
error: Unexpected '-', expected '-c', '-e', '-r' or the start of a requirement at requirements.txt:3:1
```

Now, I can still get things to work if I pass the `--config-settings editable_mode=strict` to the CLI, but it's quite tedious to always have to do that. Furthermore, maybe I'd not want to pass it for a different requirement, but now it applies to all of them.

Note that the same also doesn't work with `uv pip compile` (when putting the `--config-settings editable_mode=strict` inside a requirements.in) and also not with `uv pip sync`. 

It would be awesome if `uv` could parse the `--config-settings` inside a requirements.txt file.

### Why is this useful?

If you use an editable install of a package built with setuptools, you need to pass either `editable_mode=compat` or `editable_mode=strict` in order for pyright/pylance typing to work for the package (see [this StackOverflow question](https://stackoverflow.com/questions/76213501/python-packages-imported-in-editable-mode-cant-be-resolved-by-pylance-in-vscode)). Supporting this means that it becomes a lot simpler to depend on other local packages without requiring them to be fully factored out as PyPI or Git repository packages (all you need is a very basic pyproject.toml with setuptools as the build backend).

My example setup looks like this:

```
lib/
├─ lib.py
├─ pyproject.toml
app/
├─ requirements.in
├─ app.py
```

Also, thanks for this great project! Using it has been a blast so far!

---

_Comment by @charliermarsh on 2024-05-17 13:59_

Interesting, I've never seen this -- thanks for raising. Looks like it's https://github.com/pypa/pip/issues/11325 in pip.

---

_Label `enhancement` added by @charliermarsh on 2024-05-17 13:59_

---

_Label `compatibility` added by @charliermarsh on 2024-05-17 13:59_

---

_Label `great writeup` added by @zanieb on 2024-05-17 15:09_

---

_Comment by @zanieb on 2024-05-17 15:10_

Interesting. I wonder if there should be a way to express this in the CLI as well.

---

_Comment by @tiptenbrink on 2024-05-17 19:59_

Small note: If anyone is running into my specific use case: if you use hatchling instead of setuptools as your build backend, pyright will work by default. 

---

_Comment by @Jorricks on 2024-06-20 10:26_

Also faced with this issue unfortunately; https://github.com/microsoft/LightGBM/pull/6493
TLDR; LightGBM throws errors on build arguments it doesn't understand. Hence it fails to `uv sync` when I have both editable installations (requiring `editable_mode=strict`) and `LightGBM`.

Arguably, `LightGBM` should ignore the arguments. However, there is also a case to be made that we should not pass the editable build args to each individual item.

---

_Comment by @zanieb on 2024-06-20 13:27_

Considering some options here...

- `--config-setting <package-name>__<key>=<val>` e.g. `--config-setting foo__editable_mode=strict`
- `--config-setting <package>=<key=val>` e.g. `--config-setting foo=editable_mode=strict`
- `--config-setting-includes <package>` only pass config settings to the given packages
- `--config-setting-excludes <package` pass config settings to all packages but the given packages
- `--config-setting-includes <package>=<key>` only pass the given config setting to the given packages
- `--config-setting-excludes <package>=<key>` do not pass the given config setting to the given packages

---

_Comment by @ion-elgreco on 2024-07-16 14:54_

Any chance this will get some attention soon? We have quite some editable installs which we now install one by one

---

_Label `needs-design` added by @zanieb on 2024-07-16 14:56_

---

_Comment by @zanieb on 2024-07-16 14:56_

We need to settle on a design — we're prioritizing some other things right now but if someone wants to push the design forward I can review.

---

_Comment by @notatallshaw on 2024-09-20 03:41_

I'm honestly +0 on this issue, but I thought it worth mentioning I hit this with using `uv pip sync`, at work we have some internal packages that are built with setuptools, and developed in a multi repo environment, to get IDEs to play nice I have to use `--config-settings editable_mode=compat` to install them. 

Because I can't insert that straight into the requirements file I have to do this slightly awkard to of `uv pip sync` followed by `uv pip install {my_editable_projects} --config-settings editable_mode=compat`.

Maybe I'm missing something obvious though.

---

_Comment by @s-rog on 2025-02-12 19:33_

Can package config-settings be specified in toml files?
Or is this currently a cli exclusive feature?

---

_Comment by @harshil21 on 2025-02-15 08:18_

My use case is that I have 2 similar libraries in my [dependency-group], both take the exact same arguments via `config_settings` to the build backend, but I need to only give the arguments to one of the libraries, otherwise it fails to install the other one when I use `uv sync --all-groups`.

Best way to initially support this would be directly in the .toml, so I don't have to do a workaround like this:
```
[dependency-groups]
# Run `uv pip install rpi-libcamera -C setup-args="-Drepository=https://github.com/raspberrypi/libcamera.git" -C setup-args="-Dversion=v0.4.0+53-29156679"`
# if you are installing this and you have a version mismatch of rpi-libcamera and libcamera installed via apt.
camera = [
    "picamera2>=0.3.25",
    "rpi-libcamera>=0.1a8;",
    "rpi-kms>=0.1a1",
]
```
 

---

_Comment by @raffaem on 2025-05-01 11:33_

Also hit that: https://github.com/HandsOnLLM/Hands-On-Large-Language-Models/issues/62

---

_Comment by @ds-steventondeur on 2025-05-21 13:38_

I've also been hitting the issue. My use case is a namespace package consisting of several subpackages (each one a wheel). In our dev environment we 'self-install' all subpackages in editable mode via `requirements.xt`. In order to play nice with IDEs (VSCode, PyCharm) we need `--config-settings editable_mode=compat`

I.e. something like

```
mypkg/
   + src/
       + mypkg-core/
               + setup.py
       + mypkg-tools/
               + setup.py
       + ...
       (no setup.py / pyproject.toml on the upper level)
```

and `requirements.txt` containing
```
...
-e ./src/mypkg-core  --config-settings editable_mode=compat
-e ./src/mypkg-tools  --config-settings editable_mode=compat
...
```

I'm working in a risk-averse production environment, where we set up our venv using a self-developed minimal tool that basically creates a venv and then runs `pip install -r requirements.txt`. 
We're planning to transition towards `uv` but must do so via a 'vanilla pip'-compatible route, such that each project may decide to use `uv` or not. This means that - in practice - I have to find a way to run `uv pip` on a requirements file that contains config-settings. If that wouldn't have been the case I would've simply used a `uv.toml` file containing
```
config-settings = { editable_mode = "compat" }
```

but alas...

---

_Comment by @ds-steventondeur on 2025-05-22 07:19_

> Can package config-settings be specified in toml files? Or is this currently a cli exclusive feature?

@s-rog Yes this can be done for `uv`. I was able to do this via a `uv.toml` file containing at the top level:
```
# Compatible mode so IDEs pick up editable installs correctly.
config-settings = { editable_mode = "compat" }
```

This can also be done via `pyproject.toml`, see https://docs.astral.sh/uv/reference/settings/#config-settings

For vanilla pip however there is no such way, it has to be set via CLI.


---

_Comment by @s-rog on 2025-06-17 04:47_

> > Can package config-settings be specified in toml files? Or is this currently a cli exclusive feature?
> 
> [@s-rog](https://github.com/s-rog) Yes this can be done for `uv`. I was able to do this via a `uv.toml` file containing at the top level:
> 
> ```
> # Compatible mode so IDEs pick up editable installs correctly.
> config-settings = { editable_mode = "compat" }
> ```
> 
> This can also be done via `pyproject.toml`, see https://docs.astral.sh/uv/reference/settings/#config-settings
> 
> For vanilla pip however there is no such way, it has to be set via CLI.

I'm pretty sure it applies to all packages in the env though.

---
