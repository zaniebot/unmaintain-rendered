```yaml
number: 2744
title: Include ---no-ignore-files in --unrestricted option?
type: issue
state: closed
author: rieje
labels:
  - wontfix
assignees: []
created_at: 2024-03-01T21:48:10Z
updated_at: 2025-10-11T01:57:09Z
url: https://github.com/BurntSushi/ripgrep/issues/2744
synced_at: 2026-01-12T16:13:24Z
```

# Include ---no-ignore-files in --unrestricted option?

---

_@rieje_

I thought `-uuu` would essentially search through everything ignoring all ignores, but it did not get that behavior until I realized I have the following in my rg config:

    --ignore-file
    /home/rieje/.config/rg/general.rgignore

I was hoping for an easy-to-remember one-argument to exclude all filters including ignore files (especially with all the `--no-ignore-*` options and multiple `-u` options). It seems currently `-uuu --no-ignore-files` should do that, but would it make sense to have a `-uuuu` to do the equivalent or a `--no-ignore-all`? That feels more intuitive.



---

_Comment by @BurntSushi on 2024-03-02 01:18_

The man documents `-u` as being equivalent to `--no-ignore`, and also from the man page:

```
       --no-ignore
           When set, ignore files such as .gitignore, .ignore and .rgignore
           will  not  be  respected. This implies --no-ignore-dot, --no-ig‐
           nore-exclude, --no-ignore-global, --no-ignore-parent  and  --no-
           ignore-vcs.

           This  does  not  imply --no-ignore-files, since --ignore-file is
           specified explicitly as a command line argument.

           When given only once, the -u/--unrestricted flag is identical in
           behavior to this flag and can be considered an  alias.  However,
           subsequent -u/--unrestricted flags have additional effects.

           This flag can be disabled with --ignore.
```

So basically, `--no-ignore` disables all ignore files except for `--ignore-file`. The thinking there is `--ignore-file` is being explicitly specified on the CLI (or config in this case), and so it shouldn't be overridden.

Unfortunately I don't think I'm willing to add a fourth `-u` or a `--no-ignore-all`. Both of those things seem like pretty incredible complications. There are already way too many `--no-ignore-*` flags, and having both `--no-ignore` and `--no-ignore-all` just seems pretty whacky to me unfortunately.

I'm not quite sure how to address your use case unfortunately. I'm tempted to make `--no-ignore` imply `--no-ignore-files` (or at least, to any such `--ignore-file` flags that come before `--no-ignore`), but I'm worried about that being a compatibility hazard for folks that rely on `--no-ignore` _not_ disabling `--ignore-file`.

---

_Label `question` added by @BurntSushi on 2024-03-02 01:18_

---

_Closed by @BurntSushi on 2025-10-11 01:57_

---

_Label `question` removed by @BurntSushi on 2025-10-11 01:57_

---

_Label `wontfix` added by @BurntSushi on 2025-10-11 01:57_

---
