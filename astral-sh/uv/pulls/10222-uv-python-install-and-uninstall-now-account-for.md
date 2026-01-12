```yaml
number: 10222
title: uv python install and uninstall now account for the UV_PYTHON env var
type: pull_request
state: closed
author: Choudhry18
labels:
  - breaking
assignees: []
base: tracking/060
head: env_var_python_install
created_at: 2024-12-29T23:55:11Z
updated_at: 2025-02-13T22:17:53Z
url: https://github.com/astral-sh/uv/pull/10222
synced_at: 2026-01-12T16:09:11Z
```

# uv python install and uninstall now account for the UV_PYTHON env var

---

_@Choudhry18_

## Summary

Checks UV_PYTHON for version when `uv python install` or `uv python uninstall ` ran

## Test Plan

I added new snapshot tests: 
 uv/tests/it/python_install::python_install_with_uv_python_env that sets UV_PYTHON to and tests if that version is installed and not the latest. UV_PYTHON=3.12 uv python install would install 3.12 and not the latest version

 uv/tests/it/python_install::python_install_with_uv_python_env_and_arg that sets the env variable and the target and tests if the target takes precedence. UV_PYTHON=3.12 uv python install 3.8 would install python 3.8

## Requirement Concern

The order of precedence on the python install command is : target specified through cli -> environment var UV_PYTHON -> .python-versions if none other specified. 

The order of precedence on the python uninstall command is: target specified through cli (required unless UV_PYTHON set) -> environment var UV_PYTHON (required unless target provided in cli). As the --all arg conflicts with target it also conflicts UV_PYTHON env variable

closes #10128 

---

_Assigned to @zanieb by @zanieb on 2024-12-30 02:36_

---

_@samypr100 reviewed on 2025-01-01 01:38_

---

_Review comment by @samypr100 on `crates/uv/src/settings.rs`:802 on 2025-01-01 01:38_

```suggestion
    match std::env::var(EnvVars::UV_PYTHON) {
```

---

_@samypr100 reviewed on 2025-01-01 02:04_

---

_Review comment by @samypr100 on `crates/uv/tests/it/python_install.rs`:887 on 2025-01-01 02:04_

Might want to move this to the contexts using .env() instead

---

_@Choudhry18 reviewed on 2025-01-02 20:05_

---

_Review comment by @Choudhry18 on `crates/uv/tests/it/python_install.rs`:887 on 2025-01-02 20:05_

Thank you for the review changed to using .env in tests now

---

_Review requested from @samypr100 by @Choudhry18 on 2025-01-03 10:04_

---

_Label `breaking` added by @zanieb on 2025-01-23 23:16_

---

_Added to milestone `v0.6.0` by @zanieb on 2025-01-23 23:16_

---

_Comment by @zanieb on 2025-01-23 23:25_

Hey @Choudhry18 I fixed some merge conflicts and expanded the test cases.

I have one comment; otherwise this looks good to me!

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1060 on 2025-01-23 23:25_

This test failure is problematic — this needs to work still.

---

_@zanieb reviewed on 2025-01-23 23:25_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1060 on 2025-01-23 23:26_

I see you mentioned this at

>  As the --all arg conflicts with target it also conflicts UV_PYTHON env variable

We might need to implement this differently to work around this. Perhaps `--all` should just override the targets?

---

_@zanieb reviewed on 2025-01-23 23:26_

---

_@Choudhry18 reviewed on 2025-01-28 04:39_

---

_Review comment by @Choudhry18 on `crates/uv/tests/it/python_install.rs`:1060 on 2025-01-28 04:39_

I do agree that `-all` should override targets as I don't see a case where a user would add -all argument without intending to delete all the version. So `all` should override targets set through argument or environment. Do we need to give the user a warning (I personally don't think it's needed) like:

> Setting targets with -all is redundant and just result in all managed python versions being uninstalled

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1060 on 2025-01-28 06:00_

Yeah I don't think a warning is needed, it's a weird case but it seems fine to just ignore the target silently unless someone reports a confusing interaction.

---

_@zanieb reviewed on 2025-01-28 06:00_

---

_@zanieb reviewed on 2025-01-28 06:01_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1060 on 2025-01-28 06:01_

Ah are you suggesting that we should remove the conflict entirely and let someone do `uv python uninstall foo --all`? I think that could be too confusing :/

---

_@Choudhry18 reviewed on 2025-01-28 07:44_

---

_Review comment by @Choudhry18 on `crates/uv/tests/it/python_install.rs`:1060 on 2025-01-28 07:44_

yes that is what I had in mind that if `--all` is set target shouldn't matter . I do agree invalid targets with `--all` not giving an error can be a bit confusing. Currently `uv python uninstall foo -all` doesn't really tell the user that their target is invalid it just tells them of the conflict of target with `--all`

So what I understand is you suggest that `--all` should override target but if it target is invalid it should throw an error

---

_@zanieb reviewed on 2025-01-30 01:02_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1060 on 2025-01-30 01:02_

Oh sorry I mean `uv python uninstall 3.11 --all` would be confusing too — just used `foo` as a silly example.

---

_@Choudhry18 reviewed on 2025-01-30 07:10_

---

_Review comment by @Choudhry18 on `crates/uv/tests/it/python_install.rs`:1060 on 2025-01-30 07:10_

oh so you suggest the target set as an argument should behave differently in case of use with `--all` (throw an error citing a conflict) vs target set through the env variable should be ignored silently. Apologies that took me a minute to understand :/

That makes sense for usage here the only concern I have is are there any other env variable that behave like this in uv. Most of the env variable I have seen they are sort of a replacement for just having to type to argument again and again and behave exactly the same as the cli argument. So this behaving differently might be not be very idiomatic. I also might just be overthinking this one.  


---

_@zanieb reviewed on 2025-01-31 20:42_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1060 on 2025-01-31 20:42_

> oh so you suggest the target set as an argument should behave differently in case of use with --all (throw an error citing a conflict) vs target set through the env variable should be ignored silently. Apologies that took me a minute to understand :/

Yeah, that's the suggestion. No problem on the confusion.

> Most of the env variable I have seen they are sort of a replacement for just having to type to argument again and again and behave exactly the same as the cli argument. So this behaving differently might be not be very idiomatic. I also might just be overthinking this one.

I think it's okay in this case. Though I'm not sure of the implications implementation-wise.

---

_@Choudhry18 reviewed on 2025-02-06 07:23_

---

_Review comment by @Choudhry18 on `crates/uv/tests/it/python_install.rs`:1060 on 2025-02-06 07:23_

I couldn't find a suitable way to implement  it using clap's derive API. My workaround was writing a custom validation check that is ran after the parsing and before the uninstall Settings are resolved. The implementation does get a bit ugly in order to replicate the format of the error message without using the derive API  

---

_Review requested from @zanieb by @zanieb on 2025-02-08 02:38_

---

_Comment by @zanieb on 2025-02-13 18:36_

I found the early-validation a bit awkward (as you noted) and moved it into the implementation.

I'm finding myself a little hesitant about changing the `uninstall` behavior at all now? I feel like this change would definitely be simpler if we just handled `UV_PYTHON` on `install` and left uninstall explicit?

---

_Comment by @zanieb on 2025-02-13 18:51_

Exploring that in https://github.com/astral-sh/uv/pull/11487

---

_Closed by @zanieb on 2025-02-13 22:17_

---
