```yaml
number: 611
title: "Use `.venv` over `base` conda environment"
type: issue
state: closed
author: Danielkonge
labels:
  - imports
assignees: []
created_at: 2025-06-08T16:28:15Z
updated_at: 2025-08-20T13:07:44Z
url: https://github.com/astral-sh/ty/issues/611
synced_at: 2026-01-12T15:54:23Z
```

# Use `.venv` over `base` conda environment

---

_@Danielkonge_

### Summary

In some cases, one can have a `.venv` without activating it (e.g. a `.venv` created by bazel), but it will currently not be found with a standard setup, where conda is installed. 

This is because even without activating a conda environment, you will still usually have ~ ~`CONDA_DEFAULT_ENV=base`~ `CONDA_PREFIX=/opt/anaconda3`, so ty will use site-packages from your base conda environment and not look in the `.venv`. 

I think ty should generally prefer the `.venv` over the base conda environment here. It is less clear if you would prefer the `.venv` if any non-standard environment is activated, but at least for the base environment I think that the current behavior is unexpected.

### Version

ty 0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Comment by @AlexWaygood on 2025-06-08 17:33_

Heh, we discussed exactly this situation in https://github.com/astral-sh/ruff/pull/18267#discussion_r2104676295, and decided that if the `VIRTUAL_ENV` environment variable had not been set (indicating that the local virtual environment had not been explicitly activated) but the `CONDA_PREFIX` environment variable _was_ set (indicating that the conda environment _had_ been explicitly activated), then the explicitly activated environment should take priority over any implicit virtual environment discovery.

I've never used bazel — can I ask why you're not able to explicitly activate your local virtual environment? If that's totally out of the question, would setting the `VIRTUAL_ENV` environment variable manually, or using `--python=.venv`, work for you?

(The `--python` argument can also be specified in a config file: `tool.ty.environment.python` in a pyproject.toml file, or just `environment.python` in a ty.toml file.)

---

_Label `imports` added by @AlexWaygood on 2025-06-08 17:33_

---

_Comment by @AlexWaygood on 2025-06-08 17:34_

I don't think we look at the `CONDA_DEFAULT_ENV` environment variable at all at the moment BTW — we just look at the `CONDA_PREFIX` environment variable

---

_Comment by @Danielkonge on 2025-06-08 18:13_

> I don't think we look at the CONDA_DEFAULT_ENV environment variable at all at the moment BTW — we just look at the CONDA_PREFIX environment variable

Ah, you are probably right, the default prefix is just `CONDA_PREFIX=/opt/anaconda3`, so that is probably where you get the path from.

> I've never used bazel — can I ask why you're not able to explicitly activate your local virtual environment? If that's totally out of the question, would setting the VIRTUAL_ENV environment variable manually, or using --python=.venv, work for you?

It is definitely possible to make an activate script, but I don't think that is the common way to do it in bazel (though I could be wrong here). Usually I would just point to the correct path and that should be fine, but I couldn't figure out how to give the path correctly for `ty server`. (Though to be fair, I just quickly tried it.) For now I solved it by just deactivating the default conda, but it would be nice to have a better solution.

Correct me if I'm wrong here, but is `--python=.venv` just for `ty check`? I will try to specify a `ty.toml` file instead for now (I do something similar with ruff, where the `ruffconfig.toml` works fine).

---

_Comment by @AlexWaygood on 2025-06-08 22:39_

> Correct me if I'm wrong here, but is `--python=.venv` just for `ty check`?

Yup, you'd do something like `ty check --python=.venv` if you wanted to specify the location of your Python installation from the CLI

---

_Comment by @Danielkonge on 2025-06-08 22:43_

> Yup, you'd do something like ty check --python=.venv if you wanted to specify the location of your Python installation from the CLI

Is there a reason for not allowing `ty server --python=.venv`? (Not that this is a big problem, since you can give the server a config file.)

---

_Comment by @AlexWaygood on 2025-06-08 23:48_

> Is there a reason for not allowing `ty server --python=.venv`?

hmm, that might be reasonable... @MichaReiser, WDYT?

---

_Comment by @MichaReiser on 2025-06-09 05:17_

The LSP uses a different model. The client sends the settings during the initialize request or the server can explicitly request them from the client. This is due to most users never seeing the CLI when using the server, e.g. because VS code starts the server for them. This also enables workspace or user wide LSP settings that you wouldn't get with CLI arguments.

We do plan to implement client settings support for ty and the python option seems like an important one, we just haven't gotten to it yet


---

_Comment by @dhruvmanila on 2025-06-09 05:20_

Related: https://github.com/astral-sh/ty-vscode/issues/26, https://github.com/astral-sh/ty/issues/82

---

_Comment by @zanieb on 2025-06-09 13:49_

> Heh, we discussed exactly this situation in https://github.com/astral-sh/ruff/pull/18267#discussion_r2104676295, and decided that if the VIRTUAL_ENV environment variable had not been set (indicating that the local virtual environment had not been explicitly activated) but the CONDA_PREFIX environment variable was set (indicating that the conda environment had been explicitly activated), then the explicitly activated environment should take priority over any implicit virtual environment discovery.

I think this is the wrong behavior and does not match uv. `CONDA_PREFIX` can be set to the base environment without it being explicitly activated. In uv, it was originally implemented as done here, but we needed to change it in a subsequent version.

See https://github.com/astral-sh/uv/pull/7691 (most of the discussion is in https://github.com/astral-sh/uv/issues/7124)

---

_Comment by @zanieb on 2025-06-09 13:52_

(If `CONDA_PREFIX` is set to something other than the base environment, I agree it should take precedence over any implicit discovery)

---

_Comment by @AlexWaygood on 2025-06-09 13:54_

Ah, https://github.com/astral-sh/uv/pull/7691/files is a great reference, thanks. I wasn't aware that this was how conda base environments worked. In that case, I definitely agree we should change our behaviour to match uv's here.

---

_Comment by @griffinbaker12 on 2025-07-26 21:29_

Hi everyone,

Just wanted to add some information to this issue as I'm running into a related problem when using `ty` with Conda environments.

Following the conversation here and in the linked issues, I can confirm that the current environment detection causes a server panic. When I have a Conda environment activated, the `$VIRTUAL_ENV` variable it sets causes the `ty` LSP server to crash on startup.

Here is the key error from my `lsp.log`:

```
ERROR panicked at crates/ty_server/src/session.rs:391:30:
Default configuration to be valid: Failed to discover local Python environment

Caused by:
   0: Invalid `VIRTUAL_ENV` environment variable `/path/to/my/conda/envs/my-env`: points to a broken venv with no pyvenv.cfg file
   1: No such file or directory (os error 2)
```

I was able to work around this by modifying my LSP client's startup command to `sh -c 'unset VIRTUAL_ENV && ty server'`, which seems to confirm that the server is misinterpreting the Conda environment.

I see the plan is to align the behavior with `uv`. If this isn't already being worked on, I'd be excited to try tackling it as my first contribution. Please let me know if that's something you'd be open to.

Thanks for all your work on this great tool!

---

_Comment by @MichaReiser on 2025-07-28 06:59_

Thanks @griffinbaker12 for reporting. This is the same underlying issue as https://github.com/astral-sh/ty/issues/859

---

_Comment by @zanieb on 2025-07-28 13:05_

I'm happy to review a change to make the logic match uv.

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:07_

---

_Assigned to @Gankra by @carljm on 2025-08-15 15:05_

---

_Closed by @Gankra on 2025-08-20 13:07_

---
