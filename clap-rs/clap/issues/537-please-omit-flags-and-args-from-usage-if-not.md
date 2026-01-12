```yaml
number: 537
title: "Please omit \"[FLAGS]\" and \"[ARGS]\" from usage if not applicable"
type: issue
state: closed
author: joshtriplett
labels:
  - C-enhancement
assignees: []
created_at: 2016-06-23T21:43:10Z
updated_at: 2020-04-12T10:09:27Z
url: https://github.com/clap-rs/clap/issues/537
synced_at: 2026-01-12T16:14:09Z
```

# Please omit "[FLAGS]" and "[ARGS]" from usage if not applicable

---

_@joshtriplett_

The automatically generated usage message includes placeholders for "[FLAGS]" and "[ARGS]", even when every flag or arg is already shown directly in the usage message.  For instance, for the example in https://github.com/kbknapp/clap-rs/issues/533#issuecomment-227947296 , the usage string looks like this:

```
USAGE:
    rebase [FLAGS] <onto|--interactive> [ARGS]
```

However, this command doesn't take any flags other than `--interactive` (and `--help`), and doesn't take any args other than `onto`.  So ideally the usage message should look like:

```
USAGE:
    rebase <onto|--interactive>
```


---

_Comment by @kbknapp on 2016-06-24 00:21_

It's detecting `--help` or `--version` for "flags" but what I could do is omit the `[FLAGS]` if the only non-required args are `--help` or `--version` as I agree...it's not super useful to keep the flags tag there just for those two.


---

_Label `T: enhancement` added by @kbknapp on 2016-06-24 00:22_

---

_Label `P4: nice to have` added by @kbknapp on 2016-06-24 00:22_

---

_Label `C: args` added by @kbknapp on 2016-06-24 00:22_

---

_Label `D: easy` added by @kbknapp on 2016-06-24 00:22_

---

_Label `W: 2.x` added by @kbknapp on 2016-06-24 00:22_

---

_Label `C: usage strings` added by @kbknapp on 2016-06-24 00:22_

---

_Added to milestone `2.7.0` by @kbknapp on 2016-06-24 00:26_

---

_Referenced in [clap-rs/clap#533](../../clap-rs/clap/issues/533.md) on 2016-06-24 00:33_

---

_Comment by @joshtriplett on 2016-06-24 00:47_

That sounds reasonable.

`[ARGS]` similarly shouldn't appear if all the arguments are required or otherwise part of a group that already displays them.


---

_Comment by @kbknapp on 2016-06-24 01:20_

There was a recent commit that removes `[ARGS]` when it's only a single arg (required or not), but it currently doesn't do any determination as to whether or not all args are required, which makes sense to me so that's do-able.


---

_Comment by @kbknapp on 2016-06-24 03:56_

This is now implemented in a local branch. Pushing the PR shortly.


---

_Closed by @homu on 2016-06-24 05:00_

---

_Label `C: usage strings` added by @pksunkara on 2020-04-12 10:09_

---

_Label `C: usage string parser` removed by @pksunkara on 2020-04-12 10:09_

---
