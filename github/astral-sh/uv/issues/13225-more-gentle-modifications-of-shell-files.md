---
number: 13225
title: More gentle modifications of shell files
type: issue
state: open
author: nedbat
labels:
  - enhancement
assignees: []
created_at: 2025-04-30T11:28:36Z
updated_at: 2025-05-02T10:06:26Z
url: https://github.com/astral-sh/uv/issues/13225
synced_at: 2026-01-07T13:12:18-06:00
---

# More gentle modifications of shell files

---

_Issue opened by @nedbat on 2025-04-30 11:28_

### Summary

By default during installation, my shell startup files are modified.  I know the docs say this will happen, but I think there are ways to do this more gracefully.  Here are some possibilities:

1. Examine $PATH and see that .local/bin is already on the PATH so there's no need to modify the files at all (best).
1. When adding a line to a shell file, include a comment indicating that it was uv that added the line.
1. During installation, mention that you changed the file.
1. During uninstallation, either undo the change (risky and difficult) or at least mention that the user might want to undo those changes with specific mentions of the files to look at.

### Example

_No response_

---

_Label `enhancement` added by @nedbat on 2025-04-30 11:28_

---

_Comment by @konstin on 2025-04-30 11:31_

> Examine $PATH and see that .local/bin is already on the PATH so there's no need to modify the files at all (best).

This should already be the behavior (https://github.com/axodotdev/cargo-dist/pull/1264), is it adding an entry for you even though `~/.local/bin` is on `PATH`?

CC @Gankra for the other items

---

_Comment by @nedbat on 2025-04-30 11:38_

> This should already be the behavior

My PATH has `/Users/ned/.local/bin` in it.  The env file uv installs checks to see if `$HOME/.local/share/../bin` is in the PATH. It's the same place, but the wildcard matching of `*:"$HOME/.local/share/../bin":*` fails, so it adds it again.

I don't know if the shell files would be untouched if the path matching worked.

---

_Comment by @nedbat on 2025-04-30 11:53_

I have `XDG_DATA_HOME=/Users/ned/.local/share`, and the installation directory is calculated as `$XDG_DATA_HOME/../bin`.

---

_Comment by @Gankra on 2025-04-30 20:22_

Yeah this is always a struggle to make the no-op detection precise (suggestion 1).

I think adding the comment (suggestion 2) would be reasonable/nice.

I think suggestion 3 would probably be a lot of needless noise and spook people (we edit *a lot* of rcfiles "just in case" because we can't reliably assume you'll keep using the exact shell you're using now, and don't want your PATH to break when you swap shells).

Suggestion 4 we could also potentially do, as long as the detection is really strict and not too clever (so it's willing to fail to remove stuff from PATH). But more generally we just don't really *have* uninstallation support at all yet. If we did, this would be one of the features for sure.

---

_Comment by @nedbat on 2025-04-30 21:45_

> But more generally we just don't really have uninstallation support at all yet

True, but there's a uninstallation section in the docs.  Perhaps at least a mention there?

---

_Comment by @konstin on 2025-05-02 10:06_

For the installation, we use the XDG bin dir as it is usually already in the user's PATH (and users without it in PATH will often use other installation methods than our script). The default `bash` package on Ubuntu for example includes:

```
# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/.local/bin" ] ; then
    PATH="$HOME/.local/bin:$PATH"
fi
```

This means that ideally, we should barely ever have to modify dotfiles at all. For this case specifically, I think we need fix our path detection more robust to also capture cases such as `$HOME/.local/share/../bin`. This hopefully reduces the emphasis on how we modify dotfiles, though I agree that the messaging could be improved.

---
