```yaml
number: 1419
title: "Add option to upgrade all packages in the environment, e.g., `upgrade --all`"
type: issue
state: open
author: ikrommyd
labels:
  - enhancement
  - needs-design
  - uv pip
assignees: []
created_at: 2024-02-16T03:48:14Z
updated_at: 2025-12-17T21:02:10Z
url: https://github.com/astral-sh/uv/issues/1419
synced_at: 2026-01-10T03:11:31Z
```

# Add option to upgrade all packages in the environment, e.g., `upgrade --all`

---

_Issue opened by @ikrommyd on 2024-02-16 03:48_

Something missing from pip is the ability to do `upgrade --all` like conda can do `update --all` in an environment.
Wrappers around pip that do this are oftentimes slow because python. Given that you already have some resolution strategies available, it would be really nice to see something like that. And it's also gonna be very fast in your case I believe.
The exact resolution strategy for something like that is up for discussion but I think resolution strategies like the one conda uses have been liked by the community.

--- 

Maintainer edit: See https://github.com/astral-sh/uv/issues/1419#issuecomment-2849420114 for a current overview of problems and solutions

---

_Comment by @kdeldycke on 2024-02-16 05:11_

Relates to:
- https://github.com/pypa/pip/issues/4551
- https://github.com/pypa/pip/pull/10491

---

_Label `enhancement` added by @zanieb on 2024-02-16 05:20_

---

_Comment by @zanieb on 2024-02-16 05:21_

Thanks! We should also clarify how `pip install --upgrade` is _different_ than this in our documentation.

---

_Comment by @sh-shahrokhi on 2024-03-09 20:27_

Hi, just wanted to add my voice for this one.  Thanks.

---

_Comment by @msminhas93 on 2024-03-22 00:08_

This would be a great feature! Any updates?

---

_Comment by @zanieb on 2024-03-22 03:28_

Hi, please just upvote the original post with 👍 if you want to see this. Otherwise we'll report back with any updates and would appreciate if the thread was focused on substantive discussion.

For this to move forward two things need to happen:

- A contributor willing to prototype it needs to come forward, this could be someone from the Astral team but we currently have other priorities.
- We need to answer some design questions, drawing a lot from the discussion over in `pip`, e.g.
    - Why isn't this being implemented in `pip`?
    - What is the desired behavior? Is there more than one expectation?
    - What is the main user-story for this? What use-cases are we addressing?



---

_Label `needs-design` added by @zanieb on 2024-03-22 03:28_

---

_Comment by @dlasusa on 2024-04-02 18:48_

I would really love to see this feature added.  I'm not sure if it's helpful, but I can describe how I currently update the packages in my virtual python environments.  These are all run from an activated venv.  In this case I have an environment for some polars projects.

I first run:
```powershell
pip list --outdated
```
Which produces a list:
```
Package   Version Latest  Type
--------- ------- ------- -----
ipykernel 6.29.3  6.29.4  wheel
ipython   8.22.2  8.23.0  wheel
polars    0.20.16 0.20.18 wheel
```
This example is a mercifully short list since Polars doesn't have other Python dependencies.  Plus, since it's a playground for testing one off things I want to try in polars, I don't have a ton of packages in this environment....but this list can get very long for larger projects (which is time consuming to manually update)

I'll then go one by one running (for example):
```powershell
pip install --upgrade polars
```
There are times I'm unable to update all the packages.  Usually if I have a failure, it's because `pip` can't find a suitable package/version.  Depending on how critical the package is, or if it has new functionality that will be useful in a project, I may create an entirely new venv and do a new `pip install ...` on all the packages I need.  I just updated all the packages in this example, and none failed, so I'm unable to provide more details about what I failure would look like.  Basically, it's a dependency resolution failure. 

@zanieb To answer some of the questions you listed (granted these are just MY opinion or my use case)

_Why isn't this being implemented in pip?_

This is a great question and something I've often wondered as well!  I've come across both [pip-tools](https://github.com/jazzband/pip-tools) and [pip-review](https://github.com/jgonggrijp/pip-review) which is/was a fork of pip-tools and I think it's no longer maintained.
More reading: https://stackoverflow.com/questions/2720014/how-to-upgrade-all-python-packages-with-pip

_What is the desired behavior? Is there more than one expectation?_

The simple version of what I would want: simply update all the packages in the environment, and report what was/wasn't updated:
```
2 Packages Updated:
ipython   8.22.2  -> 8.23.0 
polars    0.20.16 -> 0.20.18

1 Package Not Updated:
ipykernel 6.29.3  No suitable version found
```
I'm of course open to other ideas too.  

_What is the main user-story for this? What use-cases are we addressing?_

I'm not sure if it's just the work I do, but I'm often seeing new functionality in packages and/or I just like to stay current.  Especially right now with many tools being updated/written in Rust, so I like to try out different things and see where development is at.   I've never done (nor would I plan to) and "update all" in production.   Though if enough tools are written in Rust and don't have Python dependencies, maybe much of this goes away?

Apologies if any of that is unclear.  Just let me know if I can further clarify anything. 

Thanks!!

---

_Comment by @jvdavim on 2024-11-05 14:24_

I'm interested in this feature too. I don't know if it would help, but I'm currently updating the packages using:
```
rm uv.lock
uv lock
uv sync
```

**Edit:**

As commented by @bryant1410 and @D1no, I agree it's better:
```
uv lock --upgrade
uv sync
```
It works fine if using the uv as project manager.

---

_Comment by @msminhas93 on 2024-11-05 18:38_

> Hi, please just upvote the original post with 👍 if you want to see this. Otherwise we'll report back with any updates and would appreciate if the thread was focused on substantive discussion.
> 
> For this to move forward two things need to happen:
> 
> * A contributor willing to prototype it needs to come forward, this could be someone from the Astral team but we currently have other priorities.
> * We need to answer some design questions, drawing a lot from the discussion over in `pip`, e.g.
>   
>   * Why isn't this being implemented in `pip`?
>   * What is the desired behavior? Is there more than one expectation?
>   * What is the main user-story for this? What use-cases are we addressing?

The request for a uv update command to upgrade all packages is rooted in the fact that uv is more than just a package manager - it also handles environment management like conda, micromamba, and poetry. 

These tools already offer similar functionality:

- conda/micromamba: conda update --all or micromamba update --all
- poetry: poetry update

This point talks about why it makes more sense to implement it at the higher level tool instead of pip https://github.com/pypa/pip/issues/4551#issuecomment-2325495099 

The primary use case is for development environments, particularly in fields like deep learning or data science, where frequent package updates are necessary.

---

_Comment by @bryant1410 on 2024-11-12 13:53_

How is this different from

```bash
uv lock --upgrade
```

?

---

_Comment by @ikrommyd on 2024-11-14 08:25_

> How is this different from
> 
> ```shell
> uv lock --upgrade
> ```
> 
> ?

Am I mistaken or would this only apply to cases where you're using `uv` to manage your project. When I first opened the issue on day 1 of `uv` release I was thinking of an `upgrade --all` option to a generic python venv in a similar way conda has `upgrade --all` for a conda environment.

---

_Comment by @D1no on 2024-11-22 19:19_

Yeah please add an `update` or `upgrade` like all package managers under the sun so normal engineering peasants like me don't need to do this odd mental gymnastics. Preferably a simple

update command that actually updates the version strings in the pyproject.toml for explicitness

- update packagename
- update -> leads to selection which packages to update
- update --all -> updates all of them

instead of having to do
`uv lock --upgrade`
`uv sync`
manually type

---

_Renamed from "Enhancement: add `upgrade --all` option" to "Add option to upgrade all packages in the environment, e.g., `upgrade --all`" by @zanieb on 2024-11-26 23:49_

---

_Comment by @yhoiseth on 2024-12-04 12:56_

I use [this tiny script](https://gist.github.com/yhoiseth/c80c1e44a7036307e424fce616eed25e) to upgrade all dependencies to their latest version regardless of what is specified in `pyproject.toml`.

---

_Comment by @johnthagen on 2024-12-05 03:01_

> update command that actually updates the version strings in the pyproject.toml for explicitness

This specific part of your request is tracked in 

- #6794

---

_Comment by @ssbarnea on 2024-12-16 15:44_

If `uv pip list --outdated` would have supported a plain format, this could be seen as a decent workaround:
```shell
uv pip list --outdated --format=plain | uv pip install -U
```

My current workaround for this looks like:
```shell
uv pip list --outdated --format=json | jq '.[].name' | xargs -I {} uv pip install --system -q -U {}
```

---

_Comment by @WilliamStam on 2025-01-13 11:43_

```
uv lock --upgrade
uv sync
```

seems to do what is required. but please can this be added as an alias type thing in uv?

```
uv update
```

got used to `pdm update`. whenever we work on a project we upgrade the dependencies to their latest patch version ie

```
dependencies = [
    "sqlalchemy-utils==0.41.*",
    "SQLAlchemy[asyncio]==2.0.*",
    "toml==0.10.*",
    "uvicorn==0.20.*",
]
```

so being able to "update" them all is very useful. i just found the uv lock --upgrade and sync thing now. but i can guarantee i will have forgotten about it next time i need it lol. 

thanks for the project! its insanely fast

---

_Comment by @yrro on 2025-01-14 13:50_

> ```
> uv lock --upgrade
> uv sync
> ```
> 
> seems to do what is required. but please can this be added as an alias type thing in uv?
> 
> ```
> uv update
> ```

Various package managers have overlapping and/or conflicting definitions of the word `update` and `upgrade` and I'd prefer to not have to add another one to the Rosetta stone. Really sorry to bike shed but good UI is important to me.

If existing commands are sufficient then I'd rather the documentation be updated to make it clear what the right commands are. I find it all a bit confusing, especially (and this isn't uv's fault itself) coming over from Poetry which seems have both `poetry sync --lock` and `poetry lock --sync`...

That said, hopefully someone can chime in here if I'm wrong:

`uv sync --upgrade` will both upgrade the packages in the lockfile to reference the latest compatible versions specified in the project's dependencies (equivalent to `uv lock --upgrade`), and also bring the project's venv's packages in sync with the new lockfile (equivalent to `uv sync`). So that is probably the command to document (unless I've misunderstood what you're trying to accomplish).

And to check if there are updates, I use `uv lock --upgrade --locked`, which doesn't tell you which packages are outdated, just whether any outdated packages exist. `uv sync --upgrade --locked` will do that & also bring the venv in sync if it's not already in sync for whatever reason.

---

_Comment by @heliocastro on 2025-01-29 09:48_

And how about on sync pyproject with uv.lock after `uv lock --upgrade` ?

---

_Comment by @krishan-patel001 on 2025-01-29 10:02_

> And how about on sync pyproject with uv.lock after `uv lock --upgrade` ?

I like the idea of this, wouldn't break the existing functionality then

---

_Comment by @yrro on 2025-01-29 11:22_

> And to check if there are updates, I use `uv lock --upgrade --locked`, which doesn't tell you which packages are outdated, just whether any outdated packages exist. `uv sync --upgrade --locked` will do that & also bring the venv in sync if it's not already in sync for whatever reason.

... or at least I used to. It seems now `uv lock --upgrade --locked` fails with `error: the argument '--upgrade' cannot be used with '--check'` which is... confusing. And `uv sync --upgrade --locked` fails with `error: the argument '--upgrade' cannot be used with '--locked'`, which at least matches the options I specified on the command line... but neither tell me _why_ this is no longer possible.

Perhaps `uv lock --upgrade --dry-run` is the replacement? But unfortunately, this exits successfully if there are packages to update, so it's no good for checking if there are upgradable packages.

---

_Comment by @zanieb on 2025-01-29 14:20_

`--locked` is an alias of `--check` in `uv lock`, but I don't think we can distinguish between which was provided.
 
While I'm not sure it's the best way to solve this problem, I'm fine with allowing `--upgrade --check`.

See https://github.com/astral-sh/uv/issues/10273 for the motivation behind making those options conflict.

---

_Comment by @yrro on 2025-01-29 18:22_

Thanks. `--upgrade --check` would make sense to me, but there is overlap with `--upgrade --dry-run`; if `--dry-run` and `--check` were mutually exclusive then that would resolve and it would be clear that `--check` is expected to fail of there are pending updates, whereas `--dry-run` is expected to succeed.

---

_Comment by @aaronmaxcarver on 2025-02-22 21:06_

Here's a temporary and patchy solution I've used. It's similar to comments by [yhoiseth](https://github.com/yhoiseth) and others...

This [this pyproject_yeeter.py gist](https://gist.github.com/aaronmaxcarver/9a2dbc5b26c66c473e78243c030faacd) will:
- blindly crunch through a `pyproject.toml` and update everything by calling `uv remove` then `uv add` on each dep
  - this results in upgrades to both the `pyproject.toml` and the `uv.lock`
  - the script accounts for groups like the very common "dev" group, e.g. for `ruff`
  - will upgrade across breaking changes and such, makes no attempt to avoid this


---

_Comment by @rosmur on 2025-03-15 11:52_

would like to request a method to also upgrade all items in a uv venv (that is not a project)

I was able to do it by this single line shell command:

`for pkg in $(uv pip freeze | grep -v '^-e'); do uv pip install -U $pkg; done`

although it didnt seem to miss items

---

_Comment by @notatallshaw on 2025-03-20 16:13_

FYI, for those who use a pip workflow (like myself), and those not already aware, this is the purpose of the `uv pip compile`: 

1. Define your unpinned requirements in a `requirements.in`
2. Compile them to a `requirements.txt`, e.g. `uv pip compile --upgrade requirements.in -o requirements.txt`
3. Sync your environment with the upgraded requirements `uv pip sync requirements.txt`

I have a couple of small scripts to turn this into a single command, with various options I prefer.

And I beleive `uv pip compile` also supports implicitly extracting the requirements out of a `pyproject.toml`.

---

_Comment by @ikrommyd on 2025-04-01 01:40_

> would like to request a method to also upgrade all items in a uv venv (that is not a project)
> 
> I was able to do it by this single line shell command:
> 
> `for pkg in $(uv pip freeze | grep -v '^-e'); do uv pip install -U $pkg; done`
> 
> although it didnt seem to miss items

This was my original reason for making the issue. Upgrade all packages in environment that is not a project while at the same time caring for dependencies. Sort of like `conda update --all` but for pip packages.

---

_Comment by @notatallshaw on 2025-04-01 01:51_

> Sort of like `conda update --all` but for pip packages.

Conda keeps track of what packages were user requested, pip and the uv pip interface doesn't.

So to get the same benefits of conda, which can remove dependencies no longer needed, you need to use the pip compile workflow I explain above, or use uv's project level API (uv add, uv sync, etc.).

---

_Comment by @stevenwalton on 2025-04-14 18:29_

You can always do something hacky like

```bash
uv pip list \
   | tail -n +3 \
   | cut -d ' ' -f1 \
   | xargs uv pip install --upgrade
```
- `uv pip list` shows all the packages 
- `tail -n +3` removes the first two lines which are just a header
- `cut -d ' ' -f1` deliminates on spaces and takes the first field (second field is the package version)
- `xargs uv pip install --upgrade` will run `uv pip install --upgrade` with all the things we just collected as the argument

This should be a bit faster than @rosmur's solution which will instead run a `uv pip install --upgrade` once for each package. Could be a bit faster with `uv pip list --outdated` and it should be 
(Someone might want to double check. I always mess up `xargs` but this seemed to work. But doing `xargs --verbose` makes it look like the right command). Note though that this isn't exactly "safe" and you can probably run it multiple times and you'll see things upgrade and downgrade. 

This is pretty useful when you don't have a requirements file. I have a lot of virtual environments and even user versions. Many distros of linux don't want you doing `pip` installs on the system python (for good reason) and using a `uv venv` in a home directory is just a great solution.  

---

_Comment by @The-Compiler on 2025-04-14 19:43_

@stevenwalton FWIW instead of parsing a format that was never intended to be parsed, you could use `uv pip list --format=freeze | cut -d= -f1` or `uv pip list --format=json | jq -r '.[].name'` instead.

---

_Comment by @stevenwalton on 2025-04-14 20:12_

@The-Compiler I wouldn't say "never intended to be parsed", and certainly don't think freeze is any less parsable. We accomplished it with the intended parsing tools.

I'd still pick my solution over the `jq` version for higher portability. `jq` is not default on systems but `tail` and `cut` are , as they're part of [Core Utils](https://en.wikipedia.org/wiki/List_of_GNU_Core_Utilities_commands). `xargs`, which all solutions still require, is mandatory in [POSIX commands](https://en.wikipedia.org/wiki/List_of_POSIX_commands) so you'll have extreme compatibility (all linux, most BSD, and mac. Should work even on Alpine and busybox but I haven't verified and I'm pretty sure `uv` doesn't work there anyways. Though I'd love to have support for it in [`termux`](https://termux.dev/en/))

But we're also splitting hairs at this point. The solutions all work. There's even more and we could even write a one pipe `xargs` solution but why be less readable haha. 

---

_Comment by @rosmur on 2025-04-16 06:30_

@stevenwalton interesting, but I tried making your command a zsh alias and I get the following error:

`cut: option requires an argument -- d`

any ideas why? It works fine when run directly in zsh

---

_Comment by @stevenwalton on 2025-04-17 00:43_

@rosmur I can't replicate on my side. If you can post I can tell you the error. 

But I have a suspicion! I'd guess that this has to do with [quotes](https://mywiki.wooledge.org/Quotes). 
```bash
# Gives error message `cut: option requires an argument -- 'd'`
alias uvupdate='uv pip list | tail -n +3 | cut -d ' ' -f1 | xargs uv pip install --upgrade'
# Works as intended
alias uvupdate="uv pip list | tail -n +3 | cut -d ' ' -f1 | xargs uv pip install --upgrade"
# ALSO Works as intended
alias uvupdate='uv pip list | tail -n +3 | cut -d " " -f1 | xargs uv pip install --upgrade'
```

Though, I would suggest another solution if you want to edit your rc file. Use a function!

```bash
# Extend uv command to include `pip install --upgrade --all` 
function uv() {
   # Check that the command `uv` exists
   if [[ $(command -v "uv") ]];
   then
      # We only want to extend the command when given this specific argument
      if [[ "${@}" == 'pip install --upgrade --all' ]];
      then
         uv pip list \
            | tail -n +3 \
            | cut -d ' ' -f1 \
            | xargs uv pip install --upgrade
      # Otherwise just take the arguments and pass them back to uv as normal
      else
         command uv "$@"
      fi
   else
      echo "Command 'uv' not found in PATH"
   fi
}
```
You can find the function [here in my dotfiles](https://github.com/stevenwalton/.dotfiles/blob/master/rc_files/zsh/aliases.zsh#L386) <sub><sup>Sorry for the commit noise. I forgot it does that... 🤦 </sup></sub>

---

_Comment by @modbender on 2025-05-03 15:45_

Poetry has a very nice update command
https://python-poetry.org/docs/cli/#update
If this is a drop in replacement why is this not supported?

---

_Comment by @notatallshaw on 2025-05-03 16:29_

> Poetry has a very nice update command
> https://python-poetry.org/docs/cli/#update
> If this is a drop in replacement why is this not supported?

1. uv isn't a poetry drop in replacement
2. The equivalent command to `poetry update` is ~~`uv sync`~~ `uv sync --upgrade`
3. This issue is about the pip-like CLI, not the project CLI (the poetry equivalent)
4. Pip does not support upgrading all packages in an environment
5. The best practise approach to upgrading all your packages with a pip-like API is to use pip compile and pip sync: [issuecomment-2740995346](https://github.com/astral-sh/uv/issues/1419#issuecomment-2740995346)

Perhaps the issue title should be updated to reflect that this request was raised prior to the project CLI and is about the pip-like CLI?

---

_Comment by @modbender on 2025-05-03 16:43_

I agree with all others, but not 2
In a project like below

```
[project]
name = "manga-extractor"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "autopep8>=1.7.0",
    "beautifulsoup4>=4.10.0",
    "cloudscraper>=1.2.66",
    "lxml>=5.4.0",
    "pillow>=10.3.0",
]
```
running both `uv sync` and `poetry update` gives different outputs
![Image](https://github.com/user-attachments/assets/78d5294f-4713-47f4-ad02-c7340a7304e3)

though I thought it was upgrading in pyproject.toml too but now that I tried it, it didn't.

---

_Comment by @notatallshaw on 2025-05-03 16:48_

> I agree with all others, but not 2

Sorry, I don't actually use the project CLI very much, I guess it's `uv sync --upgrade` or `uv lock --upgrade` (https://docs.astral.sh/uv/concepts/projects/sync/#upgrading-locked-package-versions), I've updated my comment.

---

_Comment by @konstin on 2025-05-04 21:14_

Through this thread, different features were discussed so I want to enumerate some of them and their solutions, workarounds or tracking issues:

1. You're in project mode (you have `pyproject.toml`/`uv.lock`), and want to update all packages to compatible new versions (without changing `pyproject.toml`):

    ```
    uv sync --upgrade
    ```
2. You're in project mode, and want to upgrade all packages to the latest version, updating `pyproject.toml`: This is the feature request in #6794, tentatively dubbed `uv upgrade`.
3. You want to upgrade all packages in a venv:

    ```
    # With jq
    uv pip list --format=json | jq -r '.[].name' | uv pip install --upgrade -r -
    # With Unix utils
	uv pip list | tail -n +3 | cut -d ' ' -f1 | uv pip install --upgrade -r -
	```

    Note that this can potentially downgrade packages when there are dependency conflicts and it will not remove packages not needed anymore.
4. You want to upgrade the originally requested installed packages in a venv. See https://github.com/pypa/pip/issues/4551 and https://github.com/pypa/pip/pull/10491 for prior discussion on this. The difficulty here is to track what was requested; An alternative can be using `uv pip compile --upgrade`/`uv pip sync` with a list of packages as input, or the more brute `uv venv && uv pip install -r requirements.txt`.

---

_Comment by @stevenwalton on 2025-05-05 21:36_

@konstin , I think there are two main categories: project virtual environments and system virtual environments

Project venvs seem much easier to manage and really what the `uv` project has explicitly been about. 

System venvs are a bit of a different ballgame, but a need by a larger number of people (anyone running Linux or BSD/OSX). You don't want users to mess with system pythons and users don't want a copy of each Python and each package cluttering their system. `uv` effectively solves this. 

But there's a big difference in package management. In projects you want strong version control. In system you want latest and containerization. One way to solve this is make the venv with `uv venv --seed` but this is frequently missed and it's good to have an alternative.

I'd also strongly suggest against `jq`. I have an alternative solution [here](https://github.com/astral-sh/uv/issues/1419#issuecomment-2811350228). This helps avoid dependency creep. `tail`, `cut`, and `xargs` (not necessary) will almost certainly be on anyone's system 

---

_Comment by @skhaz on 2025-05-05 21:50_

Is jq not a single binary?

---

_Comment by @stevenwalton on 2025-05-05 22:49_

@skhaz, the point is `jq` needs to be installed. It's not about size, it is about portability. 

My solution works on ***all nix and BSD platforms***. 

The `jq` command works on all nix and BSD platforms ***that have `jq` installed***

---

_Comment by @shawnoster on 2025-05-06 18:33_

For Windows users developing in PowerShell the usage of `jq` is a more portable solution. While many *also* develop in WSL that is an `and`, not an `or`. It's easier to install `jq` vs. the suite of busybox tools in stock PowerShell 7.

Given the growing ubiquity of `jq` as a dependency for working with the output of popular CLIs, and its inclusion in all GitHub runner images (`ubuntu-*`,  `macos-*`, and `windows-*`), the debate around which tools to work with the JSON for a **cross-OS tool** like `uv` seems moot?

---

_Comment by @moble on 2025-05-06 21:41_

What seems moot to me is the idea that we should even care about which tool is ubiquitous when there is one that literally everyone interested in this thread will have: `uv`.  It shouldn't take some obscure command with multiple pipes to just achieve the goal; it should be built in to `uv`.

(And for what it's worth, I have linux boxes that don't have `jq` installed, but do have `tail`, `cut`, and `xargs`.)

---

_Comment by @konstin on 2025-05-06 22:47_

I've updated the post to include both the jq version and the Unix tools version, at the user's choice.

---

_Comment by @skhaz on 2025-05-06 22:57_

This feature shouldn’t rely on third-party tools—or even system utilities. It’s really a shame; without it, Poetry will continue to dominate.

---

_Comment by @shawnoster on 2025-05-06 23:08_

@moble @skhaz 100%, debating the best hack only takes us further from what we actually need.

---

_Comment by @notatallshaw on 2025-05-07 03:20_

So the request to the uv team can be clear, are people asking for a version of "3." to be built into uv as described in https://github.com/astral-sh/uv/issues/1419#issuecomment-2849420114, accepting the following limitations:

1. `uv pip` CLI only (unrelated to the project CLI / poetry equivalent)
2. Not match any existing feature in pip (which has so far rejected such a feature due to the below limitations)
3. Will attempt to upgrade all packages even if they weren't requested by the user
4. Will not track if a user requested an extra
5. May downgrade packages due to conflicts

? (Thumbs up for yes, or reply with clarification and thumbs up somebody else's clarification if they've described what you need).

As mentioned in https://github.com/astral-sh/uv/issues/1419#issuecomment-2849420114, for the poetry equivalant see `uv sync --upgrade` / https://github.com/astral-sh/uv/issues/6794.


---

_Label `uv pip` added by @notatallshaw on 2025-05-08 03:34_

---

_Comment by @qiuxiaomu on 2025-05-15 21:26_

#6794 is really confusing to me; the prior art listed, particularly poetry one, is basically the same as poetry update, but with a bit more functionality. The title seemed to state that updating the version pins in pyproject, i.e., not updating the actual dependencies but the version pins only, in the pyproject file ( hence one may run the true "update/upgrade" command to upgrade the deps with the latest mandate from deps themselves). 

With konstin's 2, it is still a fairly remote use case for me: I can't see how upgrade all deps to latest possible and updating pyproject to whatever versions are installed, NOT breaking the dependencies ( in that the deps themselves have dictated the latest, which is different from a pip latest and updating to pip latest surely risks breaking the deps). 



---

_Comment by @qiuxiaomu on 2025-05-16 13:08_

Also, the help pages for `sync`, `lock`, and `tree` show that all of them come with:

```
Resolver options:
  -U, --upgrade                            Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
  -P, --upgrade-package <UPGRADE_PACKAGE>  Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
      --resolution <RESOLUTION>            The strategy to use when selecting between the different compatible versions for a given package requirement [env: UV_RESOLUTION=] [possible values: highest, lowest, lowest-direct]
      --prerelease <PRERELEASE>            The strategy to use when considering pre-release versions [env: UV_PRERELEASE=] [possible values: disallow, allow, if-necessary, explicit, if-necessary-or-explicit]
      --fork-strategy <FORK_STRATEGY>      The strategy to use when selecting multiple versions of a given package across Python versions and platforms [env: UV_FORK_STRATEGY=] [possible values: fewest, requires-python]
      --exclude-newer <EXCLUDE_NEWER>      Limit candidate packages to those that were uploaded prior to the given date [env: UV_EXCLUDE_NEWER=]
      --no-sources                         Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local
                                           path sources
```

but apparently `tree` isn't as pronounced as the other two and probably having one unified interface for these commands would be better. 

The intro for `upgrade` was to ignore the pinned version in any existing output files; are we referring to existing `pyproject.toml` file here? I am asking because apparently this sub-command respects the version pins and only update to the permissible versions, so the output file, i.e., `pyproject.toml`, is not ignored. 

---

_Comment by @notatallshaw on 2025-05-16 13:45_

@qiuxiaomu you're talking about the project CLI, so I think you want https://github.com/astral-sh/uv/issues/6794 or a new issue. 

This issue was created prior to the project CLI existing, and is therefore about `uv pip`. 

---

_Comment by @cesarqdt on 2025-05-22 15:25_

This seems like a related topic/issue. I'm experiencing no changes when I run `uv lock --upgrade-package ...` with the version, even when there are newer versions of the package available. The output is always: 
```
Resolved 194 packages in 75ms
Audited 190 packages in 0.03ms
```
Looking at the pyproject.toml the version does not change.

---

_Comment by @notatallshaw on 2025-05-22 15:28_

> This seems like a related topic/issue. I'm experiencing no changes when I run `uv lock --upgrade-package ...` with the version, even when there are newer versions of the package available. The output is always:

No, this is about the `uv pip` interface, you should make a new issue with a reproducible example.

---

_Comment by @Michallote on 2025-06-09 03:43_

@notatallshaw So does this mean it is available with the intended `uv` regular workflow? I think as long as all your dependencies are specified with `>=`, the command `uv sync --upgrade` will upgrade all packages. Will it not?

> > This seems like a related topic/issue. I'm experiencing no changes when I run `uv lock --upgrade-package ...` with the version, even when there are newer versions of the package available. The output is always:
> 
> No, this is about the `uv pip` interface, you should make a new issue with a reproducible example.



---

_Comment by @notatallshaw on 2025-06-09 04:06_

@Michallote it means you are posting in the wrong issue. You should read https://github.com/astral-sh/uv/issues/1419#issuecomment-2849420114

---

_Comment by @konstin on 2025-06-10 10:54_

For `>=` specifically, yeah `--upgrade` should work. Note that sometimes, when there are version conflicts, you may not get the latest version, but that is a general limitation that everyone including pip has, not one of `--upgrade`. (The classic example is that you depend on foo and bar, both with versions 1 and 2, and foo 2 depends bar 1: We can't select foo 2 and bar 2 at the same time)

---

_Comment by @Danipulok on 2025-06-11 04:01_

> This seems like a related topic/issue. I'm experiencing no changes when I run `uv lock --upgrade-package ...` with the version, even when there are newer versions of the package available. The output is always:
> 
> ```
> Resolved 194 packages in 75ms
> Audited 190 packages in 0.03ms
> ```
> 
> Looking at the pyproject.toml the version does not change.

@cesarqdt hi here. It seems I have the same "problem". The problem is that you can not update any/some of your packages because of other packages restrictions. For example, my situation:

Latest `pydantic` version as of now is `2.11.5`.
I have in `.venv`: 
```bash
$ uv pip show pydantic
Name: pydantic
Version: 2.9.2
```

And when I do:
```
$ uv add pydantic -U
Resolved 30 packages in 191ms
Audited 29 packages in 0.03ms
```

If I do `uv tree`, I get:
```
$ uv tree
Resolved 30 packages in 1ms
shazam-bot v0.1.0
├── aiogram v3.20.0.post0
│   ├── aiofiles v23.2.1
│   ├── aiohttp v3.11.18
│   │   ├── aiohappyeyeballs v2.6.1
│   │   ├── aiosignal v1.3.2
│   │   │   └── frozenlist v1.7.0
│   │   ├── attrs v25.3.0
│   │   ├── frozenlist v1.7.0
│   │   ├── multidict v6.4.4
│   │   ├── propcache v0.3.2
│   │   └── yarl v1.20.1
│   │       ├── idna v3.10
│   │       ├── multidict v6.4.4
│   │       └── propcache v0.3.2
│   ├── certifi v2025.4.26
│   ├── magic-filter v1.0.12
│   ├── pydantic v2.9.2
│   │   ├── annotated-types v0.7.0
│   │   ├── pydantic-core v2.23.4
│   │   │   └── typing-extensions v4.14.0
│   │   └── typing-extensions v4.14.0
│   └── typing-extensions v4.14.0
├── pydantic v2.9.2 (*)
├── pydantic-settings v2.9.1
│   ├── pydantic v2.9.2 (*)
│   ├── python-dotenv v1.1.0
│   └── typing-inspection v0.4.1
│       └── typing-extensions v4.14.0
├── shazamio v0.8.0
│   ├── aiofiles v23.2.1
│   ├── aiohttp v3.11.18 (*)
│   ├── aiohttp-retry v2.9.1
│   │   └── aiohttp v3.11.18 (*)
│   ├── anyio v4.3.0
│   │   ├── idna v3.10
│   │   └── sniffio v1.3.1
│   ├── dataclass-factory v2.16
│   ├── numpy v2.2.2
│   ├── pydantic v2.9.2 (*)
│   ├── pydub v0.25.1
│   └── shazamio-core v1.1.2
└── structlog v25.4.0
(*) Package tree already displaye
```

I'm not quite sure how to check versions restrictions in `uv.lock` or with commands (and if it's possible at all), but in `shazamio` there's a [hard](https://github.com/shazamio/ShazamIO/blob/c1a81f92f050b68893a4bc287ba82104bb6ab3c3/pyproject.toml#L25) requirement for `pydantic = "2.9.2"`.

To proof that this is the cause of the issue:
```bash
$ uv remove shazamio
Resolved 22 packages in 25ms
Uninstalled 8 packages in 181ms
 - aiohttp-retry==2.9.1
 - anyio==4.3.0
 - dataclass-factory==2.16
 - numpy==2.2.2
 - pydub==0.25.1
 - shazamio==0.8.0
 - shazamio-core==1.1.2
 - sniffio==1.3.1

$ uv add -U pydantic
Resolved 22 packages in 152ms
Prepared 3 packages in 2ms
Uninstalled 3 packages in 24ms
░░░░░░░░░░░░░░░░░░░░ [0/3] Installing wheels...                                                                                                                                                          warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.                                                                                                     
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 3 packages in 47ms
 - aiofiles==23.2.1
 + aiofiles==24.1.0
 - pydantic==2.9.2
 + pydantic==2.11.5
 - pydantic-core==2.23.4
 + pydantic-core==2.33.2
```

@zanieb hey!
Sorry for pinging. Is there a way/plan to maybe notify user in the result of `uv add -U <package>` or `uv lock -U` why packages update is not possible? I can create a new issue if it's needed and will help.

---

_Comment by @Danipulok on 2025-06-11 04:03_

> For `>=` specifically, yeah `--upgrade` should work. Note that sometimes, when there are version conflicts, you may not get the latest version, but that is a general limitation that everyone including pip has, not one of `--upgrade`. (The classic example is that you depend on foo and bar, both with versions 1 and 2, and foo 2 depends bar 1: We can't select foo 2 and bar 2 at the same time)

Noticed this just now. It's understandable and nice and should work like this. But when you execute the command and see 
```bash
$ uv add pydantic -U
Resolved 30 packages in 191ms
Audited 29 packages in 0.03ms
```
with no additional info it's not intuitive at all why there's no update and the user will probably think something is broken.

---

_Comment by @zanieb on 2025-06-11 15:28_

These comments aren't really on topic for this issue, I've opened a couple new issues for discussion

- https://github.com/astral-sh/uv/issues/13968
- https://github.com/astral-sh/uv/issues/13970

---

_Comment by @Danipulok on 2025-06-11 18:16_

> These comments aren't really on topic for this issue, I've opened a couple new issues for discussion
> 
> - https://github.com/astral-sh/uv/issues/13968
> - https://github.com/astral-sh/uv/issues/13970

@zanieb thank you very much! Really appreciate it!

---

_Comment by @Rikhil-Nell on 2025-12-17 05:15_

@bryant1410 

in the method you suggested

```
uv lock --upgrade
uv sync
```
I have noticed that the pyproject.toml doesn't reflect the update versions, any suggestions for this?

---

_Comment by @Danipulok on 2025-12-17 05:45_

@Rikhil-Nell this is okay and that's how it's supposed to work. Your project supports both 1.1 and 1.2 versions, for example. If you want to drop support for 1.1, then you have to manually mark it in `pyproject.toml`. But cases when this is actually needed are pretty rare

---

_Comment by @ba05 on 2025-12-17 21:02_

> [@bryant1410](https://github.com/bryant1410)
> 
> in the method you suggested
> 
> ```
> uv lock --upgrade
> uv sync
> ```
> 
> I have noticed that the pyproject.toml doesn't reflect the update versions, any suggestions for this?

Would be nice if there was an “—update-toml-deps” or similar argument.

---
