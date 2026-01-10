```yaml
number: 5802
title: "Suggestion: `uv bundle`, `uv build --release` or similar to create a contained executable a la pyinstaller, py2exe"
type: issue
state: open
author: matterhorn103
labels:
  - wish
assignees: []
created_at: 2024-08-05T23:26:31Z
updated_at: 2025-12-26T18:33:31Z
url: https://github.com/astral-sh/uv/issues/5802
synced_at: 2026-01-10T03:11:31Z
```

# Suggestion: `uv bundle`, `uv build --release` or similar to create a contained executable a la pyinstaller, py2exe

---

_Issue opened by @matterhorn103 on 2024-08-05 23:26_

I regularly use pyinstaller for a project, and the frequency of questions about it - or the task more generally of "compiling" an executable for distribution - around the web indicates to me that it's widely popular.

It would be incredibly cool if uv offered a `uv bundle` command (or whatever name) that essentially did what pyinstaller does and bundles together everything to afford an executable that contains the dependencies and interpreter and can be distributed to users as-is. And I think this falls into the realm of what things a "Cargo for Python" might be expected to be able to do.

I assume that pyinstaller, py2exe etc. are complex projects and implementing such functionality would be not trivial in the slightest, so I would have thought it would be a case of using `pyinstaller` itself in the background like the way that build backends are used.

Presumably there will one day be a `uv build` command but I assume that will be similar to `rye build` in that it will create an sdist or wheel. Maybe there could be a `uv build --executable` option, or `uv build --release` (by analogy to `cargo build --release`)?

---

_Renamed from "Suggestion: `uv bundle` or similar to create a contained executable a la pyinstaller, py2exe" to "Suggestion: `uv bundle`, `uv build --release` or similar to create a contained executable a la pyinstaller, py2exe" by @matterhorn103 on 2024-08-05 23:45_

---

_Comment by @matterhorn103 on 2024-08-06 00:08_

Some relevant links that I'm adding to keep track of them for myself as much as anyone else:

- It seems someone [asked for the same in Rye](https://github.com/astral-sh/rye/issues/325) previously
- ofek's [PyApp](https://github.com/ofek/pyapp) and Gregory Szorc's (indygreg) now-dead [PyOxidizer](https://github.com/indygreg/PyOxidizer) are other, more modern executable generators that I didn't know about until now; both seem simpler than pyinstaller, and both are written in Rust
- Gregory Szorc once did a [comparison](https://pyoxidizer.readthedocs.io/en/stable/pyoxidizer_comparisons.html) of the various alternatives to PyOxidizer
- One missing from that list, presumably because it's not generally applicable, is the [`pyside6-deploy` tool](https://doc.qt.io/qtforpython-6/deployment/deployment-pyside6-deploy.html), but it uses Nuitka in the background in any case
- Hatch seems to have implemented essentially this functionality, as part of a [Hatch plugin](https://hatch.pypa.io/latest/plugins/builder/binary/) that uses PyApp, and it is invoked there by having "binary" as a build target

---

_Label `wish` added by @zanieb on 2024-08-06 00:17_

---

_Comment by @chrisrodrigue on 2024-08-06 09:51_

To add… despite its popularity, `pyinstaller` suffers from slowness because has to unzip all the dependencies and the interpreter when the executable is launched. It offers users the option to display a splash image to distract them from the perceived slowness.

---

_Comment by @chrisrodrigue on 2024-08-06 10:03_

Python to Rust transpilation would be pretty cool and there’s at least one project out there working on this:
-  [pyrs](https://github.com/konchunas/pyrs)

Imagine writing idiomatic Python, transpiling it to Rust and compiling a native executable. My mind would be blown.

I don’t know how one could gracefully handle dependencies though. A lot of Python libs incorporate C extensions. Dealing with those would be extremely difficult, but there’s a DARPA program called [TRACTOR](https://www.darpa.mil/program/translating-all-c-to-rust) working on that.



---

_Comment by @paveldikov on 2024-08-06 14:47_

An equivalent of `pyinstaller` that actually understands standard `pyproject.toml`-driven projects and console/GUI script entrypoints (instead of being fixated on script development, and overcompensating with its dependency detection magic) would be huge.

(somewhat related: #5653)

---

_Comment by @my1e5 on 2024-08-07 09:42_

I want to give a mention to [Nuitka](https://github.com/Nuitka/Nuitka) which is serving me well. It's as straightforward as running
```
uv add --dev nuitka
```
```
uv run python -m nuitka --standalone src/main.py
```
which spits out an executable.

It's not really the right tool to be including with uv, it is essentially an alternate Python interpreter which compiles Python to C. But it's very easy to add and has worked well for me. And my experience with the developers has been very good. They were quick to add support for Rye when I encountered bugs - https://github.com/Nuitka/Nuitka/issues/2933

---

_Comment by @matterhorn103 on 2024-08-21 07:17_

@my1e5 why do you say that Nuitka would be unsuitable for inclusion with uv?

---

_Comment by @my1e5 on 2024-08-27 09:39_

@matterhorn103, I guess what I mean is that Nuitka is perhaps quite an opinionated way of packaging your Python code into an executable - as it is essentially an alternate Python interpreter which compiles Python to C. Whereas tools like Pyinstaller are more like a bundler - taking the Python interpreter, Python files and dependencies and packaging them into an executable. This is definitely more straightforward than the Nuitka approach, but it does make the Pyinstaller executable very easy to un-package and see the underlying source code (see https://github.com/extremecoders-re/pyinstxtractor). Which might be a caveat needed when including certain 'executable creators' within uv - you need to make users aware that their exe can easily be un-packaged.

In terms of licensing, Nuitka is MIT licensed. But it does also have a commercial tier - which may or may not complicate matters if Astral were to want to bundle it with uv, I don't know.

---

_Comment by @zanieb on 2024-09-03 23:23_

I think this is a duplicate of https://github.com/astral-sh/uv/issues/2799, though there's more discussion here.

---

_Comment by @martimlobao on 2024-09-04 09:18_

Also want to call out pex, a tool for generating .pex files (Python EXecutable) which are self-contained zipped executable Python environments containing sources, requirements, and dependencies.

https://github.com/pex-tool/pex

---

_Comment by @jbvsmo on 2024-10-05 21:15_

I am interested in a solution like this. I think the bundler should be somewhat coupled with `uv run` since you want to turn any entry point into an executable. 

Example
```bash
$ uv init --script foo.py
$ uv add requests --script foo.py
$ uv run foo.py
Hello from foo.py!
```

Then you could make an executable which is composed of
1. bootstrap code to download uv if needed and run it
2. target code 
3. uv executable **(optional)**
4. python executable **(optional from `uv python`)** 
5. populated venv **(optional)**

```bash
$ uv bundle foo.py -o foo --include-uv --include-python --include-dependencies
$ ./foo 
Hello from foo.py!
```
This above would be a self contained binary (roughly 30MB as uv is ~12MB and python ~17MB).

If downloading stuff on the fly is ok, the bootstrap code would just download uv, then uv would download python, install dependencies and run all during 1st execution.

It should also be possible to create binaries cross platform since uv supports install/sync with `--python-platform`

```bash
$ uv bundle foo.py -o foo.exe --python-platform windows
```

_In a 2nd iteration_, it would be nice to be have installers such as to create .dmg (mac) or an .exe windows installer file.




---

_Comment by @dbrtly on 2024-10-18 23:43_

what if the command was `uv run ruff --output-file ruff.uvx`
with the -o/--output agument as an indicator to create the binary file rather than execute this tool right now.
and when an output file is specified, then include python by default

---

_Comment by @stefanondisponibile on 2024-10-20 17:46_

I love this idea and @jbvsmo proposal for CLI usage.

I am a bit confused on the best choice for the bundling "backend" though, I see different options on the table and I'm not sure, how would you choose one over the other? Should maybe `uv` propose some protocol for each library to support bundling via `uv`?

This feature would be great also for bundling pyspark apps (thinking about pex support mentioned by @martimlobao here)

---

_Comment by @AKuederle on 2024-11-28 13:08_

I think the bootstrap option mentioned by @jbvsmo is almost achievable with the existing uv options.

You could do the following:

Within your project folder have a script (written in the scrippting language supported by your operating system) that does the following:

1. Unmanaged installation of uv (https://docs.astral.sh/uv/configuration/installer/#unmanaged-installations) can be used to just get the uv executable in a subdir of the project folder.
2. Setup the cache dir to be inside your project folder (https://docs.astral.sh/uv/concepts/cache/#cache-directory)
3. Then install Python
4. Install project deps
5. Use uv run to execute a script within the project enviroment. The easiest option would be to just "run" the package using a `__main__.py` within the project

Step 1 could of course be skipped, if the uv executable is already recognized within the specified subdir. And step 3-5 are basically what happens when you run uvx. So maybe this is actually a 2 line script.

The great thing is that this script would be project independent! So someone just would need to write a script for each operating system and you could throw these scripts in your project folder.

Then you just need to distribute this project folder and have people execute the script corresponding to their operating system.

This process could then of course be wrapped further by an installer, but seems relatively straight forward to at least address the cases of "Please provide an exe".


PS: If instead of creating a shell script to run the steps above, you use the darkmagic of [cosmopolitan](https://github.com/jart/cosmopolitan?tab=readme-ov-file) you could even create a single binary that dropped into a python package project anywhere makes it run by "double-clicking" the file.

---

_Comment by @jromal on 2024-12-04 11:43_

For me as a short term solution it would be enough to have `uv run` with some options.

For example I have done a program (as module) using PySide6.
I can do `uv run -m my_module` from the folder above the module and it works. Fast, efficient, impressive.
But there are two things that can help as an intermediate solution before a `uv bundle`.
* Read a compressed file for the full project: `uv run --source my_file.zip`
* Allow an option to hide or close the terminal for qt programs: `uv run --source my_file.zip --no-terminal`

With this solution I could distribute on Windows the files with "Inno Setup", adding `uv.exe`, the zip file, and maybe a ".bat" file with a logo to execute the `uv -run` command.




---

_Comment by @johnthagen on 2024-12-05 03:05_

As much as I like Pyinstaller (to me it's best of current options), one other thing that it suffers from is that anti-virus vendors are _always_ flagging generic Pyinstaller binaries as malware. This is (at least in part, I think) because Pyinstaller uses the same bootloader binary for all built applications, thus if someone bundles malware with Pyinstaller version X, all other users who bundle legitimate applications using Pyinstaller X can get flagged as malware too. I've seen this hit AV software across both Windows and Linux over the years. Not sure I have any ideas on how Astral could improve _that_ issue, but wanted to bring it up as a current (very annoying) real world challenge.

---

_Comment by @ceejatec on 2025-01-10 10:49_

It's not exactly the same thing, but I've suggested a relatively simple feature to `uv` that would solve at least a subset of the requirements of delivering a single downloadable executable for a Python program: #10465

Basically it would let `uv` serve as a limited runtime-only alternative to PyApp.

---

_Comment by @jromal on 2025-01-10 12:40_

Thanks for your request and for commenting here.

For me the final tool would be a third party tool, part of uv (for Windows, uv.exe, uvx.exe and the new one, let's call it `uvd.exe`).

And that third tool should be `PyOxidizer`. I do not know if you know it. But the single python binaries that Astral uses are a "byproduct" of PyOxidizer. A project from Gregory Szorc (indygreg on GitHub). It is a rust tool for byilding executables including a full python run time environment. Gregory had no time for any of the projects. The python binaries are now generated by Astral. PyOxidizer is not maintained since many months. I considered it as a future better alternative to Pyinstaller or Nuitka or ...

Maybe we could get a simple tool and a complex one with all the hooks for the different packages. Maybe could also have different license models to allow Astral to capitalize the investment. But should be continued as a natural evolution of uv.

But as a you say a wrapper of uvx would do it.

Let's wait and see. 

---

_Comment by @Nardol on 2025-01-10 13:21_

Unfortunately https://github.com/indygreg/PyOxidizer only support Python up to 3.10 as specified in the latest stable documentation, except that it looks promising. Latest commit is two months old but you say it is not maintained again?

---

_Comment by @paul-gauthier on 2025-01-17 20:10_

I would also be interested in more direct support for using uv as an "installer" for python CLI tools.

I've been using uv in novel ways for this purpose already. I recently wrote a bit about these approaches. Leaving the link here in case folks find it interesting or helpful:

https://aider.chat/2025/01/15/uv.html

---

_Comment by @thewh1teagle on 2025-01-27 02:02_

It will be useful if uv add `compile` command like in bun.js which will produce self contained executable.
The only important thing is NOT to work like pyinstaller and extract the contents but somehow load the interpreter into the memory, and tell cpython to run the bundled script directly from the memory.
Just like bun.js / nodejs does.

---

_Comment by @davidlanouette on 2025-02-10 20:00_

It's worth noting that it looks like PyOxidizer has been taken over ty Astral-sh - in the form of [python-build-standalone](https://github.com/astral-sh/python-build-standalone).

So, this issue can likely be closed.

---

_Comment by @zanieb on 2025-02-10 20:06_

No, we don't own PyOxidizer — we're just maintaining the standalone Python distributions. These ideas are separate.

---

_Comment by @davidlanouette on 2025-02-10 20:12_

Interesting.  That's not how I interpreted the [blog post](https://astral.sh/blog/python-build-standalone)

> TL;DR: On 2024-12-17, we'll be taking stewardship of [python-build-standalone](https://github.com/indygreg/python-build-standalone), [Gregory Szorc's](https://gregoryszorc.com/) foundational project for building and installing portable Python distributions. You can read Gregory's own announcement [here](https://gregoryszorc.com/blog/2024/12/03/transferring-python-build-standalone-stewardship-to-astral/).

But, it does explain why the releases seem to only contain python distributions.


---

_Comment by @zanieb on 2025-02-10 21:07_

Yes we are the owners of `python-build-standalone`, but not PyOxidizer  — that's built on _top_ of the standalone distributions.

---

_Comment by @jdegenstein on 2025-02-14 23:14_

This is a bit off topic, but I had a related discussion on discord about this topic and thought I would share A solution to MY problem that could be relevant to others. I feel that `python-build-standalone` has huge potential QoL improvements for the python package management ecosystem because it (finally) seems to attempt to address the local vs. global installation "problem".

My problem statement: create a zipped folder with standalone python, standalone packages and their dependencies that I can distribute to other users ("download, unzip and run"). Non-goals: a single executable -- for me a folder containing lots of subdirectories/files is fine but an obvious "entry point" is desirable. Obviously the result will be OS and CPU type specific.

Steps I followed:
1. Download relevant https://github.com/astral-sh/python-build-standalone/releases and unzip
2. chdir to python subdir
3. execute e.g. `python.exe -m pip install uv` (there is no specific need to use uv by the way)
4. execute e.g. `python.exe -m uv pip install <somepackage>`
5. for my application I created `somepackage.bat` (one directory "up") containing `call python/python.exe -m <somepackage>`. Obviously shell scripts would be useful on other platforms, and I am sure there are other solutions as well.
6. zip the parent folder and distribute to others with same OS and CPU type

The reason I like this solution is multifold (1) uses standard package installation methods (2) everything (?) is local to the folder (3) no e.g. pyinstaller-hooks-contrib to maintain. Are there downsides to this approach? Probably, but in my quick testing did not run across any issues -- it "just worked". This is contrast to my prior experiences with nuitka and pyinstaller which work perfectly for projects with pure python dependencies, but can require more "handholding" when that is not the case.

Finally back to the topic of this post -- this would be very nice if supported by uv directly in some form.

p.s. thanks to @geofft for their help

---

_Comment by @jromal on 2025-02-15 08:10_

The creator of the standalone python (Gregor Szorc) did it for his tool PyOxidizer. A rust substitute of pyinstaller or Nuitka. 

I am repeating when I can that Astral should take it and finish it. Gregor (indygreg) has no time and his work was really good. Just unfinished. But he did one of the biggest changes in python. The standalone python. Do not let his work disappear. 

---

_Comment by @dbrtly on 2025-02-15 08:28_

@jdegenstein it sounds like you want to do `uv tool install my_app` but have it work even if uv is not installed on the client machine. 

---

_Comment by @charliermarsh on 2025-02-15 13:45_

(@jromal -- incase you haven't seen, we did assume stewardship of python-build-standalone: https://github.com/astral-sh/python-build-standalone)

---

_Comment by @matterhorn103 on 2025-02-15 16:46_

Since I originally created this issue it's been very popular, and it's also seen many comments offering suggestions.

Naturally it is always a bit presumptuous to attempt to speak on behalf of a diverse and anonymous group, so maybe I'll be contradicted here, but I reckon that the vast majority of those who have voted for this issue or chimed in are united in hoping for something defined *not* by its technical implementation but by key *behaviour* – something that is:

- a single executable (or appears to be)
- easy-to-use by non-technical users i.e. it it looks and behaves like an application generally does
- easily distributed through sharing the executable itself i.e. it is fairly self-contained and portable (maybe cross-OS, maybe not)
- not less performant than running the original Python code with a normal interpreter

This is essentially about being able to take a Python program on our machines and give it to non-programmers, right? It could be achieved with a variety of technical solutions, as shown by the slew of existing tools, and by people's contributions in this thread.

I kind of want to clarify that while many people have been advocating for a wide variety of things, this issue as originally written wasn't advocating for a particular technical solution or requesting anything beyond that listed above.

Going by past form, no doubt any `uv` implementation would be an excellent one.

(In the original post I suggested that maybe it would use `pyinstaller` in the background, but that was mostly because it seemed to me a bit rude to ask the team to write a whole new thing from scratch at a time when they were still getting core functionality up and running.)

---

_Comment by @tjnd89 on 2025-02-15 17:45_

@matterhorn103 FWIW, this is exactly my use case. I'm building a an "app" for non-programmers. I use uv with hatch to build the distributions. I have a custom hatch build script that kicks off pyinstaller to build the "executable". In my case, I'm using the "one_dir" option because along with the exectuable I have to ship various data files that are needed. One thing I ran into is that running pyinstaller standalone from the command line worked great. My custom build script that issued the same command line arguments was failing to find all the dependencies because of uv's build isolation. I had to set 

```
[tool.uv]
no-build-isolation = true
```

and then it all worked well. Here's the snippets of relevant code.

hatch_build.py

```
import subprocess

from hatchling.builders.hooks.plugin.interface import BuildHookInterface


class CustomHook(BuildHookInterface):
    """USed to kick off Pyinstaller as part of a standard uv build.
    """
    def initialize(self, version, build_data):
        cmd_options = ['pyinstaller',
                       '--noconfirm',
                       # add your command line options here
                       'blah.py'   # your script you are wrapping.
                       ]

        subprocess.run(cmd_options, check=False)
```

pyproject.toml

```
[tool.hatch.build.targets.sdist.hooks.custom]
    dependencies = ["pyinstaller"]
```

This should get you what want I think.

---

_Comment by @jdegenstein on 2025-02-16 02:51_

@dbrtly Yes, that is correct -- in my experience with developing apps for new python users or python beginners as soon as the phrase "virtual environment" is uttered about half of the potential users will just give up (again my stated goal for my users is "download (my app), unzip and run"). Single executables are nice, as are installation wizards, but that introduces another layer of OS specific considerations that are not worth it for me (yet).

---

_Comment by @dbrtly on 2025-02-17 09:45_

It seems that you gave to draw the line somewhere and make an assumption about pre-requisites on the target machine: git? Is user local admin? Is the user sophisticated enough to use a cli tool at all? Is pyinstaller available? Is uv available?

You could go a long way with something like this:

```
# install uv and put it on path
uv tool install my_app \
  --default-index https://custom.pkg.dev/index 
```

Am curious about what specific features you can see potential for uv to improve on for this user experience. 

---

_Comment by @chrisrodrigue on 2025-02-25 14:11_

I posted some ideas over in #11746 about what I think a `uv layout` command might look like.

`uv bundle` would be distinct from `uv layout` and both would be valuable to developers.

From [my comment](https://github.com/astral-sh/uv/issues/11746#issuecomment-2681843706):
> I think the intent of `uv bundle` is to be able to combine an app, its dependencies, and Python for non-developer end users. It would produce an executable binary, likely having a GUI or CLI. This would enable devs to distribute their apps without relying on users to know what uv, Python, PyPI, or virtual environments are.
>
>The intent of `uv layout` ([inspiration](https://learn.microsoft.com/en-us/visualstudio/install/create-an-offline-installation-of-visual-studio?view=vs-2022#step-2---create-a-local-layout)) is to be able to combine uv, packages, and Python for developers. It would produce a directory, likely having a `uv.toml` to configure uv to work with that directory layout when invoked. This would enable devs to create projects without relying on an internet connection.
>
>Both of these commands would be valuable to developers, with `uv bundle` artifacts targeted at users and `uv layout` artifacts targeted at developers. Both commands could read a `pyproject.toml` to produce the desired artifact for the current platform by default, or take any number of `--platform <target-triple>` options (or `--platform all`) to produce artifacts for different platforms.

`uv compile` could also be good idea, distinct from both `uv bundle` and `uv layout`. It would be similar to `uv bundle`, but it might produce an artifact more akin to that of `nuitka` rather than `pyinstaller` and would be much more complicated to generalize and implement.

---

_Comment by @chrisrodrigue on 2025-02-25 20:36_

Summary of some of the ideas mentioned. The command names are flexible but I'm just using these for now to tell them apart 🙂

| Command | Use-case | Behavior | Similar to | Implementation(s) | Issue(s) |
| --- | --- | --- | --- | --- | --- |
| `uv bundle` | Create a portable executable for users | Outputs a platform-specific executable or [APE](https://justine.lol/ape.html) **with embeddings**  such as Python interpreter and package dependencies | [pyinstaller](https://github.com/pyinstaller/pyinstaller), [pyapp](https://github.com/ofek/pyapp), [pyoxy](https://github.com/indygreg/PyOxidizer) | On-disk execution (slower?), [in-memory execution](https://en.wikipedia.org/wiki/In-memory_processing) (faster?) | #5802 |
| `uv compile` | Create a portable executable for users | Outputs a platform-specific executable or [APE](https://justine.lol/ape.html) **without embeddings** | [Nuitka](https://github.com/Nuitka/Nuitka), [py2wasm](https://wasmer.io/posts/py2wasm-a-python-to-wasm-compiler) | Compilation/transpilation | #5802 |
| `uv layout` | Create a portable Python distribution for developers | Outputs a directory that contains platform-specific uv, Python interpreter, and wheels | [WinPython](https://winpython.github.io/#overview), [Anaconda](https://docs.anaconda.com/anaconda/release-notes/), [ActiveState](https://github.com/ActiveState/cli) | `fs::create_dir` | #11746 |

---

_Comment by @paveldikov on 2025-02-26 11:24_

@chrisrodrigue `uv layout` can be approximated by stitching together `uv venv --relocatable` with `uv python download` + a launcher script that rewrites `pyvenv.cfg` (because the base interpreter definition is not portable -- you can specify relative paths but those are implicitly relative to the current working directory)

I have done this for a proof-of-concept and, whilst clunky, it definitely does work. In fact, by repackaging its output into something like an RPM/DEB/Choco/pick-your-favourite-OS-package-manager -- or a self-extracting executable -- you end up with a passable (though non-optimised) `uv bundle` too.

---

_Comment by @chrisrodrigue on 2025-03-06 11:35_

> [@chrisrodrigue](https://github.com/chrisrodrigue) `uv layout` can be approximated by stitching together `uv venv --relocatable` with `uv python download` + a launcher script that rewrites `pyvenv.cfg` (because the base interpreter definition is not portable -- you can specify relative paths but those are implicitly relative to the current working directory)
> 
> I have done this for a proof-of-concept and, whilst clunky, it definitely does work. In fact, by repackaging its output into something like an RPM/DEB/Choco/pick-your-favourite-OS-package-manager -- or a self-extracting executable -- you end up with a passable (though non-optimised) `uv bundle` too.

Interesting, thanks @paveldikov. `uv python download` doesn’t appear in the [docs](https://docs.astral.sh/uv/reference/cli/#uv-python), but I’ll try experimenting with it today. 

An open question I have about venvs… how can one install packages from one venv into another? AFAIK, there is no way to do this because you need wheels to do real installation into a venv. 

You could add the source environment to PATH and make new venvs with `--system-site-packages` to use packages from the source environment, but there are downsides:
- `--system-site-packages` is an all or nothing approach. You get all packages regardless of whether you need all of them.
- System site packages aren’t installed to the venv, so you can’t pack up your project to share.
- All bets are off with the tooling. You need to manually check that any dependencies in `pyproject.toml` are in the source environment you’re using, which adds toil.

These reasons are why I advocate for using wheels with layouts. However, venvs may be better for bundles since they compress better. The checksum of the bundle executable should be good enough for attestation.

---

_Comment by @chrisrodrigue on 2025-03-06 15:52_

I tried running `uv pip download` and `uv python download` to manually create a layout that can be bundled/packed/archived, but alas, neither of these commands exist.

First, you will want `uv`:
```
curl -LSsf uv.zip https://github.com/astral-sh/uv/releases/download/0.6.4/uv-x86_64-pc-windows-msvc.zip | tar -x
```

To grab wheels in lieu of `uv pip download`: 
```
python -m pip download -r requirements.txt -d pypackages
```

To grab the interpreter in lieu of `uv python download`:
```
curl -LSsf https://github.com/astral-sh/python-build-standalone/releases/download/20250212/cpython-3.13.2+20250212-x86_64-pc-windows-msvc-install_only_stripped.tar.gz | tar -x
```

Configuring this `uv` to use `python` and `pypackages` is the real bounty 😁

I feel like a `uv.toml` adjacent to the executable would be appropriate for configuration, to set this `python` directory for the interpreter and `pypackages` directory as an index. Another setting to add `uv` to the system PATH on invocation would be sweet. A no setup, offline, portable Python distro that you can use as batteries for Python development anywhere offline on your target platform, or wrap up into an executable to share with others.

---

_Comment by @paveldikov on 2025-03-06 19:04_

Sorry -- it's `uv python install`... some poetic licence is necessary when citing commands off by heart 🙃

It will give you a known-good relocatable python site.  You can then manipulate that as you please, e.g. by further relocating it to sit inside of a relocatable venv. And then manipulate `pyvenv.cfg` accordingly.

p.s. and on posix, re-point the venv's `bin/python*` symlinks accordingly, e.g. to `../interpreter/bin/$1`

---

_Comment by @chrismatix on 2025-03-07 13:13_

>  @martimlobao Also want to call out pex, a tool for generating .pex files (Python EXecutable) which are self-contained zipped executable Python environments containing sources, requirements, and dependencies.
> https://github.com/pex-tool/pex

To anyone who was looking into this option: I put together a recipe repository for bundling uv workspaces by compiling their dependencies using `uv pip compile` and then using that lock file to build an executable with `pex`: https://github.com/chrismatix/uv-pex-monorepo

If you check it out, please let me know about any edge cases it does not accommodate!

---

_Comment by @Lauszus on 2025-03-07 14:52_

> > [@martimlobao](https://github.com/martimlobao) Also want to call out pex, a tool for generating .pex files (Python EXecutable) which are self-contained zipped executable Python environments containing sources, requirements, and dependencies.
> > https://github.com/pex-tool/pex
> 
> To anyone who was looking into this option: I put together a recipe repository for bundling uv workspaces by compiling their dependencies using `uv pip compile` and then using that lock file to build an executable with `pex`: https://github.com/chrismatix/uv-pex-monorepo
> 
> If you check it out, please let me know about any edge cases it does not accommodate!

Nice. I'm using something similar in production. Hopefully pex would support uv.lock directly someday.

---

_Comment by @luigi311 on 2025-03-25 20:01_

Great ideas guys. I went with something similar on my project. `uv build` which generates a whl file i then used pex on that whl and then finally used fpm to generate a deb for me to distribute. Hopefully something built into uv comes that makes this more simple but for now this works without complicating my source files with duplicate files that need to be manged. Can find my initial implementation here https://github.com/luigi311/immich_upload_daemon/blob/d1ec12c369ab4192e0ba88653010e4a0968e6e26/.github/workflows/build.yml



---

_Comment by @TanixLu on 2025-06-03 17:48_

@AKuederle My thoughts align closely with yours: create a bootstrap-style APE that downloads `uv`, installs Python, fetches dependencies, and executes the source code. I've already implemented this in a package called [pyfuze](https://github.com/TanixLu/pyfuze).

---

_Comment by @blais on 2025-06-03 19:18_

> [@AKuederle](https://github.com/AKuederle) My thoughts align closely with yours: create a bootstrap-style APE that downloads `uv`, installs Python, fetches dependencies, and executes the source code. I've already implemented this in a package called [pyfuze](https://github.com/TanixLu/pyfuze).

I don't think this is what's needed; this requires internet access and (correct me if I'm wrong) doesn't build binaries.
What's needed:
- pre-build all the binaries
- compute the transitive closure of all desired packages
- include them all in some archive format
- make it possible to import from the archive format
- create a stub at the front of it to make it runnable
The idea is that the package should be runnable without a python install and without having to install stuff from the internet (so it can be run at scale on the cloud / in a VPC)



---

_Comment by @TanixLu on 2025-06-04 02:58_

> * The idea is that the package should be runnable without a python install and without having to install stuff from the internet (so it can be run at scale on the cloud / in a VPC)

This is a trade-off. If everything is prepared in advance instead of being downloaded online, the compressed package will be large in size, and cross-platform compatibility will become problematic. This undermines the advantages of APE's cross-platform capability and uv's fast dependency downloading. For users without programming experience, having a single file that runs on all platforms is incredibly convenient. If pre-prepared cross-platform uv, Python, and dependencies are required, and APE selects them at runtime based on the operating system, the package would be even larger.  

With the current version of [pyfuze](https://github.com/TanixLu/pyfuze), you could run it once on a specific platform and then recompress it into a package, eliminating the need for an internet connection. However, the trade-off is that the application becomes platform-specific and larger in size. Perhaps I should add appropriate options to pyfuze, allowing users to opt to pre-download dependencies.  

Additionally, I noticed that [APE-is-also-a-ZIP](https://github.com/jart/cosmopolitan/issues/166). I wonder if it could be made self-extracting, which would be even more convenient.

---

_Comment by @blais on 2025-06-04 05:12_

No. The trade-off defines a completely different feature ask. I think you have an uncommon definition for the meaning of "bundling."

There's a lot of prior art on this. See Bazel "par" files and XAR files and a bunch of other packagers for Python. The ability to run an _isolated_ package without any network access is the key feature of "bundling," the isolation is the feature that allows reliable execution on a variety of hosts. Especially for cron jobs. In a private cluster with tight security you cannot rely on uv fetching resources available on the internet, resources that can change between invocations, or even uv's own behavior to change. I would argue that even if you pinned down the version of uv itself in the zip file you're possbly liable to see failures. Bundles are basically single-application lightweight docker containers, where you don't have docker or you don't want its limitations (e.g., mount/network isolation) -- you just want to run the program from a single file as if it were an a single executable file, with no dependencies.

The bundling that you provide in pyfuze is minimal (I just read your code). It's a wrapper that copies the script and a few files configuring the dependencies for uv to do the heavy lifting. I think its usefulness is limited; call it "lightweight bundling" perhaps. Except for the uv install bit (which really ought to be done once per host), a regular Python program with a main file that has a `#!/usr/bin/env -S uv run --script` hash-bang does most of what pyfuze provides IMHO.

These are two separate sets of problems. "Bundling" is the former. 


---

_Comment by @kskarthik on 2025-07-23 09:16_

This feature would be a great addition to the python ecosystem! 

Deno & Bun implemented it for the node ecosystem.

---

_Comment by @parkan on 2025-08-21 13:59_

being able to ditch all the glue code needed by pyinstaller would be *amazing*

---

_Comment by @kishaningithub on 2025-08-21 16:21_

Pyapp can also be considered an option for creating statically linked binaries

https://www.infoworld.com/article/4030697/pyapp-an-easy-way-to-package-python-apps-as-executables.html

---

_Comment by @parkan on 2025-09-06 15:44_

> Pyapp can also be considered an option for creating statically linked binaries
> 
> https://www.infoworld.com/article/4030697/pyapp-an-easy-way-to-package-python-apps-as-executables.html

this is looks great and appears to natively (though optionally) use `uv` in the build chain https://ofek.dev/pyapp/latest/runtime/

will definitely try this out!

---

_Comment by @alekssamos on 2025-10-16 19:42_

I made an sfx file using winrar. Set the options to show nothing, hide everything, and accept the defaults. Execute after installation `uv run app.py`. And it worked.

---

_Comment by @metaist on 2025-12-24 06:12_

Took me a while to rewrite the whole thing to use `uv`, but if you add an appropriate `[project.scripts]` to your `pyproject.toml` then you can do:

uvx cosmofy bundle

And it'll produce a single Cosmopolitan Python for each entry point using uv. You can also do `--script` to bundle up a single script with its inline dependencies. There are [some limitations](https://github.com/metaist/cosmofy#limitations), but it's a start. 

See: https://github.com/metaist/cosmofy

---

_Comment by @inoa-jboliveira on 2025-12-26 13:17_

Hi @metaist I'm interested in your solution, and might give it a try. One thing I noticed is that you seem to be [hosting a python binary yourself](https://github.com/metaist/cosmofy/blob/815ee899788b3e062a014960c35901b9181d1ea1/src/cosmofy/bundle.py#L38) instead of using uv to download it which would manage correctly the arch and OS.

Having this is also not auditable and some places would not accept such file. Is there any attestation where it comes from?



---

_Comment by @metaist on 2025-12-26 13:47_

> Hi [@metaist](https://github.com/metaist) I'm interested in your solution, and might give it a try. One thing I noticed is that you seem to be [hosting a python binary yourself](https://github.com/metaist/cosmofy/blob/815ee899788b3e062a014960c35901b9181d1ea1/src/cosmofy/bundle.py#L38) instead of using uv to download it which would manage correctly the arch and OS.
> 
> Having this is also not auditable and some places would not accept such file. Is there any attestation where it comes from?

Thanks @inoa-jboliveira!

1. The whole point of [**Cosmopolitan** Python](https://ahgamut.github.io/2021/07/13/ape-python/) is that it doesn't depend on arch and OS. It's a single binary file that is actually a very clever zip that runs on all the OSes without changes. Part of the reason I wanted to build this is so that I don't have to rebuild different binaries for every OS. It's just one binary. Note that [I do use `uv` to build a temporary venv](https://github.com/metaist/cosmofy/blob/815ee899788b3e062a014960c35901b9181d1ea1/src/cosmofy/bundle.py#L227) of your project (or script) which captures all the dependencies properly on the OS you happen to be on and then I bundle them into the Cosmopolitan Python executable.

2. The point about audits/attestation is valid and strong; this is one of the limitations of the current approach (see https://github.com/metaist/cosmofy/issues/44). I'm currently using the python binary built and hosted by Justine Tunney (@jart) through [superconfigure](https://github.com/ahgamut/superconfigure) by @ahgamut. Every single version of python has to be rebuilt to be a Cosmopolitan Python (superconfigure has lots of patch files to convert the paths and make other adjustments) and right now they're only up to 3.12.3. A longer-term goal I have is to make a dedicated repo (superconfigure builds lots of binaries besides python) for tracking all the changes and having a single attestable build per python version similar to how uv used and then took over [`python-build-standalone`](https://github.com/astral-sh/python-build-standalone).

3. This is also why you can override the default Cosmopolitan Python URL through `COSMOFY_PYTHON_URL`  or `--python-url` to point at a Cosmopolitan Python binary that you've verified or self-host.

---

_Comment by @inoa-jboliveira on 2025-12-26 15:44_

>      [**Cosmopolitan** Python](https://ahgamut.github.io/2021/07/13/ape-python/)

I had no idea what this is and still I'm kinda intrigued (being nerdsniped here). At the same time, this is not what I'm looking for right now as it will not support C/Rust compiled libraries. 



---

_Comment by @metaist on 2025-12-26 18:33_

Yes, it's kinda wild that with the right combinations of magic numbers this is possible. 

C extensions is also on my future list: https://github.com/metaist/cosmofy/issues/94

In theory, anything that can be compiled with libc can be recompiled with cosmopolitan libc, but in practice it takes some work.

---
