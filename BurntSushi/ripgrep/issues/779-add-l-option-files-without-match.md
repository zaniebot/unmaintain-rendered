```yaml
number: 779
title: add -L option (--files-without-match)
type: issue
state: closed
author: hyperknot
labels:
  - invalid
  - question
assignees: []
created_at: 2018-02-07T14:57:11Z
updated_at: 2018-02-07T15:39:53Z
url: https://github.com/BurntSushi/ripgrep/issues/779
synced_at: 2026-01-12T16:13:22Z
```

# add -L option (--files-without-match)

---

_@hyperknot_

#### What version of ripgrep are you using?
0.7.1

#### What operating system are you using ripgrep on?
macOS 10.13.3

#### Describe your question, feature request, or bug.
Feature request: add support for -L or --files-without-match.

This happens to be the same switch across grep, ack, and ag.

I didn't find any such function in ripgrep, at least not in the documentation.

---

_Comment by @BurntSushi on 2018-02-07 14:58_

It already exists:

```
$ rg -h | rg files-without-match
        --files-without-match               Only print the paths that contain zero matches.
```

---

_Label `invalid` added by @BurntSushi on 2018-02-07 14:58_

---

_Label `question` added by @BurntSushi on 2018-02-07 14:58_

---

_Closed by @BurntSushi on 2018-02-07 14:58_

---

_Comment by @hyperknot on 2018-02-07 15:21_

I looked for "not", "don't", "invert" words but I didn't find this one. It's ironic as I got the `--files-without-match` from grep manual.
So any reason why not to support -L as well?

---

_Comment by @BurntSushi on 2018-02-07 15:26_

@hyperknot Because it's already used to tell ripgrep to follow symbolic links, which is consistent with `find`'s flag for the same behavior. This is also documented:

```
$ rg -h | rg -e -L
    -L, --follow                            Follow symbolic links.
```

---

_Comment by @BurntSushi on 2018-02-07 15:27_

`ag` uses `-L` for `--files-without-match`, and also has a `-f/--follow` flag, but `-f` is used in ripgrep (and grep) for searching for many patterns from a file, which is a feature that `ag` does not have.

---

_Comment by @BurntSushi on 2018-02-07 15:29_

grep uses `-R` for following symbolic links during recursive search, where `-r` is the normal variant. In hindsight, it is plausible that ripgrep should have used `-R` as well (although the lowercase `-r` variant doesn't make sense since that's ripgrep's default behavior), which would have left `-L` open for `--files-without-match`. However, that is *certainly* not happening now.

My feeling is that `--files-without-match` isn't used too often, and adding a short flag for it other than `-L` would confuse matters even more. But if someone has a better idea, I'm all ears.

---

_Comment by @hyperknot on 2018-02-07 15:31_

I see, I agree there shouldn't be a new short switch then. 

---

_Comment by @hyperknot on 2018-02-07 15:31_

Maybe just add the words "invert" or "not" or something like this to the help, as the other options use these words for negation.

---

_Comment by @BurntSushi on 2018-02-07 15:39_

@hyperknot I can do that. It's in my local changes which should get pushed out soon and will be in the next release.

---
