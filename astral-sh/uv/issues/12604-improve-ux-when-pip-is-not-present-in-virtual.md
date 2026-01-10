---
number: 12604
title: "Improve ux when `pip` is not present in virtual environments"
type: issue
state: open
author: zanieb
labels:
  - needs-design
  - needs-decision
assignees: []
created_at: 2025-04-01T17:54:11Z
updated_at: 2025-12-10T21:24:16Z
url: https://github.com/astral-sh/uv/issues/12604
synced_at: 2026-01-10T01:25:22Z
---

# Improve ux when `pip` is not present in virtual environments

---

_Issue opened by @zanieb on 2025-04-01 17:54_

There are various previous issues discussing this topic:
 
- https://github.com/astral-sh/uv/issues/12247
- https://github.com/astral-sh/uv/issues/12199
- https://github.com/astral-sh/uv/issues/1657
- https://github.com/astral-sh/uv/issues/1331
- https://github.com/astral-sh/uv/issues/7534
- #6593
- https://github.com/astral-sh/uv/issues/10949
- https://github.com/astral-sh/uv/issues/1330

Suggestions range from "uv should seed `pip` by default" to "uv should shim `pip` to `uv pip`".

To summarize our previous reactions:

- We do not want to seed `pip` by default: it's a regression in the experience we're trying to innovate on.
- We do not want to shim `pip` to `uv pip`: we don't want to pretend to be another tool, we have some intentional differences in behavior that may be surprising.

However, there are still problems, e.g.:

- The `pip` from a parent environment can be used, which results in mutation of the wrong environment.

It's increasingly clear we should do _something_ to improve the experience and reduce confusion.

At the very least, we could:

1. Add a `pip` shim which _warns_ and forwards to the system `pip`.

Or:

2. Add a `pip` shim which _errors_ explaining usage.

And, going further:

3. Add a `pip` shim which has a configurable behavior, e.g., you can warn, error, or forward to `uv pip`. 

Or, going in a different direction:

4. Add warnings to `pip` if it's invoked and is not installed in the active virtual environment.

And, going further (controversially!):

5. Add warnings to `pip` if a mutable operation is used in the system environment.

---

_Referenced in [astral-sh/uv#12247](../../astral-sh/uv/issues/12247.md) on 2025-04-01 17:54_

---

_Referenced in [astral-sh/uv#1331](../../astral-sh/uv/issues/1331.md) on 2025-04-01 17:55_

---

_Referenced in [astral-sh/uv#12199](../../astral-sh/uv/issues/12199.md) on 2025-04-01 17:56_

---

_Label `needs-design` added by @zanieb on 2025-04-01 17:57_

---

_Label `needs-decision` added by @zanieb on 2025-04-01 17:57_

---

_Comment by @zanieb on 2025-04-01 17:59_

In the vein of (4), https://github.com/astral-sh/uv/issues/6593#issuecomment-2311352464 is a great recommendation for avoiding accidental use of the system `pip`. 

---

_Comment by @pfmoore on 2025-04-01 18:53_

It's worth noting that pip can be installed [as a zipapp](https://pip.pypa.io/en/stable/installation/#standalone-zip-application), and in that situation it is normal for pip to not be present in a virtualenv, but to be run from `$PATH`, while still acting by default on the currently active virtualenv. If `uv` installs a `pip` executable that behaves as you suggest, this will disrupt that approach to using pip.

I'm -1 on uv installing a `pip` shim (modifying the behaviour of another application is at best impolite, and at worst actively harmful), but if you do decide to do it, please ensure that there's a way for users to globally opt out of the behaviour.

What I don't want to happen in particular is for pip to start getting bug reports when users hit issues running the uv shim - it doesn't seem reasonable to describe such problems as "user error", so we'd probably end up characterising it as a misbehaviour of uv, and sending the user back here. That's going to result in user frustration, as well as causing problems in the relationship between the pip and uv teams.

I'm happy to work with you on finding an alternative solution, but I don't think that hijacking the pip command in a virtual environment created by uv is the right answer.

---

_Renamed from "Add a `pip` shim to virtual environments" to "Improve ux when `pip` is not present in virtual environments" by @zanieb on 2025-04-01 19:58_

---

_Comment by @zanieb on 2025-04-01 20:01_

Thanks for the thoughts @pfmoore, I think we're fairly aligned (and I've retitled the issue to clarify the broader goal). I'd very much prefer not to add a shim, I think that's sort of a "last resort" to address confusion. Would you be open to warnings in `pip`?

---

_Comment by @pfmoore on 2025-04-01 20:11_

I'd like to first understand what problem you're trying to solve here.

My understanding is that `uv` doesn't install `pip` when creating a virtual environment. That's no different to `python -m venv --without-pip` and `virtualenv --no-pip`. So my first question is, why is `uv` any different from those cases? Is it simply that you omit pip by default and therefore you're hitting the problem more often? If so, then I think this might be something that `uv` *shouldn't* "solve" - it's not your problem, at the end of the day. It's partly pip's problem, and partly an ecosystem problem resulting from the historical issue with pip being installed in every environment by default.

You might like to read [this thread on Discourse](https://discuss.python.org/t/pip-plans-to-introduce-an-alternative-zipapp-deployment-method/17431/1) for some background.

Maybe a new thread on Discourse would be a better way forward here.

---

_Comment by @zanieb on 2025-04-01 20:15_

> Is it simply that you omit pip by default and therefore you're hitting the problem more often?

Yes, that's my presumption.

> If so, then I think this might be something that uv shouldn't "solve" - it's not your problem, at the end of the day. It's partly pip's problem, and partly an ecosystem problem resulting from the historical issue with pip being installed in every environment by default.

I agree with the sentiment, but user experience problems with Python (and packaging in particular) are our problems too.

---

_Comment by @pfmoore on 2025-04-01 22:12_

My concern is that a unilateral "fix" from uv will ultimately make it harder to find a long-term solution. Specifically, if you hijack the `pip` command in a uv-created virtual environment, you perpetuate the problem that people assume pip is available in every environment, while at the same time making it impossible for people to assume consistent behaviour.

For example, suppose we decided that the right thing to do was to drop the `pip` executable altogether, and require people to use `python -m pip`, ensuring that they *had* to be explicit about the environment they wanted to target. If `uv` is providing a `pip` executable, that makes it impossible to make that message consistent. I'm not saying that is the right solution, just that if you make it so that the pip project (in effect) no longer "owns" the command name `pip`, you will ultimately make the problem worse for the ecosystem as a whole.

That's why I think this should be discussed in Discourse, where the whole community can be involved.

I appreciate that user experience issues should be addressed, but that should be done in a way that *doesn't* diverge from the behaviour of the rest of the ecosystem. It's arguable that [PEP 453](https://peps.python.org/pep-0453/) normalises the idea that pip must be present in virtual environments, and we need to revisit *that*, as well.

The simplest solution I can see is for uv to seed pip by default. That conforms to the expectations set by PEP 453, and is consistent with all other virtual environment creation mechanisms. There was no real discussion in #12247, simply a blunt "we won't be doing that". I strongly urge that you reconsider that position - if you have good arguments for not seeding, then by all means provide them, but it's hard to find a compromise when faced with options being excluded without explanation.

It's worth noting that I don't actually *like* seeding pip by default. In my personal workflow, I don't include pip in virtual environments (I use `--without-pip`/`--no-pip` and configure it globally when possible). So I'm not advocating seeding as a long term answer - rather, I see it as a short-term way of mitigating the user experience issues while working towards a longer term future where pip doesn't need to be present in every environment. But as I say, please read the Discourse thread I linked to get an idea of why that's something that will take time to achieve.

---

_Comment by @zanieb on 2025-04-01 22:49_

> My concern is that a unilateral "fix" from uv will ultimately make it harder to find a long-term solution. 

This does not seem particularly fair. There are ideas for non-unilateral actions here and we've demonstrated a great deal of willingness to engage on standards and solutions to problems outside of this project.

> Specifically, if you hijack the pip command in a uv-created virtual environment, you perpetuate the problem that people assume pip is available in every environment, while at the same time making it impossible for people to assume consistent behaviour.

This is specifically a behavior we do _not_ intend to do. If we added a shim, it'd only be to a pip that is _already_ available outside the environment so we can insert a warning — we would not be making pip available in the environment. However, as I've previously noted (e.g., https://github.com/astral-sh/uv/issues/6593#issuecomment-2308865228), I agree this is a tenuous approach.

> For example, suppose we decided that the right thing to do was to drop the pip executable altogether, and require people to use python -m pip, ensuring that they had to be explicit about the environment they wanted to target. If uv is providing a pip executable, that makes it impossible to make that message consistent.

I don't this is true. For example, if Python 3.14 dropped the `pip` executable entirely, it'd be easy and reasonable for us to do the same. I think there would be _many_ other downstream workflows and consumers to consider here, e.g., Linux distributions or pyenv. It doesn't seem fair to say uv's actions here would be particularly critical.

> The simplest solution I can see is for uv to seed pip by default ...  it's hard to find a compromise when faced with options being excluded without explanation.

Sorry, I did not expound further there because I wanted to centralize the discussion here (and am leaning towards brevity while attempting to consolidate many related conversations). I briefly commented further in the first post: "it's a regression in the experience we're trying to innovate on" — but I can say more here.

I agree this is the simplest solution, but I am concerned it will lead to more long-term confusion for users. We already see many beginners that struggle to understand the role of `pip` (and `uv pip`) in their projects. I think people will also be confused that we're not installing `uv pip` (as users have already asked for). It's also confusing to have an unlocked package in the environment or for us to lock a package that's not a part of the project's dependencies.

I'm sure most users are fine with `pip` not being present in the environments — we just don't hear from them because it's not a problem. I don't see changing our default behavior to include `pip` as a compelling short-term solution, it feels like too big of a step backwards on the goal to untangle `pip` from environments.

> But as I say, please read the Discourse thread I linked to get an idea of why that's something that will take time to achieve.

I will!

---

_Comment by @pfmoore on 2025-04-01 23:09_

> This does not seem particularly fair. 

Sorry. I should have been more careful in my wording. I'll admit I haven't read all of the linked discussions, I was mostly reacting to the idea of uv adding a `pip` command to the environment.

(I'm going to avoid responding to most of your other points right now - it's late here, so I don't have time, and in any case I would prefer to see other people get involved as well. Making this a dialogue between the two of us isn't the best way of moving forward).

> We already see many beginners that struggle to understand the role of pip (and uv pip) in their projects.

I will respond to this, though. I agree that this is something uv needs to clarify, but by education, not by UI. The `uv pip` subcommand is, in my view, a large part of the issue here - it's almost, but not quite, the same as pip (and one way it's *not* the same is in the fact that it doesn't need to be installed in the environment). I understand why you created it (being a "faster pip" was a great way of bootstrapping your user base) but it's left you with a problem now because `uv pip` doesn't really fit with your native workflow, and yet it's sufficiently unlike pip that "use it when you'd use pip" leads to the sort of confusion you refer to when it doesn't work like pip. Or alternatively, when someone uses pip and finds that it doesn't work like `uv pip`.

IMO, if the role of `uv pip`, and the fact that it isn't `pip`, were better explained[^1] and clearer in people's minds, the UX issues you're seeing would go away naturally. In my experience, people are perfectly comfortable with the fact that you don't need to install uv in your environment, but you do need to install pip[^2]. It's the "almost pip" nature of `uv pip` that triggers the confusion.

[^1]: I don't know what that explanation *is*, though. I'm just as confused as anyone else as to what `uv pip`'s role is in the `uv` ecosystem.

[^2]: We have users who encounter issues because they get confused over which environment they are picking pip up from, but they generally aren't hard to address, and people accept the behaviour as "just how pip works". (Maybe with some reluctance, but that's why we'd like to improve things in pip).

---

_Comment by @zanieb on 2025-04-01 23:22_

> Making this a dialogue between the two of us isn't the best way of moving forward

Agree :) 

> IMO, if the role of uv pip, and the fact that it isn't pip, were better explained...

Hopefully, I can capture this in the migration guides I'm working on (e.g., the incomplete #12382). Education is definitely a good path forward here. I think it'll be hard to teach, but it's doable. The next problem will be directing people to read it, which may require some sort of "hint" (i.e., warnings) that what you're doing is not correct. I'm in no rush here though — if documentation solves all the problems, I think that's a great outcome.

---

_Comment by @pfmoore on 2025-04-02 10:43_

A couple more thoughts, and some historical background, and then I'll save any further thoughts for a discussion on Discourse.

* Why haven't you been sending the users who try to run `pip` in a uv-created virtualenv over to the pip tracker? You're not doing anything wrong by omitting pip from a virtual environment, and if people find the fact that `pip` ends up working on the wrong environment a problem, that's something the pip maintainers should know about. We already *do* know, of course, but we don't have a good feel for the scale of the problem, and if the way uv creates environments is increasing the impact of the problem, that's something we should be aware of (and until this issue appeared, we weren't...)
* One option would be for uv to create a `uvp` shortcut for `uv pip`. That wouldn't address the confusion people have with using `pip` in a uv-created virtual environment directory, but it would start the process of increasing awareness of the idea that not all installers are called `pip`. And that would again help with the process of moving away from the status quo.

The following is for background, mainly. Feel free to ignore it if you don't care.

The root of this issue goes back to PEP 453, as I said. At the time that PEP was written, pip was the only installer available, and it *had* to be installed in an environment to work[^1]. The PEP wasn't specifically intended to "bless" pip as the official installer, even though that's what it did in practice. Its goal was to answer the question "how can a user who has just installed Python get access to PyPI?" Because this involved a chicken-and-egg problem (*everything* that wasn't core at the time was hosted on PyPI), the only real solution was bundling a copy of pip. This was far from uncontroversial - the core developers at the time wanted as little as possible to do with packaging, and distributors like conda and the Linux distros weren't keen on Python having a package manager that bypassed theirs. But it was the best solution we had at the time.

Things have changed since then. People want virtual environments to be more and more lightweight. Having "stuff" installed by default is seen as a problem. Zipapps are (finally!) becoming more common, although they are still a long way from being easy to use. Tools like `uv` are being distributed outside of PyPI. And pip is no longer the only option as an installer. But the problems PEP 453 was intended to solve remains - how do people who have just downloaded python from python.org bootstrap into being able to use PyPI? And when we want to tell people to install such-and-such package, what do we tell them to type? The answers to those questions may have changed, in which case we need a *new* PEP to give today's answers, but we can't avoid giving some answer. And like with any change, we have to work out how to handle transition in a way that minimises disruption to users.

Regardless of whether we like it, PEP 453 has left us with a legacy where users expect the command `pip` to work, and to affect the "obvious" environment (where "obvious" means the same one as "python" does[^2]). That legacy also includes related assumptions, such as the fact that `subprocess.run([sys.executable, "-m", "pip", ...])` runs pip, and the whole command structure of pip as "how to do installs"[^3]. Continuing to patch over the possibility that PEP 453 is no longer the right solution doesn't help, it simply entrenches the existing approach, by making the compatibility issues of a change harder.

[^1]: The pip zipapp is very recent, and has only been possible since one of our dependencies, certifi, changed to become safe for use from a zipfile.

[^2]: I'm ignoring complications like `pip3` and `python3`. Those are (IMO) bad design decisions that have caused more problems than they solved, no matter how necessary `python3` might have been on Unix.

[^3]: If we didn't have those assumptions, a `uv pip` that mirrored pip's user interface would have been far less necessary.

---

_Referenced in [astral-sh/uv#12738](../../astral-sh/uv/issues/12738.md) on 2025-04-08 17:25_

---

_Assigned to @zanieb by @zanieb on 2025-04-11 14:52_

---

_Referenced in [astral-sh/uv#13098](../../astral-sh/uv/issues/13098.md) on 2025-04-25 16:33_

---

_Referenced in [astral-sh/uv#14873](../../astral-sh/uv/issues/14873.md) on 2025-07-24 15:58_

---

_Comment by @gwillen on 2025-08-18 23:17_

As a user of uv, here is the situation I think I am in:
- The system contains 'pip' on my path. I cannot easily fix that, without modifying global properties of the system which may not be practical for me to modify. I would like such modifications to be considered out of scope.
- I am a naive person who does not know what a 'zipapp' is, and does not super want to have to learn, seeing as python packaging is already the most catastrophically complicated thing I have to regularly deal with. Maybe second to npm.
- I have created and activated a uv virtualenv.
- I know that I should type `uv pip install` to install packages.
- However, instead, once in awhile I accidentally type `pip install`, because I am a dumb idiot who hasn't had my coffee yet. (Somehow making me stop being a dumb idiot is also out of scope.)
- This results in trashing my local user site-packages, in a confusing way that is potentially kind of hard to fix, if I have strong feelings about what's in them, and did not write down what it was before I screwed up.

I agree that it's probably bad that users have a belief that every venv has pip in it, but they do have that belief, and even though I know that's false, I will keep accidentally typing 'pip' and trashing my site-packages for a long time before my fingers get the memo.

I would _really really_ like some way to configure uv, ~~_once_ per system (not per venv)~~*, to have behavior that protects me from shooting myself in the foot with this gun. Personally I think 'install a stub bin/pip that prints an error message' seems like a great solution. (By contrast, 'print a warning and then dispatch to system pip' does not solve my problem at all, because my site-packages are already trashed by the time I have read the warning.)

I think renaming 'uv pip' to something else would probably help, although I do not think it will solve my problem entirely. There are various tricks I could do that would solve the problem (perhaps I should just 'alias pip="echo noooooooo"'), and maybe that's the way to go, but it doesn't feel great.

I love pip and I appreciate everything that pip has done for me. However, the program I'm running is uv, and I would like it to display behavior that is useful for users of uv (which I am), and if that means that I can sign up for "please override pip in my uv venvs", well, that's between me and my uv configuration, yes?

*Actually, on second thought, per-venv would be fine, as long as I can default it in `/etc/uv/uv.toml` or `~/.config/uv/uv.toml` or what have you. So really all I need is a "--stub-pip" flag to `uv venv`, I think? This is weaker than it might be, since it will not protect new users (which I once was) from their confusion, but at least they will only have to learn once.

---

_Comment by @notatallshaw on 2025-08-19 15:17_

While not helpful to the default UX of either pip or uv, you can protect yourself from installing into the user site by setting the following with your system level pip:

```bash
pip config set install.user false
```

I previously set this as the default configuration for pip in a corporate environment to stop a similar situation from happening (it tended to trash their Anaconda environments). 

Also, you could ask your system owner (e.g. Linux distribution maintainer) why they don't set an externally managed file: https://packaging.python.org/en/latest/specifications/externally-managed-environments/#externally-managed-environments. This will also prevent you from accidentally installing into the system and system user sites, with a message that can be customized by the system owner.

I understand both of these solutions only help if someone has had the foresight to set them up,

---

_Comment by @gwillen on 2025-08-19 18:58_

@notatallshaw Thanks, this is interesting and helpful.

`install.user false` apparently causes pip to fall back on installing systemwide, which then fails with a permission error. That does prevent the issue (assuming one never runs pip as root, which I at least know not to do), although the ergonomics are kind of bad, since it goes through the whole install process first, up to the point where it actually tries to write to the disk (and the resulting error message is unhelpful.) So it's really only useful as a reminder to someone who already knows what's happening, and just wishes to be reminded. (But that is the state I'm in, and I will start using this setting -- thanks!)

Regarding `EXTERNALLY-MANAGED` -- in my specific situation this appears to be impossible, because I am on Ubuntu 22.04 LTS, which will be suported until 2027, (with "extended support" until 2032, and "legacy support" until 2034), and it shipped with Python 3.10. (`EXTERNALLY-MANAGED` seems to have been introduced in Python 3.11.)

However, it looks like Ubuntu did start using this mechanism with Ubuntu 23, so this problem will tend to solve itself as people update. This honestly makes me a lot less fussed about the whole thing, so thanks for teaching me that this mechanism exists -- I was not aware of it.

(In fact, I think `EXTERNALLY-MANAGED` is the correct mechanism to address the specific problem that this issue caused me, which was an Ubuntu distro package that was broken by the existence of a python package at the wrong version, accidentally installed with `pip` into the user site-packages directory.)

---

_Referenced in [astral-sh/uv#17066](../../astral-sh/uv/issues/17066.md) on 2025-12-10 13:16_

---

_Comment by @Stefanhg on 2025-12-10 13:50_

> At the very least, we could:
> 
> 1. Add a `pip` shim which _warns_ and forwards to the system `pip`.

I had a short read on the conversations provided here.

I think warning the user and forwards it, is a good solution, just ensure the warning is thrown after the command has been executed, so the user is sure to see it. 

---

_Comment by @andersmmg on 2025-12-10 21:24_

An issue I sometimes have is I create a uv venv for running a project (that someone else made) and it tries to automatically install some needed packages. There are some popular projects that do this, and to make them work I have to install pip in the venv. It would be nice to at least have an option or a command to forward pip to uv pip per project or something, so it can get the benefits without much manual work. For now I am thinking I might add an alias in my .zshrc and see how well that works

---
