```yaml
number: 284
title: "[QUESTION/SUGGESTION] Does/should ripgrep support ag/ack's -g flag for searching filenames?"
type: issue
state: closed
author: YPCrumble
labels:
  - doc
assignees: []
created_at: 2016-12-21T15:27:14Z
updated_at: 2017-04-02T20:38:36Z
url: https://github.com/BurntSushi/ripgrep/issues/284
synced_at: 2026-01-12T16:13:21Z
```

# [QUESTION/SUGGESTION] Does/should ripgrep support ag/ack's -g flag for searching filenames?

---

_@YPCrumble_

Per the ack documentation:

> -g PATTERN Print files where the relative path + filename matches PATTERN.  This option can be combined with --color to make it easier to spot the match.

I've searched the documentation and can't find this feature. Is this something that you'd be interested in for ripgrep? I'd be happy to submit a PR for it - I'm not a Rust expert but would love to jump in.

Thanks for writing such a great search tool!

---

_Comment by @BurntSushi on 2016-12-21 15:38_

Dupe of #193, #54, #75, #91, #48 

> I've searched the documentation and can't find this feature.

Three things are clear to me:

1. This is a desirable feature.
2. It exists in ripgrep, but under a different name from ag.
3. (2) is hard to discover.

Could you help me figure out how to make this feature more discoverable?

---

_Label `doc` added by @BurntSushi on 2016-12-21 15:38_

---

_Comment by @YPCrumble on 2016-12-21 15:40_

@BurntSushi :) I was _just_ typing up the nuance in one of those issues. I don't think the -g flag works the way I'm looking for (how it works using ag or ack). Let me know if I can explain further?

---

_Comment by @BurntSushi on 2016-12-21 15:41_

@YPCrumble Did you click on the duplicate links? The `-g` flag applies globs to file paths for filtering. The `--files` flag modifies what ripgrep actually prints.

---

_Comment by @YPCrumble on 2016-12-21 15:41_

@BurntSushi I'm going to create a quick repo to explain

---

_Comment by @BurntSushi on 2016-12-21 16:01_

@YPCrumble Have you tried the `--files` flag yet?

```
[andrew@Serval 193] mkdir stories
[andrew@Serval 193] echo 'Once upon a time' > stories/story.txt
[andrew@Serval 193] rg -g 'story*' --files
stories/story.txt
```

Let's try to keep discussion on one issue.

---

_Comment by @YPCrumble on 2016-12-21 16:35_

@BurntSushi would you take a look at https://github.com/YPCrumble/rg_example - I added my questions in the `README.md`. Thank you for taking the time! Hoping I can help you resolve all confusion at a minimum by adding clarity to the docs :).

---

_Comment by @YPCrumble on 2016-12-21 16:36_

@BurntSushi also I'm by no means an expert I apologize in advance if this is just confusion or a silly error on my end!

---

_Comment by @BurntSushi on 2016-12-21 16:48_

@YPCrumble Are you not quoting the values you give to `-g`? If you don't quote them, your shell will expand them and probably result in behavior you didn't expect.

---

_Comment by @YPCrumble on 2016-12-21 17:07_

@BurntSushi OK now I got it. This solves my issue. What do you think about either adding either an example of how to search file/path names OR an alias (ala your -p flag) that would add the -g --files and the quotes around the searched term?

I would be happy to try implementing the alias if that's interesting. I would suggest using the -P/--path flag as -p is already taken. My ideal would be to use the same -g flag as ag/ack or use -P for --pretty and -p for --path but seems like it would be a breaking (and therefore undesirable) change.

For updating the docs option I suggest adding an example to both `-g` and `--files`, like so:

**-g**
> Include or exclude files for searching that match the given glob.  This always overrides any other ignore logic.  Multiple  glob  flags  may  be used.  Globbing rules match .gitignore globs.  Precede a glob with a '!' to exclude it. Values given to -g must be quoted or your shell will expand them and result in unexpected behavior. Combine with the `--files` flag to return matched filenames (i.e., to replicate ack/ag's -g flag). For example: `rg -g "<pattern>" --files`.

**--files**
> Print each file that would be searched (but don't search). Combine with the `-g` flag to return matched paths, for example: `rg -g "<pattern>" --files`.

Thank you for your patience!

---

_Comment by @BurntSushi on 2016-12-21 17:21_

I like your documentation suggestions. :-)

I'd like to hold off on aliases for now since I think it makes the tool more confusing to use overall. I made a special exception for `-p` because it was useful to add quickly if you piped your results through a pager but still wanted the nice colored output. (If I could go back in time, I probably wouldn't have added this alias.)

Here is something that might be helpful to you though:

```
$ cat $HOME/bin/rgg
#!/bin/sh

exec rg --files -g "$1"
```

Then you can just do `rgg 'file*'`. :-)

---

_Comment by @YPCrumble on 2016-12-21 17:22_

Cool, will put into a PR!

---

_Comment by @BurntSushi on 2016-12-21 17:29_

@YPCrumble Yay thanks! Don't forget to modify `doc/rg.1.md` and run `cd doc && ./convert-to-man`. If you don't have `pandoc` that's OK, I can run the conversion script.

---

_Closed by @BurntSushi on 2016-12-22 12:21_

---

_Comment by @jmatraszek on 2017-04-02 20:20_

Hi @BurntSushi and @YPCrumble,
sorry for commenting on so old thread, but I found one case that this is not working (or more precisely: is working differently then I expect;)).

First of all, for me the whole point of using `--files -g` (or `ag`'s `-g`) is to use it as a `g:unite_source_rec_async_command` (if anyone is not familiar with Vim's Unite plugin: it lets quickly open a file that matches given pattern and uses this command to collect matches).

The workaround proposed in this thread works quite well, except one thing: the glob does not recursively go into directories (ergo: it's not usable in Unite). One way to fix it is to enable recursive globs by `shopt -s globstar` and then use it like that: `rgg **/*'PATTERN_FROM_UNITE'*`. However, this works terribly slow. For example a query that returns 20 matching files (in a directory that contains in all its subdirectories 1430 files combined) takes 1-2 seconds, while `ag` runs for 0.1 second.

Is there a way to benefit from ripgrep's raw speed in Unite, or `ag` is the only way to go? Or am I missing something and I've been using this wrong?

My `rgg`:

```
#!/bin/sh

exec rg --follow --color=never --no-heading --hidden --files -g *$1*
```

Thanks!

---

_Comment by @BurntSushi on 2017-04-02 20:37_

@jmatraszek It's too hard for me to reason about your desired behavior without examples. Could you craft some specific examples using *plain* `rg` (no aliases) and *plain* `ag` on this repo? https://github.com/BurntSushi/linux Thanks.

---

_Comment by @BurntSushi on 2017-04-02 20:38_

@jmatraszek Could you also please open a new issue? Bumping old closed issues is a sure fire way for your feedback to fall off my radar. :-(

---
