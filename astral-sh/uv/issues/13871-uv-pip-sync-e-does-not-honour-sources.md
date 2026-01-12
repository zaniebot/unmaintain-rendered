```yaml
number: 13871
title: "`uv pip sync - <<<\"-e .\"` does not honour `sources`"
type: issue
state: open
author: paveldikov
labels:
  - bug
assignees: []
created_at: 2025-06-05T19:20:38Z
updated_at: 2025-06-06T15:35:56Z
url: https://github.com/astral-sh/uv/issues/13871
synced_at: 2026-01-12T16:01:38Z
```

# `uv pip sync - <<<"-e ."` does not honour `sources`

---

_@paveldikov_

### Summary

I am wanting to refactor a bunch of my
```
uv pip install -e . -c constraints.txt
```
usages into `uv pip sync` usages -- for a modest performance improvement, but also the ability to easily remove superfluous junk from manual `uv pip install` usage.

In my quest, I tried:
```
uv pip sync constraints.txt - <<<"-e ."
```
and it works great -- except in cases where I use `sources` to define a dependency on a local package. In those cases, the installation simply fails because it cannot find any distribution for said local package (it's local -- not published to any index.)

I.e. for some reason, `uv pip install` is `sources`-aware, and `uv pip sync` is not.

(wondering: is this deliberate?)

### Platform

Linux

### Version

uv 0.7.11

### Python version

_No response_

---

_Label `bug` added by @paveldikov on 2025-06-05 19:20_

---

_Comment by @charliermarsh on 2025-06-05 19:21_

Not totally sure what's going on, will look into this, thanks @paveldikov.

---

_Comment by @charliermarsh on 2025-06-06 00:16_

When you use `uv pip sync`, we don't look at any dependencies, and sources are applied to a package's dependencies. So here, we get the requirement on `.`, then any requirements defined in `constraints.txt`, but we never pick up the sources in `.` because they'd only be applied when iterating over its dependencies.

I'm unsure if this should change. I agree it's a little confusing.


---

_Comment by @paveldikov on 2025-06-06 12:05_

>When you use uv pip sync, we don't look at any dependencies, and sources are applied to a package's dependencies.

So the inexplicable thing is that -- yes, it does not _install_ dependencies -- but it is clearly _evaluating_ them at some point in the crit 
path, because otherwise it wouldn't have failed?

That aside... I *could* have enumerated the editable local dependency in the constraints file -- arguably should -- but then it's no longer valid as a constraints file... gah! `pylock.toml` is better at this, but not fully usable at this point in time.

Maybe what I'm actually looking for is `uv pip install ... --clean` so that I can lean on that to clear miscellaneous cruft from the venv...?

---

_Comment by @charliermarsh on 2025-06-06 12:07_

Wait, so above, `constraints.txt` doesn't include a reference to the failing path?

---

_Comment by @paveldikov on 2025-06-06 12:11_

No, because it's not valid for a constraints file to list an editable dependency (it seems) -- I tried:
```
-e foo
foo @ -e foo
```
Either is discarded at input validation stage.

What does work (end to end!), but kills the editability:
```
foo @ foo
```

---

_Comment by @charliermarsh on 2025-06-06 12:13_

(As an aside, can you remind me which features of `pylock.toml` are missing for you?)

---

_Comment by @paveldikov on 2025-06-06 12:20_

>(As an aside, can you remind me which features of pylock.toml are missing for you?)

Either of:
* Ability to use a lockfile as a constraints file -- this lets me be very lazy BUT also requires that `pip` support this exact same flow, and that is not to be taken for granted
    * (background: we use `pip` to bootstrap `uv`, and ideally it should be the right version of `uv` i.e. the one in the lockfile)
* Ability to preserve dependency groups in `uv pip compile -o pylock.toml`
    * also I'd need to be able to feed an additional dependency group inline (`- <<<"foo"`) -- but I imagine that's just a case of attaching a marker to `foo`? `foo ; 'my_grp' in dependency_groups`? and then I need a way of getting `my_grp` listed in the top-level listing of available groups... gah!

---

_Comment by @notatallshaw on 2025-06-06 14:23_

FWIW, I use basically the same workflow as @paveldikov and have similiar desires out of a lock file.

And I pass in the local path to the sync command, I don't use sources, my pip sync command looks like:

```bash
echo -e $EDITABLE_PACKAGES | cat - "$CONSTRAINTS_PATH" | uv pip sync -
```

Where `$EDITABLE_PACKAGES`  looks like:

```
EDITABLE_PACKAGES="\n-e ${_PROJECT_ROOT}/sub/dir/to/package\n-e ${_PROJECT_ROOT}/sub/dir/to/other/package\n"
```

---

_Comment by @paveldikov on 2025-06-06 15:35_

Yeah. I may have somewhat painted myself in a corner by trying to use a constraints file as a legacy-friendly approximation of a proper lockfile. It works fine for the most part, but the stipulation against editable installs (fwiw, that's baked into the spec @ PyPA) is pretty limiting.

I could, however, see myself just dropping constraints files cold-turkey in favour of PEP 751/`pylock.toml`. This does, as mentioned, require that multi-use lockfiles are properly supported -- and to be honest this needs to happen anyway, so I'd be equally happy with WONTFIX'ing this scenario if `pylock.toml` is to handle it more structurally.

---
