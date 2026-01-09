---
number: 12072
title: Export extras from uv export
type: issue
state: open
author: kuza55
labels:
  - question
assignees: []
created_at: 2025-03-09T03:40:02Z
updated_at: 2025-09-04T14:34:38Z
url: https://github.com/astral-sh/uv/issues/12072
synced_at: 2026-01-07T13:12:18-06:00
---

# Export extras from uv export

---

_Issue opened by @kuza55 on 2025-03-09 03:40_

### Summary

Hi,

I am trying to use uv in combination with [pants](https://github.com/pantsbuild/pants)

I am exporting the uv requirements to a requirements.txt for consumption by pants.

It seems like uv doesn't export extras, which leads to some problems in later requirements inference, so I was hoping uv could emit these extras.

The command I am running is

```
pants uv export --all-packages --format requirements-txt > requirements.txt
```

### Example

_No response_

---

_Label `enhancement` added by @kuza55 on 2025-03-09 03:40_

---

_Comment by @charliermarsh on 2025-03-09 15:02_

You can include extras with flags like —all-extras.

---

_Label `enhancement` removed by @charliermarsh on 2025-03-09 15:02_

---

_Label `question` added by @charliermarsh on 2025-03-09 15:02_

---

_Comment by @kuza55 on 2025-03-09 17:40_

I tried --all-extras and it doesn't do what I want.

Specifically, if my pyproject.toml has "fakeredis[lua,json]" in the dependencies, I want the uv export output to say "fakeredis[lua,json]==2.27.0 ...".

--all-extras does not seem to do this.

---

_Comment by @charliermarsh on 2025-03-09 17:41_

No, we don't support that in the `uv export` interface. Why do you need it, though? There is no semantic difference.

---

_Comment by @kuza55 on 2025-03-09 18:46_

I was trying to wire this up to pants (a build system for monorepos) that subsets lockfiles to only bring in the deps that are needed for a given binary, so pants needed to know there was a dependency between the fakeredis and the json extras, rather than the json extras being extraneous.

---

_Referenced in [astral-sh/uv#12543](../../astral-sh/uv/issues/12543.md) on 2025-03-29 18:13_

---

_Comment by @michael-pplx on 2025-07-20 23:22_

I've encountered the same limitation with Bazel as well. I can achieve what I need using `uv pip compile`, but it is way slower and I'd like for this to be able run in a pre-push hook.

---

_Comment by @bolinocroustibat on 2025-09-04 14:33_

`pylock.toml` [has extras and dependency groups](https://packaging.python.org/en/latest/specifications/pylock-toml/), it would be great if we could also export the groups in `uv.lock` to `pylock.toml`.

---

_Referenced in [astral-sh/uv#16016](../../astral-sh/uv/issues/16016.md) on 2025-09-24 14:27_

---
