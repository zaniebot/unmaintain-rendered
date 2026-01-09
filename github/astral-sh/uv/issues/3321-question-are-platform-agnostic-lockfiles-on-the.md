---
number: 3321
title: "[Question] Are platform-agnostic lockfiles on the roadmap?"
type: issue
state: closed
author: Turakar
labels:
  - question
assignees: []
created_at: 2024-04-30T12:17:08Z
updated_at: 2024-07-08T21:31:59Z
url: https://github.com/astral-sh/uv/issues/3321
synced_at: 2026-01-07T13:12:17-06:00
---

# [Question] Are platform-agnostic lockfiles on the roadmap?

---

_Issue opened by @Turakar on 2024-04-30 12:17_

The uv README discusses the fact that uv generates platform-specific lockfiles, while other tools like PDM and poetry generate platform-agnostic lockfiles. While uv is able to generate cross-platform lockfiles, the question remains: Does uv plan to at some point be able to create platform-agnostic lockfiles?

At the moment, one would need to manage multiple lockfiles for a cross-platform project. This might be tedious and lead to inconsistencies. However, a unified lockfile might require conditionals, so I think that there is a tradeoff here.

I am interested in your opinions on that topic. Please link instances where this issue has been discussed previously.


---

_Comment by @BurntSushi on 2024-04-30 12:44_

Yes, we are planning to do this. The [blog post announcing `uv` sketches our high level vision](https://astral.sh/blog/uv#a-cargo-for-python-uv-and-rye). I don't think we've discussed platform-agnostic lock files publicly in any open issues yet because it's still in the very early design phase. But we're looking to tools like Cargo and Poetry for inspiration.

---

_Closed by @BurntSushi on 2024-04-30 12:44_

---

_Label `question` added by @BurntSushi on 2024-04-30 12:44_

---

_Comment by @adminy on 2024-05-02 15:21_

would you support poetry lock files? Its a well defined format and if its already present, why not just use it?

---

_Comment by @BurntSushi on 2024-05-02 15:30_

We don't have any plans to support Poetry lock files. As I understand it, Poetry needs to do a second resolution in order to install from the lock file (using the lock file as its "universe"). We're trying to avoid that, and I do believe it will require additional metadata to be in the lock file to accomplish that goal.

But more practically speaking, I don't think Poetry's lock file is well defined? I'm not aware of a spec for it. And we've generally avoided trying to read the config files of other tools. (A lock file isn't the same as a config file, but if the definition of Poetry's lock file is "whatever Poetry implements," then it likely has a similar set of problems as reading other tools' config files. See #1404 for more discussion on that point.)

---

_Comment by @adminy on 2024-05-02 16:07_

So the only feature that I care about from poetry is the ability to define different packages inside project.toml file. and install just the ones needed per use case. To some extent that is now possible with pip as a builtin now. If uv supports that I'm happy to just switch away from poetry, lock files aside.

---

_Comment by @chrisrodrigue on 2024-07-08 21:31_

I had the same question which was answered here: https://github.com/astral-sh/uv/issues/3347

---
