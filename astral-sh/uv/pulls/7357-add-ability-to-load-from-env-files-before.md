```yaml
number: 7357
title: add ability to load from .env files before invoking the command
type: pull_request
state: closed
author: haydn-j-evans
labels: []
assignees: []
base: main
head: feature/1384/support-dotens-in-run-command
created_at: 2024-09-13T11:17:57Z
updated_at: 2025-03-26T13:08:05Z
url: https://github.com/astral-sh/uv/pull/7357
synced_at: 2026-01-12T16:07:47Z
```

# add ability to load from .env files before invoking the command

---

_@haydn-j-evans_

## Summary

Pipenv and other equivalent tools have the ability to load an .env file during execution of a script etc

https://pipenv.pypa.io/en/stable/shell.html#automatic-loading-of-env

The PR adds a new flag `--load-dotenv` and a new ENV `UV_RUN_LOAD_DOTENV`  that enables this feature for `uv run`

resolves #1384 

## Test Plan

Have tested it locally by successfully invoking scripts that require ENV's to be set.




---

_Comment by @eth3lbert on 2024-09-13 11:28_

`dotenv` is implicitly unmaintained. 
Ref: https://github.com/rustsec/advisory-db/issues/1254

---

_Comment by @haydn-j-evans on 2024-09-13 11:31_

@eth3lbert I will switch it to [dotenvy](https://github.com/allan2/dotenvy) if you are happy with the idea of the PR :) 

---

_Assigned to @zanieb by @zanieb on 2024-09-15 21:59_

---

_Comment by @theintz on 2024-09-26 10:29_

Can't wait for this to land, we're excited to switch to uv, but are currently blocked by this.

---

_Assigned to @charliermarsh by @zanieb on 2024-10-01 18:01_

---

_Unassigned @zanieb by @zanieb on 2024-10-01 18:01_

---

_Comment by @haydn-j-evans on 2024-10-09 12:23_

@charliermarsh This PR is currently configured to be opt in, but in light of the discussion in the issue thread, should we instead make it opt out?

i.e. 

`--no-load-dotenv`

`UV_RUN_NO_LOAD_DOTENV`

And also the naming can be discussed, I kept it scoped to `UV_RUN_ ` as this is a run specific variable, but perhaps if there is a need for full uv configuration with a .env, we just call it `UV_NO_LOAD_DOTENV` and then just expand its use within uv later.

---

_Comment by @PhilipVinc on 2024-10-09 12:26_

Could this be used to set `UV_PROJECT_ENVIRONMENT` in the `.env` file, offering a slightly better workaround to the issues discussed in #7642 and #1495 ?

---

_Comment by @haydn-j-evans on 2024-10-09 12:57_

@PhilipVinc currently it only affects the `run` command, it would have to be further expanded to support uv global config.

---

_@haydn-j-evans reviewed on 2024-10-09 13:08_

---

_Review comment by @haydn-j-evans on `crates/uv/src/commands/project/run.rs`:833 on 2024-10-09 13:08_

currently this will just fail silently if the file is malformed or missing - is this what we want?

---

_@sminnee reviewed on 2024-10-09 20:47_

---

_Review comment by @sminnee on `crates/uv/src/commands/project/run.rs`:833 on 2024-10-09 20:47_

IMO missing shouldn't show an error, because the chance that the file is missing means that someone doesn't want to use .env is high.

However, a malformed file should be indicated to the developer. The chance that they have intentionally created a .env file that is malformed, in order for it not to be loaded by uv, is very low. I would suggest a warning rather than an error, because execution can still continue.

---

_Comment by @charliermarsh on 2024-10-09 22:15_

Yeah I think my preference would be:

- Read `.env` by default (only in `uv run` for now; we may expand it later).
- Allow users to pass `--env-file` (or `UV_ENV_FILE`) to override the file.
- Add `--no-env-file` and `UV_NO_ENV_FILE` to disable it.

I think we can omit the `UV_RUN_` prefixes.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:833 on 2024-10-09 22:20_

Yeah, I think we should show a user-facing warning if there's an error other than "file does not exist". We could also show a warning if it's "file does not exist" and the user provided an explicit path to a dotenv file, but I'm not certain if we should and it's not blocking.

---

_@charliermarsh reviewed on 2024-10-09 22:20_

---

_Comment by @haydn-j-evans on 2024-11-05 12:25_

closing this as it seems to be implemented already

---

_Closed by @haydn-j-evans on 2024-11-05 12:25_

---

_Comment by @shreyansp on 2025-03-26 11:55_

> Yeah I think my preference would be:
> 
> * Read `.env` by default (only in `uv run` for now; we may expand it later).
> * Allow users to pass `--env-file` (or `UV_ENV_FILE`) to override the file.
> * Add `--no-env-file` and `UV_NO_ENV_FILE` to disable it.
> 
> I think we can omit the `UV_RUN_` prefixes.

@haydn-j-evans  @charliermarsh 
Thanks for the summary of features regarding `.env` setup.
Before I raise a bug, I wanted to check if the first option in the above list ever implemented?
In the latest version 0.6.9, I can confirm the second option to set .`env` manually every time works, 
but not the first option where `uv run` is suppose to pick up the `.env` by default

UPDATE: Never mind just found: #9381 



---
