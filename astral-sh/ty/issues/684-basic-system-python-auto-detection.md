```yaml
number: 684
title: Basic system Python auto-detection
type: issue
state: closed
author: Andrej730
labels:
  - question
assignees: []
created_at: 2025-06-19T18:30:32Z
updated_at: 2025-11-14T15:14:45Z
url: https://github.com/astral-sh/ty/issues/684
synced_at: 2026-01-10T02:06:24Z
```

# Basic system Python auto-detection

---

_Issue opened by @Andrej730 on 2025-06-19 18:30_

### Question

Would be nice to have some simple way to make `ty` detect system Python.

Currently the best way to do it is to set `python` setting to the path like `L:\system\python\path\python.exe`, but it's very system dependent.

I understand that robust system Python detection is on the roadmap, but maybe as a very basic implementation that would work for majority of cases  it would make sense to have something simple like:
-  `use-system-python` setting in `[environment]` that would be `false` by default, but if it's set to `true` it would just try to find python from PATH (as in `which python` / `where python`) and use for `python` setting (if it's unset).
- `system-python-name` which would affect the name `use-system-python` is going to use for the search, defaulting to `python`.

### Version

_No response_

---

_Label `question` added by @Andrej730 on 2025-06-19 18:30_

---

_Comment by @MichaReiser on 2025-06-19 18:38_

What would be the advantage of this detection if it requires explicit opt-in vs specifying the python version? 

It's a thin line to walk how far ty should go but our ultimate goal is to rely on uv instead of implementing anything fancy in ty. That also has the advantage that we can understand script dependencies etc. 

---

_Comment by @AlexWaygood on 2025-06-19 19:13_

I somewhat strongly feel that we shouldn't do this. Not all Python installations appear on PATH:
- Managed installations such as those installed by uv or Hatch do not appear on PATH
- Nor do all Windows installations -- whether it appears on PATH or not depends on a tickbox in the GUI installer if you install your Windows Python installation from python.org. IIRC there are also differences between the python.org installer and Python installed from the Windows store. I think the canonical way you're _meant_ to look for Python installations on Windows is via the [Windows registry](https://peps.python.org/pep-0514/) but [that's controversial](https://github.com/astral-sh/uv/issues/1310#issuecomment-1951464381)

It's a huge can of worms for us to be making decisions about which Pythons from which sources should be prioritised over other Pythons from other sources, and I don't think it's something we should be in the business of doing.

But the only reason why ty needs to know what Python installation you're using is to resolve imports that refer to third-party packages. It's generally not a good idea anyway to install third-party packages into a system installation; you're much better off if you use an isolated virtual environment for each project. If you invoke ty via a project manager such as uv, hatch, poetry or pdm (`uv run --with=ty ty check` is probably the best command if you're a uv user), the project management tool will implicitly activate your project's virtual environment for you before invoking ty. That means that you don't have to specify `--python` on the command line. Manually creating and activating a virtual environment the old-fashioned way should also do the job just as well.

Whether or not users use a project management tool such as uv/hatch/pdm/poetry, most users will use virtual environments (or Conda/pixi environments). And I don't think we should go out of our way to support users who are installing packages into system environments, because that's not something we should encourage. So I would strongly advocate leaving things as they are here.

---

_Comment by @Andrej730 on 2025-06-19 19:32_

> What would be the advantage of this detection if it requires explicit opt-in vs specifying the python version?

An option to use system Python dependencies, specifying Python version would work without any dependencies if there is no venv around.

Personally when I first used `ty` I had an expectation (I guess it comes from using `mypy`, `pyright` or other tools) that running `ty check xxx.py` would use system Python dependencies by default, if it couldn't find venv. But I understand that it's part of general `uv` philosophy to keep things self-contained and detecting system dependencies then makes sense only as opt-in setting. 

I don't have any statistics, so I might be overestimating how many users would be using system Python / use system Python from PATH, but it seems to be the most common way people start using Python and the most common way for running simple one-file scripts.

Is there any "the least controversial way to do it"? So it wouldn't be indeed a can of worms, when adding system Python option would lead to users coming and asking to add support for more and more edge cases.


---

_Comment by @zanieb on 2025-06-19 19:37_

See also, https://github.com/astral-sh/ty/issues/526#issuecomment-2914455599

---

_Comment by @AlexWaygood on 2025-06-19 19:38_

> Is there any "the least controversial way to do it"?

As Micha says, uv has a _lot_ of complicated logic it's developed for figuring out what Python installation the user probably wants it to use for a given command. I'm not excited about duplicating that logic in ty, and I think as soon as we add _some_ form of this feature we'd get requests to add more features along these lines, so it would quickly spiral into a lot of complicated logic that looks similar to what uv has.

We've discussed internally the possibility of uv adding a `uv metadata` command -- that would mean that if a user is using uv, we could detect that, invoke `uv metadata` and figure out what Python installation the user probably wants us to use. But I'm not sure it would really be that useful for this specific problem, since if the user is already using uv, they're likely already using virtual environments, so we wouldn't need the `uv metadata` command to figure out what Python installation we should be using.

---

_Comment by @MichaReiser on 2025-06-19 20:08_

> We've discussed internally the possibility of uv adding a uv metadata command -

I think we can detect whether uv is installed and, if so (and it's a compatible version), use it to find python.

---

_Comment by @AlexWaygood on 2025-06-19 20:16_

I agree that we _can_, but I'm not sure it would be worth the added complexity or maintenance burden, for the reasons I detailed above:
- If they're using uv anyway, they're very likely to be using virtual environments anyway, so we wouldn't need to query uv.
- In the unlikely circumstance that they're using uv to install packages into a system installation, that's not something we want to encourage anyway, so I think it's okay to say that if users _really_ want the loss of per-project isolation this entails then they should have to specify `--python` manually.

---

_Comment by @AlexWaygood on 2025-06-20 10:49_

> Personally when I first used `ty` I had an expectation (I guess it comes from using `mypy`, `pyright` or other tools) that running `ty check xxx.py` would use system Python dependencies by default, if it couldn't find venv.

Mypy has it easy here, because it's written in Python. It just uses whatever Python environment it was executed in, and all the information it needs is easily available from inside that Python environment.

Pyright is a more analogous situation to what we have. If the path to the interpreter isn't specified by the user and no virtual environment is activated, what pyright does is to just execute `python3` in a shell (or `python` on Windows) and then uses that to get the search paths. (This is documented [here](https://github.com/microsoft/pyright/blob/main/docs/import-resolution.md#configuring-your-python-environment), and implemented [here](https://github.com/microsoft/pyright/blob/5ba9dcf9802382b4db529221ba353ae3edc597ad/packages/pyright-internal/src/common/fullAccessHost.ts#L88-L107).) I think that heuristic has worked out pretty well, for them, it's fairly simple, and it has the advantage that it will work for users who aren't using uv. We could consider doing the same.

I wouldn't want to add a `system-python-name` configuration option for it, however, as was suggested in the issue description here. This whole genre of problems is solved if you use virtual environments, either via a project manager or manually. I feel like we should be steering users towards that as the preferred way of invoking ty.

---

_Comment by @carljm on 2025-11-14 15:14_

We now respect the Python environment that ty itself is installed in. I think that's as far as we should go here. I don't think ty should be automatically discovering or using Python installations that are not local to the project being checked and that ty itself is not installed in.

---

_Closed by @carljm on 2025-11-14 15:14_

---
