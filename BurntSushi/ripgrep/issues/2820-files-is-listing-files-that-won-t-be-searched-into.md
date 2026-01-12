```yaml
number: 2820
title: "--files is listing files that won't be searched into "
type: issue
state: closed
author: fabiospampinato
labels:
  - invalid
assignees: []
created_at: 2024-05-26T12:17:15Z
updated_at: 2024-05-26T12:51:24Z
url: https://github.com/BurntSushi/ripgrep/issues/2820
synced_at: 2026-01-12T16:13:25Z
```

# --files is listing files that won't be searched into 

---

_@fabiospampinato_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

13.0.0

### How did you install ripgrep?

brew install ripgrep

### What operating system are you using ripgrep on?

macOS 12.5.1

### Describe your bug.

The description for the "--files" flag says that it will list files that would be searched into, but if I execute something like this: `rg '.' node_modules --files | sort` I see top-level files listed also, like `./tsconfig.json`, which won't actually be searched into, as if I remove the `--files` flag I don't see any matches from those top-level files.

### What are the steps to reproduce the behavior?

Run `rg . [PATH] --files`, inside a directory containing some top-level files.

### What is the actual behavior?

I see some files listed that exist outside of the root search path I specified.

### What is the expected behavior?

I shouldn't see any files listed that exist outside of the root search path.

---

_Comment by @BurntSushi on 2024-05-26 12:27_

In the docs, the usage pattern for `--files` is:

```
rg [OPTIONS] --files [PATH...]
```

You appear to be using it as if the usage pattern is this:

```
rg [OPTIONS] <pattern> --files [PATH...]
```

The point of `rg --files` is that it doesn't accept a pattern. You're passing `.`, which is interpreted as a file path to the current working directory. Which, I believe, explains the behavior you're seeing...

---

_Label `invalid` added by @BurntSushi on 2024-05-26 12:27_

---

_Closed by @BurntSushi on 2024-05-26 12:27_

---

_Comment by @fabiospampinato on 2024-05-26 12:45_

Aaaaaah ok, thanks for explaining 👍 

Should it not accept a pattern though? 🤔 if it doesn't accept a pattern (even if it doesn't really do anything with it) then this workflow kinda breaks:

1. running a normal `rg` command
2. appending "--files" to the previous command to check if the file you expected to be searched into was actually searched into.

Requiring the user to delete the pattern when passing the "--files" flag seems kinda arbitrary/unexpected to me.

---

_Comment by @BurntSushi on 2024-05-26 12:49_

I don't understand why `--files` would take a pattern as an input and then not use it. You'll need to delete the pattern.

And even if I agreed with you that it should accept a pattern, it would be a giant breaking change. So it's a non-starter. It's not like it's just some optional thing it can ignore if it's there and be fine if it isn't. It's a positional argument. So ripgrep would have to always treat the first positional argument to `--files` as a pattern.

---

_Comment by @fabiospampinato on 2024-05-26 12:51_

I guess the idea is that it would accept a pattern for consistency with other ways of running the program. I'm not sure if every other non-trivial way of running `rg` accepts a pattern though so maybe that argument is not that strong.

Probably not worth breaking the world anyway 👍

---
