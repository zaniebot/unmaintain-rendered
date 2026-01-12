```yaml
number: 733
title: "[WIP] Hgignore support"
type: pull_request
state: closed
author: kalekseev
labels: []
assignees: []
base: master
head: hgignore
created_at: 2018-01-06T18:07:20Z
updated_at: 2018-07-21T17:48:54Z
url: https://github.com/BurntSushi/ripgrep/pull/733
synced_at: 2026-01-12T18:23:13Z
```

# [WIP] Hgignore support

---

_@kalekseev_

Hi @BurntSushi, last time you said that you would accept hg support with minimal feature set support https://github.com/BurntSushi/ripgrep/issues/6/ so during the holidays I refactored my original pull request https://github.com/BurntSushi/ripgrep/pull/477.

Now globset is unchanged, everything live in hgignore module therefore current code shouldn't be affected by merging in hgignore support.

The places that I think should be fixed before merge are marked by FIXME, please review the code and tell me what do you think, is this the right direction to move?

---

_@kalekseev reviewed on 2018-01-07 13:02_

---

_Review comment by @kalekseev on `ignore/src/hgignore.rs`:397 on 2018-01-07 13:02_

Can anyone describe what leading slash mean in regular glob or especially in hgignore? I can't find a case that would match glob with leading slash in hgignore, maybe just skip them?

---

_Comment by @BurntSushi on 2018-01-07 14:08_

@kalekseev I just wanted to quickly chime in and say thanks so much for this PR! It might take me a bit to review though. :)

---

_@BurntSushi reviewed on 2018-01-07 14:09_

---

_Review comment by @BurntSushi on `ignore/src/hgignore.rs`:397 on 2018-01-07 14:09_

I think `man gitignore` says it best:

> A leading slash matches the beginning of the pathname. For example, "/*.c" matches "cat-file.c" but not "mozilla-sha1/sha1.c".

It other words, it's a form of an anchor.

---

_@BurntSushi reviewed on 2018-01-07 14:09_

---

_Review comment by @BurntSushi on `ignore/src/hgignore.rs`:397 on 2018-01-07 14:09_

(I don't know whether it's a thing in `hgignore` or not. It seems easy enough to test though?)

---

_@kalekseev reviewed on 2018-01-07 18:18_

---

_Review comment by @kalekseev on `ignore/src/hgignore.rs`:397 on 2018-01-07 18:18_

> I don't know whether it's a thing in hgignore or not.

```
$ cat > .hgignore
 syntax: glob
 /foo
 bar

$ touch foo
$ touch bar
$ hg status
 ? .hgignore
 ? foo

$ mkdir xxx
$ touch xxx/foo
$ hg status
 ? .hgignore
 ? foo
 ? xxx/foo
```

I've tested it and haven't found a case where leading slash would match anything, I believe git uses leading slash because standard glob doesn't support leading slash at all. That's why I assumed that we can skip such rules.

Globs in .hgignore are not rooted if you want rooted glob you should use `glob:`, from the `hg help patterns`

```
To use an extended glob, start a name with "glob:". Globs are rooted at
    the current directory; a glob such as "*.c" will only match files in the
    current directory ending with ".c".
```

---

_Comment by @kalekseev on 2018-04-22 08:10_

Hi @BurntSushi, I will have a couple of holidays at the beginning of May and I want to finish this pr, could you briefly review it or just comment what do you think about these two places:
 
1) Here I need the path https://github.com/BurntSushi/ripgrep/pull/733/files#diff-90bb8ac326bac2bf846c8c831a2abba9R180 as in Candidate 
 https://github.com/BurntSushi/ripgrep/pull/733/files#diff-258f99315c5f915cf0fdea294e12a8efR480 should I copy methods from globset::pathutils into ignore::pathutils to bulid that path?

2) We have to skip lines with invalid regex. I use regexset and I think maybe we can build regex in `add_regex_line` method to be sure that it's valid or this is too bad for performance? 
https://github.com/BurntSushi/ripgrep/pull/733/files#diff-90bb8ac326bac2bf846c8c831a2abba9R369

---

_Comment by @ngoldbaum on 2018-04-24 18:49_

It looks like you're mostly testing hgignore files that only have glob patterns. How are you dealing with regex patterns? What about differences between the python regex engine used by mercurial and rust's regex engine?

---

_Comment by @ngoldbaum on 2018-04-24 18:54_

For more test cases it may be worth looking at mercurial's own test suite:

https://www.mercurial-scm.org/repo/hg/file/tip/tests/test-hgignore.t

---

_Comment by @kalekseev on 2018-04-24 19:07_

Yeah I will add more testcases with regexes, right now I just copied tests from gitignore.

> What about differences between the python regex engine used by mercurial and rust's regex engine?

I was orienting on this @BurntSushi comment https://github.com/BurntSushi/ripgrep/pull/477#pullrequestreview-37025693

> Surely the regex syntax supported by mercurial is not in precise correspondence with Rust's regex engine. Is the plan to just emit an error message and suffer wontfix bug reports for eternity? (I say this with some snark as the maintainer, but I suppose there isn't a good alternative.)

This is why I'm asking advice about how to check regex before adding it to regexset. I want to check if it valid rust regexp and if not print debug message about that line.

---

_Comment by @kalekseev on 2018-05-07 07:12_

If anyone subscribed to that pr then please test current version with your mercurial setup.
```
$ git clone git@github.com:kalekseev/ripgrep.git
$ cd ripgrep
$ git checkout hgignore
$ cargo build --release
$  cp target/release/rg ~/.local/bin/rg # change to your PATH dir
```
Things that are not implemented (skipped by ripgrep):
 - Extended mercurial patterns: path, rootfilesin, listfile, listfile0, include, subinclude
 - python specific regexes (that are not valid rust regexes)


---

_Comment by @BurntSushi on 2018-07-21 17:48_

@kalekseev I hate to close this out, and I'm very appreciative of your hard work on this feature, but after thinking about this for a long time, I no longer believe its viable for ripgrep to support Mercurial. I left a longer explanation here: https://github.com/BurntSushi/ripgrep/issues/6#issuecomment-406812551

---

_Closed by @BurntSushi on 2018-07-21 17:48_

---
