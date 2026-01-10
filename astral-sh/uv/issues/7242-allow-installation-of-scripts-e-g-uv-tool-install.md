---
number: 7242
title: "Allow installation of scripts, e.g., `uv tool install --script <path>` or `uv script install`"
type: issue
state: open
author: shoucandanghehe
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2024-09-10T06:00:44Z
updated_at: 2025-10-30T19:04:38Z
url: https://github.com/astral-sh/uv/issues/7242
synced_at: 2026-01-10T01:24:12Z
---

# Allow installation of scripts, e.g., `uv tool install --script <path>` or `uv script install`

---

_Issue opened by @shoucandanghehe on 2024-09-10 06:00_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I often write small standalone python scripts and I'll use them anywhere.

If using `uv run /path/to/script.py` is a bit cumbersome for me (mostly typing the path)
So I want a `uv script` command to globally install these scripts

Maybe something like `uv tool`, but `uv script` manages scripts, `uv tool` manages tools.


Related: https://github.com/pypa/pipx/issues/1388 (although pipx doesn't implement it too)

---

_Renamed from "Feature request: add uv script to support global scripts" to "Feature request: add uv script to manage global scripts" by @shoucandanghehe on 2024-09-10 07:30_

---

_Comment by @nikhilweee on 2024-09-10 15:00_

Is this related to #5903? Or do you specifically want `uv` to install scripts globally? 

---

_Comment by @shoucandanghehe on 2024-09-10 19:40_

> Is this related to 5903? Or do you specifically want `uv` to install scripts globally?

It's not quite the same, 5903 is a project-level command **alias**, something like `npm run xxx`
Whereas what I want is to make a single-file script available globally, like `uv tool` does for packages

---

_Comment by @zanieb on 2024-09-10 20:00_

I guess we could support something like `uv tool install ./script.py` 🤔 

---

_Comment by @kj-9 on 2024-10-23 13:24_

I'm also looking forward to this feature.

In the meantime, I created a template that generates the minimum required files for `uv tool install` as a temporary solution:
https://github.com/kj-9/uv-tool-min-copier

---

_Comment by @Werni2A on 2025-01-13 18:52_

Now that a lot of the inline script meta-data (PEP 723) functionality has been merged, something like `uv tool install --script ./script.py` would complement the other CLI commands.

---

_Referenced in [astral-sh/uv#10997](../../astral-sh/uv/issues/10997.md) on 2025-01-27 18:53_

---

_Label `enhancement` added by @zanieb on 2025-01-27 19:30_

---

_Renamed from "Feature request: add uv script to manage global scripts" to "Allow installation of scripts, e.g., `uv tool install --script <path>` or `uv script install`" by @zanieb on 2025-01-27 19:30_

---

_Comment by @zanieb on 2025-01-27 19:31_

I'm in favor of this, but I think we'll need to figure out what the design looks like.

---

_Label `needs-design` added by @zanieb on 2025-01-27 19:54_

---

_Comment by @zachvalenta on 2025-01-29 13:19_

some recent examples that might be relevant here:

* https://akrabat.com/using-uv-as-your-shebang-line/
* https://aider.chat/2025/01/15/uv.html

both from a [discussion on HN](https://news.ycombinator.com/item?id=42855258)

---

_Comment by @alisterburt on 2025-02-03 18:43_

just adding a voice of support here - I tried to achieve this today with the syntax `uv tool install ./script.py` and was a bit surprised when it didn't work 🙂 

I regularly distribute scripts to less tech-savvy users, allowing installation of scripts like this would reduce the cognitive burden of using a script from
- where was that script again?
- how do I run it again?
- `uv run /path/to/script.py`

to
- `$ script.py`

To achieve this right now I need to spin up a whole package for the script and add an entry point with the same name as the package

edit: I like the proposed `uv script install` syntax

---

_Comment by @zanieb on 2025-06-03 03:05_

Would we install as `script.py` or `script`?

---

_Comment by @alisterburt on 2025-06-03 03:51_

@zanieb great catch - I prefer script to script.py

---

_Comment by @wbste on 2025-06-03 05:46_

+1

- `uv tool install --script example.py`
  - To be consistent with `uv init --script example.py` and `uv add --script example.py 'requests<3' 'rich'` is my thought here, but maybe the `--script` is superfluous since you can't add python files atm.
- Then just run it with `example` and remove the same way as other tools.

May be a really slick way to distribute helpful scripts, then you can do `uvx --from git+https://raw.githubusercontent.com/astral-sh/uv/refs/heads/main/scripts/check_registry.py` to run them and the like (just grabbed a random file, but you get the idea I hope).

---

_Comment by @amirreza8002 on 2025-09-10 23:38_

+1 on this
`uv pip install .` does generate the script, but maybe a better support would be handy

---

_Comment by @zmeir on 2025-09-11 20:58_

> Would we install as `script.py` or `script`?

Maybe `uv tool install —-script /path/to/script.py` would install as `script`, but using something like `uv tool install —-script foo —-from /path/to/script.py` could be used to customize the name?

---

_Comment by @shoucandanghehe on 2025-09-12 19:24_

> but using something like `uv tool install —-script foo —-from /path/to/script.py` could be used to customize the name?

I think this is a bit confusing. Perhaps we could use `--name`, or add an alias with `--alias`

---

_Referenced in [astral-sh/uv#16376](../../astral-sh/uv/issues/16376.md) on 2025-10-28 23:53_

---

_Comment by @lalvarezt on 2025-10-30 15:52_

while we wait for this feature to be implemented natively I went ahead and did a little [uv-helper](https://github.com/lalvarezt/uv-helper) for myself to solve the problem

## Features

- **Direct Git Installation**: Install scripts from any Git repository (GitHub, GitLab, Bitbucket, self-hosted) with one command
- **Git Refs Support**: Install from specific branches, tags, or commits
- **Dependency Management**: Automatically handle script dependencies
- **Idempotent Operations**: Re-running installs updates instead of failing
- **State Tracking**: Keep track of all installed scripts
- **Rich CLI**: Beautiful terminal output with progress bars and tables
- **Configuration**: Customize behavior via TOML config files
- **Update Management**: Update individual scripts or all at once


---

_Comment by @alisterburt on 2025-10-30 18:55_

@lalvarezt this is awesome, amazing work! I will test it out as a script distribution method and report back anything interesting 

---
