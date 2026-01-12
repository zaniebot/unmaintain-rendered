```yaml
number: 2548
title: "Allow `--force` to overwrite existing virtualenv"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/force
created_at: 2024-03-19T19:00:10Z
updated_at: 2024-05-01T16:34:53Z
url: https://github.com/astral-sh/uv/pull/2548
synced_at: 2026-01-12T16:05:05Z
```

# Allow `--force` to overwrite existing virtualenv

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/2529.

## Test Plan

- `mkdir .venv`
- `touch .venv/foo`
- `cargo run venv` (ensure failure)
- `cargo run venv --force` (ensure success)
- `cargo run venv --force` (ensure success again)


---

_Label `enhancement` added by @charliermarsh on 2024-03-19 19:00_

---

_Label `cli` added by @charliermarsh on 2024-03-19 19:00_

---

_Renamed from "Allow --force to overwrite existing virtualenv" to "Allow `--force` to overwrite existing virtualenv" by @charliermarsh on 2024-03-19 19:00_

---

_Marked ready for review by @charliermarsh on 2024-03-19 19:02_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-19 19:02_

---

_Comment by @zanieb on 2024-03-19 19:04_

Should we resolve https://github.com/astral-sh/uv/issues/1472 too? I imagined gating that behavior with `--force` when we added it.

I presume this closes https://github.com/astral-sh/uv/issues/1863 as well.

Supersedes https://github.com/astral-sh/uv/pull/1824

---

_Comment by @zanieb on 2024-03-19 19:06_

I do make use of `uv venv` clearing the environment all the time but it seems more user friendly (and safe) to require `-f`.

---

_Comment by @charliermarsh on 2024-03-19 19:06_

Before I respond to that, I should note that `--force` is different than what you're describing. `--force` does _not_ clear the folder. That was the specific request in the linked issue.

---

_Comment by @zanieb on 2024-03-19 19:10_

Ah sorry for the ambiguous language, I mean "clear the environment" as-in reset the virtual environment to an initial state not "clear the entire folder".

e.g. `uv venv foo` fails if `foo` exists at all and `uv venv foo --force` updates `foo` to be a fresh virtual environment without removing unrelated files. I don't see a need for a way to `rm -rf foo` via a flag.

---

_Comment by @charliermarsh on 2024-03-19 20:12_

That makes sense. I guess I'm not totally sold on changing the current overwrite behavior... I have a hunch that it's a difference in the API, and that leads to initial confusion, but that it's actually not a _bad_ behavior.

---

_Comment by @charliermarsh on 2024-03-19 20:12_

Relatedly, `--clear` would be the comparable `virtualenv` flag for this.

---

_Comment by @T-256 on 2024-03-19 20:31_

> Should we resolve #1472 too?

Fwiw, I also think it's better to resolve #1472 first. Actually I expected to get error on continuous `uv venv`.
Then we can consider `--force-overwrite` and `--force-recreate` flags.

---

_Comment by @charliermarsh on 2024-03-19 20:48_

I just can't think of a time when I've ever run `virtualenv` and didn't want it to reset the environment.

---

_Comment by @zanieb on 2024-03-20 00:11_

I think it would happen when you didn't realize that there was an existing environment... which would be bad if you weren't constructing your environment from lockfiles (which is fairly common for people other than us).


---

_Comment by @gaborbernat on 2024-04-12 20:42_

Any updates on this? 

---

_Comment by @gaborbernat on 2024-04-17 17:46_

@zanieb @charliermarsh did this get lost?

---

_Comment by @charliermarsh on 2024-04-17 18:13_

I think we just lost steam because it started to get entangled with the discussion around whether we should change the `uv venv` behavior for existing virtualenvs in general.

---

_Comment by @zanieb on 2024-04-17 19:31_

And what the flags should be for these various behaviors

---

_Comment by @zanieb on 2024-04-17 19:38_

Concretely, I'd be okay with:

`--force`: Create an environment without checking for unknown files
`--clear` / `--reset`: When creating, ensure site-packages is empty. Implies `--force`. 

I don't see any reason to remove unknown files on `--clear`. That feels like different functionality.



---

_Comment by @charliermarsh on 2024-04-17 19:41_

So would the behavior without any flags change? (Does `--clear` do an `rm -rf .venv`?)

---

_Comment by @zanieb on 2024-04-17 20:00_

I think the existing behavior should change to fail if `.venv` exists without one of these flags and `--clear` should not imply `rm -rf .venv` — that seems like something you should do manually if you want to remove things other than packages?

---

_Comment by @charliermarsh on 2024-04-17 20:01_

I don’t understand what clear does then. And clear would then differ from virtualenv despite having the same name. What does it do? Removes specific folders?

---

_Comment by @zanieb on 2024-04-17 20:07_

> And clear would then differ from virtualenv despite having the same name. 

Hence suggesting `--reset` instead.

Isn't what I wrote concrete?

> When creating, ensure site-packages is empty
> I don't see any reason to remove unknown files on --clear

I expect it to remove all the packages and reset the virtual environment files (i.e. that it would create) not delete every file in the directory.




---

_Comment by @zanieb on 2024-04-17 20:08_

Not tied to this proposal, just putting something concrete forward so we can make progress on it.

---

_Comment by @charliermarsh on 2024-04-17 20:19_

I think I'm just trying to understand the motivation for keeping _some_ of the content in the virtualenv. When would that be needed?

---

_Comment by @zanieb on 2024-04-17 20:39_

I'm not sure, for whatever weird thing is going on in https://github.com/astral-sh/uv/issues/2529? 

Perhaps what I'm thinking is... it feels _hard_ for a user to _just_ clear the packages themselves but very easy for a user to delete the whole directory if they want.

---

_Comment by @T-256 on 2024-04-18 06:33_

> I think the existing behavior should change to fail if `.venv` exists without one of these flags

👍

> I think I'm just trying to understand the motivation for keeping _some_ of the content in the virtualenv. When would that be needed?

It's useful when an external tool adds its specified files to `.venv` directory. when use --clear/--reset it's better avoid using external tool again to create those files/folders. I don't know real world example for such case, but I think this is one of possible scenarios.

---

_Comment by @charliermarsh on 2024-04-18 14:01_

@zanieb - thought I thought #2529 was like: they create a new directory, they add some files, then they want to add a virtualenv to it (rather than: they have an existing virtualenv that they want to clear).

---

_Comment by @gaborbernat on 2024-04-18 14:02_

> @zanieb - thought I thought #2529 was like: they create a new directory, they add some files, then they want to add a virtualenv to it (rather than: they have an existing virtualenv that they want to clear).

Yes correct, this is my use case. 

---

_Comment by @zanieb on 2024-04-18 14:26_

@charliermarsh so yeah once you're in that situation how do you clear the packages from the virtual environment in the folder?

---

_Comment by @charliermarsh on 2024-04-18 14:31_

I don't think they reuse the directory in that way, though. It sounds like they create a new directory, seed it, use it for a virtualenv, then move on. I imagine that they then repeat that entire process when they need a new virtualenv. So, I'm not sure it's worth designing around and supporting that workflow when we don't have a clear motivating use-case.

---

_Comment by @zanieb on 2024-04-22 21:11_

I think about it like this:

I use `uv venv` to clear an environment all the time. It's nice to reset the installed packages in a single command. We should provide a flag to toggle behavior when the directory exists already. I think this behavior should not be opt-out, but instead opt-in — manually managed environments are rarely disposable for real users. I would find it surprising that it performs `rm -rf .venv` and deletes files unrelated to the virtual environment itself. I cannot trivially clear packages from a virtual environment manually, as it can vary per platform. I can, however, trivially delete the folder. I'd like `uv` to take care of the non-trivial part of this workflow. I admit there is still value to performing the trivial part, i.e. it's a single invocation instead of `rm -rf .venv && uv venv`. It seems rare that you would need to clean up other state in the directory though, in which case I think the two command invocation is clearer anyway.

---

_Comment by @charliermarsh on 2024-04-24 01:37_

:shrug: IDK, personally I would find it surprising if `--clear` or `--reset` left some random files in the virtualenv. I think it's likely that we eventually hear from a user that's confused that `--clear` or `--reset` doesn't fully clear or reset the virtual environment. We'll may also see some bug eventually around files that we can't track in our implementation but that "should" be cleared (e.g., what if `virtualenv` or `python -m venv` writes a file to the top-level, and a user runs `--clear `on one of those virtualenvs?). It feels like a hard behavior to explain and implement reliably for a use-case we don't know about.

If we can't agree, then should we at least move forward with `--force` as-described, that allows you to create a virtualenv ignoring any existing files?


---

_Comment by @zanieb on 2024-04-30 13:46_

🤷‍♀️ okay let's just go forward with both `--force` and `--clear`

---

_Comment by @konstin on 2024-04-30 14:19_

I like the current behavior, it encourages treating venvs as ephemeral

---

_Comment by @zanieb on 2024-04-30 14:23_

I feel quite strongly that our current behavior is nice for us but doesn't make sense for the average user. Virtual environments are going to be ephemeral and abstract in our future workflows, we don't need to enforce this idea by making it easy to delete your environment when using a plumbing command.

---

_Comment by @charliermarsh on 2024-05-01 15:59_

Okay, let's proceed with `--force` for now. We can always decide on `--clear` or `--reset` after.

---

_Comment by @codspeed-hq[bot] on 2024-05-01 16:04_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/force)

### Merging #2548 will **not alter performance**

<sub>Comparing <code>charlie/force</code> (8f65128) with <code>main</code> (630d3fd)</sub>



### Summary

`✅ 12` untouched benchmarks






---

_Comment by @codspeed-hq[bot] on 2024-05-01 16:09_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/force)

### Merging #2548 will **not alter performance**

<sub>Comparing <code>charlie/force</code> (da4d103) with <code>main</code> (630d3fd)</sub>



### Summary

`✅ 12` untouched benchmarks






---

_Comment by @codspeed-hq[bot] on 2024-05-01 16:25_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/force)

### Merging #2548 will **not alter performance**

<sub>Comparing <code>charlie/force</code> (540aab1) with <code>main</code> (630d3fd)</sub>



### Summary

`✅ 12` untouched benchmarks






---

_Merged by @charliermarsh on 2024-05-01 16:34_

---

_Closed by @charliermarsh on 2024-05-01 16:34_

---

_Branch deleted on 2024-05-01 16:34_

---
