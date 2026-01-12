```yaml
number: 373
title: "./.gitignore: line 2: invalid use of **; must be one path component"
type: issue
state: closed
author: azhang
labels:
  - question
assignees: []
created_at: 2017-02-21T19:39:48Z
updated_at: 2019-01-24T01:00:33Z
url: https://github.com/BurntSushi/ripgrep/issues/373
synced_at: 2026-01-12T16:13:21Z
```

# ./.gitignore: line 2: invalid use of **; must be one path component

---

_@azhang_

`**/something` is valid .gitignore syntax, but rg prints an error


> A leading "\*\*" followed by a slash means match in all directories. For example, "\*\*/foo" matches file or directory "foo" anywhere, the same as pattern "foo". "\*\*/foo/bar" matches file or directory "bar" anywhere that is directly under directory "foo". (https://git-scm.com/docs/gitignore)

---

_Comment by @BurntSushi on 2017-02-21 20:53_

Could you please provide a reproducible example? I can't reproduce it using the information you've given:

```
[andrew@Cheetah ripgrep-373] tree -a
.
├── a
│   └── something
├── foo
└── .gitignore

1 directory, 3 files
[andrew@Cheetah ripgrep-373] cat .gitignore
**/something
[andrew@Cheetah ripgrep-373] cat a/something
test
[andrew@Cheetah ripgrep-373] cat foo
test
[andrew@Cheetah ripgrep-373] rg test
foo
1:test
[andrew@Cheetah ripgrep-373] rg test --debug
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        't',
        'e',
        's',
        't'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(test)], limit_size: 250, limit_class: 10 }
DEBUG:globset: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG:ignore::walk: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG:ignore::walk: ignoring ./a/something: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "**/something", actual: "**/something", is_whitelist: false, is_only_dir: false })))
foo
1:test

```

---

_Label `question` added by @BurntSushi on 2017-02-21 20:54_

---

_Comment by @BurntSushi on 2017-03-12 21:03_

Closing due to inactivity. Please feel free to reopen with a repro. Thanks!

---

_Closed by @BurntSushi on 2017-03-12 21:03_

---

_Comment by @millerjs on 2017-03-29 15:27_

@BurntSushi, ran into the same issue and isolated it with a 

```bash
mkdir scratch
cd scratch
echo '**.txt' > .gitignore
rg pattern
```

receiving a `./.gitignore: line 1: invalid use of **; must be one path component`.

---

_Comment by @BurntSushi on 2017-03-29 15:36_

Interesting. That isn't actually enough to reproduce the full issue, because it doesn't demonstrate that `**.txt` is actually recognized by `git`:

```
$ git init
Initialized empty Git repository in /tmp/ripgrep-373/.git/
$ touch hi.txt
$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        hi.txt

nothing added to commit but untracked files present (use "git add" to track)
$ echo '**.txt' > .gitignore
$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        .gitignore

nothing added to commit but untracked files present (use "git add" to track)
```

But according to the specification that I actually implemented (see `man gitignore`), the `**.txt` construction is actually invalid:

```
Two consecutive asterisks ("**") in patterns matched against full pathname may
have special meaning:

  * A leading "**" followed by a slash means match in all directories. For
    example, "**/foo" matches file or directory "foo" anywhere, the same as
    pattern "foo". "**/foo/bar" matches file or directory "bar" anywhere that
    is directly under directory "foo".

  * A trailing "/**" matches everything inside. For example, "abc/**" matches
    all files inside directory "abc", relative to the location of the
    .gitignore file, with infinite depth.

  * A slash followed by two consecutive asterisks then a slash matches zero or
    more directories. For example, "a/**/b" matches "a/b", "a/x/b", "a/x/y/b"
    and so on.

  * Other consecutive asterisks are considered invalid.
```

I'm not sure what to do here. Either `git` has a bug in their specification or their implementation is non-conforming. If it's the former, then ripgrep should probably implement `**.txt`. If it's the latter, then ripgrep should continue to recognize `**.txt` as invalid.

---

_Reopened by @BurntSushi on 2017-03-29 15:46_

---

_Comment by @derimagia on 2017-03-31 15:56_

Had the same issue, I always thought git used `fnmatch` for this. I looked through git and see that actually moved to use `wildmatch` (https://github.com/davvid/wildmatch) in recent versions. Without looking too much into it I think this may be the reason for it.

`The wildmatch extension allows ** to match / when WM_PATHNAME is present. This gives the practical benefit of being able to match all subdirectories of a path by using ** and reserves the use of the single-asterisk * character for matching within path components.`

---

_Comment by @BurntSushi on 2017-03-31 16:02_

> The wildmatch extension allows ** to match / when WM_PATHNAME is present. This gives the practical benefit of being able to match all subdirectories of a path by using ** and reserves the use of the single-asterisk * character for matching within path components.

That particular piece seems fine. It looks like that's just stating support for `**` itself. In particular, I don't think `fnmatch` supports the `**` construction at all. The issue is that particular uses of `**` are seemingly forbidden by the spec in `man gitignore`, but those forbidden uses still seem to actually work.

---

_Comment by @derimagia on 2017-03-31 16:23_

Not sure about that one reading https://github.com/davvid/wildmatch/blob/350d6cde61cc901d4d22d60878dab445675fdd02/wildmatch/wildmatch.h#L55 

But agreed, it's a documentation issue. But a quick search shows this sadly has always been an issue with gitignore even before wildmatch

---

_Comment by @BurntSushi on 2017-03-31 16:40_

@derimagia Oh, I see, `WM_PATHNAME` is changing whether `**` matches `/` or not. e.g., in the pattern `*.txt`, according to `man gitignore`, `*` matches a `/`, but in `foo/*.txt`, `*` cannot match a `/`.

I still think this is unrelated to whether patterns like `**.txt` are allowed.

---

_Comment by @millerjs on 2017-04-03 03:20_

It looks like `*literal` [will be matched](https://github.com/git/git/blob/master/dir.c#L877) prior to a call to `wildmatch`. If it's `**literal`, it will [be passed to wildmatch](https://github.com/git/git/blob/master/dir.c#L884), where the first and second wildcards [will be consumed](https://github.com/git/git/blob/master/wildmatch.c#L84-L85), the `WM_PATHNAME` flag will not have been set, the rest of the`literal` text [consumed](https://github.com/git/git/blob/master/wildmatch.c#L154), and [a successful match will be returned](https://github.com/git/git/blob/master/wildmatch.c#L161).  

Side note, wildmatch was integrated in v1.8.2-rc0 and a prior tag (v1.8.1.6) reproduces the behavior of git to match `**.txt` to `hello.txt` so it seems it wasn't a bug introduced when wildmatch was integrated.

Looks like the spec and implementation are just inconsistent...?

---

_Comment by @flippidippi on 2017-04-07 15:09_

Having this issue also. If I take out the line `**.orig` then it fixes the issue. But from what it looks like `*.orig` should behave the same way and not throw the error correct?

---

_Comment by @BurntSushi on 2017-04-07 15:11_

> But from what it looks like `*.orig` should behave the same way and not throw the error correct?

If I had to guess, yes? But nobody can say for sure, since the specification says that `**.orig` is an invalid ignore pattern.

---

_Comment by @millerjs on 2017-04-08 19:24_

@BurntSushi imho it might be worth choosing consistency with implementation over spec in this case and making a prominent note of it

---

_Comment by @BurntSushi on 2017-04-08 20:05_

@millerjs What are the semantics? Who is going to reverse engineer `git`'s implementation? What if this is a bug in `git` and they decide to fix it?

---

_Comment by @BurntSushi on 2017-05-08 22:33_

I'm going to close this because the current behavior is the conservative choice, and being conservative allows us to be flexible in the future. In particular, I think there is either a specification bug or an implementation bug in git, and I don't know which one it is.

---

_Closed by @BurntSushi on 2017-05-08 22:33_

---

_Comment by @Yggdroot on 2017-06-07 05:16_

Could rg add an option to ignore this error? For example, ` --ignore-2asterisks-error`. I think it does no harm to rg.

---

_Comment by @BurntSushi on 2017-06-07 10:57_

> I think it does no harm to rg.

You can basically say that about any particular flag. But if I add `--ignore-2asterisks-error`---which is incredibly specific---then what's to stop a deluge of other similar flags like `--ignore-file-permission-error`?

@Yggdroot Could you please say more about why the `--no-messages` flag isn't sufficient for your use case?

---

_Comment by @Yggdroot on 2017-06-07 13:34_

Error message is necessary. Up to now, the`**.txt` is not considered to be an error by git.
If you insist on your idea, I have nothing more to say.

---

_Comment by @BurntSushi on 2017-06-07 13:39_

@Yggdroot Sorry, but I don't understand. Could you say more about why `--no-mesages` isn't sufficient?

---

_Comment by @Yggdroot on 2017-06-08 01:49_

I mean the **real** error should be reported, it's necessary to see the reason why the error occurs, while with `--no-mesages`, all the error messages are suppressed. 
But `**.txt` has not been considered to be an error by now.
Anyway, It's OK for me if rg does not support this specific scenario.

---

_Comment by @kenorb on 2018-04-28 00:13_

Same here.

Workaround:

    rg --files 2> /dev/null

---

_Comment by @BurntSushi on 2018-04-28 11:43_

Current master has a `--no-ignore-messages` that will suppress just these types of error messages. It will be in the next release.

---

_Comment by @dylan-chong on 2018-11-25 03:26_

I have sent an email to the Git mailing list requesting that the **.extension format be added to the documentation of Git ignore. I will update this issue when I have gotten a response.

FYI, if anybody is using FZF.vim with `rg --files [--no-ignore-messages]` the FZF window seems to be blank if there are `**.extension`lines and the Git ignore file. If the Git maintainers decide to document such a format, supporting it with ripgrep should solve the problem. If it does not, I will create a new issue in the future.

---

_Comment by @okdana on 2018-11-25 03:30_

>I have sent an email to the Git mailing list requesting that the **.extension format be added to the documentation of Git ignore.

It already has been. See the commit referenced in #1098

---

_Reopened by @BurntSushi on 2019-01-24 00:56_

---

_Closed by @BurntSushi on 2019-01-24 00:59_

---

_Comment by @BurntSushi on 2019-01-24 01:00_

This is now fixed for good on master. ripgrep recognizes previously invalid `**` patterns as two consecutive `*` patterns, in line with [git's updated spec](https://github.com/git/git/commit/627186d0206dcb219c43f8e6670b4487802a4921).

---
