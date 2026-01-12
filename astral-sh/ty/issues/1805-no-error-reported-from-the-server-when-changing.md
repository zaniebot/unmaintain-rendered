```yaml
number: 1805
title: No error reported from the server when changing ty configuration from a valid setting to an invalid setting
type: issue
state: open
author: AlexWaygood
labels:
  - configuration
  - server
assignees: []
created_at: 2025-12-08T12:45:15Z
updated_at: 2025-12-08T13:23:41Z
url: https://github.com/astral-sh/ty/issues/1805
synced_at: 2026-01-12T15:54:25Z
```

# No error reported from the server when changing ty configuration from a valid setting to an invalid setting

---

_@AlexWaygood_

### Summary

If you change ty's configuration from a valid setting to an invalid setting while the server is running, no diagnostic or error message is reported by the server to tell you that your configuration settings are now invalid. In the following video I've opened the `./python/py-fuzzer` directory in VSCode as its own workspace, and I'm trying to change the `environment.python` setting so that ty will pick up the installed dependencies in my virtual environment. I accidentally configure it to `python = "./venv"` instead of `python = "./.venv"`, but ty doesn't report a diagnostic informing me that this configuration is invalid; it just silently continues to not resolve any of my dependencies. A fresh run of ty from the command line with this broken configuration, meanwhile, immediately reports the error:

https://github.com/user-attachments/assets/2960fda2-32d1-4795-9c4b-5ca672b018c4

If I quit and reopen VSCode, then this message appears:

<img width="1220" height="404" alt="Image" src="https://github.com/user-attachments/assets/62dfc591-1939-44a8-a0fb-5529ebcba838" />

But there's nothing to indicate that I need to do that if I edit the configuration while the server is running

---

_Label `configuration` added by @AlexWaygood on 2025-12-08 12:45_

---

_Label `server` added by @AlexWaygood on 2025-12-08 12:45_

---

_Comment by @AlexWaygood on 2025-12-08 12:51_

(I'm not totally sure why I needed to set `python = "./.venv"` in the first place -- shouldn't that be automatically picked up? Maybe VSCode has selected a different Python environment for this project, overriding our default virtual environment discovery -- but I'm not sure which one it's selected, or how to change that?)

---

_Comment by @MichaReiser on 2025-12-08 13:01_

Related to https://github.com/astral-sh/ty/issues/542. This only applies to `ProgramSetting`, see 

https://github.com/astral-sh/ruff/blob/a364195335eb920ff429c43832c4ee464051ea9a/crates/ty_project/src/db/changes.rs#L284-L292

where we handle the error case by converting it into an option diagnostic

but we don't do the same for `ProgramSettings` because the returned error is an `anyhow::Erorr`

https://github.com/astral-sh/ruff/blob/a364195335eb920ff429c43832c4ee464051ea9a/crates/ty_project/src/db/changes.rs#L264-L272

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-08 13:01_

---

_Comment by @MichaReiser on 2025-12-08 13:23_

Hmm, I tried a naive fix of including the error in `ChangeResult`, but the fact that we render "pretty errors" for some of the site package errors now works against us, because it results in an unreadable error message in VS Code

<img width="1657" height="619" alt="Image" src="https://github.com/user-attachments/assets/05f6564a-21ac-46ec-8669-0be47fc57a19" />

I guess the solution here is really to change `to_program_settings` to return a type that can be converted to a `Diagnostic`. 

---
