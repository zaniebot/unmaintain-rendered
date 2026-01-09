---
number: 16512
title: "(Documentation) Not clear which groups `uv export` exports"
type: issue
state: open
author: anentropic
labels:
  - documentation
assignees: []
created_at: 2025-10-30T14:08:27Z
updated_at: 2025-10-30T14:20:06Z
url: https://github.com/astral-sh/uv/issues/16512
synced_at: 2026-01-07T13:12:19-06:00
---

# (Documentation) Not clear which groups `uv export` exports

---

_Issue opened by @anentropic on 2025-10-30 14:08_

### Summary

https://docs.astral.sh/uv/reference/cli/#uv-export

> Export the project's lockfile to an alternate format.
> 
> At present, both requirements.txt and pylock.toml (PEP 751) formats are supported.
> 
> The project is re-locked before exporting unless the --locked or --frozen flag is provided.
> 
> uv will search for a project in the current directory or any parent directory. If a project cannot be found, uv will exit with an error.
> 
> If operating in a workspace, the root will be exported by default; however, a specific member can be selected using the --package option.

Unclear if this exports just the default group, all groups, default+optional groups but not dev groups, or what.

There are options for `--all-extras`, `--extra`, `--group`, `--no-default-groups`, `--no-dev`, `--no-extra`, `--no-group`, `--only-dev`, `--only-group` ... but none of them clarify explicitly what the default behaviour is.

### Example

It just needs a short sentence to say what the default behaviour is.

---

_Label `enhancement` added by @anentropic on 2025-10-30 14:08_

---

_Comment by @anentropic on 2025-10-30 14:14_

By running it to see... looks like it exports everything by default

At least, `dev` group deps were included

---

_Label `enhancement` removed by @konstin on 2025-10-30 14:14_

---

_Label `documentation` added by @konstin on 2025-10-30 14:14_

---

_Comment by @zanieb on 2025-10-30 14:15_

See https://docs.astral.sh/uv/concepts/projects/dependencies/#default-groups

---

_Comment by @zanieb on 2025-10-30 14:17_

It's has the same semantics as `uv run` and `uv sync`, we use the default groups. I'm not sure it's worth calling that out explicitly since all of our commands are operating that way?

---

_Comment by @anentropic on 2025-10-30 14:19_

It'd be great to have this referenced in the command docs

I looked carefully through the command docs and it did not occur to me, nor was there any hint mentioned, that the behaviour is configured that way. I doubt I would ever have looked under "Concepts"

I just wanted to lookup the behaviour of the command, not read the whole manual.

---
