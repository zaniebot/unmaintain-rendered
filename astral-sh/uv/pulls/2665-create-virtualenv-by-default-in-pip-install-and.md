```yaml
number: 2665
title: "Create virtualenv by default in `pip install` and `pip sync`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - cli
assignees: []
base: main
head: charlie/default
created_at: 2024-03-26T02:58:51Z
updated_at: 2024-04-05T01:59:48Z
url: https://github.com/astral-sh/uv/pull/2665
synced_at: 2026-01-12T16:05:09Z
```

# Create virtualenv by default in `pip install` and `pip sync`

---

_@charliermarsh_

Closes https://github.com/astral-sh/uv/issues/2661.

---

_Marked ready for review by @charliermarsh on 2024-03-26 02:58_

---

_@charliermarsh reviewed on 2024-03-26 02:59_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:135 on 2024-03-26 02:59_

Weird, this test was passing but with the "wrong" failure.

---

_Review requested from @zanieb by @charliermarsh on 2024-03-26 02:59_

---

_Review requested from @konstin by @charliermarsh on 2024-03-26 02:59_

---

_Label `cli` added by @charliermarsh on 2024-03-26 02:59_

---

_@konstin approved on 2024-03-26 08:10_

---

_Comment by @AlexWaygood on 2024-03-26 10:28_

...I feel like I'd find it quite annoying if I had a virtual environment already in a project saved in an `env` (not `.venv`) folder, and I forgot to activate it, and rather than telling me off for not activating it, `uv` then proceeded to create a second virtual environment and installed my huge `requirements.txt` file into _that_ virtual environment rather than the one I wanted to use.

Could there maybe be a quick "Create and activate a new virtual environment? [y/n]" prompt before creating one and going ahead with the installation?

---

_Comment by @charliermarsh on 2024-03-26 13:22_

Is it really so bad if `uv` installs into a newly-created environment, if installation is ~free?

---

_Comment by @AlexWaygood on 2024-03-26 13:24_

> Is it really so bad if `uv` installs into a newly-created environment, if installation is ~free?

It's going to create me more hassle though, right, because now I have two virtual environments when I only need one, so now I have to delete one of them?

---

_Comment by @charliermarsh on 2024-03-26 13:28_

Yes! But today, you have to re-run two commands anyway in this failure path. And this way, it's (1) right some of the time, but more importantly (2) encourages users to follow the happy path.


---

_Comment by @AlexWaygood on 2024-03-26 13:37_

Today if I'm in a repo locally on the command line, I run `uv pip install -r requirements.txt`, and `uv` complains I don't have a venv activated, I do the following:

1. Run `source env/bin/activate` (I usually call my venvs `env` rather than `.venv`, don't judge me)
2. Press the "up" arrow twice and hit enter to rerun the `uv pip install` command

With this PR, I'd have to do the following:

1. `uv` starts creating a new virtual environment and installing deps into it. If for some reason it's taking longer than expected, I might hit CTRL-C to cancel it.
2. I then have to type `rm -rf .venv` to delete the new virtual environment it created that I didn't want 
3. I then run `source env/bin/activate`
4. I then press the "up" arrow twice and hit enter to rerun the `uv pip install` command

That's one, possibly two more steps by my reckoning ;)

> And this way, it's (1) right some of the time, but more importantly (2) encourages users to follow the happy path.

I agree, and I like the idea of making this easier! I'm just asking that we refuse the temptation to guess, and ask users to type "y" to confirm that this is really what they wanted before we proceed

---

_Comment by @zanieb on 2024-03-26 14:37_

I'm not a fan of interactive prompts at all. It sounds like what you want is #1422 :)

---

_Comment by @AlexWaygood on 2024-03-26 15:21_

> I'm not a fan of interactive prompts at all.

I'm not a fan at all of tools guessing what the user wanted to do when it's not explicit :/

> It sounds like what you want is #1422 :)

eh, I'm not sure that's what I want. I actually only learned today that `uv` searches for a virtual environment named `.venv` if no virtual environment is specified or activated, and assumes that's the environment you wanted the dependencies installed into. Honestly, I'm not sure I love that assumption either -- I'd much rather the tool explicitly checked with me that that's what I wanted before it proceeded :/

But I know we've had that behaviour for a while now. Perhaps I'm being too curmudgeonly :)

---

_Comment by @zanieb on 2024-03-26 15:35_

> I'm not a fan at all of tools guessing what the user wanted to do when it's not explicit :/

:) Yeah we've gone back and forth on this. If we think know what you want to do should we just do it? Should we prompt? Should we error and tell you how to fix it? I think we should have a consistent policy here but we haven't come to one yet. @MichaReiser has had a different opinion than me in the past so it might be worth hearing what he thinks.

I think that we want to hide the virtual environment abstraction — things should "just work" in a project directory. We also want to innovate on the idea that "virtual environments are disposable and free". Previously, they were not free and that they are is a big part of our value proposition. What happens now that they are cheap? How can workflows evolve? What can we do to push boundaries based on this idea? I find these questions motivating me to explore things I would otherwise find surprising.

>  uv searches for a virtual environment named .venv if no virtual environment is specified or activated

I find this a very strong argument for the behavior proposed in this pull request. It's a continuation of our existing philosophy. Whether or not we should have done that in the first place is another discussion I guess.

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_install.rs`:138 on 2024-03-26 15:39_

As noted by Damian, we should definitely not create this when `--dry-run` is enabled.

---

_@zanieb reviewed on 2024-03-26 15:39_

---

_@zanieb requested changes on 2024-03-26 15:40_

Needs a test case with `pip install --dry-run`

---

_Comment by @notatallshaw on 2024-03-26 15:49_

I'll also add maybe I use conda environments, not virtual environments, to install my packages. uv will always do the wrong thing in that case.

As I said in the issue, as a non beginner I would definitely want some way to switch the behavior back, I would want it to error.

---

_Comment by @charliermarsh on 2024-03-26 15:51_

Yeah, this wouldn't happen when you have a Conda environment active, just as we don't error today if you have a Conda environment active.

---

_Comment by @notatallshaw on 2024-03-26 15:54_

> Yeah, this wouldn't happen when you have a Conda environment active, just as we don't error today if you have a Conda environment active.

Sorry I wasn't clear, the scenario I was thinking of is you have conda installed, but you have set conda to not activate any conda environment by default (not even base), and you use uv to install.

uv will not see an activated conda environment, so presumably will create a virtual environment, but as the user only uses conda environments this will be the wrong behavior for the user.

This was another example where I would like to be able to set a switch somewhere for uv to not do this.

---

_Comment by @AlexWaygood on 2024-03-26 15:55_

> we've gone back and forth on this. If we think know what you want to do should we just do it? Should we prompt? Should we error and tell you how to fix it?

I'm personally a big fan of prompts, because they force the user to be explicit about what they wanted, and you generally only have to hit a single key on the keyboard to get the behaviour that you were probably trying to get all along :)

> I think that we want to hide the virtual environment abstraction

I love that as a goal, and I'm very much in favour of us trying to innovate new workflows for Python packaging. But right now, where the virtual environment is located seems pretty relevant and unabstracted. You still need to run `source .venv/bin/activate` (or run Python using `.venv/bin/python <whatever>` rather than simply `python <whatever>`) to actually use the dependencies that have been installed into the virtual environment. With uv's current capabilities, the user still needs to know exactly what their virtual environment is called and where it is located in order to actually use it.

> It's a continuation of our existing philosophy. Whether or not we should have done that in the first place is another discussion I guess.

Yeah, agreed that it's consistent with some of the decisions that have already been taken. It does feel like another step down that road, though

---

_Comment by @notatallshaw on 2024-03-26 16:02_

> Is it really so bad if `uv` installs into a newly-created environment, if installation is ~free?

Btw, it might actually be very costly to write to the currently directory structure, the user could have set their current directory to a magnetic tape device, or a NAS that's on the opposite side of the world, or a write once disk.

I'm pro the defaults being beginner friendly, I would just like users to have an option to turn them off once they are burnt by those defaults.

---

_Comment by @konstin on 2024-03-26 16:06_

The experience i'm interested in is one where you git clone and `uv run` (or even just run `python` we shim), without any setup or even knowing what a venv is. Standardizing on `.venv` is an intermediate step for that; it follows a wider trend in the ecosystem (PEP 704 tried to standardize on it). With a single venv name, all tools know where to look without `source`ing manually and notably, all teaching material can use `.venv` instead of relying that the user knows their venv names. I see the point where we're basically picking a color for the bikeshed and making things more difficult for people who picked the now wrong color back when this was being explained as a free choice (where we e.g. use the wrong `../.venv` instead of the correct `./env`).

---

_Comment by @notatallshaw on 2024-03-26 16:14_

Also, as you're picking a single color. When I'm testing a open source project I often have to create 5 virtual environments, one for each major version of Python the project supports.

---

_Comment by @AlexWaygood on 2024-03-26 16:18_

I agree with the goal of attempting to standardise virtual environment names somewhat, and I think `.venv` was the correct default to choose here.

For me personally, it would not be possible for me to call _all_ my virtual environments `.venv`, as I usually have more than one virtual environment in any given project directory. This is so that I can easily flit between Python versions for testing/REPL sessions/whatever. Maybe now that uv has made virtualenv creation extremely cheap, I shouldn't have so many virtual environments in each project directory anymore -- maybe I should just spin them up on the fly if I want to do tests or REPL sessions like that. But it's still not _free_ to create a new virtual environment -- even if uv is extremely fast, I still need to manually run `uv venv && source .venv/bin/activate && uv pip install -r test-requirements.txt`, which is a lot to type! It's much easier to just activate a pre-existing virtual environment for Python 3.8 (or whatever) that still has all my dependencies installed from last time I needed to test with Python 3.8 for this project.

---

_Comment by @MichaReiser on 2024-03-26 16:52_

> @MichaReiser has had a different opinion than me in the past so it might be worth hearing what he thinks.

I don't think I have a strong opinion on this. To me this seems like a logical next step from how `uv venv` and `uv pip install` already work today: `venv` uses a default name and `pip install` installs even when the environment isn't activated. 

My only concern, but that's a pre-existing one with how `uv pip install` works today, is that the error message when running a binary fails because the venv isn't active  is worse than when I get an error message during installation where pip tells me what the problem is. That's where it would be nice if the venv would be automatically activated.

 I feel like a middle ground here would be if we have our own configuration file and uv could be configured to opt in of this behavior. That might still be confusing because said configuration is unlikely to exist in CI

---

_Comment by @potiuk on 2024-03-27 21:48_

I wrote it before in other discussions, but let me rehash it here again.

If I may suggest - I think it's quite bad idea to create such venv dynamically in current working directory.  This might lead to a numer of problems and a little magical experience for the user - where for example they can end up with multiple such venvs created in different directories semi-randomly and where various`uv` commands produce different results depending on what the `cwd` is. All those extremely difficult to debug and reason about.

IMHO it's a good idea to create such venv dynamically, but it should be created in a well-known place in the HOME directory (or equivalent on windows). My initial proposal was to use the same directory that is currently used by `--user` flag in `pip` - basically compliant with https://peps.python.org/pep-0370/ (~/.local). While this PEP does not make the `~/.local` a virtualenv - it's merely a "user-site" directory, it's ALMOST the same as a venv created in `~/.local` . It even has the right `bin` directory that is on the PATH for a number of users, so any script installed there will also be immediately runnable for such users (plus if you do not have ~/.local/bin on the PATH, uv could print a warning to do so, every time it gets to do something with it.

I think also that it is very much in line with the spirit of PEP-0370 - the whole reason for the PEP:

```
It’s not the goal of the PEP to replace the tools or to implement isolated installations of Python. 
It only implements the most common use case of an additional site-packages directory for each user.
```

I can't think of any bad side effect of choosing such venv as the "default" one (and BTW. I already applied it for the "production" image of Airflow where we install airflow in `~/.local` and it has a number of interesting properties (for example when you create venv with `--site-packages`, all packages installed in `~/.local` will also be available in the new venv, which is pretty much what users would expect. 

My 3 cents.


---

_Comment by @potiuk on 2024-03-28 09:39_

Also for some cross reference the other suggestion to suport `--user` flag might be not needed in this case https://github.com/astral-sh/uv/issues/2077, so those two might be related. 

If you go the path I propose you will simply treat all installations that have no venv nor `--python` specified as `--user` installations.  I think - again - it fulfills the intent of PEP 0370 much better than `--user` flag where - if you read it in detail - the intent was to make life easier for those who are mostly casual users who do not want to care about creating and managing the venvs. Those are roughly the same users who don't know or care which working directory they are in. I personalyl consider having per directory .venv is more of a power-user setting who really understands what venv is and wants to switch between them easily and automated choosing of the venv by the directory, should rather be done at `prompt` level than by the tooling.

Of course it wwould have to be somehow synchronized with `PEP 668` https://peps.python.org/pep-0668/ - because there are some `complications` of the `--user` folder and way how distros use it sometimes - mostly about deleting the packages  rather than installing them - but I think once (if?) PEP-668 will be implemented by the distros, some variations of the behaviours might be implemented depending on the information provided by the "externally managed" packages.

But of course - those are just suggestions, and feel free to ignore them @charliermarsh :). 

---

_Comment by @woutervh on 2024-03-30 19:26_


> `uv searches for a virtual environment named .venv if no virtual environment is specified or activated`

This is the wrong behaviour, it should look for a `pyvenv.cfg` which is the real marker of a venv. 
`.venv` is a sensible default, but it should not become a hardcoded bible.
Basically uv  stops working when you don't use that default. 

And "activating" is a anti-pattern that creates state in a single shell-window.

> Is it really so bad if uv installs into a newly-created environment, if installation is ~free?

Yes, the location of all your executables change, breaking everything else downstream. 


I have over 100 venv-based app on my system, 
only some of them are development projects:

```
/opt/venv-foo/bin/foo
/opt/venv-bar/bin/bar
```

would become with uv, with zero benefit, and many disadvantages: 
```
/opt/venv-foo/.venv/bin/foo
/opt/venv-bar/.venv/bin/bar
```
So now my users see an empty dir in /opt/venv-xxx.

When supporting junior devs or non-technical users,
my main task is usually to help find them their own executables. 




---

_Comment by @charliermarsh on 2024-03-30 20:23_

> Basically uv stops working when you don't use that default.

I’m not sure how you came to this conclusion but it seems like a significant exaggeration, and candidly the use of phrasing like this makes me less likely to spend time working through the rest of your comment. I don’t think this change has anything (?) to do with where your binaries get stored and your ability to use multiple disparate virtual environments vis-a-vis main. It changes almost nothing vis-a-vis main, apart from that what was previously an error would now fallback to a different behavior. So any workflows that “work” today would not be impacted at all.

I’m not planning to make this change now given how strongly people have replied here (I appreciate the input, thanks all) so I likely won’t spend much time debating it, but we may revisit in the future.

---

_Comment by @samypr100 on 2024-04-01 01:41_

Late to the party, but this discussion is definitely worthy of the classic [xkcd](https://xkcd.com/1172/).

I liked @AlexWaygood suggestion on prompting  (I think it benefits both sides of the argument). We'd prompt the user if one isn't found in the predetermined locations and ask if they'd like for uv to create it for them with `y` preselected by default. This will still automate running `uv venv` for them and proceed with the install when they choose `y`.

This guides a beginner user into understanding venv's and the crucial role they play in the ecosystem (no magic). Similarly, this prompting could be disabled via an env var or if `uv` can detect there's no interactive prompt, getting the best of both worlds.

For perspective, poetry already creates venvs by default unless you set `POETRY_VIRTUALENVS_CREATE=false`. It also checks for venv's in a couple of locations such as `.venv` and `venv`.

---

_Closed by @charliermarsh on 2024-04-05 01:59_

---
