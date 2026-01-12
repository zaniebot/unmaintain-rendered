```yaml
number: 13686
title: Link to web-based docs in CLI output
type: issue
state: open
author: geofft
labels: []
assignees: []
created_at: 2025-05-27T18:35:07Z
updated_at: 2025-05-27T18:39:49Z
url: https://github.com/astral-sh/uv/issues/13686
synced_at: 2026-01-12T16:01:35Z
```

# Link to web-based docs in CLI output

---

_@geofft_

It's often useful for errors or warnings to give you a link to a documentation page on the web hosting longer docs than can be reasonably stuffed into a terminal-sized error message. For instance, this came up in https://github.com/astral-sh/uv/pull/13455#discussion_r2109387403 regarding the error message when resolution works for your _current_ Python version or platform, but your project isn't declared as Python-version-specific or platform-specific and some _other_ version/platform fails, and it'd be nice to have some examples of how to declare those constraints if that's the right answer for your project:

>> Also, is it possible (as in, is it house style) for both of these errors to link to some page in the uv docs explaining why this might happen and what to do about it? 
>
> We do not do this yet because we don't have the ability to permalink to documentation (i.e., it's not versioned)

Here's my understanding of what's needed:

* We should have versioned documentation, so that a particular client version can link to its own docs. (I think in some cases we would actually prefer to link to the latest docs so we are able to update the suggestions if the ecosystem around us changes or we document something better, but certainly in some cases we would want versioned docs, so we should have them.)
* Ideally, we would have something approximating a URL shortener hosted on an `astral.sh` subdomain so you can output relatively short links on the terminal, maybe `https://docs.astral.sh/link/abc123` on the existing docs subdomain, maybe `https://help.astral.sh/abc123/`, whatever. It doesn't need a web-based UI, but it does need the ability to be deployed independently of actually releasing some project.
* We definitely need a way to check that the links on this URL shortener are still valid. If they link to our own docs, it would be super nice if a pull request to break the link target got a CI failure. But at the least it should alert on live failures (Discord, GH issue, something).
* It might be nice to use the fancy new OSC 8 control sequence for doing hyperlinks in terminals, documented a bit at https://github.com/Alhadis/OSC8-Adoption/ (which also mentions adoption by other CLI tools/libraries, e.g., I find the GCC implementation interesting), but certainly lowest priority.
* @zanieb says: "There are other problems too, like we can't link from the long help to the docs and the generated CLI reference docs can't link to other pages."

Note that parts of this, like the shortener, aren't specific to uv (I want this in python-build-standalone, for instance).

---

_Comment by @zanieb on 2025-05-27 18:39_

xrefs

- https://github.com/astral-sh/uv/issues/5605#issuecomment-2614533188
- https://github.com/astral-sh/uv/pull/12017#issuecomment-2709033445
- https://github.com/astral-sh/uv/issues/9132

---
