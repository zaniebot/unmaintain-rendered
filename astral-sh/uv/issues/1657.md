```yaml
number: 1657
title: "Change the name of `uv pip`"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2024-02-18T17:25:35Z
updated_at: 2025-01-25T06:25:57Z
url: https://github.com/astral-sh/uv/issues/1657
synced_at: 2026-01-10T04:27:57Z
```

# Change the name of `uv pip`

---

_Issue opened by @zanieb on 2024-02-18 17:25_

Originally discussed in https://github.com/astral-sh/uv/issues/1331

The idea here is that `pip` is confusing to users who also use `pip`. We could rename `uv pip` e.g. `uvp` or `uv pkg` and make `uv pip` a hidden alias.

I'd really prefer not to do a lot of bike-shedding over names here. We'll probably make this decision internally. Instead, I'd love to hear if this would improve or degrade your experience as a user.



---

_Label `enhancement` added by @zanieb on 2024-02-18 17:25_

---

_Comment by @SnoopJ on 2024-02-18 17:41_

A perhaps-contentious suggestion: it would make sense to me to flatten the namespace currently under `uv pip`, so that `uv pip install` becomes just `uv install`, and so on.

I could understand `uv pkg …` at least in the case of things that `pip` does, but I would be slightly confused if the compile/sync commands continued to live in that namespace since those operations are less to do with the concept of "package" and more to do with entire graphs of packages. But I don't think there would be any ambiguity if the namespace was done away with entirely?

---

_Comment by @mjclarke94 on 2024-02-18 17:51_

I think using `pip` as a noun (...verb? both?) here will make people draw parallels which aren't necessarily wanted/helpful when learning the new tool.  Especially if the long term goal is for some of the functionality of `rye` to make its way over to `uv` (and the hierarchy of commands to be heavily influenced by `cargo`), it's probably better to stick to terms which don't require you to think "Oh, pip...so it's something to do with installing packages into this environment...but it isn't pip?".

Possibly an overcorrection, but is there an argument that all the things currently living under `pip` could be under `venv`, in that they are commands which manipulate the local virtual environment. (`venv create` -> make a virtual environment. `venv sync` -> synchronise packages into the virtual environment. etc.)

---

_Comment by @woutervh on 2024-02-18 19:04_

Reading this issue made me just add to my bashrc/zshrc:

```
alias uve="uv venv"
alias uvp="uv pip"
```

IMHO the pip subcommand is fine, as long as  a big enough subset is implemented.
Ducktyping:  if it looks like a pip , behaves like a pip, then it's a pip.




---

_Comment by @woutervh on 2024-02-18 19:10_

By default, usually subcommands show help:

```
> uv pip
Resolve and install Python packages

Usage: uv pip [OPTIONS] <COMMAND>

Commands:
  compile    Compile a `requirements.in` file to a `requirements.txt` file
  sync       Sync dependencies from a `requirements.txt` file
  install    Install packages into the current environment
  uninstall  Uninstall packages from the current environment
  freeze     Enumerate the installed packages in the current environment
  help       Print this message or the help of the given subcommand(s)

Options:
  -q, --quiet                  Do not print any output
  -v, --verbose                Use verbose output
      --color <COLOR>          Control colors in output [default: auto] [possible values: auto, always, never]
  -n, --no-cache               Avoid reading from or writing to the cache [env: UV_NO_CACHE=]
      --cache-dir <CACHE_DIR>  Path to the cache directory [env: UV_CACHE_DIR=]
  -h, --help                   Print help (see more with '--help')
  -V, --version                Print version
```


But venv does not:

```
> uv venv
uv venv
Using Python 3.11.6 interpreter at /opt/tools/pyenv/var/repo/versions/3.11.6/bin/python3
Creating virtualenv at: .venv

> uv venv --help
Create a virtual environment

Usage: uv venv [OPTIONS] [NAME]
...
```

I find it rather unexpected to not have to provide a path like:

```
uv venv .
uv venv .venv
```



---

_Comment by @zanieb on 2024-02-18 20:28_

> A perhaps-contentious suggestion: it would make sense to me to flatten the namespace currently under uv pip, so that uv pip install becomes just uv install, and so on.

We won't do this because we're reserving the root namespace for our own workflow e.g. `uv install` may be similar to `cargo install` and install a package into an isolated environment in the global namespace.

> I could understand uv pkg … at least in the case of things that pip does, but I would be slightly confused if the compile/sync commands continued to live in that namespace since those operations are less to do with the concept of "package" and more to do with entire graphs of packages.

I agree with this sentiment. I don't like `uv pkg` personally, but it had been proposed elsewhere. Here, it's just a placeholder for the idea of continuing to using a subcommand instead of a new base command i.e. `uvp`.

>  I think using pip as a noun (...verb? both?) here will make people draw parallels which aren't necessarily wanted/helpful when learning the new tool. 

We generally don't think of it as a verb here, we're thinking of it as "this is our `pip`-compatible interface". We'll be populating the rest of the `uv` namespace with commands that are designed the way _we_ think they should be rather than striving for compatibility with old interfaces.

> By default, usually subcommands show help: ...

This looks off-topic, can you open a new issue for this instead?

---

_Comment by @hauntsaninja on 2024-02-19 01:17_

I like `uv pip`. To basically any Python user in 2024, it's clear how the subcommand is roughly expected to behave and how to start using it, which isn't as true of a subcommand like `uvp`. It's been easy for my muscle memory.

I was intrigued by mjclarke94's suggestion of moving commands under `uv venv` (for where the command "should" live in a blank slate design) and adding `uv venv create`. With the exception of `uv pip compile`, all the `uv pip` commands are all low-level mutation or description of a venv. When uv introduces high level workflow commands, users will hopefully not think much about specific venv and if they do `uv venv` is a reasonable place to start. You could maybe use `uv env` to work around the back compat issue (this is shorter and would be more accurate if it optionally lets you do global installs), `uv env freeze`, `uv env uninstall` all read quite nicely.

But yeah, a "uvp" that doesn't immediately say "oh this is like pip" and isn't the blank slate design you want, feels a little bit nowhere land to me

---

_Comment by @woutervh on 2024-02-19 01:24_

@zanieb  created https://github.com/astral-sh/uv/issues/1674

---

_Comment by @inigohidalgo on 2024-02-19 10:21_

For me the reasoning for keeping all the pip commands under the `uv pip` namespace was clear, and didn't really lead to any confusion. I might decide to alias it on my end to something like `uvp`, but making it clear that this subcommand is aiming at replicating (a subset of) the pip API is pretty good reasoning. If this were a poll I would vote for keeping things as-is.

EDIT: actually answering the question:
>  I'd love to hear if this would improve or degrade your experience as a user

It wouldn't really have made a difference either way. It's true that if somebody wasn't reading the documentation `uv pip` could lead somebody to think that `uv` is "calling" `pip` under the hood or something. But the clarity introduced by the combination of the namespace with the reasoning explained in the documentation was enough for me to start working.

I don't think `uvp` would've degraded my experience though, as it's still showing that it's not a top-level API for uv, rather a subcommand.

---

_Comment by @Lawouach on 2024-02-19 11:09_

Hi all,

Just chiming as a new user (congrats a great tool by the way). I was a bit confused initially to read `uv pip install`, it made me wonder if `uv` was "simply" calling `pip` underneath, if I needed `pip` installed first... I guess I then understood because I had to create a venv with `uv venv` (though I didn't know if I needed to manually activate it or if `uv pip `would pick it up on its own). pdm feels like it has a more natural UX here.

In some respect, it comes down to the question "do I need to know anything about virtual env?". The reason I'm interested in `uv` is that it's a self-contained binary. Python has a chicken and egg problem when it comes to let users install applications. Users of tools I write don't want to know about venv and pip. They just want to run "COMMAND install TOOL". To me, more than speed, uv looks like a partial solution to this issue. I can think of asking my users to do:

* install `uv`
* run `uv install <tool>`
* run `<tool>`

That's it. THey wouldn't know anything beyond this. Underneath, `uv` would take care of creating a shared (or dir-local?) venv and drop whatever I globally install into it.

This is for the use case of an application, not a developer environment of course so this may not align at all with your goals.

Also, it made me wonder about trademark/copyright of the `pip` name altogether. 

Just minor comments in passing.

Cheers again.



---

_Comment by @mkleinbort-ic on 2024-02-19 11:42_

Congrats on the bike shedding thread 😅

I like `uv pip ...`, it gave me confidence to test it as a drop-in replacement of my `pip` based workflow.

EDIT: Sorry @Lawouach - it was a little joke in reference to the original message in this thread, not you.

---

_Comment by @baggiponte on 2024-02-19 13:33_

Hey there. I like the `uv pip` naming. I think it was a good choice to release `uv` right now - even if it "only" does pip + pip-compile + virtualenv and is not a tool like PDM/cargo/Poetry. So I understand you don't want to create a `uv install` command that will change behaviour in a very breaking way in 6/9 months.

---

_Comment by @zanieb on 2024-02-19 15:20_

Hey @Lawouach — this is intentional. We've released a `pip`-compatible API before designing our full workflow (which will take care of all of the points you mentioned) so we can prove out our resolver and other foundational implementations.

Thanks for the feedback everyone! 

---

_Comment by @stinovlas on 2024-02-19 16:17_

`uv pkg` sounds good to me. Better to avoid confusion with `pip` since the two are bound to have their differences.

---

_Comment by @Lawouach on 2024-02-19 17:34_

Thanks @zanieb that makes sense.

No worries @mkleinbort-ic it certainly is a discussion that can lead to bike shedding :)

---

_Comment by @JacobCoffee on 2024-02-19 19:17_

> Instead, I'd love to hear if this would improve or degrade your experience as a user.

Moving away from `pip` into some arbitrarily named command would probably be a positive thing imo

---

_Comment by @ncoghlan on 2024-02-20 01:03_

Something like "uv pip-compat" or "uv pip-cli" or "uv compat" would more clearly convey that the subcommands are there to provide some level of CLI compatibility with existing tools, without actually *being* those tools (the last example would also allow one subcommand to offer CLI compatibility for multiple tools as long as they had orthogonal command sets).

(Congrats on the public launch!)

---

_Comment by @merwok on 2024-02-20 03:16_

What about: `uv env ...` ?

Someone said that you could think of pip operations as editions of a venv.
`uv env install something` or `uv env list` make sense! (if uv is (nearly) always working with a virtualenv of course). I use `env` on purpose to refer to the concept of «python environment», not specifically reference `venv` as a module or a specific concept.

---

_Comment by @astrojuanlu on 2024-02-22 11:43_

I like `uv pip` because it's short. I'm not sure the naming is important, folks who have strong opinions on that are free to create the alias of their choosing?

Reserving the root namespace seems like a wise choice and turning `uv pip` into `uvp` would be unfortunate IMHO. Apart from that, I really don't care.

---

_Comment by @merwok on 2024-02-22 16:01_

A strong point was made on the Python discuss forum: many people will know that pip is run with commands such as `python -m pip`, `poetry run pip`, `pipenv run pip`, `some-wrapper pip`, so it’s very misleading that `uv pip` runs not-pip-but-maybe-mostly-compatible.

pip is one specific tool, not a generic concept for a Python installer.

---

_Comment by @gotmax23 on 2024-02-23 04:14_

Duplicate of https://github.com/astral-sh/uv/issues/1899:

> Thanks for your work on uv and ruff! I would like to suggest changing the uv pip subcommand name. uv and pip are different tools. uv is not necessarily going to implement the same interface as pip. It feels a bit crummy to write a replacement to another tool and then take its name. I would suggest renaming the subcommand to pkg or something similar that's more descriptive and won't lead to unnecessary confusion. Leaving pip as an undocumented alias makes sense, but I don't think it should be the default name for the package management subcommand.

---

_Comment by @jordantshaw on 2024-03-07 19:55_

I would love to see something besides `uv pip`.  I understand the thought process behind it, but I think to the casual user, this could be confusing. I think it should be clear that uv is not using pip behind the scenes or anything.  I would suggest for something more along the lines of `uv pkg install`.

---

_Comment by @pfmoore on 2024-03-19 11:55_

-1 on having a second command. I like having everything under the one `uv` main command.

My concern about using `uv pip` is that you don't implement a fully pip-compatible interface, and that's a stumbling block for users. Whenever I try out `uv`[^1] I always get caught out by the lack of `uv pip list --outdated`, for example. Yes, this will improve over time, but the message here is *very* confused - if you want to match pip, then `uv pip` makes a certain amount of sense, but in that case, the `uv pip install --resolution` option (among others) is out of place as it's not "matching pip" but improving on it. Also `uv pip compile` and `uv pip sync` are *not* pip subcommands, which again causes confusion (in my mind). The `uv pkg` suggestion makes sense to me, but I can see that it doesn't fit with `uv pip sync` and `uv pip compile`. That's precisely because those *aren't* pip commands, though.

> We won't do this because we're reserving the root namespace for our own workflow e.g. uv install may be similar to cargo install and install a package into an isolated environment in the global namespace.

That's entirely fair, but I presume the implication is that `uv pip` will remain as the long-term interface to "low level" package management commands. Otherwise, `uv pip` becomes a particularly nasty "bait and switch", as people get enthusiastic about using `uv` as a pip replacement, and then find that they are expected to migrate to the more opinionated workflow commands. So if we assume `uv pip` is intended to be around for the long term, then I think that's an even better reason to get the naming correct early on.

[^1]: As a pip maintainer, I feel like switching fully would be a bit "off brand", but the speed *is* tempting 😉

---

_Comment by @mqudsi on 2024-03-19 13:34_

I assumed a symlink called pip could be created pointing to uv and it would execute `uv pip` but that isn’t the case. This could be a middle ground if you set up a bash function in your activate.sh to essentially fork bash then exec with arg0 set to “pip” or “pipx”, etc. 

---

_Comment by @astrojuanlu on 2024-06-24 16:09_

With #3272, I suppose `uv pip` is departing more and more from the original `pip` CLI. I welcome the addition of `uv pip tree` anyway 😄 

---

_Comment by @zanieb on 2024-06-24 16:34_

At this point, I think it's fair to say that we won't be changing the name of this sub-command. I appreciate all of the feedback, but I don't think it ended up being confusing for most users and the breaking change hasn't felt worth it. Although we introduce a few new sub-commands that `pip` doesn't support, we've also generally been moving closer to compatibility to `pip` in this interface.

---

_Closed by @zanieb on 2024-06-24 16:34_

---

_Comment by @stevenwalton on 2025-01-25 00:17_

I think the lack of an alias (or a quick shortcut to make an alias like `uv alias pip`) provides extra confusion to users. 
This is because creating virtual environments the normal way places a `pip` command in that virtual environment 

```bash
# Make virtual environment native way
$ python -m venv native_venv
$ ls native_venv/bin
 activate   activate.csh   activate.fish   Activate.ps1   pip  
 pip3       pip3.12        python          python3        python3.12

# virtual environment with uv
$ uv venv uv_venv
$ ls uv_venv/bin
 activate      activate.bat   activate.csh       activate.fish  
 activate.nu   activate.ps1   activate_this.py   deactivate.bat  
 pydoc.bat     python         python3            python3.10
```

I'm sympathetic to users getting confused thinking it is "real" pip but the current answer seems to leave the user is a confusing position no matter what.

Could a compromise not be to place a similar `pip` file within that directory?
```bash
$ cat native_venv/bin/pip
#!/Users/steven/test/native_venv/bin/python3.12
# -*- coding: utf-8 -*-
import re
import sys
from pip._internal.cli.main import main
if __name__ == '__main__':
    sys.argv[0] = re.sub(r'(-script\.pyw|\.exe)?$', '', sys.argv[0])
    sys.exit(main())
```
All that needs to be changed is that shebang. While it wouldn't be the `uv pip` it could at least solve some issues like how users might run scripts that use `pip` in them (such as install scripts). I'd much prefer a way to alias `uv pip` to `pip` but this might be a way to at least make everyone (most?) happy.

For anyone that wants this functionality, [here's something](https://github.com/stevenwalton/.dotfiles/blob/master/scripts/basic_functions.sh#L97-L112) you can put in your `.bashrc` or `.zshrc`

```bash
alias_pip() {
    if [[ $(command -v "uv") ]]
    then
        PIP_LOC="$(dirname "$(which python)")"
        # If you want to only run from the root directory (which .venv) exists
        # in then check with this conditional (allows arbitrary name for .venv)
        #if [[ "${PIP_LOC}" == "${PWD%/}/"*"/bin" ]]
        echo -e "#!/usr/bin/env bash\n\npip() {\n\tuv pip \"\$@\"\n}\n\npip \"\$@\"" > "${PIP_LOC%/}/pip"
        chmod +x "${PIP_LOC%/}/pip"
    fi
}
```

---

_Comment by @notatallshaw on 2025-01-25 05:03_

Isn't your problem solved by `uv venv --seed`? This will install pip into your venv.

uv's venv doesn't install pip, so creating a script to point to pip without installing it will often produce an error, no?



---

_Comment by @stevenwalton on 2025-01-25 06:25_

Thanks. I missed that option.

Though I'll still keep my alias because it is aliasing `pip` to `uv pip` and for all cases I've faced that's been acceptable, so I'd rather get the `uv` benefits. 

> so creating a script to point to pip without installing it will often produce an error, no?

The issue is when I use someone else's project. Of course there are times where it doesn't work out.
(Though would probably work better by using `eval "uv pip $@"` or filtering out certain flags, and I have done this)


---
