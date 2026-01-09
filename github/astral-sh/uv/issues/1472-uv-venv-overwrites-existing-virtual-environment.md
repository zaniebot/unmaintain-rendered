---
number: 1472
title: "`uv venv` overwrites existing virtual environment on subsequent run"
type: issue
state: closed
author: Slyfest
labels:
  - virtualenv
  - breaking
assignees: []
created_at: 2024-02-16T10:14:04Z
updated_at: 2025-07-17T22:20:26Z
url: https://github.com/astral-sh/uv/issues/1472
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv venv` overwrites existing virtual environment on subsequent run

---

_Issue opened by @Slyfest on 2024-02-16 10:14_

**Current behaviour**: subsequent runs of `uv venv` overwrite the existing virtual environment.

**Suggested behaviour**: How about adding a prompt like "the virtual env is already created, confirm to recreate from scratch".

---

_Comment by @zanieb on 2024-02-16 14:47_

Thanks for the report, related https://github.com/astral-sh/uv/issues/1274

---

_Renamed from "uv venv overwrites existing virtual environment on subsequent run" to "`uv venv` overwrites existing virtual environment on subsequent run" by @zanieb on 2024-02-16 14:47_

---

_Label `bug` added by @zanieb on 2024-02-16 14:47_

---

_Comment by @zanieb on 2024-02-17 04:14_

I don't think we should delete them by default cc @charliermarsh 

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:38_

---

_Comment by @gaborbernat on 2024-02-20 02:13_

For reference, the upstream `virtualenv` has a `--clear` flag, and when that's not set it will update the virtual environment and the seeded packages, but without touching anything else. This way, running without a `--clear` is essentially a repair option.

---

_Referenced in [astral-sh/uv#1777](../../astral-sh/uv/issues/1777.md) on 2024-02-20 20:49_

---

_Comment by @flyaroundme on 2024-02-21 18:06_

So, what do you think on the UX of this command? For me it also seems unintuitive when I am using `uv venv` and the existing env is overwritten with an empty one.
I propose a simple thing, like adding a `--clear` flag for `uv venv` command. if there is an existing venv and the flag is not set, then we simply do nothing. In other cases we preserve the old behaviour. What do you think?

---

_Comment by @Slyfest on 2024-02-21 18:10_

@flyaroundme that would definitely be a much less surprising behavior. Good suggestion!

---

_Referenced in [astral-sh/uv#1824](../../astral-sh/uv/pulls/1824.md) on 2024-02-21 18:47_

---

_Comment by @gaborbernat on 2024-02-21 19:12_

> if there is an existing venv and the flag is not set, then we simply do nothing.

I'd imagine a better UX would be to fail hard and loud.

---

_Label `bug` removed by @charliermarsh on 2024-03-18 20:54_

---

_Referenced in [astral-sh/uv#2548](../../astral-sh/uv/pulls/2548.md) on 2024-03-19 19:04_

---

_Comment by @Artalus on 2024-04-30 10:19_

+1 on "fail hard and loud" to avoid ambiguity - but then I believe `uv venv` should support both "force keep" and "force clean" workflows. F.x. I have been happily using `python -m venv` for the last few years somewhat like this:
```bash
#  run `source ./project.sh` to get the project and environment for it up and running
export SOMETHING=something
python -m venv
source ./venv/bin/activate
python -m pip install -e .[dev]
```
, keeping the project and its deps in sync - but not wiping any additional tools if I have put there for some experiments. Yet I can easily imagine that one would like to wipe the existing venv instead.

---

_Comment by @zanieb on 2024-04-30 13:39_

Lots of ux discussion in https://github.com/astral-sh/uv/pull/2548

---

_Referenced in [astral-sh/uv#4276](../../astral-sh/uv/issues/4276.md) on 2024-06-12 15:35_

---

_Comment by @verveguy on 2024-08-23 14:29_

`uv venv --allow-existing` provides the desired behavior - don't recreate the venv every time.

Perhaps the question is whether the sense of the default should be inverted i.e. update existing by default, only replace if `--clear` is specified.

---

_Comment by @janimo on 2024-09-11 13:37_

I find it very unusual for a cli tool to overwrite and existing directory by default, without warning or prompting.

Now I have learned this is the intended behavior, but the number of people reporting being confused by this suggests that some UI change would be helpful.

---

_Comment by @zanieb on 2024-09-11 13:39_

I don't love the existing behavior, but, for what it's worth, this issue has about three upvotes and we see an order of magnitude more than that on issues that many people encounter.

---

_Comment by @usetheforce on 2024-09-28 11:28_

+100 on "fail hard and loud". Just lost a bunch of files because of this. 
uv mimics pip's behaviour in many places, but not here -- where damage can be done really easily? python -m venv leaves any existing files in place (which are not part of the venv-local python).

I mean: what's the win in the current behaviour? Spare one rm -rf *?

uv is no doubt a brilliant tool and I love it. But PLEASE change this.

---

_Comment by @kmalinich on 2024-10-10 22:22_

+10000 on fail hard and loud.

Because of this I accidentally grenaded my entire home assistant installation. Now my smart home is borderline useless. No heat, no lights, nothing; I have my whole house in there. 

Yes I have backups, but I have to retrieve them and restoring them isn't an immediate process - meanwhile my baby is crying, my wife's pissed, the list goes on.

I've been a Linux engineer for close to a decade, in some high caliber environments - and I think I can count on one hand the times where I used a tool, with only one or two arguments, copied and pasted from the --help usage message, that automatically assumed that I wanted to completely blow away everything in the current directory, and was so confident in it, that it didn't even give me a simple "You sure? [y/N]" .

Maybe I just wanted to share my story, or just vent.. but damn man this one hurt. 

---

_Comment by @charliermarsh on 2024-10-10 22:36_

@kmalinich -- Hey -- regardless of what decision we make here, just wanted to say that I can feel your frustration coming through and I'm really sorry that you ran into this in such a critical path. A lot for us to consider in your message.

---

_Comment by @kmalinich on 2024-10-11 00:44_

@charliermarsh -- I want you to know that your words meant a lot to me. Thank you. I'm all back up and running here, and I've calmed down a fair bit. Sorry about that, that was some pretty explosive messaging on my part.

`uv` is an awesome tool, and I totally understand that I'm not using it in a way that matches a majority of its users, and that's a big part of what led to my situation occurring. Totally my fault.

Anyways, thanks again for the kind words. It's nice to .. just be heard, sometimes.

---

_Comment by @zanieb on 2024-10-11 02:17_

I'm down to change this in 0.5.0 — but we'll need to discuss this as a team and figure out if there's consensus on a new ux.

---

_Comment by @vishnucoder1 on 2024-12-28 09:37_

Started using uv and this default behaviour of overwriting the venvs is confusing.

---

_Added to milestone `v0.6.0` by @zanieb on 2024-12-28 17:16_

---

_Comment by @zanieb on 2024-12-28 17:16_

Adding this to the 0.6.0 milestone so we consider it in the next breaking release.

---

_Referenced in [astral-sh/uv#10325](../../astral-sh/uv/issues/10325.md) on 2025-01-06 11:14_

---

_Comment by @knirch on 2025-02-12 22:45_

> I don't love the existing behavior, but, for what it's worth, this issue has about three upvotes and we see an order of magnitude more than that on issues that many people encounter.

As someone coming back from a hiatus, who typed uv venv in wrong window, got that non uv managed .venv nuked, and not fluent in github jargon; where do I upvote?

---

_Comment by @zanieb on 2025-02-12 22:48_

There's a 👍  at <img width="1072" alt="Image" src="https://github.com/user-attachments/assets/e7890a6d-511e-430e-829d-1a38b91ca6b8" />

---

_Removed from milestone `v0.6.0` by @zanieb on 2025-02-15 02:34_

---

_Added to milestone `v0.7.0` by @zanieb on 2025-02-15 02:34_

---

_Comment by @cagdemir on 2025-02-19 07:41_

just nuked a virtual environment I created, interesting choice of behavior for uv venv

---

_Referenced in [astral-sh/uv#11893](../../astral-sh/uv/issues/11893.md) on 2025-03-02 15:08_

---

_Comment by @dakota-hawkins on 2025-03-04 15:25_

Just commenting to voice my support for "hard and loud" failing, and definitely **not** overwrite be default. Thanks for the great tool! It's really been a breeze to work with other than this small hiccup :)

---

_Comment by @chschroeder on 2025-03-09 09:55_

Same here. Just lost a venv due to this. Please don't override by default.

---

_Comment by @Sora233 on 2025-03-31 07:27_

surprised and end up here. it is confuseing.

---

_Label `breaking` added by @zanieb on 2025-04-01 21:32_

---

_Referenced in [astral-sh/uv#12910](../../astral-sh/uv/pulls/12910.md) on 2025-04-15 21:54_

---

_Removed from milestone `v0.7.0` by @zanieb on 2025-04-21 19:15_

---

_Added to milestone `v0.8.0` by @zanieb on 2025-04-21 19:15_

---

_Referenced in [astral-sh/uv#13624](../../astral-sh/uv/pulls/13624.md) on 2025-05-23 18:53_

---

_Comment by @stevenae on 2025-05-23 18:53_

Let's at least fix the documentation, which says it will do the opposite:

https://github.com/astral-sh/uv/pull/13624

---

_Referenced in [astral-sh/uv#13684](../../astral-sh/uv/pulls/13684.md) on 2025-05-27 15:16_

---

_Comment by @gsimard on 2025-06-18 21:40_

Still not resolved ? People here are being extremely polite about this issue. I just nuked my environment as well.

---

_Assigned to @jtfmumm by @jtfmumm on 2025-06-24 08:22_

---

_Referenced in [astral-sh/uv#14309](../../astral-sh/uv/pulls/14309.md) on 2025-06-27 10:02_

---

_Comment by @Stefan-Heimersheim on 2025-07-03 17:17_

The silent overwrite seems very unexpected, just happened to me as well.

I've read through a few of the linked issues but I'm confused what the status on this issue is:
- What are the arguments for overwriting existing environments? (in particular previously-created uv environments)
- I've seen various arguments against in this thread and others, generally deleting files without checking seems risky
- Is this a breaking backwards compatibility issue (scripts using `uv venv` assuming it will overwrite without asking)?

Are you looking for help (someone to make a PR?), more user input, or something else? Can I help?

---

_Comment by @zanieb on 2025-07-03 17:38_

The primary argument for overwriting environments is that it's aligned with other behaviors in uv, e.g., `uv sync -p 3.11` followed by `uv sync -p 3.12` will delete the `.venv` and reconstruct it. Philosophically, the virtual environment shouldn't have manually curated packages via imperative commands, it should be constructed from a declarative file and therefore the deletion of it is inconsequential. Of course, that's not how everyone works, which is why we're considering changing the behavior — especially when you're using `uv venv` explicitly rather than through our implicit project commands.

Additionally, we've already shipped the overwrite behavior so there's a breaking backwards compatibility concern for existing users that rely on this behavior which we have no way to measure — the rough metric is the number of 👍 here which is significantly lower than other popular issues (though still in the top 25). We are generally _very_ wary of releasing breaking changes that will cause users' workflows to fail.

Anyway, that's just to answer your bullets. There's a draft approach at

- https://github.com/astral-sh/uv/pull/14309

which we're considering for the next breaking release. You can provide feedback on that approach if you'd like to help!



---

_Comment by @stevenae on 2025-07-03 17:40_

I would like to advocate for prominently documenting this behavior as a
compromise.

On Thu, Jul 3, 2025, 1:38 PM Zanie Blue ***@***.***> wrote:

> *zanieb* left a comment (astral-sh/uv#1472)
> <https://github.com/astral-sh/uv/issues/1472#issuecomment-3033051920>
>
> The primary argument for overwriting environments is that it's aligned
> with other behaviors in uv, e.g., uv sync -p 3.11 followed by uv sync -p
> 3.12 will delete the .venv and reconstruct it. Philosophically, the
> virtual environment shouldn't have manually curated packages via imperative
> commands, it should be constructed from a declarative file and therefore
> the deletion of it is inconsequential. Of course, that's not how everyone
> works, which is why we're considering changing the behavior — especially
> when you're using uv venv explicitly rather than through our implicit
> project commands.
>
> Additionally, we've already shipped the overwrite behavior so there's a
> breaking backwards compatibility concern for existing users that rely on
> this behavior which we have no way to measure — the rough metric is the
> number of 👍 here which is significantly lower than other popular issues
> (though still in the top 25). We are generally *very* wary of releasing
> breaking changes that will cause user's workflows to fail.
>
> Anyway, that's just to answer your bullets. There's a draft approach at
>
>    - #14309 <https://github.com/astral-sh/uv/pull/14309>
>
> which we're considering for the next breaking release. You can provide
> feedback on that approach if you'd like to help!
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/1472#issuecomment-3033051920>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAFZOHLDUB2Q7F2NWFLYML33GVTBLAVCNFSM6AAAAABDLYS7OOVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTAMZTGA2TCOJSGA>
> .
> You are receiving this because you are subscribed to this thread.Message
> ID: ***@***.***>
>


---

_Comment by @YodaEmbedding on 2025-07-15 04:02_

The current `uv venv` is also inconsistent w.r.t. symlinks. Three separate behaviors depending on directory state:

---

If symlinked directory does not exist:

```bash
λ rm -rf .venv
λ ln -rs ~/.local/state/uv/virtualenvs/"${PWD##*/}" .venv

λ uv venv
Using CPython 3.8.20
Creating virtual environment at: .venv
uv::venv::creation

  × Failed to create virtualenv
  ╰─▶ failed to create directory `$PWD/.venv`: File exists (os error 17)
```

---

If symlinked directory exists:

```bash
λ rm -rf .venv
λ ln -rs ~/.local/state/uv/virtualenvs/"${PWD##*/}" .venv
λ mkdir ~/.local/state/uv/virtualenvs/"${PWD##*/}"

λ uv venv
Using CPython 3.8.20
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

λ ls -la .venv
lrwxr-xr-x 1 user group 54 2025-07-14 00:00:00 .venv -> ../../../.local/state/uv/virtualenvs/"${PWD##*/}"

# Yay!
```

---

If symlinked directory exists, but `uv venv` is run *twice*: 

```bash
λ rm -rf .venv
λ ln -rs ~/.local/state/uv/virtualenvs/"${PWD##*/}" .venv
λ mkdir ~/.local/state/uv/virtualenvs/"${PWD##*/}"

λ uv venv
Using CPython 3.8.20
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

λ ls -la .venv
lrwxr-xr-x 1 user group 54 2025-07-14 00:00:00 .venv -> ../../../.local/state/uv/virtualenvs/"${PWD##*/}"

λ uv venv
Using CPython 3.8.20
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

λ ls -la .venv
total 12K
drwxr-xr-x  7 user group  224 2025-07-14 20:56:17 .
drwxr-xr-x 35 user group 1.1K 2025-07-14 20:56:17 ..
drwxr-xr-x 14 user group  448 2025-07-14 20:56:17 bin
drwxr-xr-x  3 user group   96 2025-07-14 20:56:17 lib
-rw-r--r--  1 user group    1 2025-07-14 20:56:17 .gitignore
-rw-r--r--  1 user group   43 2025-07-14 20:56:17 CACHEDIR.TAG
-rw-r--r--  1 user group  211 2025-07-14 20:56:17 pyvenv.cfg

# Destroyed the symlink?!
```

---

_Comment by @zanieb on 2025-07-16 17:39_

@YodaEmbedding please open a new issue so we can discuss it independently.

---

_Referenced in [astral-sh/uv#14670](../../astral-sh/uv/issues/14670.md) on 2025-07-16 19:20_

---

_Closed by @zanieb on 2025-07-17 22:20_

---
