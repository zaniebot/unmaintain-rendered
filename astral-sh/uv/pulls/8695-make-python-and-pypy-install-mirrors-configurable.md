```yaml
number: 8695
title: Make Python and PyPy install mirrors configurable in uv.toml
type: pull_request
state: merged
author: owenbrooks
labels:
  - enhancement
assignees: []
merged: true
base: main
head: mirror_configs
created_at: 2024-10-30T11:31:12Z
updated_at: 2024-11-13T19:12:06Z
url: https://github.com/astral-sh/uv/pull/8695
synced_at: 2026-01-12T16:08:27Z
```

# Make Python and PyPy install mirrors configurable in uv.toml

---

_@owenbrooks_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
-->

## Summary

Adds python-install-mirror and pypy-install-mirror as keys for uv.toml, and cli args for `uv python install`. 

Could leave the cli args out if we think the env vars and configs are sufficient. 

Fixes #8186 

<!-- What's the purpose of the change? What does it do, and why? -->



---

_Review requested from @zanieb by @konstin on 2024-10-30 11:32_

---

_@charliermarsh reviewed on 2024-10-30 13:01_

---

_Review comment by @charliermarsh on `crates/uv-python/src/installation.rs`:134 on 2024-10-30 13:01_

Do we need to thread the values through here too?

---

_@owenbrooks reviewed on 2024-10-31 07:36_

---

_Review comment by @owenbrooks on `crates/uv-python/src/installation.rs`:134 on 2024-10-31 07:36_

That would keep it consistent. Do we want the cli args? Looks like that fetch function ends up being called by quite a few commands.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3888 on 2024-10-31 13:25_

I would probably rename this to `mirror` in the CLI context

---

_@zanieb reviewed on 2024-10-31 13:25_

---

_@zanieb reviewed on 2024-10-31 13:26_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3896 on 2024-10-31 13:26_

I think this can be `pypy-mirror` in this context

---

_@zanieb reviewed on 2024-10-31 13:26_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:134 on 2024-10-31 13:26_

I don't think we want to add the CLI args everywhere. I'd propagate the settings to here instead.

---

_@zanieb reviewed on 2024-10-31 13:27_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:134 on 2024-10-31 13:27_

I guess we also have to resolve the settings with the environment variable at some point then?

---

_@s1as3r reviewed on 2024-10-31 14:07_

---

_Review comment by @s1as3r on `crates/uv-python/src/installation.rs`:134 on 2024-10-31 14:07_

Adding to this. I worked on this a bit before this pr was made. I tried doing this a bit differently. 
- I added the python and pypy install mirros to `GloablOptions` in `uv-settings`
- the env vars for the same were also added to `env` (vars not exposed as cli args) inside of the `uv` crate.
- the setting from uv.toml and the environment variables are combined in `GlobalSettings::resolve`
- There were quite a few functions that had to be changed to include the mirrors as parameters.

---

_@charliermarsh reviewed on 2024-10-31 16:10_

---

_Review comment by @charliermarsh on `crates/uv-python/src/installation.rs`:134 on 2024-10-31 16:10_

@zanieb -- Should these just be globals, like `--allow-insecure-host`? (Do they even need to be exposed on the command-line at all?)

---

_@zanieb reviewed on 2024-10-31 16:12_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:134 on 2024-10-31 16:12_

I think it's reasonable to expose them in `uv python install`, but I wouldn't expose them elsewhere.

---

_Comment by @Fau57 on 2024-11-03 09:39_

Not sure if this change has happened or will happen or once at all but I am all for it

---

_@owenbrooks reviewed on 2024-11-04 23:44_

---

_Review comment by @owenbrooks on `crates/uv-cli/src/lib.rs`:3888 on 2024-11-04 23:44_

Done

---

_Comment by @owenbrooks on 2024-11-04 23:47_

Config has been threaded through the other relevant commands. The CLI args take precedence over env vars which take precedence over config values. CLI args are present only for `python install`.

---

_Marked ready for review by @owenbrooks on 2024-11-04 23:47_

---

_Comment by @Vynce on 2024-11-12 22:48_

@owenbrooks, thanks for picking this up!

@zanieb, I think this is ready for review again unless you want a rebase/merge from main to address the conflicts first?

---

_@zanieb approved on 2024-11-13 14:45_

---

_Label `enhancement` added by @zanieb on 2024-11-13 15:58_

---

_Merged by @zanieb on 2024-11-13 16:08_

---

_Closed by @zanieb on 2024-11-13 16:08_

---

_Comment by @Vynce on 2024-11-13 19:12_

Thanks for getting this merged 😊

---
