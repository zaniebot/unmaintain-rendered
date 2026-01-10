---
number: 12103
title: Is there a way to list the extras on a package that is not yet installed?
type: issue
state: open
author: Apreche
labels:
  - question
assignees: []
created_at: 2025-03-10T18:43:51Z
updated_at: 2025-09-05T21:59:59Z
url: https://github.com/astral-sh/uv/issues/12103
synced_at: 2026-01-10T01:25:15Z
---

# Is there a way to list the extras on a package that is not yet installed?

---

_Issue opened by @Apreche on 2025-03-10 18:43_

### Question

A common scenario I run into is I have decided to install a particular package. However, I don’t know if the package has extras or not. If it does have extras, I don’t know what they are named, and I don’t know what they include. Right now all I can do is go to pypi.org, find the package, look in the left sidebar, and see if there is anything written under `Meta->Provides-Extra`. Even then, it only tells me the names of the extras, not what they include.

Would it be possible for uv to add a facility to show metadata about remote packages? I imagine it would be similar to when using `apt-cache show <packagename>` or `brew info <packagename>`. Something like `uv show <packagename>`. This could display all kinds of metadata for the package. This would include the available extras, and what dependencies they include.

Does uv already have this feature, and there is something I’m missing? I didn’t see anything in the CLI help or the web documentation.

### Platform

ALL

### Version

uv 0.6.5 (Homebrew 2025-03-06)

---

_Label `question` added by @Apreche on 2025-03-10 18:43_

---

_Comment by @peacefulotter on 2025-09-05 06:07_

This would definitely be a great feature! 

An idea would be to extend `uv tree` for discovering all extra dependency groups and the packages listed in them; something like `uv tree some_package`? 

---

_Comment by @Apreche on 2025-09-05 20:22_

> An idea would be to extend `uv tree` for discovering all extra dependency groups and the packages listed in them; something like `uv tree some_package`?

I don’t think uv tree is the right place for this. uv tree is about showing the dependency tree of the packages you already have included in your project. This request is about gathering information on packages from remote indexes.

That said some kind of parameter like `uv newcommand <remote_packagename> --tree` to display the optional and require dependencies of a remote package in a tree-like output would be very nice.

---

_Comment by @peacefulotter on 2025-09-05 21:59_

Yes I understood what you meant, I am frequently running into the same issue myself. 

I initially suggested using the uv tree command to avoid introducing an additional subcommand, since there are already quite a few and it can be hard to keep track of them all. Also, the output you're proposing is similar in structure and format to what tree already does.

That said, I would be happy if this was implemented however the devs would think is the best. 



---
