---
number: 6662
title: support installing uv to the project using a wrapper script
type: issue
state: open
author: DetachHead
labels:
  - wish
assignees: []
created_at: 2024-08-26T23:15:56Z
updated_at: 2025-02-24T15:03:14Z
url: https://github.com/astral-sh/uv/issues/6662
synced_at: 2026-01-07T13:12:17-06:00
---

# support installing uv to the project using a wrapper script

---

_Issue opened by @DetachHead on 2024-08-26 23:15_

re-raising https://github.com/astral-sh/rye/issues/944 here now that uv has many of rye's features:

tools like [pyprojectx](https://pyprojectx.github.io/) and [gradle](https://docs.gradle.org/current/userguide/gradle_wrapper.html) support being installed and run with a wrapper script, which is a script in the project root that gets committed which allows anybody to start working on your repo even if they don't have uv installed.

the script could be called `uw` (uv wrapper) or something, and users can run `./uw sync` which will automatically install uv to the project and then run the `sync` command

this also allows you to pin the uv version in your project

---

_Label `wish` added by @zanieb on 2024-08-27 01:48_

---

_Comment by @zanieb on 2024-08-27 01:49_

Sort of related to #6533 

I'm honestly kind of into this. It's not on the immediate roadmap though.

---

_Referenced in [astral-sh/uv#6679](../../astral-sh/uv/issues/6679.md) on 2024-08-27 11:36_

---

_Comment by @chrisrodrigue on 2024-09-19 01:05_

Yeah a wrapper script that lives in the root of the project would be really awesome.

https://github.com/astral-sh/uv/issues/6505 is also a cool idea. I know that binaries are frowned upon in git but I would make an exception for a cross platform/APE `uv` binary in my project root.

---

_Comment by @bluetech on 2025-02-10 08:55_

It's important that the wrapper script be able to:

- Install a specific version of uv
- Installs uv for the project only, e.g. to a `.uv/` directory in the project

This can help solve the boostrap problem for users with these requirements:

- Multiple projects
- Each project pins the uv version for consistency across devs/CI/deployment
- Projects are independent and may have differing uv pins

Current solutions I can think of are:
- What I use: Nix flakes + direnv. Sort of heavy weight for pure Python projects.
- `pipx` or `python -m venv .venv && .venv/bin/pip install uv==0.5.29` etc. Requires Python to already be available.
- Commit uv binary: bloats the repo, architecture specific. Not very nice.
- Write own wrapper script: possible, but would be nice if uv provides it.

(Would love to hear other solutions)

The installer script already allows to specify a version, checks preconditions and detects OS/architecture. If it could be made to install to a project directory instead of $HOME, and exec uv, that's pretty close. It will need to read some file to know which uv version to download (in Gradle projects that's `gradle/wrapper/gradle-wrapper.properties`).

> the script could be called uw (uv wrapper) or something

I would call it `uvw` following the `gradlew` precedent.

---

_Comment by @DetachHead on 2025-02-10 09:17_

> (Would love to hear other solutions)

the solution i'm using for now is [pyprojectx](https://pyprojectx.github.io/). i mentioned it in the OP but i didn't really make it clear that it can be used to manage your uv installation.

```toml
# pyproject.toml

[tool.pyprojectx]
main = ["uv"]
```

then once you [install pyprojectx to your project root](https://pyprojectx.github.io/#installation) you can run `./pw --lock` to generate a pyprojectx lockfile and `./pw uv sync` which also automatically installs uv to your project. then just commit the `pw` and `pw.bat` wrapper scripts

---

_Comment by @bluetech on 2025-02-10 09:28_

I didn't know about pyprojectx, it looks nice. It does seem to require that Python is already installed which I hope to avoid. Basically the request is for uv to subsume pyprojectx like it did every other Python packaging tool :)

---

_Referenced in [astral-sh/uv#11639](../../astral-sh/uv/issues/11639.md) on 2025-02-20 09:17_

---

_Comment by @cliebBS on 2025-02-20 14:47_

I actually like the way that `sbt` handles this more than Gradle's approach.  `sbt` reads a `build.properties` file in the project and, if the SBT version in that file differs from its current version, it downloads the version specified in `build.properties` and runs that version instead.  This removes the hacky wrapper scripts that need to be checked into every repo and that prevent the tooling from working in any subdirectory within the project.  Instead, you just have a global `sbt` installation that can manage multiple versions of itself.

I feel like `uv` could build upon it's currently existing Python installation management (`uv python install/list/uninstall`) to also manage multiple versions of itself outside the context of the current project, and then utilize an `exec` call to replace itself with the correct version when necessary.

---

_Comment by @bluetech on 2025-02-20 15:43_

> you just have a global sbt installation that can manage multiple versions of itself.

But how do you get the global `sbt` to all team members? That's the problem that a wrapper script solves.

If I were to list a set of dream "requirements" for a wrapper script:
- It should be reasonably small in size (not a big file).
- It should be reasonably readable/auditable in plain text (not binary).
- It should be something that's committed to git repos.
- It should read the required uv version from some file, ideally pyproject.toml but most likely another file that is simple to parse.
- It should be able to verify the uv binaries it downloads using hashes committed to the repo.
- It should have minimal system dependencies/requirements.
- It should be able to bootstrap on Linux/Windows/Mac.
- It should not add noticeable overhead when the needed uv is already available.

I realize that it's not likely possible to achieve all of these at the same time.

---

_Comment by @cliebBS on 2025-02-20 16:21_

You get it to team members by having them install whatever version of uv they like (that's new enough to have this feature) via the official install script, pipx, or any other supported install method, just like you would have them get Python for their systems already.  This approach removes the need for any wrapper script to be committed to any repo, since uv replaces it.  The uv version can be baked into `pyproject.toml`, and when it isn't set the system uv just runs normally like it does today.

---

_Comment by @DetachHead on 2025-02-20 21:55_

> You get it to team members by having them install whatever version of uv they like (that's new enough to have this feature) via the official install script, pipx, or any other supported install method

this means we need to worry about the user having both a version of python installed **and** a version of uv. 

i much prefer the wrapper script approach. it's so convenient to not have to worry about whether somebody already has a version of the tool installed or not. the only benefit to not having a wrapper script is that one less file is committed, but i don't really see how thats an issue?

---

_Comment by @cliebBS on 2025-02-21 15:07_

Except they don't need a version of Python installed at all.  Installing uv doesn't require Python as a pre-requisite when using the official installer script (and your proposed `uvw` wrapper would most likely be building on top of the official installation script), just curl/wget and a smattering of standard Linux/bash commands.

The problem with the wrapper script is that:

1. It needs to be added to *every* repo that uses uv (in a microservices setup, this could be 10s to 100s of repos)
2. It forces you to run all uv/uvx commands directly from the root of the project, even though uv/uvx is capable of being run from any place within the directory hierarchy of a project
3. It requires a second script to be checked in to support uvx (and additional scripts if, in the future, additional global executables are added to the uv distribution)
4. There is no good way to assess the version of the script for when you inevitably need to update it

My proposal, however:

1. Requires no modification to repos beyond registering the desired uv version in pyproject.toml (which is already required for any potential solution)
2. uv/uvx commands can be run from anywhere inside your project (or even outside the project with the `--directory` parameter), just like you would if you aren't pinning your uv version (ie, requires no change to normal user behavior or end-user documentation)
3. Allows future additional global executables to be added (like `uvx`) without requiring any changes to repos

---

_Comment by @DetachHead on 2025-02-21 20:38_

> Except they don't need a version of Python installed at all. Installing uv doesn't require Python as a pre-requisite when using the official installer script

i'm not a fan of using install scripts to install anything globally because if it fails it leaves you with a half installed version of the tool that's difficult to find and remove. for this reason whenever i install a tool globally, i refuse to do it unless it either comes from a package manager or has an installer. so with uv, that means my only option is to install it from pypi, which means i already need a version of python installed.

i like the idea of a project level wrapper script because whenever there's an issue you can easily delete the version that was installed to the project and re-run it. this is what i've been doing with pyprojectx for a while now and it's made working with python projects so much less painful. i remember having to deal with corrupted installations of poetry and other tools that it's made me never want to install something globally ever again unless i absolutely need to, especially if it comes from an install script. 

> The problem with the wrapper script is that:
> 
> 1. It needs to be added to *every* repo that uses uv (in a microservices setup, this could be 10s to 100s of repos)

it's not like you'd be forced to use the wrapper script, and there would be a command you can use to create and update it which would make it easier to manage.

> 2. It forces you to run all uv/uvx commands directly from the root of the project, even though uv/uvx is capable of being run from any place within the directory hierarchy of a project
> 3. It requires a second script to be checked in to support uvx (and additional scripts if, in the future, additional global executables are added to the uv distribution)

i guess this depends on how exactly the wrapper script is implemented. if it works like pyprojectx this isn't an issue because it installs its tools to a location in the project root that can be "activated" similar to a venv but it can also be added to your `PATH`. in my projects i can configure vscode to do this automatically:

```jsonc
// .vscode/settings.json
{
    "terminal.integrated.env.windows": {
      "PATH": "${workspaceFolder}/.pyprojectx/main;${env:PATH}"
    },
    "terminal.integrated.env.osx": {
      "PATH": "${workspaceFolder}/.pyprojectx/main:${env:PATH}"
    },
    "terminal.integrated.env.linux": {
      "PATH": "${workspaceFolder}/.pyprojectx/main:${env:PATH}"
    }
}
```

this way the user only has to run uv through the wrapper script once, then once its installed to the project they can run uv commands the usual way and forget about the wrapper script.

also, i've never used uvx before but [it sounds like a command you wouldn't use in a project anyway](https://docs.astral.sh/uv/guides/tools/#running-tools) if you care about pinning project dependencies 

> 4. There is no good way to assess the version of the script for when you inevitably need to update it

i don't see why you couldn't just check the version with a `--version` argument and update it with a separate command that just replaces the wrapper script with the new version. this is also how pyprojectx works

> My proposal, however:
> 
> 1. Requires no modification to repos beyond registering the desired uv version in pyproject.toml (which is already required for any potential solution)

i realise this is mostly personal preference, but i am a huge advocate of the wrapper script approach because it's made my life much easier and i think it would be great if uv supported it officially. but i realize not everyone wants to manage their projects this way which is why this feature would be fully optional

---

_Comment by @cliebBS on 2025-02-21 21:44_

While I don't have any experience with pyprojectx, I have worked with `gradlew` in the past and it was nowhere near as automatic as `sbt` makes things.  With `sbt`, you can't run the wrong version of `sbt` because it will always load the correct version based on your project settings, regardless of what version you have installed globally.

As for not liking magic `curl | sh` installation methods, I 100% agree with you.  However, in uv's case, it's at least available in homebrew already, though I can't speak for Linux or Windows package managers.  When it's not available there, you can always go to the GitHub releases page for the uv project and download the binaries for your OS+arch and unzip them on your path.  The tarballs only contain two files, one each for `uv` and `uvx`, so it's pretty hard to mess up an installation by hand, and once you have things installed, you're one `uv self update` away from the latest version.

Having `uv` emit the wrapper script is certainly a nice improvement over where Gradle used to be where you needed to download `gradlew` separate from the main Gradle distribution.  Maybe, if we went with that solution, we could have uv emit a warning if the `uvw` script was out-of-date or didn't match the current version of uv.  I would still prefer to manage all of my versions from `pyproject.toml`, though, so having the `uvw` version be separate kind of goes against that wish.

---

_Comment by @KotlinIsland on 2025-02-22 03:30_

> I have worked with `gradlew` in the past and it was nowhere near as automatic as `sbt` makes things. With `sbt`, you can't run the wrong version of `sbt` because it will always load the correct version based on your project settings, regardless of what version you have installed globally.

i'm no historian, but for as long as I have used gradle, the gradle version is specified in the `gradle/wrapper/gradle-wrapper.properties` file. and that is always automatically downloaded and used when you run `gradlew`

---

_Comment by @cliebBS on 2025-02-24 15:03_

> > I have worked with `gradlew` in the past and it was nowhere near as automatic as `sbt` makes things. With `sbt`, you can't run the wrong version of `sbt` because it will always load the correct version based on your project settings, regardless of what version you have installed globally.
> 
> i'm no historian, but for as long as I have used gradle, the gradle version is specified in the `gradle/wrapper/gradle-wrapper.properties` file. and that is always automatically downloaded and used when you run `gradlew`

Sorry, I was unclear what I meant.  I wasn't saying that the version of Gradle being requested by the wrapper wasn't specified anywhere, what I meant is that the `gradlew` script is itself a versionable artifact that lacks any easy way to keep up to date or to check if it is up to date.

Poetry shows why it important to have this file versioned:  A couple of years ago, Poetry changed the install script quite drastically (I think it was with version 1.2.0), and newer versions of Poetry stopped being installable with the older version of the install script.  There's nothing stopping uv from wanting to change its install process in the future, including to address a security issue.

---

_Referenced in [astral-sh/uv#14233](../../astral-sh/uv/issues/14233.md) on 2025-06-24 09:01_

---
