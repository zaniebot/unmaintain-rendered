```yaml
number: 2150
title: "Add `pip list --outdated`"
type: issue
state: closed
author: pfmoore
labels:
  - enhancement
assignees: []
created_at: 2024-03-04T11:02:47Z
updated_at: 2025-10-23T14:08:46Z
url: https://github.com/astral-sh/uv/issues/2150
synced_at: 2026-01-10T03:23:52Z
```

# Add `pip list --outdated`

---

_Issue opened by @pfmoore on 2024-03-04 11:02_

              `pip list --outdated` is also a useful feature.

_Originally posted by @ismail in https://github.com/astral-sh/uv/issues/1401#issuecomment-1947899132_

Creating a separate issue for visibility, as the issue for creating the `uv pip list` command has now been closed as complete. Apologies if I missed an issue that already requested this - feel free to close this as a duplicate if that's the case.

I use `pip list --outdated` all the time (more than any other form of `pip list`, probably). It would be really useful to have in `uv` as well.

---

_Label `enhancement` added by @konstin on 2024-03-04 11:04_

---

_Comment by @timleslie on 2024-03-18 02:22_

@zanieb @charliermarsh I was wondering if there were any plans for adding this feature in the coming releases? This is one of the last pieces of the puzzle for our full transition to `uv` from `pip`. Any kind of ball-park estimate of if/when would help with our internal planning 🙏

---

_Comment by @zanieb on 2024-03-18 02:26_

We don't have any immediate plans to implement this although it seems like it wouldn't be particularly challenging (we have all the components needed for this).

Can you share more about your use-case?

I'd be happy to review a contribution adding support for this.


---

_Comment by @timleslie on 2024-03-18 02:39_

Thanks for the reply. We have a somewhat complicated dependency management process which precludes us from using tools like renovate or dependabot on our repo. As such we use `pip list --outdated --format=json` together with some custom postprocessing of the output and our `pyproject.toml` to create a list of updatable packages.

From that point of view this is more of a "nice to have" rather than a show-stopper, but for our team this functionality gives us a big quality of life improvement in our DX.

I'm happy to take a swing at the implementation if all the pieces are there 🤞

---

_Comment by @timleslie on 2024-03-18 02:41_

(And while I have you, many thanks for embarking on this project. `pip` install times have been a thorn in our side for an eternity, and `uv` is an absolute game changer for us 🙏)

---

_Comment by @zanieb on 2024-03-18 14:50_

Thanks for all the kind words :) that's an interesting use-case but it makes some sense to me. I imagine it'd make sense to have a future workflow that does not depend on listing the installed packages and can just give you metadata directly from a lockfile... but we can think more about that separately.

Feel free to give it a try. I'm happy to provide any guidance. I'd look at the `pip_install` implementation and the `Plan.reinstalls` field.

---

_Comment by @timleslie on 2024-03-20 01:27_

Sounds good, I'll try to chip away at this over the coming days and see where I get to. So far I've got a build going and added the CLI arg, so I now I just need to fill in:

```rust
    if outdated {
        ...
    }
```

Simple :-) Just getting familiar with the resolver structs and how all the pieces fit together. I'll reach out with any questions and ship a draft PR for feedback when I have something to look at. `pip_install` and `Plan.reinstalls` sound like good pointers 👍

---

_Comment by @timleslie on 2024-03-21 05:41_

@zanieb I've pushed up a WIP branch as #2582 which explores my first attempt. I'd appreciate any feedback on whether this is an appropriate direction, or whether there's an alternate path that might involve less noisy code 🤔

---

_Comment by @gmankab on 2024-04-03 07:12_

i am writing autotests, and one of them is checking for oudated dependencies

i use `pip list --outdated --format=json`

and it is sooo slow

i hope uv will be much faster

waiting for the pull request to be accepted

---

_Comment by @AndydeCleyre on 2024-04-11 21:04_

FWIW I think that the lack of `pip list -o` and `pip list --pre` are the last two items I need to replace all usage of pip and pip-tools with `uv` (when installed) in my project, [zpy](https://github.com/AndydeCleyre/zpy/issues/25). Although if `uv` always acts like `--pre`, that would be fine as well.

---

_Comment by @mralusw on 2024-04-14 04:05_

How can one upgrade all packages in a venv without this command (or `pip freeze --local`, which also isn't supported)? Isn't this a really common task? It's been amazingly hard (but possible), for many years now, to do this with python / pip. Without these two commands in `uv` one has to crawl back to slow `pip`.

---

_Comment by @AndydeCleyre on 2024-04-14 05:33_

@mralusw You can keep track of the packages in requirements+lock files, upgrade those, then sync.

```console
$ uv pip compile -U requirements.in -o requirements.txt
$ uv pip sync requirements.txt
```

Or without any locking or syncing, just listing top level pkgs, this may do: 

```console
$ uv pip install -Ur requirements.in
```

Otherwise you might collect the names from

```console
$ uv pip list --format json
```

then `uv pip install -U ` them.

---

_Comment by @T-256 on 2024-04-19 14:47_

Just an opinion at here:
use it as separated command, like `pip update`, which takes `requirements.txt` (compiled versions `{pkg}=={version}`) as input and output is updated list.
for current venv and its installed packages in it we can then use `uv pip freeze | uv pip update`.

Also, fwiw I opened a enhancement request for `uv pip freeze` in https://github.com/astral-sh/uv/issues/3141

---

_Comment by @gmankab on 2024-04-27 16:16_

hello, is there any progress with implementing this?

---

_Comment by @mralusw on 2024-05-13 15:33_

While there are work-arounds, note that `pip` can be used not only from the command line, but also from scripts and higher-order software. The update-check is conceptually different from the update-install step.

For example, when writing a package manager, it's a natural workflow to
1. check for available updates
2. notify the user
3. allow them to select an action, and
4.  separately perform the update.

Only the last step might need admin privileges, and only the last two need an interactive environment.

Also, it's a waste to have a resolution mechanism and not expose it (especially when there's a clear API, already exposed by an equivalent tool &mdash; as in this case).

---

_Comment by @NeilGirdhar on 2024-05-13 15:44_

@zanieb 
> Can you share more about your use-case?

I think this is a really good question and illustrates your team's desire to provide good interfaces.

For me, I use this in a few places:

* As a faster version of `poetry update --dry-run`.  Of course, if the Poetry command were faster, then that would be preferable since it would give more accurate results.
* After `poetry update` to find out if anything wasn't upgraded, and then I do `poetry show -t --ansi | less`  to find out why certain things weren't updated.
* In my system-wide Python to find out if anything needs updating.  For me, I would be okay with my system-wide Python being managed in the same way as a Poetry project—if that were possible.

Thus, I don't really need `uv pip list --outdated`.  What I need are fast versions of:
* `uv poetry update --dry-run`,
* `uv poetry show -o`, and
* some way to manage my system Python libraries as if they were in a Poetry project.

---

_Comment by @mralusw on 2024-05-13 19:14_

Just curious, is there a technical reason why the results of the package resolution process might be difficult to expose?

I'm writing a package manager that sits on top of `pip` and other package sources. I'm trying to avoid fancy tools (see for instance [Why not tell people to "simply" use pyenv, poetry, pipx or anaconda](https://www.bitecode.dev/p/why-not-tell-people-to-simply-use) and the related series for why sticking to upstream python packaging tools might avoid a host of problems). `uv pip` is not of course vanilla `pip`, but it's close, doesn't require custom workflows, and the speed is compelling.

---

_Comment by @zanieb on 2024-05-13 19:46_

I don't think it's hard to expose the package resolution results per-say, it's just a good bit of work and we're not prioritizing adding output formats right now. See https://github.com/astral-sh/uv/issues/3199 which is a bit more focused on the goal of machine readable output.

---

_Comment by @ziddey on 2024-08-28 01:55_

In case it helps someone, this is how I've been simulating `pip list --outdated`:
```
bash -c "uv pip list --format=freeze |sed 's/==.*//' | uv pip compile - --no-deps --no-header |diff <(uv pip list --format=freeze) - -y --suppress-common-lines || :"
```
Obviously not in the same format, but it serves to show held back packages.

Performance is simply dumbfounding on a raspberry pi 2w:
```
$ time pip list --outdated
Package    Version Latest Type
---------- ------- ------ -----
jsonschema 2.6.0   4.23.0 wheel

real    0m42.194s
user    0m28.463s
sys     0m0.996s

$ time bash -c "uv pip list --format=freeze |sed 's/==.*//' | uv pip compile - --no-deps --no-header |diff <(uv pip list --format=freeze) - -y --suppress-common-lines || :"
Resolved 47 packages in 871ms
jsonschema==2.6.0                                             | jsonschema==4.23.0

real    0m1.002s
user    0m0.348s
sys     0m0.463s
```

---

_Comment by @gh640 on 2024-08-29 00:45_

I also encountered this issue, so for the time being, I decided to run `uv venv --seed` to install pip in my venv and then run `uv run pip list --outdated`.

I hope this will be helpful to someone.

---

_Comment by @charliermarsh on 2024-08-30 14:58_

Bun has this now too, it's nice!

![Screenshot 2024-08-30 at 10 57 57 AM](https://github.com/user-attachments/assets/ce89a628-0973-418e-bad4-4e01d16aa74c)


---

_Comment by @dwt on 2024-09-03 12:22_

Another use case: I use `pip list --outdated` regularly when coming back to a project after some time, to quickly see what work I have ahead of me.

---

_Comment by @golgor on 2024-09-06 08:33_

@zanieb 
The use-case we have is keeping our IoT Pipeline up to date and to avoid to large required changes or tech backlog. We have a fairly large application for this with quite a lot of packages and it is simply not feasible to check everything manually.

For example, every time I work with a PR, I can just run one command to check the status of all the used packages. If I see that quite a few packages that could be updated, I would start looking into this. I do it like this using Poetry and it works very good with keeping things up to date.

The alternative would be to leave this to random chance (check when you remember to do it) or set up something like a quartely calendar event to check it.

It would also be a neater way to automate this with for example a Github Action (which is possible with the script from @ziddey, but would be better with a an official solution).

This is the one feature holding me back from migrating to UV for all our packages.

---

_Comment by @NeilGirdhar on 2024-09-06 13:04_

> Another use case: I use pip list --outdated regularly when coming back to a project after some time, to quickly see what work I have ahead of me.

Wouldn't this be better as `uv tree --outdated` (analogous to `poetry show -o`, but with a nice tree view)?   After all, it doesn't matter which dependencies have updates, but rather which dependencies _can be_ updated.

---

_Comment by @AndydeCleyre on 2024-09-06 15:12_

I don't know about a tree output, which may be nice, but FWIW regarding output format: I currently wrap `pip --disable-pip-version-check list -o --format json` with a function that presents a table for me of outdated packages across an arbitrary number of projects (venvs).

---

_Comment by @NeilGirdhar on 2024-09-06 18:26_

My point wasn't that the output format.  My point was that the use case suggested above should be showing the packages that can be updated given the lock rather than simply the outdated packages.

---

_Comment by @dwt on 2024-09-07 12:20_

This seems sensible - coming from pip, `pip list --outdated` already shows all available updates, not just the direct dependencies. Is there any reason why `uv pip list --outdated` would only show outdated direct dependencies? Or do you just mean that the format would be nicer from `uv tree --outdated`?

---

_Comment by @NeilGirdhar on 2024-09-07 20:22_

> This seems sensible - coming from pip, pip list --outdated already shows all available updates, not just the direct dependencies. 

Sorry, I think we're talking past each other.  As I said in my last comment, this has nothing to do with format.  (It also has nothing to do with direct dependencies.)  My point was that the use case suggested above should be showing _the packages that can be updated given the [project file]_ rather than _simply the outdated packages_.

Consider, for example that you have dependencies:
```
dependencies = [
    'jaxlib >=0.4.14, <=0.4.31',
    'numpy <= 2.1.0'
    ...
```
And you have jaxlib==0.4.31 and numpy==2.0.0 installed.  Suppose that jaxlib 0.5.0 and numpy 2.1.0 are available. What should be displayed?

pip list --outdated naively says: jaxlib 0.5.0 and numpy 2.1.0

That's because it doesn't see your pyproject.toml file.  I think it's more useful (considering the use cases mentioned in this thread, which are project use cases) to show only that numpy is outdated.  This is because `uv lock -U` will not update jaxlib, and it will be both useless and confusing (in my opinion)  to keep saying that it is outdated and refuse to update it.

This is what `poetry show -o` currently does.

Of course, when it comes to packaging there are more use cases than I can imagine.  Perhaps other people are more interested in the pip-like behavior.  I'm not sure.

---

_Comment by @dimbleby on 2024-09-07 20:37_

> This is what poetry show -o currently does.

no it isn't. poetry shows all packages for which newer versions are available even if `poetry lock` would not use those newer versions.

fwiw I find this behaviour more useful than telling me which packages it would upgrade if I re-locked.  (I can `update --dry-run` for that: or just update and revert if I don't like the answer).  

whereas telling me that I have an upper bound that I might want to relax is something I want to know.

---

_Comment by @NeilGirdhar on 2024-09-07 21:10_

> fwiw I find this behaviour more useful than telling me which packages it would upgrade if I re-locked. (I can `update --dry-run` for

Ah, my mistake.  For some reason I thought it was doing the dry run essentially.

> whereas telling me that I have an upper bound that I might want to relax is something I want to know.

For me, I get a lot of noise from downstream package limits that I can't relax holding things back.  That's why I'd rather just see what I can control.  Maybe it would be nice to show at least two sections:
* things that would be upgraded given `uv lock -U`, and
* things that could be upgraded if constraints in the project file were relaxed.

This would still hide some outdated packages if they are blocked by dependencies within dependencies.  Maybe those should be shown in a third section so that you can ping their maintainers.  WDYT?

---

_Comment by @dwt on 2024-09-09 05:58_

Sounds sensible, those are three use cases. The last one would be the least useful to me, but the first two I use very regularly.

---

_Comment by @ericbn on 2024-09-09 20:46_

Looks like there are different features being discussed here. I propose we use this thread to only discuss what seems to be the original feature, which is what `pip list --outdated` does and also what the script posted [above](https://github.com/astral-sh/uv/issues/2150#issuecomment-2313934453) by @ziddey does, which is: to list outdated packages **based on which packages are installed on the current virtual environment (or in the system)**. This ignores any lock files or pyproject.toml files, as it only looks at which packages are currently installed.

---

_Comment by @pep-sanwer on 2024-09-27 19:55_

(Apologies for continuing this thread for the not directly `pip --outdated` use case)

FYI - not great but functional work around:
Just ported a project over to `uv` from `poetry` and seeing a list of upgradeable packages (both uses cases specced by https://github.com/astral-sh/uv/issues/2150#issuecomment-2336453465) was a major missing functionality in the switch

Current work around (not ideal):
- Setup `pyproject.toml` via standard `uv` approach with deps pinned as strictly as you would like, make sure at least one `uv` install command has been run, like `uv sync`
- For deps that would be upgraded given `uv lock -U`:
  - Setup (and maintain) a shadow `poetry.pinned.pyproject.toml` via the minimal required `poetry` standard with the same deps and pins
- For deps that could be upgraded if constraints in the project file were relaxed.:
  - Setup (and maintain) a shadow `poetry.nopin.pyproject.toml` via the minimal required `poetry` standard with the same deps, only do not pin dep versions (or remove upper bounds)
- Whenever you'd like to check upgradeable deps (either case) - swap the actual `pyproject.toml` with the use case associated `poetry.*.pyproject.toml` and run `poetry update --dry-run` - no `venv` activation needed (both tools default to use `/path/to/project/.venv`)
- `poetry show -o` works as well after a `poetry lock` (requires a `poetry.lock`)
  - Trivial to wrap in a script

I'm not happy with this (I'm sure there are many complexities being missed here), but it works-ish

Goes with saying - thank you to the Astral team for all the incredible work y'all are doing in this space - huge strides!



---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-28 23:28_

---

_Comment by @RatulMaharaj on 2024-11-01 22:40_

I would love for this to be a thing. I mostly use `pnpm` for my node.js projects and to me `uv` feels super familiar because of that.

In that ecosystem, my favourite and most commonly used commands are `pnpm add`, `pnpm remove` and `pnpm outdated`. We already have `uv add` and `uv remove` - I think being able to run `uv outdated` would be awesome too!

---

_Closed by @charliermarsh on 2024-11-07 02:32_

---

_Closed by @charliermarsh on 2024-11-07 02:32_

---

_Comment by @rafalkrupinski on 2025-02-26 16:39_

Not sure what the command is supposed to do but with this in project.dependencies:

"boltons>=24.1.0",

it doesn't show that boltons is outdated (25.0.0 available)

---

_Comment by @AndydeCleyre on 2025-02-26 17:11_

AFAIU, like pip, it's reporting on the actually installed version vs the newest release, disregarding stated requirements.

---

_Comment by @zanieb on 2025-02-26 17:14_

Yeah this is a `pip` command which is not project-aware, it's reading the environment

```
❯ uv venv
Using CPython 3.13.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install boltons==24.1.0
Resolved 1 package in 128ms
Prepared 1 package in 65ms
Installed 1 package in 3ms
 + boltons==24.1.0
❯ uv pip list --outdated
Package Version Latest Type
------- ------- ------ -----
boltons 24.1.0  25.0.0 wheel
```

---

_Comment by @htran-ubed on 2025-04-14 09:31_

Could this be added to the documentation of managing packages or maybe somewhere else? As I could not find it anywhere in the documentation.

---

_Comment by @zanieb on 2025-04-15 20:26_

@htran-ubed we tend not to spend too much time on documenting `pip` command since it's just a parity interface. Feel free to open a pull request adding it to the documentation though.

---

_Comment by @AndydeCleyre on 2025-04-15 22:41_

For anyone documenting, note that it's not an _exact_ parity interface, e.g.:

```console
$ pip list --outdated --format columns --exclude-editable --format json --include-editable     # valid
$ uv pip list --outdated --format columns --exclude-editable --format json --include-editable  # invalid for two reasons
```

---

_Comment by @avegao on 2025-05-27 10:43_

Im using

```shell
uv tree --outdated --depth 1
````


---

_Comment by @tyilo on 2025-10-23 14:08_

> Im using
> 
> uv tree --outdated --depth 1

This doesn't seem to work anymore in uv 0.9.4. It just lists all dependencies.

`uv pip list --outdated` works instead.

---
