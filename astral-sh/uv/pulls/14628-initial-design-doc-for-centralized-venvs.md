```yaml
number: 14628
title: Initial design doc for centralized venvs
type: pull_request
state: open
author: rsyring
labels: []
assignees: []
base: main
head: 1495-centralized-venvs
created_at: 2025-07-15T14:55:53Z
updated_at: 2025-09-01T05:06:59Z
url: https://github.com/astral-sh/uv/pull/14628
synced_at: 2026-01-12T16:11:19Z
```

# Initial design doc for centralized venvs

---

_@rsyring_

refs: https://github.com/astral-sh/uv/issues/1495#issuecomment-3073898354



---

_Comment by @PhilipVinc on 2025-07-15 16:23_

+1 on my side.
I would change the default directory for the venv cache to be `UV_CACHE_DIR/venvs-v0/` because `ÙV_CACHE_DIR` already falls back to `XDG_CACHE_DIR`?

Setting the venv cache in the same filesystem of  the UV cache is what you want to do anyway to get decent performance.

I will also mention that the calculation of the path within the cache is a good idea in my opinion, and that's what I've been using for the past year or so.

---

_Comment by @rsyring on 2025-07-15 17:03_

> I would change the default directory for the venv cache to be UV_CACHE_DIR/venvs-v0/ because ÙV_CACHE_DIR already falls back to XDG_CACHE_DIR?

Will do.  The XDG_CACHE_DIR in the spec was really a placeholder b/c I'm not sure what the uv internals look like.  I'm sure there is some kind of "global" or context object that takes all env vars and settings into account and provides a `cache_dir` variable that everything else then uses.  I really intend to reference that.  I'll update the spec to clarify.

> I will also mention that the calculation of the path within the cache is a good idea in my opinion, and that's what I've been using for the past year or so.

I understand this to mean that you affirm the "Human‑readable slug + short hash" approach.  Please correct my understanding if needed.

Thanks for the feedback.

---

_Review comment by @DanCardin on `design/1495-centralized-venvs.md`:12 on 2025-07-15 17:26_

fwiw, my vote for a location would be to mirror the directory structure relative to home e.g.

`~/foo/bar/baz/` -> `$XDG_CACHE_HOME/uv/venvs/foo/bar/baz/$venvName` where $venvName defaults to `.venv-$pythonVersion`.

1. i think mirroring the path makes a lot of other things easier, like cleanup
2. having the final component be based on python version makes cross-version actions much faster by not clobbering the same venv for everything `uv run -p 3.10 pytest; uv run -p 3.11 pytest`

---

_@DanCardin reviewed on 2025-07-15 17:26_

---

_@PhilipVinc reviewed on 2025-07-15 20:23_

---

_Review comment by @PhilipVinc on `design/1495-centralized-venvs.md`:12 on 2025-07-15 20:23_

How would you handle environments in a different file system than home/cache?

(This is quite common in HPC where we have 3 file systems, one for HOME where we store few things, one WORK where we store projects but can’t store environments, and SCRATCH where we store environments…)

---

_@DanCardin reviewed on 2025-07-15 21:10_

---

_Review comment by @DanCardin on `design/1495-centralized-venvs.md`:12 on 2025-07-15 21:10_

(i'm amending my above XDG_CACHE_DIR in light of the prior comment about UV_CACHE_DIR).

i'm not sure whether i fully grasp what this system looks like/expects, but i dont think different filesystems are inherently a problem.

You'd set e.g. `UV_CACHE_DIR=/scratch` i assume. Then i would expect the calculated path to be based off the least common ancestor of $HOME and the current project path. So in "normal" cases, `/home/foo` and `/home/foo/projects/bar` -> `/scratch/venvs/projects/bar`. And in less normal cases (which maybe seems like what you're suggesting) `/home/foo` and `/work/projects/bar` you instead get `/scratch/venvs/$root/work/projects/bar` (or whatever alternative to $root to disambiguate relative vs absolute paths) because there is no common root. Or ignore relativity to $HOME at all.

whether or not I'm correctly addressing your concerns, i think it would have the same issues (or lackthereof) for the other suggested option of basing it on a hashed version of the full path.

Much of the details of my long middle paragraph is not critically important, and there's much to bikeshed there. The point is mostly to avoid losing the path information into a hash by way of actually using the known-unique original paths. And to make it intuitive to locally navigate to a venv without **needing** to run a `uv` command to calculate it for you (**that** is how `poetry` works and i find it quite annoying)

---

_@thehesiod reviewed on 2025-07-15 22:38_

---

_Review comment by @thehesiod on `design/1495-centralized-venvs.md`:12 on 2025-07-15 22:38_

or what about having something in pyproject.toml that specifies this behavior?  another option is in the venv folder having a special uv file that has a symlink back to the base folder it corresponds to, this should solve most problems as you can ensure the folder name is unique via the hashing, and have the required back references either in a symlink, or some custom meta file in the venv folder.

I'm not sure always relative to home folder is a good idea, in our environment we often put the related pyproject.toml in a folder like /usr/fbn/foo/bar.  Another option is relative to the git root.  There are so many variants that make sense for different workflows it seems like this should be configurable and opt-in.

---

_@zanieb reviewed on 2025-07-15 22:42_

---

_Review comment by @zanieb on `design/1495-centralized-venvs.md`:12 on 2025-07-15 22:42_

I don't think we want to try to replicate the entire path into this centralized location, it sounds feasible to run into path length limits (which have a been a frequent problem on Windows) and it doesn't match our typical patterns for storing paths.

It does seem annoying to need to use uv to discover an environment path, but wouldn't something like using the project name as a prefix obviate that problem for most use-cases?

---

_@thehesiod reviewed on 2025-07-15 23:00_

---

_Review comment by @thehesiod on `design/1495-centralized-venvs.md`:12 on 2025-07-15 23:00_

@zanieb for reference I use git worktree to have multiple versions of our git repo, comparatively pipenv's default behavior is pretty annoying to me as I don't know which worktree it belongs to.  In my ideal world there would be a setting in pyproject.toml saying something like:
```toml
[tool.uv]
venv_root_style = "relative_to"
venv_relative_root_path = "/Users/user/dev"
```

or something to that effect.  I mean or that could be like an opt-in suffix such that the root folder name is always the hash as discussed and that relative is appended to it just for reference, for when you're trying to select the correct venv like in the pycharm interpreter selector.

---

_Review comment by @DanCardin on `design/1495-centralized-venvs.md`:12 on 2025-07-16 14:25_

perhaps getting too complicated, but the config _could_ allow a `venv_path_format =` option that's essentially a format string allowing path/hash/python-version/whatever context to make it configurable. Then you can choose whatever default, and I can set `{project}-{hash}-{python_version}` or `{relative_path}/{project]/{python_version}` (or however it'd work)

not sure if the complexity is worth the squeeze, but it seems like it sidesteps displeasing anyone's particular workflow, while letting you choose whatever default behavior you think is most foolproof/generally applicable.

And as I mentioned above, regardless of specific path impl, including the python version in the path (or at least ensuring the same project operating with different python versions doesn't clobber the venvs) is main real concerning factor for at least myself.

---

_@DanCardin reviewed on 2025-07-16 14:25_

---

_@zanieb reviewed on 2025-07-16 14:27_

---

_Review comment by @zanieb on `design/1495-centralized-venvs.md`:12 on 2025-07-16 14:27_

It's plausible we could allow customized paths, but that sounds out of scope for the first pass on the feature.

---

_Comment by @rsyring on 2025-07-16 14:37_

I figured I'd give this another few days for comments and then address them all and update the proposal.

Where there are different preferences expressed, my intention is to make a reasonable decision with the MVP goal in mind, aware that it may potentially not satisfy everyone, giving preference to input given by current uv contributors.

If there is a "designated" decision maker for something like this in the uv org, they are welcome to step into the decision making role at any point and I'll take a step back.

Thanks everyone.

---

_Comment by @eabase on 2025-07-16 17:34_

This looks over-engineered and not like a MVP.
Why do you need to specify different python versions within the same project (or venv)?

See my comments [here](https://github.com/astral-sh/uv/issues/1495#issuecomment-3079518311).

---

_@zanieb reviewed on 2025-07-28 20:59_

---

_Review comment by @zanieb on `design/1495-centralized-venvs.md`:12 on 2025-07-28 20:59_

I started thinking about how we'd enable this feature... and we could actually allow it via customized paths without doing any additional design. Curious for your thoughts https://github.com/astral-sh/uv/pull/14937

---

_@DanCardin reviewed on 2025-07-28 21:28_

---

_Review comment by @DanCardin on `design/1495-centralized-venvs.md`:12 on 2025-07-28 21:28_

honestly seems pretty elegant because it has no effect on anyone currently manually setting that value to a static value and handling any dynamism in their own tooling.

plus you can still (again at some point) if you want, ship a dedicated setting that's either just this, or a `managed_venvs = true` or whatever with whatever semantic; but that's still just a manipulation of the default `UV_PROJECT_ENVIRONMENT`, if unspecified.

i was initially imagining the complexity of this would be getting python-style templating, but i suppose the more basic string replacement will serve here probably just as well since the values are still all owned by you.

---

_@zanieb reviewed on 2025-07-28 21:34_

---

_Review comment by @zanieb on `design/1495-centralized-venvs.md`:12 on 2025-07-28 21:34_

Yeah I imagine we could pick up a templating library, but... hopefully that's just not needed.

---

_Comment by @rsyring on 2025-07-30 15:33_

I've updated the proposal based on the feedback/discussion here.  Thoughts?

---

_Comment by @rsyring on 2025-08-11 14:22_

Any thoughts on my updated proposal?

Is there a formal approval process?  If not, at what point it it safe to start the implementation?

---

_Comment by @eabase on 2025-08-29 21:09_

@rsyring 
Hi Randy,

Regarding:
* https://github.com/astral-sh/uv/pull/14628

Sorry, but I'm not happy with your proposal, as it lacks clarity and I fail to see how it would help the current implementation. I'm also worried about the `cache` idea. For me anything that is in a *cache* can be discarded at any time without serious consequences, as it can be easily rebuilt. However, this is not the case for the venv "repo" directories we are talking about and discussing in #1495. Very often we have certain dependencies for HW related projects (like Nvidia, Nordic Semi, etc) that requires different package versions, and where nobody should be able to do a generic *"clear cache"* to wipe out all your venv's. I'm sure (or at least I hope) that I have the wrong understanding of this. 

Also, the  `"Human‑readable slug + short hash"` is just not a productivity improvement. I don't want to have to look up and carefully type in any kind of hash in order to *activate, copy, move* or *duplicate* any venv. I'm certainly missing to see the use case for this?

**So can you please update us on what exactly is the new proposal and what functionality it provides that simplifies life to the user, via UX?**


---

_Comment by @rsyring on 2025-08-29 21:20_

@eabase I can appreciate that you don't prefer this proposal.  However, I did process your previous comments on this issue and I believe they are all addressed in the "Out of Scope / Rejections" section.

I don't plan on doing any additional work on this proposal until someone from Astral gives some definitive feedback.  While I think this PR/idea had some momentum originally, the momentum/interest has seemed to die.  And I don't want to put more time into it unless there is the possibility of an implementation somewhere in the near-term.   If we get that feedback, and it aligns with your concerns/preferences, I'm happy to take another stab at revising the proposal. 

Thanks.

---

_Comment by @eabase on 2025-08-30 12:12_

@rsyring 
> they are all addressed in the "Out of Scope / Rejections" section

That's exactly my point of my last comment. I don't understand what you mean with `"Out of Scope / Rejections"`. Are you rejecting your own proposal, or the comments/proposals from everyone else?


---

_Comment by @rsyring on 2025-08-30 13:00_

@eabase 

> I don't understand what you mean with "Out of Scope / Rejections". Are you rejecting your own proposal, or the comments/proposals from everyone else?

In the design doc, the final section of the document is "Out of Scope / Rejections."  That section lists items that I considered but decided were out of scope or am rejecting for this proposal.  For example, I don't think we should be concerned with the accidental deletion of virtualenvs in the cache and I give my reasoning in that section.

Hope that helps.

---

_Review comment by @xmatthias on `design/1495-centralized-venvs.md`:56 on 2025-08-30 14:15_

I think you'll want to look at how pnpm approaches this (maybe not for this proposal, but in general)

They symlink the whole `node_modules` directory - and this also works on windows. 
No clue HOW they do it exactly - but i think it'd allow for a global cache of installed modules - greatly reducing duplicate files on the system.


---

_@xmatthias reviewed on 2025-08-30 14:15_

---

_Review comment by @eabase on `design/1495-centralized-venvs.md`:56 on 2025-08-30 14:32_

2 bad ideas baked into 1.

1. Using any symlinks in windows in general, is very rarely needed and if you use junctions, you can easily break your whole system. Very few people knows how to use those right, and any noob trying to delete one, can brick their OS. 

2. Having the venv "point back to the project directory", defeats more solutions than it solves. Why would you need this? How, then, would you use multiple project directories sharing a venv? 

---

_@eabase reviewed on 2025-08-30 14:32_

---

_@rsyring reviewed on 2025-08-30 14:33_

---

_Review comment by @rsyring on `design/1495-centralized-venvs.md`:56 on 2025-08-30 14:33_

It would appear uv already addresses this:

- https://github.com/astral-sh/uv/issues/11504
- https://docs.astral.sh/uv/reference/settings/#link-mode

---

_@eabase reviewed on 2025-08-30 14:36_

---

_Review comment by @eabase on `design/1495-centralized-venvs.md`:62 on 2025-08-30 14:36_

>  accidental deletion of a venv is inconsequential since it will be recreated by any uv command that needs it.

**This is simply not true!**

If you got a bunch of LLM or Nvidia crap in your venv, you're talking about ~ several GB of venv. Maybe even something you built/tweaked on your own for dev or private purposes. So, no the "uv command" is not gonna rebuild any of that, and at worst you may suffer substantial code/data loss. 

---

_@rsyring reviewed on 2025-08-30 14:38_

---

_Review comment by @rsyring on `design/1495-centralized-venvs.md`:56 on 2025-08-30 14:38_

@eabase please understand, those are rejected ideas in that section.

> How, then, would you use multiple project directories sharing a venv?

We wouldn't.  This proposal explicitly identifies sharing venvs as out of scope.  I understand you have a use case for this, and care a lot about it, we don't consider that use case valid for this proposal.

---

_@rsyring reviewed on 2025-08-30 14:38_

---

_Review comment by @rsyring on `design/1495-centralized-venvs.md`:62 on 2025-08-30 14:38_

 This proposal explicitly identifies sharing venvs as out of scope.  I understand you have a use case for this, and care a lot about it, but I don't consider that use case valid for __this__ proposal.

---

_@eabase reviewed on 2025-08-30 14:43_

---

_Review comment by @eabase on `design/1495-centralized-venvs.md`:62 on 2025-08-30 14:43_

What exactly is your **use case** then? 
Because *this* very proposal concerns "centralized venvs", so I don't understand how you can say *"This proposal explicitly identifies sharing venvs as out of scope."*?

---

_@rsyring reviewed on 2025-08-30 14:48_

---

_Review comment by @rsyring on `design/1495-centralized-venvs.md`:62 on 2025-08-30 14:48_

Shared venvs are not the same as centralized venvs.  They are separate and distinct use cases.

Currently, uv always stores a [project's](https://docs.astral.sh/uv/guides/projects/) virtual env in the project's root folder as `.venv`.  

I don't like having those `.venv`s scattered all over my projects and want them in a central location.  For me, it's just file/directory hygiene.  But for others working on other platforms, it might be because their project folders are on network shares and they need the venvs on local disks for performance reasons.

You can see more about my workflow and goals here: https://github.com/jdx/mise/discussions/4301

---

_@xmatthias reviewed on 2025-08-30 14:50_

---

_Review comment by @xmatthias on `design/1495-centralized-venvs.md`:56 on 2025-08-30 14:50_

> It would appear uv already addresses this:

So uv even has approaches for symlinks (also on windows) in place - making this a quite odd "rejection" - which i think should **at least** have more reasoning behind.

---

_@xmatthias reviewed on 2025-08-30 14:53_

---

_Review comment by @xmatthias on `design/1495-centralized-venvs.md`:62 on 2025-08-30 14:53_

Well i do think that
* Marking these conversations as resolved while conversation is still ongoing is an absolutely inpolite approach. @rsyring You should unresolve them until the discussion is **actually** resolved for _both parties_. Otherwise it's a "thanks for the feedback, but i'd rather ignore it" - which i don't think is what you're trying to imply.
* for the topic at hand (@eabase ):
  *  with the [symlinking](https://docs.astral.sh/uv/reference/settings/#link-mode) `uv` does - your "actual" files live in some random cache file (i don't think a central location would necessarily change this). so deletion of the venv won't delete several GB of data - it'll just remove the symlink of it. You'll hence not lose the downloaded data - just the venv - which i guess a new "uv venv" and "uv pip install <whatever>" can now pretty easily bring back.


---

_Review comment by @rsyring on `design/1495-centralized-venvs.md`:56 on 2025-08-30 14:56_

I've not looked into how uv handles the shared cache in Windows.  If the symlink equivalent works there, then having a symlink back to the project directory is feasible.  

I'd still argue a text file with the path as it's content is simpler and has less edge cases and therefore is the more appropriate solution. 

---

_@rsyring reviewed on 2025-08-30 14:56_

---

_@xmatthias reviewed on 2025-08-30 14:59_

---

_Review comment by @xmatthias on `design/1495-centralized-venvs.md`:56 on 2025-08-30 14:59_

I'm not even arguing the decision to not symlink back to the project. This may facilitate _shared_ environments (maybe in the future).

I just think it should be argued better - not with "because windows" - where the problem has actually been solved _within the same project_.

---

_@rsyring reviewed on 2025-08-30 15:03_

---

_Review comment by @rsyring on `design/1495-centralized-venvs.md`:62 on 2025-08-30 15:03_

@xmatthias  Wasn't my intention to be rude.  But it is my intention to mark the conversations as resolved in the sense that, I've considered these topics and, so far, the conversations haven't been fruitful in revealing new scenarios that would change the proposal.  I've already considered and addressed these topics.  I would like to move on.  Not as a means of being rude but as a means of making the best use of our collective time.  :)

---

_Review comment by @rsyring on `design/1495-centralized-venvs.md`:56 on 2025-08-30 15:04_

Fair enough.  If we get some indication from the project maintainers that they are likely to move on this proposal, I'll consider updating the doc at that point.

---

_@rsyring reviewed on 2025-08-30 15:04_

---

_Review comment by @xmatthias on `design/1495-centralized-venvs.md`:62 on 2025-08-30 15:08_

to be quite honest - i'm against this proposal.
It's designed for your specific need, with all comments that don't fit your exact idea as "marked as out of scope".
While i think it's good that other things have been considered and marked as out of scope - i think _what_ is out of scope should be up for discussion.

I'll take a step back from this discussion - as it's clear that you're not open for this (the topic of marking points as resolved immediately makes this VERY clear).
that's unfortunately then an "enhancement" that won't really enhance UV - not in a way that would suit more than a handful of people. 

---

_@xmatthias reviewed on 2025-08-30 15:08_

---

_@xmatthias reviewed on 2025-08-30 15:12_

---

_Review comment by @xmatthias on `design/1495-centralized-venvs.md`:46 on 2025-08-30 15:12_

`uv cache clean` already exists and has functionality ([remove all cache](https://docs.astral.sh/uv/reference/cli/#uv-cache-clean)).

I think there should be a dedicated command for this - which cleans all environments - but leaves the actual cache in place (under the assumption that centralized venvs are combined with local venvs).


---

_@xmatthias reviewed on 2025-08-30 15:13_

---

_Review comment by @xmatthias on `design/1495-centralized-venvs.md`:56 on 2025-08-30 15:13_

Well i think that's backwards, personally. 

once you get project maintainer approval / focus - they should see the final picture and be in a position to approve not a "i've waited for you to tell me to finish the proposal". That'll mean they'll have to get to the same point twice which - in a repo with 1.9k open issues - could take ages.

---

_@rsyring reviewed on 2025-08-31 17:44_

---

_Review comment by @rsyring on `design/1495-centralized-venvs.md`:46 on 2025-08-31 17:44_

> I think there should be a dedicated command for this

I agree that would be preferable.

However, in this iteration, I specifically didn't address this because I wanted to avoid specifications that would lead to additional debate or work and, perhaps, delay the initial implementation.  

IMO, there are two functions that uv could provide:

- clean: completely delete all centralized venvs
- prune: delete any centralized venv where the venv's project directory (which is tracked in `$venv_dir/uv-project-path.txt`) no longer exists

Where in the CLI those functions live is what I thought could lead to delay.  Options I've considered:

- `uv venv --cache-clean`
- `uv venv --cache-prune`
- `uv cache clean --venvs/--no-venvs`
- `uv cache prune --venvs/--no-venvs`
- `uv cache venvs clean`
- `uv cache venvs prune`

With my preference being the last two.

---

_@xmatthias reviewed on 2025-09-01 05:06_

---

_Review comment by @xmatthias on `design/1495-centralized-venvs.md`:46 on 2025-09-01 05:06_

I'm not sure i agree on your "delay implementation" stance - the current proposal to overload an existing command (without optionality, from the proposal) can have the adverse effect and (at least in my opinion / view) can/could lead to more discussions.

I think these options could be listed as "alternatives" at least - giving astral folks the option to pick the one they prefer"? 
Currently it's "we'll use `uv cache clean`." - if that doesn't fit their plans / UX view - there's no considered alternatives listed (outside of the above comment).

---
