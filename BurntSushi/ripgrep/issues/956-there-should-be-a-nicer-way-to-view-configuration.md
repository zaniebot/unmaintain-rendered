```yaml
number: 956
title: "There should be a nicer way to view configuration dot files without seeing ./git/* files"
type: issue
state: closed
author: dylan-chong
labels:
  - question
assignees: []
created_at: 2018-06-20T09:25:15Z
updated_at: 2018-06-20T22:03:11Z
url: https://github.com/BurntSushi/ripgrep/issues/956
synced_at: 2026-01-12T16:13:22Z
```

# There should be a nicer way to view configuration dot files without seeing ./git/* files

---

_@dylan-chong_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX


#### How did you install ripgrep?

homebrew

#### What operating system are you using ripgrep on?

mac 10.13.4

#### Describe your question, feature request, or bug.

Currently RG ignores files like .gitignore, and other configuration files that would be good to search by default.

The `--hidden` option enables searching these files but then we get a whole lot of rubbish from files like this:

```
.git/objects/49/1b75aa65a8dae581d366e165e298dc17a4dcb7
.git/objects/2e/79090eb54583b27f2a4943561d8f3e24778e18
.git/objects/47/e35411c0bb53aa89b0ae2605869df78ea27825
.git/objects/8b/19d74e3ae00274c088acb5e24f2eeb1b41a124
.git/objects/8b/ac8c6bc6092a17cabdad6681285757bcea4957
...
```

I'm not sure what the best solution would be here, but as an idea there could be an additional commandline option  that shows only reasonable hidden files, like configuration dot files.



---

_Comment by @BurntSushi on 2018-06-20 10:55_

> but as an idea there could be an additional commandline option that shows only reasonable hidden files, like configuration dot files.

This presupposes the ability of ripgrep to detect such things. I don't see how.

In any case, it's not clear to me what you're expecting here, because you haven't told me what you've tried. There are a _variety_ of ways to refine your search.

Starting with:

```
$ mkdir .git
$ echo test > .good
$ echo test > .git/bad
$ rg --files
$ rg --files --hidden
.good
.git/bad
```

There are lots of ways to "hide .git but show other things":

First, with an explicit override to ignore `.git`:

```
$ rg --files -g '!.git' --hidden
.good
```

Second, by whitelisting the files you want in your search via an `.ignore` file:

```
$ echo '!.good' > .ignore
$ rg --files
.good
```

Third, by blacklisting `.git`:

```
$ echo '.git' > .ignore
$ rg --files
$ rg --files --hidden
.ignore
.good
```

And fourth, any combination of (2) or (3) using the `--ignore-file` flag, which lets you keep a global list of ignore rules that can be given to every ripgrep invocation (e.g., via an alias or a config file).

Can you say why none of these methods meets your criteria?

---

_Label `question` added by @BurntSushi on 2018-06-20 12:59_

---

_Comment by @dylan-chong on 2018-06-20 22:01_

Oops, sorry I forgot to mention what I was trying to use previously. I was trying to use something very similar to `-g '!.git' --hidden`. I'm not sure why exactly I was having some problems (with fzf.vim), but it seems to be working now... Thanks so much for the help it!

EDIT: It may have been partially caused by using " not ' around `!.git`

---

_Closed by @dylan-chong on 2018-06-20 22:01_

---
