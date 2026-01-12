```yaml
number: 3177
title: "feature request: print ruff config"
type: issue
state: closed
author: upstartjohnvandivier
labels:
  - question
assignees: []
created_at: 2023-02-23T16:38:32Z
updated_at: 2023-02-24T05:07:32Z
url: https://github.com/astral-sh/ruff/issues/3177
synced_at: 2026-01-12T15:54:43Z
```

# feature request: print ruff config

---

_@upstartjohnvandivier_

as a developer, I'd like the ability to print the ruff config so that I can better troubleshoot whether an issue is a local config issue or a geniune bug in ruff

related issue where I ran into this troubleshooting limitation: https://github.com/charliermarsh/ruff/issues/3174

I have no particular opinion on the cli command syntax but let me propose a new `ruff log config` command for now

This command would print the config path used (whether pyproject.toml or something else), the python target version, selected rulesets, ignored rule information, and all of the other content that would normally be written inside the config file

this feature should also be documented clearly within https://beta.ruff.rs/docs/configuration/

possibly this print command could be added to the github ruff issue template as it will also probably assist in issue resolution

thanks!

---

_Comment by @charliermarsh on 2023-02-23 16:41_

Can you try `ruff --show-settings foo.py`? It needs some cleaning up but it should include everything you've described.

---

_Label `question` added by @charliermarsh on 2023-02-23 16:41_

---

_Comment by @upstartjohnvandivier on 2023-02-23 17:26_

This works! actually it's perhaps too verbose, including formatting and values not optimized for quick developer troubleshooting, like:
```
                    LiteralStrategy(
                        {
                            [
                                46,
                                98,
                                122,
                                114,
                            ]: [
```

I would add that I didn't see the source file used to generate the config, but it seems consistent with my `pyproject.toml` and specifies `target_version: Py37` as I expect

I also now see this flag (`--show-settings`) documented under `ruff help check`

I think I passed over it because I was looking for a substring like `config*` rather than settings

I also see this flag is enabled at the top level but not mentioned under the top level info at `ruff help`

overall, it seems my concern is basically handled and this issue can be closed. optionally, we can enhance some of those points above which are basically nits 

3 optional nits recap:
1. enhance `ruff help`
2. `--show-config` as an alias for `--show-settings`
3. rework printed content for `ruff --show-settings .` to be a bit easier to read: rm json formatting, hide low level numerical values behind a verbose flag, and/or add sourcefile for config gen


---

_Comment by @charliermarsh on 2023-02-23 17:27_

Yeah we used to manually omit some of that `LiteralStrategy` stuff but it was kind of a pain.


---

_Comment by @charliermarsh on 2023-02-23 17:31_

I thought we did show _where_ the config came from but doesn't look like it.

---

_Comment by @charliermarsh on 2023-02-23 17:47_

Let's leave this open since there are a few things to improve here. We can't really change `ruff help`, since `ruff --show-settings /path/to/file.py` only works for historical reasons. We didn't _used_ to have multiple subcommands, but then we moved linting to `ruff check`. For backwards compatibility, we treat `ruff check` as the "default" subcommand. It's a little hacky, but it does work.


---

_Comment by @charliermarsh on 2023-02-24 05:07_

Fixed one of these issues, tracking the other in #3202.

---

_Closed by @charliermarsh on 2023-02-24 05:07_

---
