```yaml
number: 4439
title: "`uv pip tree` improvement"
type: issue
state: closed
author: ibraheemdev
labels:
  - enhancement
  - help wanted
  - cli
assignees: []
created_at: 2024-06-21T19:55:47Z
updated_at: 2024-07-01T21:24:42Z
url: https://github.com/astral-sh/uv/issues/4439
synced_at: 2026-01-10T05:31:37Z
```

# `uv pip tree` improvement

---

_Issue opened by @ibraheemdev on 2024-06-21 19:55_

Some additional features that did not land as part of https://github.com/astral-sh/uv/pull/3859:
- [ ] Proper handling of extras
- [x] `--no-dedupe` (https://github.com/astral-sh/uv/pull/4449)
- [x] `--prune` and `--depth` (https://github.com/astral-sh/uv/pull/4440)
- [x] `--package` (https://github.com/astral-sh/uv/pull/4655)
- [x] `--invert` (https://github.com/astral-sh/uv/pull/4621)
- [ ] Machine readable output (e.g. JSON with `-j`)

---

_Comment by @zanieb on 2024-06-21 20:08_

Can you create specific issues for each of these so we can track contributions easier?

---

_Comment by @ChannyClaus on 2024-06-21 21:29_

how would `--no-dedupe` and the current behavior for printing out dependency cycles would interact? wouldn't it end up with an infinite loop?

---

_Comment by @zanieb on 2024-06-21 21:36_

We would need to deduplicate cycles still, but we can show anything that's not a cycle.

---

_Comment by @ChannyClaus on 2024-06-21 22:10_

should we also print `(**)` instead of `(*)` for cyclic dependencies? it may be nice to disambiguate between the regular de-duplication and cyclic dependencies.

---

_Comment by @zanieb on 2024-06-21 22:25_

If you use `--no-dedupe` we can just use `(*)` still and change the footnote to say it's a cycle. idk if we need to highlight cycles further in the default output, sounds hard to read.

---

_Comment by @kdeldycke on 2024-06-22 08:09_

May I propose to add a machine-readable output like JSON/CSV/Toml/whatever to the list of improvements?

So I can hack `uv pip tree` output to produce a mermaid graph of dependencies (and emulates `pipdeptree --mermaid > dependencies.mmd`)?

---

_Comment by @dsully on 2024-06-22 17:50_

+1 on adding JSON `-j` to `pipdeptree` and other machine readable outputs. Example output:

```json
[
    ....
    {
        "package": {
            "key": "werkzeug",
            "package_name": "Werkzeug",
            "installed_version": "3.0.3"
        },
        "dependencies": [
            {
                "key": "markupsafe",
                "package_name": "MarkupSafe",
                "installed_version": "2.1.5",
                "required_version": ">=2.1.1"
            }
        ]
    }
]
```

---

_Label `enhancement` added by @charliermarsh on 2024-06-23 17:20_

---

_Label `cli` added by @charliermarsh on 2024-06-23 17:20_

---

_Closed by @zanieb on 2024-06-24 16:54_

---

_Closed by @zanieb on 2024-06-24 16:54_

---

_Comment by @Peiffap on 2024-06-25 12:17_

@zanieb I think this accidentally got closed in #4449; #4440 hasn't been merged yet and contains some of the additional features being asked for (`--depth` and `--prune`). There was also the bullet on proper handling of extras.

You might also want to ask about creating follow-up issues for the output flags that were mentioned in comments (`--mermaid`, `-j`, etc.).

---

_Reopened by @zanieb on 2024-06-25 12:37_

---

_Comment by @hauntsaninja on 2024-06-26 22:56_

I mostly use `pipdeptree` with its `-p / --packages` and `-r / --reverse` flags; I'd love to see `uv pip tree` have equivalent functionality!

(let me know if I should open a new issue for that :-) )

---

_Comment by @zanieb on 2024-06-26 23:14_

We're on the loose with this issue right now — seems to be working fine since @ChannyClaus is just doing them all :)

Please chime in if you're interested in doing any of these improvements though!

---

_Label `help wanted` added by @zanieb on 2024-06-26 23:14_

---

_Comment by @ChannyClaus on 2024-06-27 00:09_

oh okay yah i can add those (`--packages` and `--reverse` that is) 🌵 

was going to make a PR for the extras (since this would be a "breaking" change unless it's behind a flag?) once https://github.com/astral-sh/uv/pull/4440 merged but maybe i'll just stack them on top of each other or something...

---

_Comment by @zanieb on 2024-06-27 00:33_

I don't think you need to worry about breaking changes in the text output since it's meant for users to "view".

---

_Closed by @zanieb on 2024-06-27 00:34_

---

_Reopened by @ibraheemdev on 2024-06-27 04:21_

---

_Closed by @ibraheemdev on 2024-07-01 16:58_

---

_Reopened by @ibraheemdev on 2024-07-01 16:58_

---

_Closed by @ibraheemdev on 2024-07-01 21:13_

---

_Comment by @ibraheemdev on 2024-07-01 21:24_

Thanks @ChannyClaus for all your work on this! Splitting the remaining two issues into https://github.com/astral-sh/uv/issues/4711 and https://github.com/astral-sh/uv/issues/4710.

---
