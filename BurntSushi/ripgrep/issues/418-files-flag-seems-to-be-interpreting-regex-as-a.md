```yaml
number: 418
title: "--files flag seems to be interpreting regex as a filename (or docs aren't completely clear?)"
type: issue
state: closed
author: elirnm
labels:
  - question
  - doc
assignees: []
created_at: 2017-03-23T03:49:08Z
updated_at: 2017-03-31T19:56:43Z
url: https://github.com/BurntSushi/ripgrep/issues/418
synced_at: 2026-01-12T16:13:21Z
```

# --files flag seems to be interpreting regex as a filename (or docs aren't completely clear?)

---

_@elirnm_

I'm on Windows 7, and running ripgrep with the --files flag seems to be treating my search regex as a filename and then throwing an error.

So ```rg -w Bob``` works properly and produces matches, but ```rg -w Bob --files``` produces ```Bob: The system cannot find the file specified. (os error 2)``` and exits. If I run --files without the actual search term, it works and prints the files to be searched. This happens in both cmd.exe and Powershell.

It occurs to me now that this might be expected behavior, since the search term shouldn't affect the output when rg is run with --files, but if so that should probably be documented in the help page and/or produce a more useful error. This seems implied by the synopsis in https://github.com/BurntSushi/ripgrep/blob/master/doc/rg.1.md, but that doesn't appear in the command line --help output and doesn't appear in the documentation for the --files flag.

Thanks for the great work on ripgrep.

---

_Renamed from "--files flag seems to be interpreting regex as a filename" to "--files flag seems to be interpreting regex as a filename (or docs aren't completely clear?)" by @elirnm on 2017-03-23 03:51_

---

_Comment by @BurntSushi on 2017-03-23 10:50_

@elirnm The behavior you're seeing is correct. `--files` prints file paths that *would* be searched, but doesn't actually perform the search, so specifying a pattern doesn't really make any sense. The top of the output of `rg -h`, `rg --help` and `man rg` all include this section:

```
    rg [OPTIONS] <pattern> [<path> ...]
    rg [OPTIONS] [-e PATTERN | -f FILE ]... [<path> ...]
    rg [OPTIONS] --files [<path> ...]
    rg [OPTIONS] --type-list
```

Could you suggest something that would make documentation clearer? (Note the third variant above.)

---

_Label `doc` added by @BurntSushi on 2017-03-23 10:57_

---

_Label `question` added by @BurntSushi on 2017-03-23 10:57_

---

_Comment by @elirnm on 2017-03-23 17:51_

Hmm, `rg --help` doesn't look like that for me. All I get that talks about usage is
```
USAGE:ripgrep [OPTIONS] <pattern> [--] [path]...
```
which isn't even correct because `ripgrep` doesn't trigger anything. I'm using 0.5.0.

Exploring a bit, I get the output you gave when I enter `rg` without any options, but when I include `-h` or `--help` I don't get that.

![capture](https://cloud.githubusercontent.com/assets/19817004/24262200/9841ad6a-0fb6-11e7-8e15-67ab4cabecf3.PNG)

---

_Comment by @BurntSushi on 2017-03-23 17:54_

That's... interesting. I was using an older version when I responded. It looks like ripgrep's argv parser changed behavior recently? cc @kbknapp

---

_Comment by @kbknapp on 2017-03-23 21:35_

Interesting. If so it'd be a bug. I'll investigate when I get home. Do you know which version of clap got compiled with it, or if the `Cargo.lock` was removed before building?

---

_Comment by @BurntSushi on 2017-03-23 21:39_

@kbknapp ripgrep 0.4.0 was using clap 2.19.3 and ripgrep 0.5.0 is using 2.21.1. (There were some interesting changes I needed to make for the upgrade to work: https://github.com/BurntSushi/ripgrep/commit/95bc678403b63d98b717636e79fdfbabc8949c8a)

---

_Comment by @kbknapp on 2017-03-24 00:42_

Ok I'll look into it and put out a fix, it would be a major regression to slip by all the tests! Thanks for the ping!

---

_Comment by @kbknapp on 2017-03-24 03:57_

I've got a PR in to fix this and have added a regression test to ensure it doesn't pop up. Once the PR merges i'll put out v2.22.1

---

_Comment by @kbknapp on 2017-03-24 16:10_

The new version is out which fixes this regression

---

_Comment by @BurntSushi on 2017-03-30 16:37_

The latest version of clap sadly does not fix this issue. ripgrep uses a template in addition to a custom usage, which I don't think is accounted for in the regression test in https://github.com/kbknapp/clap-rs/pull/916.

See also #426

I'll see about fixing clap when I get chance.

---

_Comment by @BurntSushi on 2017-03-31 19:56_

Closed in #429

---

_Closed by @BurntSushi on 2017-03-31 19:56_

---
