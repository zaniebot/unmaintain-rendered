```yaml
number: 3783
title: SSH authentication skips password check, hangs
type: issue
state: open
author: mishpat
labels:
  - bug
assignees: []
created_at: 2024-05-23T02:27:27Z
updated_at: 2025-12-10T18:21:56Z
url: https://github.com/astral-sh/uv/issues/3783
synced_at: 2026-01-12T15:58:45Z
```

# SSH authentication skips password check, hangs

---

_@mishpat_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Installing a package from github via ssh with
```
uv pip install git+ssh://git@{enterprise-git-repo}/{username}/{package}.git
```
hangs at "Resolving dependencies", _unless_ I run with --verbose, in which case it takes me to my public key passphrase and works after that.

I do briefly see the password prompt flash by during the "Resolving dependencies" stage but it disappears instantly.

I'm still on 0.1.45 (mirror is only updated weekly), apologies if this has been fixed but I didn't see much I could tell was relevant in the releases since then.

---

_Comment by @zanieb on 2024-05-23 02:30_

Thanks for the report, this shouldn't have changed since then. We don't technically support providing authentication interactively, I'm surprised it even prompts — we'll need to investigate that.

---

_Label `bug` added by @zanieb on 2024-05-23 02:30_

---

_Comment by @seb-kro on 2025-01-29 14:09_

I just ran into the same issue on a completely new work setup and it took me a while to figure out the problem. Using the `--no-progress` flag seems to be another workaround besides increasing verbosity. But this should probably work without having to specify any flags.

---

_Comment by @zyaoj on 2025-02-03 15:59_

Same issue occured. It is only clear when I specified `--verbose` when running `uv sync` to see why it hangs there forever. It has been waiting for my github name and password:

```
Password for 'https://github.com':
```

For the record, it wasn't the case last week when setting up the same repo.

---

_Comment by @zanieb on 2025-02-16 23:47_

Same as https://github.com/astral-sh/uv/issues/5107

---

_Comment by @jtfmumm on 2025-04-04 08:42_

Addressed by #11744

---

_Closed by @jtfmumm on 2025-04-04 08:42_

---

_Comment by @geofft on 2025-06-03 15:19_

#11744 seems to address HTTP auth, not SSH auth, which this issue was originally reported about. I can reproduce this with latest uv (0.7.9).

(I think this is the same issue as #12685 at this point.)

---

_Reopened by @geofft on 2025-06-03 15:19_

---

_Comment by @bocytko on 2025-12-10 18:21_

We use `uvx --from git+https://github.com/org/repo` as means to setup `stdio` MCP servers from VSCode and custom tooling.
 
Triaging this error took quite some time until a user seeing a "Resolving dependencies" typed their ssh key passphrase "because reasons"... `--verbose` showed it's waiting for an exclusive lock on a file in `~/.cache/uv/git..`. Running with `--no-cache` mitigated the problem, but is no real workaround as not all users control a managed config.

uv 0.9.5+

---
