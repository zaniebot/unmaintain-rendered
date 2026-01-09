---
number: 1495
title: Add an option to store virtual environments in a centralized location outside projects
type: issue
state: open
author: DanCardin
labels:
  - enhancement
  - projects
  - virtualenv
assignees: []
created_at: 2024-02-16T14:49:37Z
updated_at: 2025-12-26T02:27:56Z
url: https://github.com/astral-sh/uv/issues/1495
synced_at: 2026-01-07T13:12:16-06:00
---

# Add an option to store virtual environments in a centralized location outside projects

---

_Issue opened by @DanCardin on 2024-02-16 14:49_

I ideally dont .venv/ folders cluttering my project folders (not the least because they may not be safe to move, at least with `venv`)

In my [personal (also rust) workflow tool](https://github.com/dancardin/prp) (that i'd love to not have to maintain if `uv` could arrive at some of the same decisions 😆) there is a setting which when set to "central" puts all venvs in `$XDG_DATA_HOME/<toolname>/...`, and the path to the venv is determined by mirroring the path to the project. That is, `~/foo/bar/baz/pyproject.toml` -> `$XDG_DATA_HOME/uv/foo/bar/baz/.venv/`.

By comparison to more traditional tools, `poetry` also (by default) will automatically create venvs in a central location. Although it puts all venvs in the same folder using a somewhat inscrutable hash to disambiguate projects of the same name.

---

This ^ feature sort of implies a few other mostly separate features in order to be useful:
* `uv activate` (because it becomes impractical to self-activate, except by copy-pasting the path that gets printed)
* which then implies some `uv self init --shell zsh` (or whatever), that gives you the shell integration to automatically activate through the CLI itself.
* `uv venv --delete` or `-d`

---

_Comment by @zanieb on 2024-02-16 14:52_

Thanks for the issue!

This is the kind of thing we plan to tackle in the future, i.e. when we build an opinionated workflow for environment management. It's not in scope right this second, but we'll revisit it :)

---

_Label `enhancement` added by @zanieb on 2024-02-16 14:52_

---

_Label `project-management` added by @zanieb on 2024-02-16 14:52_

---

_Comment by @woutervh on 2024-02-16 22:21_

`By comparison to more traditional tools, poetry also (by default) will automatically create venvs in a central location. Although it puts all venvs in the same folder using a somewhat inscrutable hash to disambiguate projects of the same name.`

IMHO: 
It's one of the really bad decisions of poetry.
As a python contractor usually supporting other python developers,  I really dislike any situation where devs have no idea where their virtualenv of their project is.  Your executables are the most important thing in your project, hiding them in arcane directories should never be a default.

Activating a venv is an antipattern because it introduces state in your shell. 
I really hope it can be de-emphasized in the future.




---

_Comment by @DanCardin on 2024-02-16 22:43_

Imo there are various positive sideeffects of the venv not being local, although I dont personally like their chosen algorithm for selecting it.

But ultimately, as long as the tool can know where the venv **should** be given the settings/invocation options, then there isn't generally a **need** to activate the venv, at least for `uv` itself to function. But it's the reality that many tools require `VIRTUAL_ENV` to be set to function properly, which either means activating the venv or routing all commands through a `uv run` (which might be nice interactively but isn't ideal for committed files imo).

---

_Comment by @ResRipper on 2024-02-17 04:13_

I use Windows/Linux/Mac every day and synchronise my projects using OneDrive, having `.venv` in my project folder is such a nightmare since it doesn't have the functionality of excluding specific files/folders from syncing, and that's why I started using `pipenv`.

IMO `venv` should always be centralised because:
1. They are not part of the "context" of the project
2. `.whl` dependencies are usually platform-dependent

I'm using [this plugin](https://github.com/DonJayamanne/vscode-python-manager) to manage and switch between different environments and don't have to know their location most of the time, but I do hope there is a standard for that.

---

_Referenced in [astral-sh/uv#1578](../../astral-sh/uv/issues/1578.md) on 2024-02-17 19:34_

---

_Comment by @hauntsaninja on 2024-02-17 22:20_

Note uv currently appears to work if you make `.venv` file a symlink to your actual venv

If you don't have symlinks on your platform, this patch of uv may work for you by adding support for `.venv` files that point to the actual venv location: https://github.com/astral-sh/uv/issues/1578#issuecomment-1949911871

When uv does go in the higher level workflow direction, I'd advocate leaving a `.venv` symlink or file pointing to the central location. This makes it easy for IDEs and other tools to figure out where the environment is. This `.venv` file / symlink idea was discussed around the time PEP 704 was a thing and had fairly positive support

---

_Comment by @woutervh on 2024-02-18 02:17_

@hauntsaninja   Thanks for the tip.

I was annoyed that uv does not support the most common normal use-case  of virtualenv ootb:

```
> virtualenv foo
> cd foo
> bin/pip install <package1>
```

To make it work, the symlink indeed works
```
> uv venv foo
> cd foo
> ln -s .  .env
> uv pip install <package2>
```
 It would be nice if "uv venv" would make the link by default.

And this would be a nice solution for managing venvs centrally outside the project-folder.


@ResRipper  and for syncing via onedrive, you can point .venv to one of the os-specific .venv-files

```
.venv -> .venv-linux
.venv-linux  --> /some/linux/path
.venv-mac  --> /some/mac/path
.venv-windows  --> /some/windows/path
```

---

_Referenced in [astral-sh/uv#1422](../../astral-sh/uv/issues/1422.md) on 2024-02-18 02:23_

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:38_

---

_Comment by @matterhorn103 on 2024-04-16 14:40_

Just to add my voice that this would be really really nice to have. Different tools seem to choose either one approach or the other, and it would be great if `uv` would do both as I like and use both in different situations:

For development situations where `git` is being used, having `.venv` there in the project's folder is convenient.

For other projects e.g. scientific ones that are in maybe shared or cloud or working folders, it's undesirable behaviour, and then working with `conda` is much smoother than with Python venvs. That's pretty much the only thing that keeps me using `conda` for some things (other than the different ecosystem, naturally).

My impression was that symlinks have poor portability, so like @DanCardin I'd prefer something like a simple `-c` switch to create the venv in an automatically determined central location, and simple automatic activation, e.g.:

```bash
~/example/foo >>> uv venv -c
Using Python 3.11.9 interpreter at: /usr/bin/python3
Creating virtualenv at: /home/jdoe/.local/share/uv/example/foo/.venv
Activate with: uv venv activate
~/example/foo >>> uv venv activate
Activating virtualenv at /home/jdoe/.local/share/uv/example/foo/.venv
(foo) ~/example/foo >>> deactivate
~/example/foo >>>
```

but with the addition that it would be cool to be able to also activate the venv by name from another location, with the search resolved intelligently and some ability to disambiguate; I could imagine that looking like:

```bash
~/some/folder >>> uv activate foo
Searching for virtualenvs at /home/jdoe/.local/share/uv/**/foo/.venv
Activating virtualenv at: /home/jdoe/.local/share/uv/example/foo/.venv
(foo) ~/some/folder >>> uv activate bar
Searching for virtualenvs at /home/jdoe/.local/share/uv/**/bar/.venv
error: virtualenv name is ambiguous! The following matches were found:
  1) /home/jdoe/.local/share/uv/Documents/bar/.venv
  2) /home/jdoe/.local/share/uv/project1/bar/.venv
disambiguate venvs with the same name using parents e.g. to activate 1) use:
  uv venv activate Documents/bar
(foo) ~/some/folder >>> uv activate project1/bar
Searching for virtualenvs at /home/jdoe/.local/share/uv/**/project1/bar/.venv
Activating virtualenv at: /home/jdoe/.local/share/uv/project1/bar/.venv
(bar) ~/some/folder >>>
```

---

_Comment by @matterhorn103 on 2024-06-20 15:20_

So in addition to the "not wanting the venv to be stored in a folder that is backed up to the cloud" use case, I found another use case today:

- Creating compiled binaries of Qt apps written with PySide using `pyside6-deploy` (a nuitka wrapper) [isn't possible](https://doc.qt.io/qtforpython-6/deployment/deployment-pyside6-deploy.html) if a venv folder is present in the application folder

(In my view this is a short-sighted approach on the tool's part, but it's just another example of why it might be necessary to keep a venv elsewhere.)

---

_Referenced in [astral-sh/uv#6204](../../astral-sh/uv/issues/6204.md) on 2024-08-19 13:21_

---

_Referenced in [astral-sh/uv#6413](../../astral-sh/uv/issues/6413.md) on 2024-08-22 14:16_

---

_Referenced in [astral-sh/uv#6501](../../astral-sh/uv/issues/6501.md) on 2024-08-23 13:18_

---

_Referenced in [astral-sh/uv#6504](../../astral-sh/uv/issues/6504.md) on 2024-08-23 14:43_

---

_Referenced in [astral-sh/uv#6511](../../astral-sh/uv/issues/6511.md) on 2024-08-23 14:56_

---

_Referenced in [astral-sh/uv#6597](../../astral-sh/uv/pulls/6597.md) on 2024-08-25 08:19_

---

_Referenced in [astral-sh/uv#5229](../../astral-sh/uv/issues/5229.md) on 2024-08-25 08:27_

---

_Referenced in [astral-sh/uv#6612](../../astral-sh/uv/issues/6612.md) on 2024-08-25 16:40_

---

_Referenced in [astral-sh/uv#6669](../../astral-sh/uv/issues/6669.md) on 2024-08-28 02:18_

---

_Referenced in [astral-sh/uv#6746](../../astral-sh/uv/issues/6746.md) on 2024-08-28 13:11_

---

_Referenced in [astral-sh/uv#6834](../../astral-sh/uv/pulls/6834.md) on 2024-08-29 22:37_

---

_Referenced in [astral-sh/uv#6584](../../astral-sh/uv/issues/6584.md) on 2024-09-04 14:34_

---

_Comment by @callegar on 2024-09-18 22:34_

> When uv does go in the higher level workflow direction, I'd advocate leaving a .venv symlink or file pointing to the central location.

and

> It would be nice if "uv venv" would make the link by default.
> And this would be a nice solution for managing venvs centrally outside the project-folder.

Even this makes things more complex than needed if you sync to the cloud across multiple systems, because the link might need to be to different places on different machines.

If only for compatibility and ease of migration from one tool to another, I would offer the possibility to do what pdm does. That tool offers either the option to have a local `.venv` or the option to have venvs stored all together in a centralized place.

Note that having the venvs all stored in a single place also makes it easier to apply deduplication tools on suitable filesystems.

---

_Referenced in [astral-sh/uv#7642](../../astral-sh/uv/issues/7642.md) on 2024-09-23 16:03_

---

_Comment by @PhilipVinc on 2024-09-24 08:34_

@zanieb is this a feature you're considering for the short term, or it's not high priority?

---

_Comment by @zanieb on 2024-09-24 12:55_

This is not something we're tackling immediately — there are a few workarounds and we are prioritizing work that affects more users.

---

_Comment by @hauntsaninja on 2024-09-24 19:56_

Since it hasn't been mentioned on this issue yet:
- For the high level project workflows, you can use `UV_PROJECT_ENVIRONMENT` to control venv location. [Symlinking `.venv` also works](https://github.com/astral-sh/uv/issues/1495#issuecomment-1950442191)
- For low level `uv pip/venv` workflows, you can set `UV_PYTHON` / `--python`, or similarly symlink `.venv` as above

---

_Comment by @phi6ias on 2024-09-25 19:33_

I would like to ask to extend the thought towards a centralised location for venvs and caches in the internal network of a company. 

Besides reducing the amount of storage wasted on local hard drives, the aim would be to allow a developer to write a programme or script, add an extra entry to the script section in the (main) file, as described [here](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies), that points to the venv he created on the network share and have it circulated to colleagues _as if_ it was a standalone (i.e. no installation required other than uv itself).

---

_Comment by @callegar on 2024-09-29 10:14_

The `.venv` symlink is possibly not a good workaround for those who store their work in the cloud and use it on different platforms. Depending on the cloud-sync tool that you use, you may either:
- get your symlink also synchronized "as is", forcing you to recreate the link in case the actual venv is in different locations on the different platforms (as it is often the case)
- have your sync client broken enough to follow the link and sync the destination.

Often you can "exclude files by pattern" on sync clients, to relieve these problems, but it is not always handy. In these cases, the env variable might be a better stopgap.

---

_Referenced in [astral-sh/uv#7898](../../astral-sh/uv/issues/7898.md) on 2024-10-03 14:55_

---

_Comment by @fabienval on 2024-10-04 04:59_

I too would be keen to have venv centrally stored due to onedrive not playing nice when trying to sync .venv....
I see people suggesting symlinks, but when we are on windows, is there a way to have similar behavior?
I tried creating a shortcut named .venv but uv recreate a proper folder called .venv and install in there when I `uv sync`...
( We do not have permission to use tools like mklink)

---

_Comment by @hauntsaninja on 2024-10-04 05:08_

Does the `UV_PROJECT_ENVIRONMENT` trick work for your use case? If not, you could try rebasing this patch https://github.com/astral-sh/uv/issues/1578#issuecomment-1949911871 and see if uv is willing to merge it

---

_Referenced in [astral-sh/uv#7357](../../astral-sh/uv/pulls/7357.md) on 2024-10-09 12:26_

---

_Referenced in [astral-sh/uv#8037](../../astral-sh/uv/issues/8037.md) on 2024-10-09 13:41_

---

_Referenced in [UCL-ARC/python-tooling#419](../../UCL-ARC/python-tooling/issues/419.md) on 2024-10-22 09:50_

---

_Referenced in [astral-sh/uv#8514](../../astral-sh/uv/issues/8514.md) on 2024-10-24 06:38_

---

_Comment by @krstp on 2024-10-26 03:59_

See my solution with `.bashrc/.zshrc` function `uvactivate <named_venv>` from a central local where I keep all my venvs: https://github.com/astral-sh/uv/issues/7898

---

_Referenced in [astral-sh/uv#8830](../../astral-sh/uv/issues/8830.md) on 2024-11-05 15:39_

---

_Comment by @callegar on 2024-11-08 08:39_

Possibly, having venvs in a central location would also allow making `--link-mode=symlink` more robust. Currently, if you use it and then by mistake you clear the cache, all your venvs made with this link mode will be broken. If all the venvs are centralized, the cache clearing command could get an operating mode (maybe to be made the default) where the (centralized) environments are scanned and the stuff that has symlinks to is preserved in the cache.

---

_Referenced in [astral-sh/uv#9254](../../astral-sh/uv/issues/9254.md) on 2024-11-19 22:17_

---

_Comment by @YodaEmbedding on 2024-11-22 04:04_

@callegar Poetry puts its virtualenvs in `$XDG_CACHE_HOME/pypoetry/virtualenvs`. Theoretically, poetry virtualenvs should be reproducible, so this is logical. However, cache clearing should be a safe operation that should not have adverse effects on startup performance.

Thus, [a better location](https://github.com/python-poetry/poetry/issues/3346#issuecomment-1667394472) might be `$XDG_STATE_HOME/pypoetry/virtualenvs`, i.e. `~/.local/state/pypoetry/virtualenvs`.

---

_Referenced in [nat-n/poethepoet#257](../../nat-n/poethepoet/issues/257.md) on 2024-11-24 16:27_

---

_Referenced in [zanieb/uv#6](../../zanieb/uv/issues/6.md) on 2024-11-26 17:33_

---

_Referenced in [astral-sh/uv#9452](../../astral-sh/uv/issues/9452.md) on 2024-11-26 21:15_

---

_Renamed from "Option/setting to manage virtualenvs centrally" to "Add an option to store virtual environments in a centralized location outside projects" by @zanieb on 2024-11-26 23:53_

---

_Comment by @FranzForstmayr on 2024-11-27 14:29_

Another usecase: Some of my colleagues work mainly on network drives, however Python is really slow loading a huge env from a network drive. Poetry is currently a good fit, as the default location of the venv is somewhere on the local SSD which speeds up loading significantly.

---

_Comment by @matterhorn103 on 2024-11-27 16:25_

Coming back to this and reading my earlier comment, it's clear that `uv` wants to, and will, make activating and deactivating venvs something a user never has to think or even know about, which will be a blessing.

So that renders suggestions back in the early days like `uv activate`, an interface to delete the venvs (they could be treated as part of the normal cache), or a way to launch a shell with the venv activated, kind of irrelevant. But it honestly makes the prospect of this being implemented even more attractive...

With `uv`'s vision much clearer these days, a very dreamy API now can be imagined: I would run `uv init -c` for a new project or `uv sync -c` for an existing one, and that would be the _first and final time_ I think about it.

Thereafter I would use `uv` exactly as normal and it would know to use the central venv i.e. `uv add`, `uv run`, `uv sync` etc. all _just work_ without me having to do anything different or provide a flag every time. 😀

---

_Comment by @DanCardin on 2024-11-27 17:44_

Keep in mind tooling and/or editors still universally react to a VIRTUAL_ENV env var. So I think it'd be a shame if uv leaned too far in that direction. Certainly it's **already** clearly the case that uv itself and/or project tooling you write which uses uv can stop caring about virtualenvs and activation, but i dont think eschewing them entirely is a good idea.

---

_Comment by @jromal on 2024-11-29 16:20_

> So in addition to the "not wanting the venv to be stored in a folder that is backed up to the cloud" use case, I found another use case today:
> 
>     * Creating compiled binaries of Qt apps written with PySide using `pyside6-deploy` (a nuitka wrapper) [isn't possible](https://doc.qt.io/qtforpython-6/deployment/deployment-pyside6-deploy.html) if a venv folder is present in the application folder
> 
> 
> (In my view this is a short-sighted approach on the tool's part, but it's just another example of why it might be necessary to keep a venv elsewhere.)

Maybe you can install PySide as a tool and then set the TCL/TK paths of your uv python for running all pyside6 tools.
This is how I do it for rcc and uic. 

I agree on the ".venv" on OneDrive. But this is because OneDrive is terrible on selecting what not to backup. A ".onedriveignore" like the ".gitignore" would do it. But the problem is OneDrive, not uv. Actually, even if I use it, I do not reccomend anybody to run any python on a OneDrive folder.


---

_Comment by @jromal on 2024-11-29 16:26_

> For other projects e.g. scientific ones that are in maybe shared or cloud or working folders, it's undesirable behaviour, and then working with `conda` is much smoother than with Python venvs. That's pretty much the only thing that keeps me using `conda` for some things (other than the different ecosystem, naturally).

I moved everything I had in conda (miniforge) to uv. And I am delighted. What a change.
At the beginning I was missing this decentralized virtual environment. But not anymore.
I did lost control of what project uses what environment, and when working on another computer was a nightmare.
Now "uv sync" and done. No need to think. Furthermore, many environments are not needed.
Just use "uv run --with ???" with a --with clause for each requirement and you do wonders.
And if you work on single file scripts, check the "--script" argument for init and add. 

---

_Comment by @ostpoller on 2024-12-03 14:49_

Another use case (maybe a niche one):
My main development environment is a Linux VM on a Windows machine (Oracle VM VirtualBox). My projects are located in a directory shared between the Linux guest and the Windows host to have them available on both, quickly transfer data in and out of project directories and have them backed-up (the Windows host is hooked up to a corporate network). 
Symlinks are not possible on this shared directory (a Windows file system folder). 
I currently use conda (miniforge) and have a central location in a directory on the Linux VM where I keep all conda environments.
I would potentially keep conda for managing Python versions (via corporate internal mirrors of conda channels), but would like to use uv to manage individual projects and their dependencies.

---

_Referenced in [astral-sh/uv#9805](../../astral-sh/uv/issues/9805.md) on 2024-12-11 12:06_

---

_Comment by @dest1n1s on 2024-12-12 17:08_

> Keep in mind tooling and/or editors still universally react to a VIRTUAL_ENV env var. So I think it'd be a shame if uv leaned too far in that direction. Certainly it's already clearly the case that uv itself and/or project tooling you write which uses uv can stop caring about virtualenvs and activation, but i dont think eschewing them entirely is a good idea.

@DanCardin A `VIRTUAL_ENV` env var does not necessarily conflict with a centralized virtual env management. `uv` could set this env var correctly to the centralized location. IMO your development experience would not get worse in a centralized management. Your IDE should still be able to perform correct code jumping. You can get rid of random large cache files living with your source code (which may be tricky for some syncing tools to deal with). You can test some random code with an existing environment with something like `conda activate` while keeping the code portability and reproducibility brought by dependency specifications in `pyproject.toml`.


---

_Comment by @DanCardin on 2024-12-12 19:06_

Oh I agree, this is **my** issue after all :P. I meant leaning away from `VIRTUAL_ENV`, which they seem to already want to do. The ability to "activate" a shell, even if that's only setting `VIRTUAL_ENV` seems critical to any solution. 

Even if `uv` itself can, given a specific project in a directory, determine the central venv storage location, other tools wont natively, so being able to activate the shell becomes important (lest every other project be forced to have uv-specific adapters to figure it out)

---

_Comment by @dest1n1s on 2024-12-13 09:30_

Oh sorry for my misunderstanding. I totally agree with you.

---

_Comment by @Cornelius-Figgle on 2024-12-13 11:32_

Just to weight in, I sometimes edit projects (mainly for college) off a SMB share on my home server. I generally prefer having `.venv` in the project folder as it is neater, but the venv tools for python don't seem to play nice with SMB, so the solution is to store the venvs locally. In poetry this is rather easy as you just add a `poetry.toml` to the project that sets `in-project` to false (since my global config sets it to true for most projects). Having a way to do this in uv would be very useful, as besides this uv seems an amazing tool. 

---

_Comment by @zanieb on 2024-12-17 20:53_

@Cornelius-Figgle Does a symlink work for you, e.g., `.venv -> <somewhere else>`?

---

_Referenced in [astral-sh/uv#7778](../../astral-sh/uv/issues/7778.md) on 2024-12-28 16:53_

---

_Referenced in [astral-sh/uv#11056](../../astral-sh/uv/issues/11056.md) on 2025-01-29 14:12_

---

_Comment by @sitic on 2025-01-31 01:46_

I'm long-time `virtualenvwrapper` user and miss the convenience of activating central, named, virtual environments with a quick `workon` command.

I wrote a small bash/zsh script called [uv-virtualenvwrapper](https://github.com/sitic/uv-virtualenvwrapper) that provides core virtualenvwrapper-like functionality for uv with tab completion. `mkvirtualenv <name>` to create (arguments passed to `uv venv`), `workon <name>` to activate, `rmvirtualenv` to remove, and `lsvirtualenv` to list environments stored. Virtual environments are stored in `~/.virtualenvs` by default and the script compatible with existing `virtualenvwrapper` venvs.

---

_Comment by @Avasam on 2025-02-03 03:38_

Just hit the need for multiple as I have a project that I test on both Windows and WSL.
I thought I could simply manually activate the .venv first to get uv to use it. But uv actively denies respecting `VIRTUAL_ENV`:

![Image](https://github.com/user-attachments/assets/759948a7-1738-4a8b-9862-72cab639d59f)
```shell
avasam@Horus:/mnt/e/Users/Avasam/Documents/Git/AutoSplit$ uv venv .venv-linux
Using CPython 3.11.9 interpreter at: /home/avasam/.pyenv/versions/3.11.9/bin/python3
Creating virtual environment at: .venv-linux
Activate with: source .venv-linux/bin/activate
avasam@Horus:/mnt/e/Users/Avasam/Documents/Git/AutoSplit$ source .venv-linux/bin/activate
(.venv-linux) avasam@Horus:/mnt/e/Users/Avasam/Documents/Git/AutoSplit$ uv sync
warning: `VIRTUAL_ENV=.venv-linux` does not match the project environment path `.venv` and will be ignored
error: Project virtual environment directory `/mnt/e/Users/Avasam/Documents/Git/AutoSplit/.venv` cannot be used because it is not a valid Python environment (no Python executable was found)
```

Edit: See answer below. Not a full out-of-the-box solution. But it works as a workaround. One can use `pyenv` to manage different environments and manually update `UV_PROJECT_ENVIRONMENT` if using WSL for multiple projects.

---

_Comment by @charliermarsh on 2025-02-03 03:43_

Correct. You can use `UV_PROJECT_ENVIRONMENT` if you want to overwrite it, per the docs: https://docs.astral.sh/uv/concepts/projects/config/#project-environment-path.

---

_Comment by @woutervh on 2025-02-03 07:55_

If uv would gain this functionality, I would like the option to easily create a .venv-symlink to that centralized venv-location outside the project directory.

---

_Referenced in [python/typeshed#13482](../../python/typeshed/pulls/13482.md) on 2025-02-09 21:16_

---

_Referenced in [astral-sh/uv#11420](../../astral-sh/uv/issues/11420.md) on 2025-02-11 13:47_

---

_Comment by @Nick-Hemenway on 2025-02-11 19:49_

Are there any updates on plans to support this feature? A natively supported interface similar to virtualenvwrapper would be incredible. I think it's a pretty common desire to not want a .venv folder to be included in OneDrive/iCloud file syncing. The ability to re-use environments across different folders would be a huge plus as well.

---

_Comment by @Avasam on 2025-02-11 20:00_

For me, uv `0.5.29`'s `--active` flag resolved my need for this feature for testing though WSL:
I can manually create an environment once. Then as long as that env is activated, `uv sync --activated` will work to update my dependencies when in WSL.

![Image](https://github.com/user-attachments/assets/5d0c96fc-5e34-4829-8f0a-727fa7ec7eb6)

---

_Comment by @Nick-Hemenway on 2025-02-12 18:21_

Using the `--active` flag works and allows a user to add and remove packages to a centralized virtual environment using, e.g. `uv add --active numpy` or `uv remove --active`. It would be nice if there was a way (perhaps through an environment variable?) to turn the `--active` flag on by default so that one doesn't have to remember to type the `--active` flag on every time they invoke `uv` to update their package/environment dependencies. 

---

_Referenced in [astral-sh/uv#11458](../../astral-sh/uv/issues/11458.md) on 2025-02-12 18:40_

---

_Referenced in [astral-sh/uv#1910](../../astral-sh/uv/issues/1910.md) on 2025-02-13 10:45_

---

_Referenced in [astral-sh/uv#11476](../../astral-sh/uv/issues/11476.md) on 2025-02-13 15:41_

---

_Comment by @PhilipVinc on 2025-02-14 09:27_

I had an idea about this issue.
If I understand correctly, a counter-argument to storing virtual environments in a centralised location are other tools would not work correctly because they do not know where to look for the venv directory.
However, there are several situations where this is necessary (again, storing projects in dropbox/cloud services, or working in HPC environments where projects and virtual environments should be stored on different filesystems).

To solve the conundrum, why not have an option to store the `.venv` directory in a centralised location, as to address this issue, but symlink it to the current project directory, as to allow discoverability by all other tools?

Every time we run `uv sync --depot XYZ` or `ùv run --depot XZY` the symlink would be recreated if stale.

I believe this would not introduce any issue?

---

_Comment by @PhilipVinc on 2025-02-14 09:57_

To provide some more context to this idea:
I fully buy in `uv`'s point of view that activating environments is an anti-pattern and that the best approach is never to have to do that.

However, right now, if we are unable to store the `.venv` in the current directory for whatever reason (again, HPC setups usually forbid or prevent this, or cloud setups...) the current workaround, of declaring `VIRTUAL_ENV + --active` or `UV_PROJECT_ENVIRONMENT` is equivalent to activating an environment.

What I propose above,  to have an env variable `UV_GLOBAL_DEPOT=XYZ` which automatically stores the `.venv` in the global depot. The precise path could be something like `$UV_GLOBAL_DEPOT/$(hash project_path)`
However, **by simlinking** the venv to the project directory, the fact that the venv is stored elsewhere becomes unimportant for all existing tooling. 
This would allow to benefit from `uv`'s opinionated 'no activation workflow' on systems where the current behaviour cannot be used.

---

_Comment by @john-hen on 2025-02-14 10:19_

> I believe this would not introduce any issue?

I've tried this approach for a bit. I have my own little shell/batch script that manages my "central" venvs for me, on Windows, Mac, WSL, and HPC. One issue this introduces is that Git does not follow symlinks, so it won't see the `.gitignore` file that UV places there for our convenience. Plus, creating symlinks on Windows (usually) requires admin privileges, so UV would need to learn how to request a UAC prompt.

---

_Comment by @PhilipVinc on 2025-02-14 11:34_

indeed, you'd have to add a .gitignore entry to the gitignore in the root directory in that case...

---

_Comment by @jatonline on 2025-02-14 11:41_

@PhilipVinc
> I believe this would not introduce any issue?

In the discussion above, @ResRipper and @matterhorn103 talked about one motivation for having the virtual environment outisde the project directory being to stop cloud sync tools from uploading the `.venv` to the cloud.

If I've read the above discussion correctly, one suggestion has been to place a symlink at `.venv` to some central depot directory. I'm not sure how well-known this is, so thought it might be helpful to add that: on some systems (e.g. recent MacOS and Dropbox), if you put symlink inside the cloud sync folder, this gets followed by the sync client and so get uploaded to the cloud as well. (So I'm thinking this might not fix some of the originally stated issues?)

I've noticed this behaviour when using [detached environments with `pixi`](https://pixi.sh/dev/reference/pixi_configuration/#detached-environments), too.

---

_Comment by @callegar on 2025-02-14 16:55_

Maybe for your case it would be nice if uv could support either `.venv` be the virtual environment directory or it be a file (I think that `git` does something similar on platforms that do not support symlinks well). In the latter case, it could contain a line such as `linkto = <location>`. So it could work equivalently to a symlink, with the additional advantages of: (i) not being incorrectly followed by poor synchronization tools; and (ii) being easier on platforms where symlinks are not so well supported. Furthermore, this could be enhanced by letting `<location>` be a string where some keyworks get expanded. For instance `{hostname}`, `{project_name}`, `{path_hash}` (being the hash of the project path), or `{python_version}`. This would let one easily avoid clashes (e.g. similarly to what pdm does for centralized venvs).

---

_Comment by @DanCardin on 2025-02-14 17:14_

> However, by simlinking the venv to the project directory, the fact that the venv is stored elsewhere becomes unimportant for all existing tooling.

Most existing tooling doesn't care about the path of your venv as it is, they look for `VIRTUAL_ENV`. So, i think all all a symlink does is work around the lack of a `uv activate` command (and corresponding shell integration) which sets that env var to the central location determined by `uv`.

`uv`'s preference against venv activation makes sense certainly for uv commands themselves; i think it starts to get more annoying when i'm forced to `uv run pytest` instead of just `pytest`; and i think it devolves from there with e.g. `vim` where it needs to be in the env for editor tooling to function (or else be able to detect the venv a-la vscode).

but i'm somewhat pessimistic on whether any of this will get dealt with in any case, due to their stance on respecting VIRTUAL_ENV in the first place...

> $UV_GLOBAL_DEPOT/$(hash project_path)

per my OP, I would very much prefer a solution that allows me to have multiple named venvs per project, typically for testing multiple versions of python in quick sequence).

so while it doesnt **need** to be my suggestion, i think `$some-root-uv-path/relative/path/to/project/{venv-name}` (where venv-name defaults to `.venv.{python-version}`) makes a lot of sense, such that `uv run -p 3.11` "just works" and selects the correct venv for the selected version of python.

---

_Comment by @francois-rozet on 2025-02-15 20:33_

If you use bash, you can setup a command that looks for virtual environments at a global location (e.g. `~/.venvs`) and activates them based on the env name. For example, copy-paste the following in your `.bashrc` and now (after restarting the shell) the `aa` command allows to activate a globally stored env, with auto-completion!

```bash
aa() {
    if [[ $# -eq 0 ]]
    then
        . .venv/bin/activate
    else
        . ~/.venvs/$1/bin/activate
    fi
}

export -f aa

_activate_venv_autocomplete() {
    if [[ $COMP_CWORD == 1 ]]
    then
        local word=${COMP_WORDS[COMP_CWORD]}
        local envs=$(ls -1 $HOME/.venvs)
        COMPREPLY=($(compgen -W "$envs" -- "$word"))
    else
        COMPREPLY=()
    fi
}

complete -F _activate_venv_autocomplete aa
```

When you create a new env with `uv`, remember to store it in `~/.venvs`.

```
uv venv ~/.venvs/myenv
aa myenv
```

---

_Comment by @PhilipVinc on 2025-02-16 11:16_

My point is that uv’s mental model is to deprecate activating environments, as it can lead to problems and mismatches.

I would like to be able to use uv’s workflow in setups where I normally can’t, such as HPC clusters.
Right now it’s impossible. 

---

_Comment by @francois-rozet on 2025-02-16 11:39_

I think we are very far from being able to deprecate activating environments. Might as well make my current life easier 😉

---

_Comment by @astrojuanlu on 2025-02-16 11:56_

<details>
<summary>Off topic</summary>

> I think we are very far from being able to deprecate activating environments. 

I don't think so. `uv run` already works and takes care of everything under the hood. It's an abstraction, so in the future it could decouple itself from in-tree venvs. Or somebody else could create a `uvc run` that uses centralized locations. The point is to stop doing `source .venv/bin/activate` or whatever OS-specific command you need (see also #1910). In Node.JS and Rust lands folks are used to doing `npm run` and `cargo run`, I don't see why we can't adopt that in Python. For that though, people need to internalise this abstraction, and documentation and instructional materials need to be updated accordingly.
</details>

---

_Comment by @francois-rozet on 2025-02-16 12:17_

<details name="Off topic">

<summary>Off topic</summary>

> For that though, people need to internalise this abstraction, and documentation and instructional materials need to be updated accordingly.

Unfortunately, technology changes faster than (most) people. I already use `uv run`, it is great. But I also know people that are still using Python 2.7 and 3.6. And on HPC clusters, the norm is still (and will likely remain for a while) to create an env, activate it, install your dependencies and run your code. Improving that workflow with `uv venv` and `uv pip` is already a huge step. Deprecating activating the env can come later.

</details>

---

_Comment by @callegar on 2025-02-16 12:39_

<details>

<summary>Off topic</summary>

It is a matter of what you are used to and what you are doing. Yes, in many cases not having the shell hold the a state can be advantageous. Yet, depending on what you do typing `uv run` every time might end up more verbose than typing `uv venv --activate` once (should anything like this get supported or `. .venv/bin/activate` otherwise). Furthermore, there are really cases when you want to use uv, not to develop a project, but just to have a well defined virtual environment to play in (think working on notebooks that need a specific environment). In the latter case, you may really prefer having the shell holding the state to assure that you can `cd` to different places maintaining the state.

</details>


---

_Comment by @mil-ad on 2025-02-17 10:19_

I tried setting `UV_PROJECT_ENVIRONMENT` to something like `export UV_PROJECT_ENVIRONMENT=/scratch/$USER/venvs/${PWD#$HOME}"` for all users. 

This mostly works but I'm one issue I'm having is that `uv venv` ignores `UV_PROJECT_ENVIRONMENT` #10807

---

_Comment by @Badg on 2025-02-20 15:41_

As with others, I'm in a situation where I'm syncing source code via dropbox in addition to using git. I do this because I very frequently switch between a windows desktop computer and my (osX) laptop for development, and don't want to bother with git stash, WIP commits, etc. I just want to save my files, shut down my computer, and continue working on the go.

I've only skimmed the above discussion, so apologies if I'm missing someone commenting this already. But: at first glance, the idea of "just add a file to the project with a path to the .venv" seems like it would work (analagous to the ``.git`` file -- **not** folder -- which, side note for anyone else in this situation, [allows you to relocate your git directory](https://stackoverflow.com/a/8603156) outside of your cloud sync provider).

The problem I've run into is that this forces all of your devices to use the same location for your environments (or your git directories), because -- even though it's ignored by git -- the ``.git`` file gets synced by your cloud provider. So in my case, that means I'd need to have a parent directory to store all of my .git repos that's the same on both windows and osx. To complicate matters further, my system language is usually set to german, and localization of system paths (like the users directory) is handled differently on both platforms. This means that a subfolder of the drive root is the only reliable place I can store my git directories. In short, it's a huge pain in the ass, and it really only works because I'm the only person using it. If you were in a shared cloud sync account, that would break down really quickly.

The way other python tools handle this (at least as far as I've seen) is that, when you enable a global/user setting to store the managed virtual envs in a centralized location (for example, in poetry, by setting the ``virtualenvs.in-project`` option to false), they make some kind of combination of the project's folder name and a hash of the path to the project (to avoid collisions based on the folder name, or to allow multiple clones of the same project). I think in most cases it's also possible to set a different config option to determine which folder gets used as the parent of the virtualenvs.

Anyways, though typically I'm developing within docker and can get away with some combination of ``.dockerignore`` and/or ``UV_PROJECT_ENVIRONMENT`` to sidestep this problem, today I was trying to set up UV for the first time on my **host** machine for package development for something destined for pypi. **Because dropbox sometimes locks files while syncing, UV was completely unable to create a virtualenv in the project:**

```
PS D:\Dropbox\Projekte\taev\templatey> uv sync
Resolved 27 packages in 995ms
      Built templatey @ file:///D:/Dropbox/Projekte/taev/templatey
Prepared 2 packages in 2.59s
░░░░░░░░░░░░░░░░░░░░ [0/25] Installing wheels...                                                                                                                                                                                                                                        warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
error: Failed to install: ruff-0.9.6-py3-none-win_amd64.whl (ruff==0.9.6)
  Caused by: failed to remove directory `D:\Dropbox\Projekte\taev\templatey\.venv\Lib\site-packages\ruff-0.9.6.data`: Der Prozess kann nicht auf die Datei zugreifen, da sie von einem anderen Prozess verwendet wird. (os error 32)
```

This would have been a dealbreaker for me, but I really like UV and don't want to revert back to another tool just for package development. So I opted for the nuclear option, and replicated the venv-not-in-project behavior by creating a wrapper around UV that simply sets the ``UV_PROJECT_ENVIRONMENT`` based on the current project and then delegates the call to UV:

```python
import hashlib
import os
import subprocess
import sys
from pathlib import Path

import typer

from nix.nix_ import typer_app

ROOT_DIR_FOR_ENVS = Path.home() / '.pyvenv-nix-uv'


def _calculate_project_env_path():
    dir_to_check = Path.cwd()

    # This will search from the CWD up until the root for a pyproject.toml.
    # Note: yes, this skips the root dir, no, we don't care; just make sure
    # you don't ever put your pyproject.toml on your drive root.
    project_path = None
    while (project_dir := dir_to_check).parent != dir_to_check:
        if (project_path := (dir_to_check / 'pyproject.toml')).exists():
            break

    if project_path is None:
        raise RuntimeError('No pyproject found in CWD or parents!')

    project_name = project_dir.name
    # Note: hashing based on pyproject.toml instead of the parent directory,
    # no real reason, is it even possible to rename pyproject.toml?
    project_hash = hashlib.sha256(str(project_path).encode()).hexdigest()
    venv_dirname = f'{project_name}-{project_hash:.5}'
    return ROOT_DIR_FOR_ENVS / venv_dirname


@typer_app.command(
    'uv',
    context_settings={
        'allow_extra_args': True,
        'ignore_unknown_options': True})
def uv_passthrough_with_project_env(ctx: typer.Context):
    """As a workaround to https://github.com/astral-sh/uv/issues/1495,
    this sets UV_PROJECT_ENVIRONMENT based on the first pyproject found
    while walking up from the current working directory and then
    delegates the call to UV via subprocess.
    """
    try:
        return subprocess.run(
            ['uv', *ctx.args],
            check=True,
            env={
                'UV_PROJECT_ENVIRONMENT': str(_calculate_project_env_path()),
                **os.environ})
    except subprocess.CalledProcessError as exc:
        sys.exit(exc.returncode)

```

That, in turn, was installed as a command in a UV tool called ``nix``, which means that all I need to do for things to "just work" is run ``nix uv ...`` (note that ``templatey-90392`` is an example of the result of the hashing behavior I mentioned):

```
PS D:\Dropbox\Projekte\taev\templatey> nix uv sync
Using CPython 3.13.2
Creating virtual environment at: C:\Users\Niklas\.pyvenv-nix-uv\templatey-90392
Resolved 27 packages in 1.05s
      Built templatey @ file:///D:/Dropbox/Projekte/taev/templatey
Prepared 2 packages in 1.86s
Installed 25 packages in 807ms
 + asttokens==3.0.0
 + colorama==0.4.6
 + decorator==5.1.1
 + executing==2.2.0
 + iniconfig==2.0.0
 + ipython==8.32.0
 + jedi==0.19.2
 + matplotlib-inline==0.1.7
 + mypy==1.15.0
 + mypy-extensions==1.0.0
 + packaging==24.2
 + parso==0.8.4
 + pluggy==1.5.0
 + prompt-toolkit==3.0.50
 + pure-eval==0.2.3
 + pygments==2.19.1
 + pytest==8.3.4
 + refurb==2.0.0
 + ruff==0.9.7
 + stack-data==0.6.3
 + templatey==0.0.0.dev0 (from file:///D:/Dropbox/Projekte/taev/templatey)
 + traitlets==5.14.3
 + typing-extensions==4.12.2
 + wat-inspector==0.4.3
 + wcwidth==0.2.13
```

---

_Comment by @sradc on 2025-02-23 10:50_

Another advantage of having a centralised location is the cleanup; means not having to search for all the `.venv` dirs.
```
find ~/ -type d -name ".venv"
```

---

_Comment by @al3xandr3 on 2025-02-25 15:54_

@Badg i think you solution is great. Do you plan to make it as an easily available uv tool ?
Would love to use it myself

---

_Comment by @thehesiod on 2025-02-26 03:07_

here's my solution, added to my `.bashrc` (note using `g` versions since I'm on mac and had to use commands from `brew install coretools`):
```bash
uv_venv() {
    VENV_ROOT=$(greadlink --canonicalize ~/.local/share/uv/venv)
    SRC_ROOT=$(greadlink --canonicalize ~/dev)
    PROJ_REL_PATH=$(grealpath --relative-base=${SRC_ROOT} "${PWD}")
    export UV_PROJECT_ENVIRONMENT="${VENV_ROOT}/${PROJ_REL_PATH}"
}
```

then you can run `uv_venv` in the folder you want, then run any uv command you want.

---

_Referenced in [astral-sh/uv#11773](../../astral-sh/uv/issues/11773.md) on 2025-02-26 10:08_

---

_Comment by @eabase on 2025-02-26 13:31_

This looks like a promising discussion, TL;DR all yet. 
Just filed a similar issues, in ^ above, but not sure if this cover what I had there. 

Can someone summarize what points are on the table right now? 

UPDATE

My ticket was just closed so pasting my question here:

---

Will **this** ticket resolve and discuss:

* List **all** available venv from anywhere.
* List specific **project locations** that are using those venv's.
* Can *activate* any of those venv's from any location in the system!
* **share** venv installs locations between different venv's.
* Keep all venv installations/files (ie. all python packages) on a separate disk. 

Be able to list venv's from anywhere?

```bash
# lsven

limesdr
llm
lunar
nanovna
osint
stats

```

And here I created a Powershell comandlet to do the above.
It should be trivial (and simpler) to recreate in bash.

![Image](https://github.com/user-attachments/assets/1b04a53b-955a-42bd-a3b1-455f97349344)


---

_Comment by @luqasz on 2025-02-26 19:10_

I am using mac TimeMachine in which I've excluded `$HOME/Library/Caches` folder. Virtual envs can be large in size. No need to backup venvs since they can be reproduced. 

---

_Comment by @edmorley on 2025-02-26 19:26_

> I am using mac TimeMachine in which I've excluded `$HOME/Library/Caches` folder. Virtual envs can be large in size. No need to backup venvs since they can be reproduced.

See also #8796 for an alternative way the Time Machine issue could be resolved. (uv already creates [`CACHEDIR.TAG`](https://bford.info/cachedir/), however, Time Machine doesn't honour that and instead needs different extended attributes to be set).

---

_Comment by @luqasz on 2025-02-26 21:53_

> > I am using mac TimeMachine in which I've excluded `$HOME/Library/Caches` folder. Virtual envs can be large in size. No need to backup venvs since they can be reproduced.
> 
> See also [#8796](https://github.com/astral-sh/uv/issues/8796) for an alternative way the Time Machine issue could be resolved. (uv already creates [`CACHEDIR.TAG`](https://bford.info/cachedir/), however, Time Machine doesn't honour that and instead needs different extended attributes to be set).

Thx for that. I'll add hook in fish shell to exclude `.venv` dirs as a temporary fix.
BTW. You can 'borrow' solution from cargo ;-) 

---

_Comment by @ParticleMon on 2025-02-28 15:00_

In my workflow, multiple projects may use one venv, and only project code is synced with a repo, not venvs.

Having come from conda/mamba, I would be content with uv supporting centralized projects, to:
* Create projects centrally
* List venvs from anywhere
* Activate and deactivate from anywhere (ideally deprecated but it's unclear how this would affect IDEs)

So, I wrote three scripts (batch files, one that runs a bash script—thanks to the inclusion of recent GNU coreutils in git). High-level, the scripts:
* Create - Takes arguments specifying the venv name, Python version number, and optional dependencies .txt and a sync flag.
* Activate - With no argument, lists venvs, the Python version, and if present, the project description; or, activates the specified venv.
* Deactivate - Takes no argument.

The bash script lists the venvs, parsing dirs and files.
The de/activate scripts eliminate having to `cd`.
Additional batch files are merely aliases ("act" and "deact," convenient but otherwise unnecessary).

[uv_utils.zip](https://github.com/user-attachments/files/19091704/uv_utils.zip)
For the scripts, set these environment vars:
uv=C:\path\to\uv-dirs-parent
uv_gnu=/mnt/c/path/to/uv-dirs-parent

---

_Comment by @eabase on 2025-02-28 21:22_

@ParticleMon 
> So, I wrote three scripts 

Well, where are they?


---

_Comment by @ParticleMon on 2025-03-03 14:21_

@eabase Attached.

---

_Comment by @mustafa0x on 2025-03-03 15:35_

Using mise, this should work.

`UV_PROJECT_ENVIRONMENT = "~/.cache/.envs/{{cwd | basename}}"`

---

_Comment by @eabase on 2025-03-06 01:10_

> Using mise, this should work.

Whats "mise"?


---

_Comment by @monk-time on 2025-03-06 01:32_

Could the discussion about alternative workarounds that's not connected with `uv` implementing the feature be moved someplace else? There's way too much noise on this ticket.

---

_Comment by @xmo-odoo on 2025-03-07 10:30_

>     * `uv activate` (because it becomes impractical to self-activate, except by copy-pasting the path that gets printed)

I've not seen any mention of it so I wanted to say that pyenv has a pretty neat (I think) feature for that: the "global" virtualenvs have the same status as "pure" python versions, so if `.python-version` contains a global venv name that venv will be auto-activated.


---

_Comment by @krstp on 2025-03-12 07:26_

Yeah, I tend to fall back to multiple venv with pyenv-venv, then whatever is needed I go with uv. This provides a stable and consistent dev solution and envs. For now that is the way.

---

_Referenced in [astral-sh/uv#12146](../../astral-sh/uv/issues/12146.md) on 2025-03-13 12:59_

---

_Comment by @lukeemhigh on 2025-03-15 03:25_

I stumbled upon this discussion while looking for a way to emulate my `pyenv`+`poetry` workflow.

I generally manage my virtual environments with `pyenv-virtualenv` because I like the simplicity of setting an environment variable (`PYENV_ROOT` in case of `pyenv`, but could be equally good to use `uv`'s config file) and have all my python versions and virtual environments centrally managed, and not scattered around my workspace.

Ultimately, I think an environment variable/config file would be a better fit for this feature; to me, having to specify a flag to `uv venv` at every environment creation is an undesirable step, more easily managed with a declarative approach.

---

_Comment by @krstp on 2025-03-15 12:21_

Imho, either config file or attribute file, both options would be good to have.

Here is also an interesting turn of things... in the past days I had to work with Win11 box... as unusable as Windows-box is, it made me install virtual Ubuntu shell via WSL...

Long story short, when installing uv under virtual shell, that by default runs bash changed to zsh, some strange reference base python thing happens that instead of seeing *nix python versions, it sees the underlying Win11 pythons that clearly make it unusable or with some gimmicks will allow Win11 layer pythons usage which is super inefficient.

The core issue was, whenever I tried to instantiate uv venv, uv would not find  the right python references, uv simply could not see and recognize the *nix layer pythons!

What did came with a rescue was pyenv, that carries correct local zsh references. So running pyenv and then uv for pip sync etc works just fine.

This also shows the inner-works of core uv for python env instantiation.. such as it runs on diff python references than follow up commands, which makes sense. However, relying only on uv under WSL-ubuntu for venv was not finalizing with success, by ingesting windows python references ¯\_(ツ)_/¯ or rather unable to even correctly ingest those producing an error, which also makes sense.

<strike>Atm, I am not even opening the issue.</strike> (https://github.com/astral-sh/uv/issues/12185)
This just shows uv runs better natively. And yes I did reinstall uv under WSL, it would still find win11 references. I just wanted to give context why combination of pyenv with uv might be better in such scenario, where pyenv-venv yields some more env stability. Also this is not the first time pyenv-venv comes to the rescue, I am mostly mac and *nix user, and whatever hurdles I was dealing with in those systems in local development, the virtual env layer with pyenv-venv came each time to the rescue. o7

---

_Referenced in [astral-sh/uv#12185](../../astral-sh/uv/issues/12185.md) on 2025-03-15 12:28_

---

_Comment by @zanieb on 2025-03-16 02:29_

Is this the same as

- https://github.com/astral-sh/uv/issues/10711
- https://github.com/astral-sh/uv/issues/11529
- https://github.com/astral-sh/uv/issues/4450

---

_Comment by @krstp on 2025-03-18 14:59_

Indeed, somewhat related. 

I cannot provide now detailed steps I have taken as I was rushing through setup, but through multiple resourcing of the env, now I am able to activate uv created env. Either way, it seems to be more of an issue of virtualized system layer python reference than uv itself.

Thanks for the follow up.

---

_Comment by @ssbarnea on 2025-03-18 15:07_

> Another usecase: Some of my colleagues work mainly on network drives, however Python is really slow loading a huge env from a network drive. Poetry is currently a good fit, as the default location of the venv is somewhere on the local SSD which speeds up loading significantly.

Very similar use case: use of VMs to perform cross-platform compatibility. I use Parallels on MacOS and have VMs with Ubuntu, Fedora and Windows where I run tests on the same code (code folder is mounted on these.

We use tox for testing which creates by default `.tox` temp directory inside the project, but because for can also allow users to override the default via env variables, `TOX_DIR=.tox/$(hostname)`, we found a way to isolate these between machines. Feel free to use this as a workaround as tox-uv also adds support for uv.

Still, if you just want to use uv outside tox on different platforms, we need this bug fixed.

---

_Referenced in [astral-sh/uv#12358](../../astral-sh/uv/issues/12358.md) on 2025-03-21 08:33_

---

_Referenced in [astral-sh/uv#12363](../../astral-sh/uv/issues/12363.md) on 2025-03-21 12:35_

---

_Comment by @jnakanojp on 2025-03-29 03:16_

My workaround is to create a set_env.sh file and source it before using uv.
The contents of set_env.sh are as follows:
```
#!/bin/sh
export UV_PROJECT_ENVIRONMENT=~/.local/uv/my_special_secret_project
```

Before using uv, I run the following command to load the environment variable:

`. set_env.sh`

Since this step is manual, I sometimes forget to run it, and as a result, .venv may be created by mistake.
It might be possible to automate this process by using a tool like direnv.

---

_Referenced in [astral-sh/uv#12587](../../astral-sh/uv/issues/12587.md) on 2025-04-01 20:45_

---

_Referenced in [astral-sh/uv#12740](../../astral-sh/uv/issues/12740.md) on 2025-04-08 15:37_

---

_Referenced in [astral-sh/uv#12821](../../astral-sh/uv/issues/12821.md) on 2025-04-11 15:00_

---

_Referenced in [astral-sh/uv#1703](../../astral-sh/uv/issues/1703.md) on 2025-04-21 03:00_

---

_Comment by @Hawk777 on 2025-05-10 20:49_

Adding my own use case for this feature, since it hasn’t been mentioned yet—similar, but not identical, to some mentioned. My home directory is btrfs and I use [snapper](http://snapper.io/) to take periodic snapshots. I don’t want to waste disk space saving snapshots of cache, so I exclude that. Snapshots in btrfs operate at subvolume granularity, so while `CACHEDIR.TAG` is a nice thing to have for backup programs, it doesn’t (and can’t!) affect subvolume snapshotting. So, `~/.cache` is a separate subvolume and I would like my venvs in there. `UV_PROJECT_ENVIRONMENT` isn’t a great solution because I still want a separate venv for each project, not just one venv for everything, and there’s no mode of that variable for “absolute path, but append the project {name,path,hash,something} to get the actual venv dir”. Currently I symlink `.venv` to per-project directores in `~/.cache`, but that’s not ideal because (1) it would be nice if I could tell UV that’s where I want my venvs *once* and have it remember that, rather than doing it once for each project; and (2) every time I change Python interpreter version, UV deletes and recreates the venv, which in this case means it deletes the symlink and then creates a regular directory.

Given how fast UV is, using `UV_PROJECT_ENVIRONMENT`, pointing it at one directory, and just letting UV destroy and recreate the entire venv every time I switch projects might actually be not the worst option, but it still seems a bit silly (and obviously doesn’t work if I want to run two projects at the same time).

---

_Referenced in [astral-sh/uv#13457](../../astral-sh/uv/issues/13457.md) on 2025-05-14 21:47_

---

_Referenced in [astral-sh/uv#11315](../../astral-sh/uv/issues/11315.md) on 2025-05-18 18:25_

---

_Referenced in [astral-sh/uv#13594](../../astral-sh/uv/issues/13594.md) on 2025-05-22 18:33_

---

_Comment by @chriscomeau79 on 2025-05-23 17:00_

On Windows, this would help reduce difficulty with OneDrive as well

---

_Comment by @lgaborini on 2025-05-26 07:49_

If one is working with company policies and aggressive anti-viruses, it's also easier to whitelist a centralized repository instead of each project separately (potentially requiring asking for admin permissions each time).   
uv is so cool, but it always gets randomly blocked by Windows Defender...

---

_Comment by @bdvllrs on 2025-05-26 08:34_

We have a similar use case to @PhilipVinc where clusters useually don't provide a lot of disk space for the working directory. This feature would be very useful!

For now, I've made this wrapper that stores venv in a central store by automatically setting UV_PROJECT_ENVIRONMENT depending on your PWD: https://gist.github.com/bdvllrs/11baa227a41dd45fe866d9f56ccae50c

Set `PUV_VENV_LOCATION` to set where to store venvs.

Caveats: 
- for speed, it will create a `.venv-name` file in the project root directory with the name of the virtual-env. Add this to your `.gitignore`.
- it works with your PWD, so you need to be in a folder of your project to use it (does not work with uv's `--project` option)

---

_Comment by @PhilipVinc on 2025-05-26 08:43_

For the record, my team and I have been using for the past few months a wrapper similar to @bdvllrs 's

It was coded by ChatGPT, in a few minutes, and we called it fuv (https://gist.github.com/PhilipVinc/ede9711b752c83dd908226d448d56a73 ).
It requires setting an 'UV_DEPOT' path.

This wrapper works decently for our use case of working on large HPC clusters, allowing us to store the environments in the fast scratch file system, but it leads the occasional hassle , especially when onboarding new users.


I will reiterate to uv's developer that by not supporting a mechanism to systematically store .venv in a different location, they are making uv unsuitable for HPC computing, a field that would benefit of improved reproducibility.



---

_Comment by @Hawk777 on 2025-05-26 16:13_

I ended up working around it with this Bash function:
```bash
# Set up UV to use a venv directory in ~/.cache/uv-venv, based on the project
# root directory or the current working directory if no project can be found.
uv() {
	local uv_project_root=$(/usr/bin/uv -v python find 2>&1 >/dev/null | sed -ne '/Found project root: `/{s/^.*Found project root: `\(.*\)`$/\1/;p}')
	if [[ -n $uv_project_root ]]; then
		export UV_PROJECT_ENVIRONMENT=~/.cache/uv-venv/$uv_project_root
	else
		export UV_PROJECT_ENVIRONMENT=~/.cache/uv-venv/$PWD
	fi
	/usr/bin/uv "$@"
}
```

---

_Comment by @ktrushin on 2025-05-31 15:15_

@Hawk777, thank you for the recipe!
The only thing I would change there is using `command uv` instead of hardcoded `/usr/bin/uv` in order to accommodate different installation paths.

---

_Comment by @Hawk777 on 2025-05-31 15:27_

@ktrushin very nice, I didn’t know about `command` (and what a heck of a thing to search for in the `bash` man page…).

---

_Referenced in [dagster-io/dagster#30517](../../dagster-io/dagster/pulls/30517.md) on 2025-06-06 23:08_

---

_Comment by @sascharo on 2025-06-18 07:28_

> I use Windows/Linux/Mac every day and synchronise my projects using OneDrive, having `.venv` in my project folder is such a nightmare

Absolutely, even when not using OneDrive.

uv still doesn't support venvs in a global location?

---

_Comment by @zanieb on 2025-06-18 15:50_

> Absolutely, even when not using OneDrive.

Could you expand on why this is problematic for your use-case then?

Let's keep comments here focused on new information. If you just want to see the feature, 👍 the op instead of notifying all the subscribers. 


---

_Comment by @eabase on 2025-06-19 14:26_

@zanieb

> Let's keep comments here focused on new information.

Sure, but given that the topic is nearly 1.5 years old and that you closed several similar topics without any reason and/or discussion, I think it's a great thread above to discuss and find alternative solutions until you guys actually manage to implement one. There are a lot of great comments above. 

Also, what do you mean with "new information"? (How are we to know what is "new" to you?)


---

_Comment by @notatallshaw on 2025-06-19 14:51_

> I think it's a great thread above to discuss and find alternative solutions until you guys actually manage to implement one.

No, if you have alternative ideas please propose them on a new issue, when one posts here it probably notifies thousands of people. I am subscribed  for when this is eventually implemented or rejected, not for uv design discussions.

> Also, what do you mean with "new information"? (How are we to know what is "new" to you?)

The post being replied to (https://github.com/astral-sh/uv/issues/1495#issuecomment-2983015007) didn't contain any information on their use case of "not using OneDrive", so a prerequisite of new information is to have *any* information.

Then to see if what you're providing is new information please read through the issue and see if your use case is novel, and if not don't post, just thumbs up. If reading through issue is too high of a barrier for someone, than please also don't post and just thumbs up. 

I am not meaning to start a discussion here, I won't be following up or replying, I would rather uv devs spent their time on developing uv, not moderating discussions. 

---

_Comment by @zanieb on 2025-06-19 16:26_

> I think it's a great thread above to discuss and find alternative solutions until you guys actually manage to implement one. There are a lot of great comments above.

I don't mind people posting their alternative solutions, I agree that's helpful for the community. I think you're interpreting my comment as applying to far more than the one I was replying to.

> Also, what do you mean with "new information"?

I mean information not captured in the thread already, e.g., use-cases or motivations we're not considering. I think given the length of the thread, there really isn't a lot of new information to contribute. This was really just intended to be a constructive way to say: please don't ping the thread to say "I want this feature". We're not that strict about moderating replies, but as Damian has pointed out, each reply notifies a bunch of subscribers and we want people to be cognizant of that.

This feature is still on our roadmap; we're a small team managing thousands of questions, bug reports, and feature requests.


---

_Referenced in [nf-core/tools#3420](../../nf-core/tools/issues/3420.md) on 2025-06-30 03:12_

---

_Comment by @rsyring on 2025-07-15 00:36_

@zanieb is this something uv would consider an MVP (minimal viable product) contribution for?  

As the OP points out, there are some separate features (e.g. `uv activate`) that complicate getting this feature shipped with "full" support.

But it seems to me only a few things are needed for MVP support:

- Uv setting/environment variable indicating a preference for centralized venv location
- Adjustments to uv to use said location, which I'm assuming the "plumbing" is mostly there for given it's going to be a similar override to `UV_PROJECT_ENVIRONMENT`
- An option to show the location of the venv, e.g. `uv venv --dir`, so other tooling can use that for activating the virtualenv
- Maybe: venv deletion and/or making venv cleanup an option under `uv cache` although I'd argue this isn't strictly required for an MVP

FWIW, my use case is mostly concerned with better mise integration and documented at https://github.com/jdx/mise/discussions/4301.

Thanks.

---

_Comment by @zanieb on 2025-07-15 13:14_

Yeah I'd accept a contribution for this, but the challenging work is in the design — the implementation will be pretty straight-forward. We'll probably want to be on the same page before you implement anything.

I think we need some story for environment clean-up. I'm not sure it fits into `uv cache`, unless we store these in the cache by default? `uv cache prune` does sound nice for removing environments that don't map to an existing project anymore.

A minimal design here may just be an environment variable / setting that toggles between using the project directory and a cache bucket. Then, we could later add customization of the path for the cache bucket.

We'll also need to story to retain IDE discovery of environments, which is challenging. Maybe we can get away with a symlink in the project directory on Linux. On Windows, it's less obvious. There's a proposal floating around for a `.venv` file with an path to the environment, but I don't know if VS Code would support that — we might need to do some work there to ensure it supports what we need.

Are you expecting this would only apply to project environments, or all `uv venv` invocations?

---

_Comment by @edmorley on 2025-07-15 13:24_

Apologies if I've missed it in the earlier comments, but it seems that another open question is how uv might name the venv directory within the central venv location? 

It seems uv could either:
 
1. Use some kind of hash of attributes of the project (eg the path of the directory containing the `pyproject.toml`) for a consistent name
2. Or, generate a random directory name at initial creation, and discover that for future operations via the `.venv` symlink (or `.venv` file containing a path) mechanism that IDEs will be using for discovery
3. (Something else)

For the most part both (1) and (2) will operate the same, but I can see some subtle differences. For example, (1) leaves behind an orphaned venv if the project directory is moved. And (2) leaves behind an orphaned venv if someone cleans up the `.venv` file/symlink within the project directory.


---

_Comment by @zanieb on 2025-07-15 13:27_

I think we'd probably use (1) and some pruning mechanism to remove orphaned environments. If we're using a `.venv` marker in the project, we might be able to transparently rename the cached environment when the project moves, but that could be dangerous, i.e., if another project has been created at that path and is using the environment.

---

_Comment by @zanieb on 2025-07-15 13:30_

We could also use junctions on Windows, though they cause some weird behavior at times. The main limitation is that they won't support cross-file system environment paths which I think are a major motivation for this feature.

---

_Comment by @rsyring on 2025-07-15 14:39_

> Yeah I'd accept a contribution for this, but the challenging work is in the design — the implementation will be pretty straight-forward. We'll probably want to be on the same page before you implement anything.

Understood.

- You mention needing to "story." Is that a formal process you have or do you just mean working through the details in general?  
- Do you have a different place/process for working through design decisions or should we just keep that going here?
  - Might be better to start a spec document and put up a PR for it?  That way we can iterate on the document with version history and also discuss various things?

## Spec Proposal

I'm going to focus on a truly minimal MVP, which will drive my thinking on the design.

1. Application
   - Only applies to project based venvs
   - Does NOT apply to `uv venv` invocations where a PATH is provided
1. venv storage location:
   - a cache bucket, e.g. `$XDG_CACHE_HOME/uv/venvs-v0`
   - customizable: no
1. Trigger centralized venv storage when:
   - `UV_VENVS_IN_CACHE`: is set to any value;
   - OR `venvs-in-cache` setting is `true`
1. venv dir name calculation
   - [ChatGTP input on this question](https://chatgpt.com/share/68766176-ca84-8010-84a3-b8a3d21f4b32), I'm following it's recommendation
   - Human‑readable slug + short hash (pseudocode below)
   - `slug = slugify(basename(project_root))`
   - `hash = sha256(project_root)[0:8]`
   - `venv_dir = $XDG_CACHE_HOME/uv/venvs-v0/{slug}-{hash}`
   - One CON of this method is that, without additional work, there is no way to look at the venv and find the location of it's project.  This affects cache cleanup (see below).
     - I believe this is an acceptable CON as a future enhancement could place a file in the venv indicating the path to the project
1. Add `uv venv --dir` to output full path to venv location, e.g. continuing from the above pseudocode: `print(venv_dir)`
1. Cache cleanup
   - `uv cache clean` removes all venvs
   - Defer "smarter" mechanisms for cache cleaning to post-MVP.  E.g. `uv cache prune` could remove venvs whose projects are missing.
1. IDE Discovery method
   - Assuming users that want centralized venvs will be responsible for configuring their IDE for the location of the virtualenv
   - FWIW, I do this by by activating the venv first and then `code .` in the project directory.  VS Code picks up the correct location from `$VIRTUAL_ENV` and, I'm assuming, a lot of other IDEs will as well.
   - Other methods for IDE discovery deferred until those mechanisms (e.g. path in .venv file) are finalized/working

Defer all other considerations to post-MVP.

Comment/critique welcome.  :)

---

_Comment by @zanieb on 2025-07-15 14:43_

> You mention needing to "story." Is that a formal process you have or do you just mean working through the details in general?

No that's not a formal process, just that we should have a coherent pitch for how we want it to work.

> Do you have a different place/process for working through design decisions or should we just keep that going here?

It's probably best to move it out of this thread to avoid notifying people. We don't do a lot of design collaboration with external stakeholders. Feel free to put up a pull request creating an arbitrary document, we can iterate on that.

---

_Referenced in [astral-sh/uv#14628](../../astral-sh/uv/pulls/14628.md) on 2025-07-15 14:55_

---

_Comment by @rsyring on 2025-07-15 14:56_

Design doc PR is up at: https://github.com/astral-sh/uv/pull/14628

---

_Comment by @eabase on 2025-07-16 17:12_

@zanieb *

> We could also use junctions on Windows,

No, you really don't want to use junctions. Junctions are sketchy AF and very difficult to correctly maintain or remove, for most windows users. 

@rsyring 

> (6) Cache cleanup:
> `uv cache clean` removes all venvs

This sounds very bad, and I can imagine some smart ass installer trying to use this in automation and accidentally remove all venvs. I don't see why this is needed at all? 

For example, I keep old venvs for future projects I know may needed those particular packages, and can then simply copy and rename the `/path/to/venvs/myname` to `/path/to/venvs/myname2` and have it work instantly, in any project pointing to it.

TBH, I don't understand why you need to control the name of the created venv's, that has always been controlled by the user? 

The whole point of a a separate venv directory/location is that several project can use the same venv, and is easily portable and independent from the `$HOME`. **AND** that you can easily list all available venvs.

---

For example, I have the following powershell command to list all envs and their status. 

<img width="706" height="343" alt="Image" src="https://github.com/user-attachments/assets/f6d0d383-a05e-4d60-afa1-043d39936804" />

To remove a venv, I just `cd C:\venv\` and remove the correspondingly named sub-directory.


---

_Referenced in [getpelican/pelican#3492](../../getpelican/pelican/issues/3492.md) on 2025-07-22 06:09_

---

_Referenced in [astral-sh/uv#15163](../../astral-sh/uv/issues/15163.md) on 2025-08-08 09:56_

---

_Referenced in [astral-sh/uv#14515](../../astral-sh/uv/issues/14515.md) on 2025-08-19 15:38_

---

_Referenced in [astral-sh/uv#14937](../../astral-sh/uv/pulls/14937.md) on 2025-08-26 13:02_

---

_Comment by @stuaxo on 2025-08-28 11:15_

Re: IDE discovery of virtualenvs, Pycharm and others seem to know about the ~/.virtualenvs directory.

When I moved to centralised virtualenvs, this was a huge benefit as I could find and delete my old ones easily, before that I had many of them scattered in all sorts of project directories.

---

_Comment by @eabase on 2025-08-29 21:14_

@rsyring 
I reviewed your [Design doc PR](https://github.com/astral-sh/uv/pull/14628) and left my comments of concern there. 

What is the current state of this topic?

`to all:`
I'm also surprised nobody else commented there... Please do so, if this is of any importance to you!




---

_Comment by @xmatthias on 2025-08-30 08:41_

To be honest, i think 210 thumbs up on the initial post are more useful than everyone posting "i'd want / need this, too" -which adds nothing of value - just noise.

---

This is the point/issue preventing me from switching from pyenv. Having a venv per directory (the default) will be useful for some cases for sure - but if you work with big venvs (several G of install size, thanks to torch) - that becomes unreasonable.
pyenv allows for automatic activation and storage of the venv's in a central place for reusability - and i think uv should provide similar functionality to be a fully-fledged replacement for it.

I still have the possibility to use a local venv - but i can easily reuse "global" virtual environments, too, which greatly helps with limited disk space (10 environments with full ML dependencies are quite "sizy" ...)



---

_Comment by @rsyring on 2025-08-30 13:22_

> but if you work with big venvs (several G of install size, thanks to torch) - that becomes unreasonable.
pyenv allows for automatic activation and storage of the venv's in a central place for reusability - and i think uv should provide similar functionality to be a fully-fledged replacement for it.

FWIW, my [proposal doc PR](https://github.com/astral-sh/uv/pull/14628) considers but rejects venv re-usability as a goal as it brings up some issues that will make the first iteration more complex and my goal was to keep the proposal simple in the hope that it would more easily get accepted and implemented.  

But, I understand the use case, I just don't understand how it works in practice.  Would you have multiple projects actually controlling the venv?  And then you have to keep their dependencies in `pyproject.toml` in-sync?  Or would you have one `pyproject.toml` somewhere that controls the venv and then [UV_PROJECT_ENVIRONMENT](https://docs.astral.sh/uv/reference/environment/#uv_project_environment) is set for the other projects?  Or something else?

Or, asking a different way, at a higher level: in a more traditional Python dependency management, you are manually updating the venv with pip when needed.  Maybe using a requirements file that may or may not have versions in it.  But in `uv`, the central benefit comes when you start using `pyproject.toml` for dependencies and let uv manage the venv and it's packages.  That's a very project centric paradigm because the dependencies are installed in pyproject.toml which "belongs" to a specific project.  And uv is providing tools like `uv add` that operate on the venv and the pyproject.toml.  I don't currently see how this paradigm works with shared venvs.  

---

_Comment by @xmatthias on 2025-08-30 13:53_

I don't use `uv` for dependency management (and i currently  don't plan to start on doing so either - as that will move away from the current python standard in my opinion).
It's a mistake / shame in my eyes that uv tries to re-invent python packaging in this sense (ignoring standards like `requirements.txt`, apparently - at the same time making the same mistake that poetry made (IMHO)) - but this can be worked around pretty easily - but is a topic for another discussion.

---

Instead, i try to follow the python standard approach for applications - which means a `requirements.txt` file (multiple, for dev, ...) for CI, to run with fixed dependencies
`pyproject.toml`  with loosely pinned dependencies.

with pyenv, i then can do `pyenv virtualenv 3.13 somename` - followed by `pyenv local somename` to activate it (persistently, for this directory - by writing a `.python-version` file). 
This will re-activate the environment whenever i use this directory (i can obviously do the same with other directories - using `somename` there, too).
I don't necessarily always care about "this exact" version of dependencies - as i work on one main project, and most other projects (and worktrees, for that matter) have aligned dependencies (or a subset of these) - and canin turn reuse this environment. 

The benefit of this is - i have for example torch (and a couple other huge dependencies) installed once (per python version) on disk (in total, the venv directory is 8.3G per install) - instead of once per directory - which in my current setup - would be about 4-6 times - which i really, REALLY see as a huge waste of memory - and i don't think UV has a way to fix at the moment (for example in a similar way to how pnpm does for node by symlinking to a common cache - which oddly also works on windows - but was rejected by your document for some reason).


In turn - it's not "projects" that control the environment - but myself - by running `pip install xxx` (either `<dependency>`, or `-r requirements.txt`).

---

> FWIW, my https://github.com/astral-sh/uv/pull/14628 considers but rejects venv re-usability as a goal as it brings up some issues that will make the first iteration more complex and my goal was to keep the proposal simple in the hope that it would more easily get accepted and implemented.

I understand your approach for this (though your goal seems amiss, as astral folks don't seem to care about your proposal - which is a shame in it's own - independent of my opinion of it) - but that means it's of no use to me probably (not in it's current proposal state) - and in my opinion, will also not address this issue - at least not in it's entirety - as it's simply moving the virtual environment from A to B - but won't actually replicate the `pyenv virtualenv` / `pyenv locale` functionality (at least that's my understanding). 

---

_Comment by @rsyring on 2025-08-30 14:10_

@xmatthias thanks for your comments.  I appreciate the description you gave of your process and it's close to what I was thinking it would be.  

IMO, we should explicitly acknowledge that sharing venvs is currently not aligned with uv's project-based paradigms and out-of-scope for this issue.  That will, I hope, keep this discussion focused on per-project centralized venvs.  Different issues/discussions can be used for how uv can/should support shared venvs.

> though your goal seems amiss, as astral folks don't seem to care about your proposal

With 1.9K open issues, I'm sure they have to triage what they care about and I don't take it personally.  :)

---

FWIW, some of your workflow is already supported by uv's separate commands:

- `uv venv` to create your venv
- `uv pip` instead of just `pip` to install dependencies into the venv
- I don't think uv activates venvs yet.  I personally use mise for that as well as for knowing which venv to use with my project.

It won't be quite as automatic as you are used to, and you might have to wire a couple things together, but you'd benefit immensely from the speed improvements uv provides.  

Anyway, HTH.

---

_Comment by @xmatthias on 2025-08-30 14:19_

> IMO, we should explicitly acknowledge that sharing venvs is currently not aligned with uv's project-based paradigms and out-of-scope for this issue. That will, I hope, keep this discussion focused on per-project centralized venvs. Different issues/discussions can be used for how uv can/should support shared venvs.

If you look through linked issues - i think most if not all issues related to that have been closed as duplicate to this issue - so i tend to disagree with you on this.
A central option to store virtual environments is completely pointless in my eyes if it doesn't also allow the sharing of this directory from different files.

> It won't be quite as automatic as you are used to, and you might have to wire a couple things together, but you'd benefit immensely from the speed improvements uv provides.

Well unfortunately not - as i don't reinstall my environment every other day.
It's a great enhancement on install - but that's about it (at least for my current workflow) - so i'll benefit about it once. Python won't run faster because it's installed or managed through UV - nor will my programs (correct me if i'm wrong - either would be great news for me).
if i need x new tools for that - it's a step back instead of a step forward - especially if (as it currently is) it'll mean that i have 5 parallel virtual environments on my disk.

---

_Comment by @mil-ad on 2025-08-30 14:27_

>  A central option to store virtual environments is completely pointless in my eyes if it doesn't also allow the sharing of this directory from different files.

That is not pointless. It would address the issue where the project is on a network drive and you'd want the venvs to be on local disk

---

_Comment by @rsyring on 2025-08-30 14:32_

>  A central option to store virtual environments is completely pointless in my eyes if it doesn't also allow the sharing of this directory from different files.

There are a lot of reasons for a centralized venv being useful, even without shared venvs.  If there wasn't, this issue wouldn't have so many thumbs up.  :)

---

Regarding the shared venvs discussion, we might be missing the forest for the trees.  uv already uses techniques to limit disk usage by venvs sharing the same dependencies:

- https://github.com/astral-sh/uv/issues/11504
- https://docs.astral.sh/uv/reference/settings/#link-mode

This seems like it would negate the primary need for shared venvs in most use cases?

---

_Comment by @xmatthias on 2025-08-30 14:54_

> That is not pointless. It would address the issue where the project is on a network drive and you'd want the venvs to be local disk

That's a good point - i should change my above answer to "mostly pointless" - as there's edge-cases where it's still useful.

> There are a lot of reasons for a centralized venv being useful, even without shared venvs. If there wasn't, this issue wouldn't have so many thumbs up. :)

Thumbs up don't necessarily consider the _limitations_ the acutal implementation idea brings.
Looking at the first comment (the one with the tumbs up) - there's no mention of "central location but can't be shared" or anything simlar to this.

I think it'd have far less if this were mentioned explicitly (it wouldn't have had mine, for one).

---

> Regarding the shared venvs discussion, we might be missing the forest for the trees. uv already uses techniques to limit disk usage by venvs sharing the same dependencies:

That's true enough - it seems like i forgot about this (funnily, i remember having read about it a while ago).

---

it still doesn't really adress the need for shared venvs in my eyes (as they're shared, they should be in a central location, not live in one random project).
That doesn't replace local venvs where you only treat one project to.

If you develop 3 packages that depend on each other - you'll want to have the develop version of all 3 of them installed (as linked packages, really) - not 3 separate venvs.
That's what a central location is (also) about - not just "where the data lives".

---

_Comment by @eabase on 2025-08-30 15:31_

@rsyring 
> But, I understand the use case, I just don't understand how it works in practice. Would you have multiple projects actually controlling the venv? And then you have to keep their dependencies in pyproject.toml in-sync? Or would you have one pyproject.toml somewhere that controls the venv and...

I'm starting to see where you're going. But it's a bit concerning for the very same reasons already mentioned by @xmatthias. 

At the moment I would not dare to use uv to *create, activate*, or *manage* any of venv's, because they all work now, as I like it. However, the issue (for everyone) will always be how to track them. Because early python venv developer failed miserably in implementing a mechanism to track and manage venvs. (Just as they broke and failed to provide a command line tool to search for packages.) Mine now works exactly as I like it, but only because I had to install **[venvlink](https://github.com/fohrloop/venvlink)** and  create my own scripts to track them and provide me with their project connections and status. (As shown in my post further up in this thread above.)

**`So the major part of the centralized venv issues have already been solved 5 years ago!`** 

What was missing is the tracking of the venv's themselves. But nobody gave AF until now, when huge terabyte sized AI, LLM and Torch installation packages & requirements started appearing.

I suggest you have a look at this: 
* https://github.com/fohrloop/venvlink/issues/9

So what I am doing these days is:
- create project directory (or git clone, etc) 
- use **venvlink** to create venv's using:  
  `python -m venvlink -S <venvname>`
- `activate` to start venv
- use uv to install needed packages
- use my PS1 script (easily converted to Bash) to check status of venvs

<img width="954" height="282" alt="Image" src="https://github.com/user-attachments/assets/c60fd9dc-1560-4faa-98c0-ab6b34385cbb" />

---

**Q**: *So how can `uv` do something better?*  
**A**: They can take venvlink`*` and implement it with my scripts to unify UX and functionality described above.

<sub> `venvlink` is no longer updated, and maintenance status is unknown. However, it's been working just perfect for the last 5 years.</sub>


---

> Would you have multiple projects actually controlling the venv?

Yes. And if the user understand the basics of venvs, they will also be able to understand when they can be used and mixed, and when not. It is also very easy to copy one venv to another and modify it in the new venv, if something else need to change. 

> And then you have to keep their dependencies in pyproject.toml in-sync?

I see you want some kind of automation here, and that's fine, but then the uv venv *"manager"* need to inform the user what will happen to any other dependencies. So the answer is probably **no**.

 

---

_Referenced in [fohrloop/venvlink#11](../../fohrloop/venvlink/issues/11.md) on 2025-08-30 15:57_

---

_Comment by @kompre on 2025-08-30 17:04_

> > That is not pointless. It would address the issue where the project is on a network drive and you'd want the venvs to be local disk
> 
> That's a good point - i should change my above answer to "mostly pointless" - as there's edge-cases where it's still useful.

One person edge case is another person main case.

For example I want to store my venv in another location rather the project folder because in my case python is just one of the tools I use for my overall project, therefore the git paradigm that works well for developing a package, would not work for me because the projects are stored on a network location, accessible by different users on different machines.

During the year I may open 30 or 40 different projects, that may be active concurrently for many months. Those projects have all similar requirements, but I don't care about reusing venv, I actually prefer that they have each their own separate venv, because they may have different version requirements of some package solely based on the time of the year I initiated the project.

Having the venv folder in the project folder is a pain of the project reside on a network location because the copying of file is limited by lan speed, whereas on the local machine uv feels a instant. If have to access a old and inactive project just for reference, having to recreate a venv becomes a burden on network, while if the venv is stored on local machine, it would be almost instant, and then I could proceed to forget it.

Most of the use case I described is solved by poetry. The only issue I have with poetry now is that it is slow to create a new venv with chunky packages required, and it doesn't manage python version. Having to install a python version is something I do so rarely that I always forgot how to do and need to relearn each time.

So what I personally wish for this feature? To let uv do its magic, but on a different folder on my local machine. 

---

_Comment by @Hawk777 on 2025-08-30 17:38_

It seems to me that there are two axes here, somewhat but not completely independent from each other:
1. Where, physically, in the filesystem, does the venv live?
2. Is the venv a discardable derived artifact (which is derived from `pyproject.toml` and `uv.lock` and which, when those are present, can be regenerated at the cost of a bit of CPU time and maybe network bandwidth but no human effort), or is it a human-maintained, handcrafted artifact into which you manually install the packages you want for a particular purpose?

Most of uv is built around the first perspective on (2). Lots of people like that perspective—not everyone, but lots of people, myself included. This feature request, unless I’m seriously misunderstanding, is to change (1) without touching (2), and from what I can tell, the proposed design does that in a way that would satisfy most people’s needs in terms of (1). Furthermore, putting the venvs in a cache location makes perfect sense given the first perspective on (2), because that’s what they are in that perspective: the cost of deleting a venv is that the next `uv sync` (or other command that implies it) takes longer, but involves no extra human effort.

@eabase if I understand correctly, the first perspective on (2) doesn’t do what you want, and you want the second perspective (human-maintained, handcrafted venvs). Given that, it makes sense you’d also want to manually control where the venvs live—if you’re managing their contents by hand, then you should also be able to manage their locations by hand, and they’re definitely *not* cache. It seems to me that, if you wanted to use uv, you probably want to use a workflow along the lines of `uv venv` to create a venv wherever you want (you get to choose), activate the venv, and then `uv pip` to install things into it. I haven’t personally used it, but according to the docs, that workflow already exists and already lets you control the location of the venv, including putting it somewhere where you can share it between projects. If you want, I would presume you can symlink `.venv` in a project dir to the shared venv you want to use with that project, and then skip manual activation in many cases. All that said, it also seems like you might not get much benefit from using uv compared to other tools, since you’re looking for a perspective that doesn’t align with how most of uv is built (it seems like the necessary pieces are there, I’m just not sure they’d provide much benefit compared to other tools).

So, what’s missing? It seems to me that the proposed design meets a bunch of people’s needs (including mine). Is there a workflow it would actively harm? That seems unlikely, since the whole thing is controlled by a setting that people don’t have to turn on if they don’t want to.

I am, of course, nobody special, not a member of Astral, so take everything I said with a big grain of salt.

---

_Comment by @xmatthias on 2025-08-30 17:49_

@Hawk777 
I'd not see the 2nd uescase as "handcrafted" venv - already using `requirements.txt` (with pinned dependencies, whereas pyproject.toml's are losely pinned) is not really aligned with uv's approach of "i want to control and maintain everything".

so what you're proposing for the 2nd usecase would be to use `uv venv ~/.venvs/Myvnvname`, which then obviously would have to be activated accordingly "somehow"?
in principle / theory - this will also work with the proposal i guess - as you'll simply set "~/.venvs" as storage location, you'd just need to specify a name (`uv venv myvnvname` - which then stores it in `~/.venvs/myvenvname`) ? 

---

_Comment by @Hawk777 on 2025-08-30 18:17_

@xmatthias Ah, I missed that intermediate point on the spectrum. Do I understand that you *do* want your venvs to be derived artifacts, but you want them to be derived from `requirements.txt` instead of `uv.lock`? You mentioned in one of your previous messages that your reason for doing that is because it’s more Python-standard. That in turn means that uv won’t be able to automatically create your venvs, and you would have to create them yourself.

So, is your reason for wanting to share venvs, that you don’t want to have to run `uv pip install -r requirements.txt` half a dozen times in different project directories to get all their venvs going, when the venvs should all be the same? If so, could you solve that by keeping the proposed “venvs in cache” option turned off, and instead making `.venv` in each project a symlink to a single shared venv? That would be a small bit of manual setup, but it seems like it’s manual setup that *has* to happen—there’s no way uv could magically know which of your projects ought to share a single venv and which ought to get their own.

---

_Comment by @xmatthias on 2025-08-31 06:23_

> That in turn means that uv won’t be able to automatically create your venvs, and you would have to create them yourself.

well - run ing `uv venv && uv pip install ...` is not quite the big task - and i see little to no difference to `uv sync` or similar.

> So, is your reason for wanting to share venvs, that you don’t want to have to run uv pip install -r requirements.txt half a dozen times in different project directories to get all their venvs going, when the venvs should all be the same?

not really - it's that i want it to be a linked install (from several projects).
so Project A depends on ProjectB and ProjectC - so i want 1 venv with all 3 projects installed as linked install - so modifications to either of the projects can be used immediately in the other projects.
This shouldn't / can't go to pyproject or requirements - as that's not something that should be commited / pushed.

symlinking would work in theory - but it's still odd and i don't think that will work without problems.

It's a workflow that pyenv has already - and which i think works well and has several advantages, too.
you run `pyenv venv <somevenv>` - and then `pyenv local <someenv>` (so it writes the `.python-version` file).
this in turn auto-activates the environment - and all X projects/folders i decide to do so.

it's clear that uv can't know which ones to combine - but i don't think uv _should_ do this automatic for me, either.
Right now, it's however patronizing  me to "you must use one venv per project" - taking this possibility away from me explicitly (while there may be workarounds, they remain workarounds which can break at any moment - as they're not a supported approach).

---

_Comment by @Hawk777 on 2025-08-31 07:02_

Ah, I see.

> symlinking would work in theory - but it's still odd and i don't think that will work without problems.

Well, up to you, but it *does* work. There’s [an earlier comment](https://github.com/astral-sh/uv/issues/1495#issuecomment-1950442191) saying as much. I think I’ve also done it myself a few times without any trouble. I don’t see it as odd at all, personally. A symlink is a perfectly normal way to move a directory from where it’s expected to be found, to a different place where you want it to live.

>  Right now, it's however patronizing me to "you must use one venv per project" - taking this possibility away from me explicitly (while there may be workarounds, they remain workarounds which can break at any moment - as they're not a supported approach).

I’m definitely not saying “more tooling for people who want more control over their venvs” is a bad thing. I just don’t see why *this* request should get delayed simply because it’s not that.

---

_Comment by @xmatthias on 2025-08-31 08:07_

> I’m definitely not saying “more tooling for people who want more control over their venvs” is a bad thing. I just don’t see why this request should get delayed simply because it’s not that.

I'm not saying the request should be delayed - but i don't think the current proposal fully closes this issue.

If you carefully read the title - it says "Add an option to store virtual environments in a centralized location outside projects"
the "outside projects" implies to me that the 1:1 relation between venv and project is removed.
The proposal explicitly excludes that (no shared environments) - which for me means - it's a proposal moving _towards_ the closure of this issue - but only half-way, explicitly excluding the other half.

---

_Comment by @zanieb on 2025-09-03 14:29_

Briefly...

> the "outside projects" implies to me that the 1:1 relation between venv and project is removed.

I don't think this is implied by the title of the issue nor is it something I've considered a goal here. I appreciate you sharing your perspective, but if you want to discuss this topic further, please open a new issue dedicated to it — you're notifying a lot of people on this thread.

---

_Comment by @eabase on 2025-09-04 09:55_

@Hawk777 
Hi Christopher,
Sorry for late reply. I was out for a few days. 

It seem that you have understood some of our use case correctly. 

However, regarding your 2nd "axis":

> Is the venv a discardable derived artifact (which is derived from pyproject.toml and uv.lock and which, when those are present, can be regenerated at the cost of a bit of CPU time and maybe network bandwidth but no human effort), or is it a human-maintained, handcrafted artifact into which you manually install the packages you want for a particular purpose?

I think some of you is totally missing our (me + @xmatthias) point, possibly because you're not using anything *"large"* in your development projects. Someone mentioned 30-40 projects and don't mind having uv update (*sync*) all of them in one go. That is nearly an absurd statement, as most projects have completely different dependencies and sizes. 

**Question:** *What's our "problem"?*
**Answer:**

> *can be regenerated at the cost of a bit of CPU time and maybe network bandwidth*

Let me illustrate by showing you the size of only 2 venvs: 

```bash
# \du -Hh -d 1
7.1G    ./llm
4.5G    ./lunar
...
```

I have about 10 more directories with sizes on the same order 3-8 GB, then some even larger, and many smaller, but still in the 500 MB range. Sometimes I need to wipe them, and sometimes I need to copy them, but every time I need to completely reload or rebuild, I hate it. 

Trying to conserve some of my SSD space is not critical but sane practice, and mostly time saving, including backup. Given the time, ssd and cpu needed for a uv sync process to complete, I risk to be blocked several times a day, if using uv in the way proposed. (Or did I misunderstand the proposal?) In general, uv have fantastic speed, UX clarity (including useful & clear help pages) for updating and retrieving packages, and that's why I am using it. 

**Do you see the problem now?**

---

**The 2nd Problem**

Nobody has yet addressed the secondary problem of **keeping track of, and enumerating (the named) different venvs**. Why is that? Probably because what's actually being proposed, is to just use some generic *"cache"*  with embedded container for uv packages. That's when I will stop using uv. 

Why would I stop using uv?

Because I could no longer go to any other project and do "activate some_venv2", but would need to completely rebuild and change the entire venv, for using other than the latest versions of those packages. In the natural sciences, you'd be surprised to see how many projects critically depends outdated projects and packages. And you'd be shocked to know how many old python projects doesn't even have a `requirements.txt` file! And you're perfectly right, most developers are lazy and just want their venv's automatically activated when they enter the project directory, just like git. So my use case would indeed be *fringe*, and uv not useful, except for updating the parts still under my own control. 

**The 3rd Problem**

How do you propose uv to handle packages locally compiled and built when added to venvs?  

**The 4th Problem**

How do you propose to handle locally produced packages that need to be pinned from the official builds with the same name?

---

_Comment by @krstp on 2025-09-04 12:01_

This is ridiculous for how long this thread is opened (2yrs!) and how many messages it has for relatively simple thing to do, on top of that the issue is pushed back encouraging participants to open a new thread 🤯. Such mentality is a cancer of every business.

I just gave up. Use `pyenv` then use `uv` inside each env for package install. Works like a charm.

---

_Comment by @alippai on 2025-09-04 12:52_

Other use case.
You have a single project and you need many self contained deployments:
* deployments/version_001
* deployments/version_002
* deployments/version_003

The environments change slowly, but you may have multiple deployments a day.

Storing the same env multiple times can be prohibitive. Currently packaging in docker is a workaround (because of the overlayfs/layers), but I’d assume uv doesn’t want to require docker for a simple prod deployment.

---

_Comment by @kompre on 2025-09-04 15:17_

> Someone mentioned 30-40 projects and don't mind having uv update (sync) all of them in one go. That is nearly an absurd statement

Oh I agree. `uv sync` should work as usual, per project basis, and not on all of them in one go. Making changes in project I'm not currently working, it's not desidreable.

I would say that there is a fundamental difference in interpretation of the term  *cache* at play here. For me, because I'm used to `poetry`, it just means that the venv folder is not stored in the project folder, but somewhere else on my pc; I have no expectation for the venv created for one project to be reused by any other project, even if they would have the same dependency.

The other interpretation, I guess, is that all the packages resided in big cache somewhere on the pc, and each project pull their dependency from it, as specified in requirement file, without copying dependencies around.

It's my understanding that `uv` treat the term cache more like the second interpretation, except it copies the dependencies in the project folder.

So maybe the term cached should be used in the context of storing venv outside the project folder. 

---

_Comment by @Hawk777 on 2025-09-05 00:54_

> I think some of you is totally missing our (me + [@xmatthias](https://github.com/xmatthias)) point, possibly because you're not using anything _"large"_ in your development projects. Someone mentioned 30-40 projects and don't mind having uv update (_sync_) all of them in one go. That is nearly an absurd statement, as most projects have completely different dependencies and sizes.

I’ll see if I can talk about each point separately.

> > _can be regenerated at the cost of a bit of CPU time and maybe network bandwidth_
> 
> Let me illustrate by showing you the size of only 2 venvs:
> 
> # \du -Hh -d 1
> 7.1G    ./llm
> 4.5G    ./lunar
> ...
> 
> I have about 10 more directories with sizes on the same order 3-8 GB, then some even larger, and many smaller, but still in the 500 MB range. Sometimes I need to wipe them, and sometimes I need to copy them, but every time I need to completely reload or rebuild, I hate it.
> 
> Trying to conserve some of my SSD space is not critical but sane practice, and mostly time saving, including backup. Given the time, ssd and cpu needed for a uv sync process to complete, I risk to be blocked several times a day, if using uv in the way proposed. (Or did I misunderstand the proposal?) In general, uv have fantastic speed, UX clarity (including useful & clear help pages) for updating and retrieving packages, and that's why I am using it.
> 
> **Do you see the problem now?**

I actually *don’t* see the problem, I’m afraid:
* Regenerating these venvs should not take network bandwidth, because uv caches downloads in its own cache, which is separate from the venvs.
* Regenerating these venvs should not take a lot of CPU time (e.g. for recompiling native-code packages), because uv caches (again, in its own cache, outside the venvs) wheels.
* Regenerating these venvs should not take a lot of disk space nor I/O time because uv hardlinks the files into the venvs; it does not copy them, so the size is irrelevant.

The only case I can see where it actually takes a long time would be if there are so *many* files (not large files, but *number* of files) in the venv that the mere act of hardlinking them takes a long time. Is that your situation?

All that having been said, consider two other points:
* Just because venvs are in a cache location, doesn’t mean they *automatically* get deleted; having a clean command has been proposed, but automatic deletion has not. If regenerating a venv is painful for you, just don’t delete it.
* Nobody’s *forcing* venvs to live in cache. If you don’t like it, don’t turn that option on. AFAICT nobody’s even proposing to turn it on by default, never mind making it the only choice.

> **The 2nd Problem**
> 
> Nobody has yet addressed the secondary problem of **keeping track of, and enumerating (the named) different venvs**. Why is that? Probably because what's actually being proposed, is to just use some generic _"cache"_ with embedded container for uv packages. That's when I will stop using uv.
> 
> Why would I stop using uv?
> 
> Because I could no longer go to any other project and do "activate some_venv2", but would need to completely rebuild and change the entire venv, for using other than the latest versions of those packages. In the natural sciences, you'd be surprised to see how many projects critically depends outdated projects and packages. And you'd be shocked to know how many old python projects doesn't even have a `requirements.txt` file! And you're perfectly right, most developers are lazy and just want their venv's automatically activated when they enter the project directory, just like git. So my use case would indeed be _fringe_, and uv not useful, except for updating the parts still under my own control.

I’m a bit confused here. Why could you no longer `activate some_venv2`? If you want to manually maintain `some_venv2` (“manually” just meaning “`uv sync` doesn’t automatically remove and install packages”, you might still be using some other automation or other tools), you can still do that. It’s not named `<projectdir>/.venv` so it’s not the one that uv would automatically manage, but that’s already true today—you can have `<projectdir>/.venv` that uv automatically syncs, or you can have some venvs that you manage by hand with e.g. `uv venv` and `uv pip`, or you could even have both. Just because the `<projectdir>/.venv` can be moved somewhere else, doesn’t mean that `uv venv` and `uv pip` are going away.

> **The 3rd Problem**
> 
> How do you propose uv to handle packages locally compiled and built when added to venvs?

If you’re talking about your own venvs made with `uv venv` in specific places, I don’t see how the proposed change would affect this.

> **The 4th Problem**
> 
> How do you propose to handle locally produced packages that need to be pinned from the official builds with the same name?

I don’t actually understand what this means. Can you clarify? What is a “locally produced package that needs to be pinned from the official build with the same name”? Do you mean a copy of an upstream package to which you’ve applied patches? Or something else?

---

_Referenced in [astral-sh/uv#15885](../../astral-sh/uv/issues/15885.md) on 2025-09-16 16:05_

---

_Referenced in [level12/coppy#56](../../level12/coppy/issues/56.md) on 2025-09-19 22:26_

---

_Referenced in [astral-sh/uv#15960](../../astral-sh/uv/issues/15960.md) on 2025-09-20 16:16_

---

_Referenced in [astral-sh/uv#16202](../../astral-sh/uv/issues/16202.md) on 2025-10-09 10:00_

---

_Referenced in [astral-sh/uv#16218](../../astral-sh/uv/issues/16218.md) on 2025-10-21 15:55_

---

_Referenced in [astral-sh/uv#16424](../../astral-sh/uv/issues/16424.md) on 2025-10-24 13:21_

---

_Referenced in [c0rychu/uvlink#3](../../c0rychu/uvlink/issues/3.md) on 2025-11-17 08:42_

---

_Comment by @c0rychu on 2025-11-17 13:11_

I made a little tool - [`uvlink`](https://github.com/c0rychu/uvlink) as a "solution."

It basically creates/manages the `venv` dir under `~/.local/share/uvlink/cache/` and creates a symlink `.venv` in the project dir pointing to the cache location.

It can be installed by
```
$ uv tool install uvlink
```

~~I only tested it on an Apple Silicon Mac running macOS 26.1 because that's all I have.~~
Edit: it now should support macOS, Linux, and Windows. We have run the tests against all three platforms in CI.

Any feedback/issues/PR are welcome. But I still hope that someday `uv` can support this natively, and I just created this as a temporary solution to avoid putting .venv into my Dropbox 💀.

---

_Referenced in [c0rychu/uvlink#5](../../c0rychu/uvlink/issues/5.md) on 2025-11-17 14:28_

---

_Comment by @sm-Fifteen on 2025-11-17 20:54_

> I made a little tool - [`uvlink`](https://github.com/c0rychu/uvlink) as a "solution."
> 
> It basically creates/manages the `venv` dir under `~/.local/share/uvlink/cache/` and creates a symlink `.venv` in the project dir pointing to the cache location.
> 
> It can be installed by
> 
> ```
> $ uv tool install uvlink
> ```
> 
> I only tested it on an Apple Silicon Mac running macOS 26.1 because that's all I have. Any feedback/issues/PR are welcome. But I still hope that someday `uv` can support this natively, and I just created this as a temporary solution to avoid putting .venv into my Dropbox 💀.

There's also [uv-workon](https://pages.nist.gov/uv-workon/) which is maintained by a guy who seems to be at least partly working on it as part of his job at NIST.

---

_Comment by @wpk-nist-gov on 2025-11-17 21:07_


> 
> There's also [uv-workon](https://pages.nist.gov/uv-workon/) which is maintained by a guy who seems to be at least partly working on it as part of his job at NIST.

Cheers for the shout out @sm-Fifteen!  Let me say that `uv-workon` takes the opposite perspective to much of what's being discussed above. Instead of putting virtual environments in a central location and linking as needed (like `uvlink` does), `uv-workon` links project virtual environments to a central location, to simplify reusing virtual environments across data projects.  


---

_Comment by @sm-Fifteen on 2025-11-17 21:16_

> 
> > 
> > There's also [uv-workon](https://pages.nist.gov/uv-workon/) which is maintained by a guy who seems to be at least partly working on it as part of his job at NIST.
> 
> Cheers for the shout out @sm-Fifteen!  Let me say that `uv-workon` takes the opposite perspective to much of what's being discussed above. Instead of putting virtual environments in a central location and linking as needed (like `uvlink` does), `uv-workon` links project virtual environments to a central location, to simplify reusing virtual environments across data projects.  
> 

Ahh, gotcha, thanks for clarifying! It's a subtle distinction when you read the documentation, and one could still use that to get to the same result (i.e: having  "centralized/separate venvs" that live outside the directory of the project using it, which still works around the Dropbox/OneDrive syncing issue). You're right, though, that in the end, the resulting venvs are managed by the user, instead of privately by the tool.

---

_Comment by @eabase on 2025-11-22 02:01_

@c0rychu 
@wpk-nist-gov 
@sm-Fifteen 

Always interesting to see that people still prefer to reinvent the wheel, instead of reading previous comments...

5 year old solution that still works great today (especially combined with an adjusted uv work-flow): 
* https://github.com/fohrloop/venvlink

2 new *re-inventions*:
* https://github.com/c0rychu/uvlink
* https://github.com/usnistgov/uv-workon

Your documentation for those tools are far from clear and lacks visual examples.

- (a) Can you guys please explain what you do better or different from `venvlink`?
- (b) How do you manage, keep track of, and list the venv's in those tools?


---

_Comment by @c0rychu on 2025-11-22 03:42_

Hi @eabase,

Yeah, sorry for the unclear documentation. I added a little [GIF Demo](https://github.com/c0rychu/uvlink?tab=readme-ov-file#demo). I'm not sure if that helps. If it actually creates more confusion, I can remove that.

## Short answer
`venvlink` claims: it is currently supporting only Windows. In contrast, `uvlink` now supports macOS, Linux, and Windows(beta).

## About `uvlink`
### Can you guys please explain what you do differently from venvlink?
`uvlink` does only one thing: create a symlink `.venv` at where you run `uvlink link`
After that, 
1. if you want to install packages, use [Astral's uv](https://docs.astral.sh/uv/), such as `uv sync, uv add, uv pip install, ...` 
2. if you want to remove the symlink, use `rm .venv`

### How do you manage, keep track of, and list the venv's in those tools?
1. `uvlink ls` list all caches 
2. If you have done `rm .venv` in your project, `uvlink ls` shows that the corresponding cache is "Not Linked"
3. You can either run `uvlink link` again to relink or just do `uvlink gc` to remove all "Not Linked" caches.


### By the way
As far as I know, `uv add`, `uv sync`, `uv pip install` use hardlinks to save disk space. So, I personally don't mind having a separate venv in every project I work on and do not reuse them. Also, for instance, if I want to reuse them, in VS Code, I can just do `> Python: Select Interpreter`. If, for some reason, I really want a few environments that would be used system-wide, I'll just create a conda env instead.

---

_Comment by @eabase on 2025-11-22 23:33_

@c0rychu 
Thanks for feedback!. I've posted more comments/questions in your repo. 
* https://github.com/c0rychu/uvlink/issues/13

> venvlink claims: it is currently supporting only Windows.

Hmm, that doesn't sounds right, let me see.  Ok, it works in *Cygwin*, but someone else need to try it in a real Linux environment. But whatever issue you're facing, I think it is easily fixed. The original dev, is only using linux now... 

> If, for some reason, ..., I'll just create a conda env instead.

There is no such *reason* in the world. Conda is the nasty infectious destroyer of python and portability! I'm just gonna pretend I didn't hear that. 🤢 

---

_Comment by @eabase on 2025-11-22 23:40_

@wpk-nist-gov 

> **Instead of putting virtual environments in a central location** and ..., 
uv-workon **links project virtual environments to a central location**, ...

With self-contradictory and confusing comments like that, I dunno... 👎 

---

_Comment by @zanieb on 2025-11-23 15:18_

@eabase please take this discussion out of this thread into the issue trackers for the respective projects.

---

_Comment by @MDSvensson on 2025-11-24 23:34_

I love uv but for my personal studying projects which i don't use git for but instead use a synchronized folder in dropbox, i will have to use poetry for now. Centralizing virtual environments has a special utility for me - it saves me cloud space in my hybrid use where i sync more than just code - pdf files, code and more. 

---

_Referenced in [astral-sh/uv#1384](../../astral-sh/uv/issues/1384.md) on 2025-11-25 12:09_

---

_Comment by @krstp on 2025-11-26 23:55_

2 years and counting. Good the status is still in open.

---

_Comment by @gem7318 on 2025-12-09 18:54_

> I use Windows/Linux/Mac every day and synchronise my projects using OneDrive, having `.venv` in my project folder is such a nightmare, and that's why I started using `pipenv`. IMO `venv` should always be centralised as environments and packages are platform dependent, they are not part of the "content" of the project.
> 
> I'm using [this plugin](https://github.com/DonJayamanne/vscode-python-manager) to manage and switch between different environments and don't have to know their location most of the time, but I do hope there is a standard for that.

This is the exact situation I'm dealing with—I'm an Engineer working on a mac but build libraries that Windows/OneDrive based folks use, and this makes it impossible for them to install packages in and work within the same OneDrive directory across users.

---

_Comment by @Carbaz on 2025-12-10 12:01_

> > I use Windows/Linux/Mac every day and synchronize my projects using OneDrive, having `.venv` in my project folder is such a nightmare, and that's why I started using `pipenv`. IMO `venv` should always be centralized as environments and packages are platform dependent, they are not part of the "content" of the project.
> > I'm using [this plugin](https://github.com/DonJayamanne/vscode-python-manager) to manage and switch between different environments and don't have to know their location most of the time, but I do hope there is a standard for that.
> 
> This is the exact situation I'm dealing with—I'm an Engineer working on a mac but build libraries that Windows/OneDrive based folks use, and this makes it impossible for them to install packages in and work within the same OneDrive directory across users.

A bit off topic but, in general, all those strange scenarios that are being described here by some of you are solved using GIT and "dockerized environments", but you need to be doing quite arch/os bound code to need that on Python as it's quite multiplatform...

I used Mac, Windows and Linux collaborating to colleagues doing the same, we never needed anything else to sync but GIT.
***With properly set `.gitignore` file: `.venv` folder and `.env` file must always be ignored.***

Development environments should not be shared, they are personal and host bound.
*I.e: Their own `.env`, their own env vars, their own containers set up, etc.*
(Even their own specific lib builds if you have both X64 and ARM machines)
Only code needs to be sync with a properly set `.gitignore`, environment is created an managed locally and specifically.

Using OneDrive or similar services to sync/share code is scaring, I'm sorry to say but your real issue is not `uv` centralizing or reading `.env` files, is using the wrong services to sync code and collaborators. Investing on GIT and environment management training could be a great idea. 😉 


**Still, I'm a big supporter for the "centralized `.venv` folder and to keep clean the repository folder, not having this **option** is my biggest deterrent to move completely to `uv` from `pipenv`.**

---

_Comment by @lgaborini on 2025-12-12 07:57_

It's not only OneDrive/Dropbox, it has been written so many times in this discussion. It's also about network drives, external/faster storage, antivirus, disk space saving, working on filesystems without symlinks, or just to avoid cluttering project directories (to move project directories easily or using indexing tools that do not support exclusions). And more opinionated usage proposals such as sharing the same `.venv`.

---

_Comment by @cpbotha on 2025-12-12 11:16_

> I used Mac, Windows and Linux collaborating to colleagues doing the same, we never needed anything else to sync but GIT.
> _**With properly set `.gitignore` file: `.venv` folder and `.env` file must always be ignored.**_

Your parent posters need real-time sync because they, like many of us, are switching between different machines and operating systems more often than you, in any case on a scale smaller than the atomic git commit.

Real-time syncing is the right tool for this job, not git synchronization.

I work on my mac, then when I get home I might sit behind my Windows workstation for a while, later spending some more time on my own Linux machine.

I rely on the project I'm working on being synchronized behind the scenes, at a much higher cadence than my atomic git commits.

Making a git commit in order to be able to work on one of my other machines is not the best tool for the job; real-time sync is.

 I will git commit when my atomic change is ready, NOT when I want to continue working on that same piece of code on my other machine, because I have moved to the couch for example. 

Many people seem to have trouble understanding this 100% valid use-case. It could just be because not everyone moves between their own different workstations as regularly and fluidly as some of us do.

I wrote a bit more here: https://cpbotha.net/2022/11/11/weekly-head-voices-248-oh-snap/#yes-you-can-keep-your-checked-out-source-code-in-dropbox-or-onedrive


---

_Comment by @kompre on 2025-12-12 17:00_

After long pondering and testing I think that `uv` already has built-in the capability to use virtual environment in any location,  it's just a bit inconvenient. Let me explain:

For `uv` to properly respect a venv location, the environment variable `UV_PROJECT_ENVIRONMENT` needs to be set for any `uv` call that does venv stuff. You coudl either:

1. export `UV_PROJECT_ENVIRONMENT` in your terminal session, but it will be available only for that session
2. save `UV_PROJECT_ENVIRONMENT=<path/to/venv>` to a .env file

The second option is particularly interesting because while `uv` won't load a `.env` file to execute a command, `uv` can run any app with support for `.env` file providing the option `--env-file <path/to/.env>`, **including itself.**

This is indeed valid syntax:

```bash
uv run --env-file .env -- uv [args] 
```

## Enter `prime-uve`

Based on the above approach I develop [`prime-uve`](https://github.com/kompre/prime-uve), a cli tool for facilitate the process of setting up the venv. 

`prime-uve` provides 2 cli interface:

- `uve`: it's _almost_ just an alias for `uv run --env-file .env.uve -- uv [args]`, but it will also:
  - **automatically discover the `.env.file`** in the current folder or parents (walk up the directory tree), akin `uv` normally would discover a `.venv` (this means you can call `uve ...` in a subdirectory of your project, and the correct venv will be picked up)
  - **resolve the appropriate path** for the cache location according to the platform used (linux, macos, windows)
- `prime-uve`: interface for initial setup (creating `.env.uve` file, interpolating venv path, list/prune of cached venvs, configuring vscode, ...)

**This tool has been specifically designed to work with project on network location**

A intended "side" effect is that all the environment variables contained in the `.env.file` will be loaded when using `uve`, solving another issue people have (#1384).

I like this approach because it's explicit: user want to automatically load env variables? use `uve`; they don't? use `uv`

## Quick example

1. Initialise a project

```bash 
cd your_project/
uv init # will create the pyproject.toml
prime-uve init # will create .env.uve file and populate with `UV_PROJECT_ENVIRONMENT`
```  

2. use `uve` instead of `uv` for any call inside the project

```bash
uve sync
uve add keecas
...
````

And that's it. 

## venvs management

`prime-uve` provide the command `list` and `prune` to manages the venvs in the cache location. It's pretty basic, all the heavy lifting is done by `uv` itself.

## VScode integration

To let vscode know about the venv, you can just do this:

```bash
uve run code .
```

vscode will be launched and know already about the correct venv. This works well on linux and macos, while windows is more finicky because `code` available in the terminal is not actually pointing to `code.exe` but to `code.cmd` which is wrapper that mess it up the venv recognition (it will label it as `system`), but if you take the time to add `code.exe` to path, it will work.

If instead you normally open vscode from GUI, or double click on code-workspace file, I provided a `prime-uve configure vscode` that essentially will set the "python.defaultInterpreter" in code-workspace.

While generally `prime-uve` is cross platform, setting up the default interpreter in the code-workspace will be platform specific because the path to the interpreter is different on windows machine rather then on a linux/macos one. Some workaround/compromise are provided to mitigate that, but if you access the project with only OS type, then the provided path will work across machines. 

## Try it out!

Let me know if typing `uve` instead of `uv` is an acceptable compromise. I see it akin to `uvx` which is alias for `uv tool run`.

---

_Comment by @Carbaz on 2025-12-12 17:40_

Shorted quotes for space save.

> > I used Mac, Windows and Linux collaborating to colleagues doing the same, we never needed anything else to sync but GIT.
> > _**With properly set `.gitignore` file: `.venv` folder and `.env` file must always be ignored.**_
> 
> Your parent posters need real-time sync because they, like many of us, are switching between different machines and operating systems more often than you, in any case on a scale smaller than the atomic git commit.
> 
> Real-time syncing is the right tool for this job, not git synchronization.
> 
> I work on my mac, then when I get home I might sit behind my Windows workstation for a while, later spending some more time on my own Linux machine
>
>...
> 
> I wrote a bit more here: https://cpbotha.net/2022/11/11/weekly-head-voices-248-oh-snap/#yes-you-can-keep-your-checked-out-source-code-in-dropbox-or-onedrive

There may be reasonable scenarios, of course, and as I said before, I support the centralized storage for `.venv` folders.

Your's seems forced to me, but we are getting oot, if that usage works for you it's good, as said before, local dev environment is something personal and if you want your local be distributed that way that's your call 👍

> After long pondering and testing I think that `uv` already has built-in the capability to use virtual environment in any location, it's just a bit inconvenient
>
>...
>
> Based on the above approach I develop [`prime-uve`](https://github.com/kompre/prime-uve), a cli tool for facilitate the process of setting up the venv.
> ## Try it out!
> Let me know if typing `uve` instead of `uv` is an acceptable compromise. I see it akin to `uvx` which is alias for `uv tool run`.
Shorted quote for space shake.

I see several problems and maybe misconceptions on that approach.

On the usage of `.env` files:

Having on same `.env` file values for the virtual environment and the host environment at the same time is a bad idea on several points, we discussed it on the same issue you link there. (that's actually closed as the requested feature is already provided 😉 )

---

On the "centralized storage thing":

Having an specific environment location configuration on each project may lead to miss alignments, having a centralized venv storage implies having a centralized configuration of that central location.

Centralized is not the same as "coincidental" (I.e: It's not centralized but it happens that all of them are configured to the same location.)

A solution to the request on the issue is a configuration to set the `uv` central storage location, a sort of "UV_PROJECTS_ENVIRONMENTS" (Note is plural) and then a way to tell `uv` that a given repo should use it. (to keep default behavior as is so it does not disturb any one currently using `uv`)

Then we could use `uv -centralized ...` so uv knows we want to use centralized storage whose path is on UV_PROJECTS_ENVIRONMENTS.  it can be polished like also adding an UV_USE_CENTRALIZED to make it the default behavior, etc...

It should also make `uv` to handle environment activation and make a real wrap over `venv` not just the creation.
`uv activate` instead of using directly venv. (Because not all is just 'run' single commands 😃 )

In summary: the request implies not a complicated usage, it could relay on a few new set options that could be supported internally by mechanisms already present, UV_PROJECT_ENVIRONMENT, but just make it transparent and convenient for users.

---

Ofc we can make ourselves a script similar to yours, that's a workaround for a missing feature, but it's configuration should be based on a host system's env vars and or conf file, some sort of uve.conf located on users folder, UVE_XYZ env vars, or so.

And I wont store the UV_USE_CENTRALIZED anywhere but to calculate it dynamically on each call.

---

_Comment by @kompre on 2025-12-12 20:15_

> I see several problems and maybe misconceptions on that approach.
> 
> On the usage of .env files:
> 
> Having on same .env file values for the virtual environment and the host environment at the same time is a bad idea on several points, we discussed it on the same issue you link there. (that's actually closed as the requested feature is already provided 😉 )

My main take away from that issue is that uv should not automatically load env file, because it could lead to unexpected results.

I agree with that.

That's why I designed my tool to be explicit and opt-in. It will be user choice to use `uve ...` instead of `uv ...`, using `uve [args]` is really not much different from using `uv run --env-file .env.uve --  uv [args]`.

Unless this latter syntax is frown upon, I don't see why I shouldn't load env variables like `UV_PROJECT_ENVIRONMENT` from dotenv file.

Maybe a point that wasn't entirely clear is that the `UV_PROJECT_ENVIRONMENT` saved in dotenv file is set to a dynamic value, that is resolved at each call of `uve` to provide the correct path to cache for each platform. The actual dotenv file look like:

```
UV_PROJECT_ENVIRONMENT=${PRIMEUVE_VENVS_PATH}/project-name_hash
```

with `PRIMEUVE_VENVS_PATH` to be calculated at each call, while `project-name_hash` is calculated only once at set up.

A user could add env variable to the dotenv file that may break on other system, but so would be running `uv run --env-file .env.uve -- uv ... `

Let's be honest, mine is a workaround that works within the boundary of uv, until uv may implement a centralized location feature. But this issue has been open for quite some time now, so workaround it needs to be. 

---

_Comment by @ResRipper on 2025-12-13 03:20_

I would like to know if this is something we can expect in the near future, since this is the second most commented-on issue, and many different wheels have been invented for this, or if this is still not within scope for now.

---

_Comment by @Carbaz on 2025-12-14 11:01_

> Unless this latter syntax is frown upon, I don't see why I shouldn't load env variables like `UV_PROJECT_ENVIRONMENT` from dotenv file.

OK, There is a bit of "inception" thing 😃 

There are two issues there:

* First: **Why not loading from `.env` files?**
  
  This is more opinionated, but as I see them `.env` files contains environment variables definitions, **they are not the environment**, so IMO apps should read directly from environment variables or from a dedicated configuration file for them. `.env` files are to be loaded into the environment explicitly.

  So the `--env-file .env.xyz` is fine because you are telling `uv` to load it inside the virtual environment before running the app so the app can read from the environment directly ignoring the `.env` file.

* Second: **The real problem is the *"mixture"* of environments.**

  On your project's `.env` file you store variables to be loaded **inside** the virtual environment your app will run, but the `uv` tools execution happens out of there, `uv` runs on your host environment and so use your host environment variables.

  So putting `uv` configuration environment variables inside the projects `.env` file ends with a file that defines variables for two different environments, host and virtual, that's wrong

So combining both ideas you get that your tool should get it's config from the host environment variables directly or from a specific and centralized configuration file tailored for it, like a `.uve_config` or similar, stored on your user or config folder so there is only one place where you define your host `uv` configuration.


If you read `uv` documentation  [here](https://docs.astral.sh/uv/concepts/configuration-files/) this is what they do. You can read on the first ***"NOTE"*** the "user-level" (the way they call host-level) configs do not come from project's config files but from a user or system level file.

Roughly I.e: 
* `uv` as dependency manager takes its config from the project's files or project's virtual environment vars.
* `uv` as environment manager takes its config from the host's files or host's environment vars.


Hope this clarifies my previous post. 😉 

---

_Comment by @zanieb on 2025-12-14 18:39_

> I would like to know if this is something we can expect in the near future, since this is the second most commented-on issue, and many different wheels have been invented for this, or if this is still not within scope for now.

Yes, this is something we're planning to work on in early 2026.

---

_Comment by @NiyazNz on 2025-12-16 14:58_

I just wanted to share how I use centrally managed virtual environments outside of the project with `uv`.

I have been using [`virtualenvwrapper`](https://virtualenvwrapper.readthedocs.io/en/latest/index.html) for venv management for a long time. It stores virtual environments centrally in `$WORKON_HOME` and provides useful commands such as `lsvirtualenv`, `mkvirtualenv`, `rmvirtualenv` and `cpvirtualenv` to manage them, as well as `workon` to switch environments. 

In an activated Python environment, the `VIRTUAL_ENV` variable is usually set to virtual environment's location. However, for some reason, `uv` doesn't respect this variable; it uses its own `UV_PROJECT_ENVIRONMENT` variable instead.
We can therefore set this variable to point to `VIRTUAL_ENV` when activating the environment and unset it when deactivating.
This can be done using virtualenvwrapper hooks.

Assuming `virtualenvwrapper` is configured as [described](https://virtualenvwrapper.readthedocs.io/en/latest/install.html#shell-startup-file) and `uv` is installed,
modify the `postactivate` and `postdeactivate` hooks.
```shell
echo "export UV_PROJECT_ENVIRONMENT=\$VIRTUAL_ENV" >> $VIRTUALENVWRAPPER_HOOK_DIR/postactivate
echo "unset UV_PROJECT_ENVIRONMENT" >> $VIRTUALENVWRAPPER_HOOK_DIR/postdeactivate
```

This will allow you to benefit from any virtualenv management commands provided by `virtualenvwrapper`, as well as the speed of installation thanks to `uv`.

For new projects, simply use:
```shell
mkvirtualenv project_name
uv sync
```

For existing environments:
```shell
workon project_name
uv sync
```

I also don't find it useful or comfortable to prefix all commands with `uv run`. When you switch venvs explicitly (i.e. activate them using workon or bin/activate), you can just run python scripts or any other command available in the activated environment as you used to, without wrapping it in `uv run`.

---

_Comment by @OdyAsh on 2025-12-26 02:27_

> > I would like to know if this is something we can expect in the near future, since this is the second most commented-on issue, and many different wheels have been invented for this, or if this is still not within scope for now.
> 
> Yes, this is something we're planning to work on in early 2026.

Lovely! <3 ^^

---
