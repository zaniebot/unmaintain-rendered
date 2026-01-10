```yaml
number: 12591
title: keyring support not working on MacOS
type: issue
state: open
author: TheFriendlyCoder
labels:
  - bug
  - external
assignees: []
created_at: 2025-03-31T18:44:54Z
updated_at: 2025-04-02T22:50:49Z
url: https://github.com/astral-sh/uv/issues/12591
synced_at: 2026-01-10T03:41:47Z
```

# keyring support not working on MacOS

---

_Issue opened by @TheFriendlyCoder on 2025-03-31 18:44_

### Summary

I've been trying to get the keyring feature working on my MacOS platform, based on the instructions provided in your user docs. After doing considerable digging, I found the root cause to be a bug in the [keyring tool](https://github.com/jaraco/keyring/issues/711) itself. This was not an easy problem to find, but I wanted to report it here upstream as well, so there is a record of it.

I'm not sure if there is anything that can be done from the `uv` side, unless someone can come up with a workaround.

It might be helpful to provide some more details around the setup for keyring and how it interacts with `uv` to make it easier for users to set this up in the future. For example, if you could provide some example CLI commands that could be run, outside of `uv`, that mirror the underlying implementation so that users can more easily debug problems with their keyring setup, it might help avoid confusion in the future.

I've only been using `uv` for a few days now, but I have to say, other than this one small hiccup, I am very impressed with your tool, the docs, community support, and everything. Keep up the great work!

### Platform

MacOS Sequoia v15.3.2

### Version

uv 0.6.11 (Homebrew 2025-03-30)

### Python version

3.9.18

---

_Label `bug` added by @TheFriendlyCoder on 2025-03-31 18:44_

---

_Comment by @zanieb on 2025-04-01 20:08_

Thank you so much for the report!

In the long-term, we'll probably try to phase out use of the `keyring` package entirely in favor of something we have more control over. Hopefully that solves the "root cause" of the problem. I'm hesitant to sink too much time into documenting its behaviors.

You could adopt our test keyring plugin for investigating issues https://github.com/astral-sh/uv/blob/37c25f2a9da329a4870167c778f0403e9e12bbc9/scripts/packages/keyring_test_plugin

> I'm not sure if there is anything that can be done from the uv side, unless someone can come up with a workaround.

Can you provide the username? We'll avoid the `--creds` endpoint then (that's fairly new, see #12316)

---

_Label `external` added by @zanieb on 2025-04-01 20:08_

---

_Comment by @TheFriendlyCoder on 2025-04-02 22:50_

I will test out the keyring plugin as soon as I get some time.

In the interim, how exactly would I provide the username to the keyring query? It wasn't exactly clear on how that interaction worked based on the docs.

---
