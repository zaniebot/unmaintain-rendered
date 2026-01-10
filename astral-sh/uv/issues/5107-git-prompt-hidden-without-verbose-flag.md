```yaml
number: 5107
title: "Git prompt hidden without `--verbose` flag"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - cli
assignees: []
created_at: 2024-07-16T14:34:16Z
updated_at: 2025-02-25T19:30:57Z
url: https://github.com/astral-sh/uv/issues/5107
synced_at: 2026-01-10T03:50:30Z
```

# Git prompt hidden without `--verbose` flag

---

_Issue opened by @zanieb on 2024-07-16 14:34_

The following command hung for a while:
```
❯ uv pip compile docs/requirements-insiders.in -o docs/requirements-insiders.txt --universal -p 3.12
Updating ssh://git@github.com/astral-sh/mkdocs-material-insiders.git (38c0b8187325c3bab386b666daf3518ac036f2f4)
The authenticity of host 'github.com (140.82.112.4)' can't be established.                                                            
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
Updating ssh://git@github.com/astral-sh/mkdocs-material-insiders.git (38c0b8187325c3bab386b666daf3518ac036f2f4)
⠹ Resolving dependencies...                                                                                             
              ^C
```

I cancelled it and re-ran with `--verbose`:
```
❯ uv pip compile docs/requirements-insiders.in -o docs/requirements-insiders.txt --universal -p 3.12 -v
DEBUG uv 0.2.23
DEBUG Starting Python discovery for Python 3.12
DEBUG Looking for exact match for request Python 3.12
DEBUG Searching for Python 3.12 in system path
DEBUG Found cpython 3.12.1 at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.1 interpreter at .venv/bin/python3 for builds
DEBUG Using request timeout of 30s
DEBUG Fetching source distribution from Git: ssh://git@github.com/astral-sh/mkdocs-material-insiders.git
DEBUG Acquired lock for `ssh://github.com/astral-sh/mkdocs-material-insiders`
DEBUG Updating git source `Url { scheme: "ssh", cannot_be_a_base: false, username: "git", password: None, host: Some(Domain("github.com")), port: None, path: "/astral-sh/mkdocs-material-insiders.git", query: None, fragment: None }`
DEBUG Attempting GitHub fast path for: https://api.github.com/repos/astral-sh/mkdocs-material-insiders/commits/38c0b8187325c3bab386b666daf3518ac036f2f4
DEBUG failed to check github HTTP status client error (404 Not Found) for url (https://api.github.com/repos/astral-sh/mkdocs-material-insiders/commits/38c0b8187325c3bab386b666daf3518ac036f2f4)
DEBUG Performing a Git fetch for: ssh://git@github.com/astral-sh/mkdocs-material-insiders.git
The authenticity of host 'github.com (140.82.112.4)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```

Then I could provide the information for the git prompt and move forward.

---

_Label `bug` added by @zanieb on 2024-07-16 14:34_

---

_Label `cli` added by @zanieb on 2024-07-16 14:34_

---

_Comment by @charliermarsh on 2024-07-16 15:00_

@ibraheemdev - Is this straightforward?

---

_Comment by @zanieb on 2024-07-16 15:02_

I don't see where we're gating this with the verbose flag with a casual look.

---

_Comment by @charliermarsh on 2024-07-18 02:10_

Not clear to me how this is appearing _even with_ `--verbose`.

---

_Comment by @zanieb on 2024-07-18 03:59_

Yeah I was confused... haha. Tried to write a MRE but it just fails:

```
❯ cat ~/bin/git
#/usr/bin/env bash

echo "Content please"
read test

❯ uv pip install --no-cache "git+https://github.com/astral-test/uv-public-pypackage@0dacfd662c64cb4ceb16e6cf65a157a8b715b979"
Updating https://github.com/astral-test/uv-public-pypackage (0dacfd662c64cb4ceb16e6cf65a157a8b715b979)                                error: Git operation failed
  Caused by: process didn't exit successfully: `git init` (exit status: 1)
--- stdout
Content please

❯ uv pip install --no-cache "git+https://github.com/astral-test/uv-public-pypackage@0dacfd662c64cb4ceb16e6cf65a157a8b715b979" --verbose
DEBUG uv 0.2.26
DEBUG Searching for Python interpreter in system path
DEBUG Found cpython 3.12.3 at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: git+https://github.com/astral-test/uv-public-pypackage@0dacfd662c64cb4ceb16e6cf65a157a8b715b979
DEBUG Using request timeout of 30s
DEBUG Fetching source distribution from Git: https://github.com/astral-test/uv-public-pypackage
DEBUG Acquired lock for `https://github.com/astral-test/uv-public-pypackage`
DEBUG Updating git source `Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/astral-test/uv-public-pypackage", query: None, fragment: None }`
error: Git operation failed
  Caused by: process didn't exit successfully: `git init` (exit status: 1)
--- stdout
Content please
```

---

_Comment by @rhoboro on 2024-10-21 09:25_

@zanieb @charliermarsh 
Hi, I'm a big fan of uv! Thank you as always.

I have found the cause of this.
It seems that the tick of ResolverReporter is erasing the prompt for the git command.
Therefore, if this line is removed, no hang-up will occur.
https://github.com/astral-sh/uv/blob/ec8eee0fde0915e2e2cef3f19538856486cc144f/crates/uv/src/commands/reporters.rs#L323

Could you fix this?
If you don't have time, please point me in the right direction and I'll try to create a PR.
( I am not an expert in Rust, so this may take some time 😅 )


---

_Comment by @charliermarsh on 2024-10-21 16:22_

Oh wow, that’s a great find and explains a lot!

---

_Comment by @zanieb on 2024-10-21 16:59_

🤔 

https://github.com/Agilent/yb/blob/main/yb/src/main.rs#L72 from https://github.com/console-rs/indicatif/issues/594 seems helpful?

---

_Comment by @Viicos on 2024-12-17 17:19_

I was hit by a similar issue when running:

```sh
uv pip install "git+https://github.com/typing/typing_extensions@main"
```

Note that the correct URL is https://github.com/python/typing_extensions. Without using the `--verbose` flag, I get the following output and it hangs forever:

```sh
Updating https://github.com/typing/typing_extensions (main)
Updating https://github.com/typing/typing_extensions (main)
```

Only when running in verbose mode, I get:

```sh
DEBUG failed to check github HTTP status client error (404 Not Found) for url (https://api.github.com/repos/typing/typing_extensions/commits/main)
DEBUG Performing a Git fetch for: https://github.com/typing/typing_extensions
Username for 'https://github.com':
```

I assume because the repo can't be found, `uv` delegates to the git CLI, and because the HTTPS URL was used, the git CLI will use HTTP auth (which is deprecated iirc, maybe it doesn't work anymore).

---

_Comment by @zanieb on 2025-02-16 23:47_

Same as https://github.com/astral-sh/uv/issues/3783

---

_Comment by @piegamesde on 2025-02-19 14:17_

Looking at the code and how to resolve this, I fear that an animated spinner is fundamentally incompatible with an interactive prompt. I suggest either removing the animation, or entirely replacing all interactive prompts with a failure. Either way, the current situation is a huge foot gun and source of confusion. (There may be a third solution of detecting interactive prompts and disabling the spinner only then, but that sounds like it would be a lot of work and very brittle)

---

_Comment by @piegamesde on 2025-02-25 13:26_

As an alternative workaround to running `uv` with `-v`, setting `GIT_TERMINAL_PROMPT=0` also works.

---

_Comment by @RazerM on 2025-02-25 14:25_

This is something I've ran into. It's not the ideal or common scenario but there are use cases for providing credentials interactively.

`pip` does this well. `pipx` has the same problem as `uv` where the progress covers the prompt. I agree that disabling the prompt is better than appearing to hang, but it would be nice if an interactive prompt is at least possible with, for example, an `--interactive` option.

---

_Comment by @piegamesde on 2025-02-25 14:44_

An interactive prompt is still possible via git's SSH_ASKPASS configuration. uv does not need to bother handling these, as they start a UI element. I think there are other options for configuring git auth as well.

---

_Comment by @RazerM on 2025-02-25 14:50_

The scenarios I am thinking of are not using SSH and do not have a GUI available.

---

_Comment by @piegamesde on 2025-02-25 15:02_

Confusingly, SSH_ASKPASS is not specific to SSH auth and can also prompt for basic HTTP auth. If no GUI is available, I recommend setting up a custom credentials-helper. I recently did this to pass the GitLab auth CI token into git via an environment variable

Of course it would be possible to have an option in uv to not set `GIT_TERMINAL_PROMPT=0` while disabling the spinner animation, I'll leave this up to the uv devs to decide if such a feature would be worth its price

---

_Closed by @zanieb on 2025-02-25 19:30_

---
