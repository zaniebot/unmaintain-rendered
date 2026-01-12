```yaml
number: 830
title: "rg 0.8.1: regression in glob pattern matching"
type: issue
state: closed
author: mario-grgic
labels:
  - question
  - wontfix
assignees: []
created_at: 2018-02-22T20:38:11Z
updated_at: 2021-03-16T12:18:38Z
url: https://github.com/BurntSushi/ripgrep/issues/830
synced_at: 2026-01-12T16:13:22Z
```

# rg 0.8.1: regression in glob pattern matching

---

_@mario-grgic_

#### What version of ripgrep are you using?
```
$ rg --version
ripgrep 0.8.1 (rev c8e9f25b85)
+SIMD -AVX
```

#### What operating system are you using ripgrep on?

```
$ uname -a
Darwin Jupiter.local 17.4.0 Darwin Kernel Version 17.4.0: Sun Dec 17 09:19:54 PST 2017; root:xnu-4570.41.2~1/RELEASE_X86_64 x86_64
```

#### Describe your question, feature request, or bug.

I'm using ripgrep to search for files excluding certain directories. For example:

```
rg --files --no-ignore --hidden --follow -g '!{.git,node_modules}/*'
```

This used to exclude .git, and node_modules directories in current directory and all subdirectories in ripgrep 0.7.1. But ripgrep 0.8.1 lists the contents of .git and node_modules directories in subdirectories of current directory. 



---

_Comment by @BurntSushi on 2018-02-22 21:21_

I can't reproduce this. Could you please show the output of the command with the `--debug` flag as the issue template requested? Thanks.

---

_Comment by @mario-grgic on 2018-02-22 21:55_

The following script will create a test directory structure:

```sh
#!/bin/sh

mkdir -p test/a/b/c/
mkdir -p test/a/exclude
mkdir -p test/a/b/exclude
touch test/a/exclude/file1
touch test/a/b/exclude/file2
```

```
cd test
$ rg --debug --files --no-ignore --hidden --follow -g '!exclude/*'
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "exclude/*", re: "(?-u)^exclude/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([Literal('e'), Literal('x'), Literal('c'), Literal('l'), Literal('u'), Literal('d'), Literal('e'), Literal('/'), ZeroOrMore]) }
DEBUG/globset/globset/src/lib.rs:401: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Repeat {
    e: Literal {
        chars: [
            'z'
        ],
        casei: false
    },
    r: Range {
        min: 0,
        max: Some(
            0
        )
    },
    greedy: true
}
a/exclude/file1
a/b/exclude/file2
```

And with ripgrep version 0.7.1:

```
$ rg --debug --files --no-ignore --hidden --follow -g '!exclude/*'
DEBUG:globset: glob converted to regex: Glob { glob: "**/exclude/*", re: "(?-u)^(?:/?|.*/)exclude/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('e'), Literal('x'), Literal('c'), Literal('l'), Literal('u'), Literal('d'), Literal('e'), Literal('/'), ZeroOrMore]) }
DEBUG:globset: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG:grep::search: regex ast:
Repeat {
    e: Literal {
        chars: [
            'z'
        ],
        casei: false
    },
    r: Range {
        min: 0,
        max: Some(
            0
        )
    },
    greedy: true
}
DEBUG:ignore::walk: ignoring ./a/exclude/file1: Ignore(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "!exclude/*", actual: "**/exclude/*", is_whitelist: true, is_only_dir: false })))))
DEBUG:ignore::walk: ignoring ./a/b/exclude/file2: Ignore(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "!exclude/*", actual: "**/exclude/*", is_whitelist: true, is_only_dir: false })))))
```


---

_Comment by @BurntSushi on 2018-02-22 22:08_

@mario-grgic Much better, thank you. :-) I can reproduce that.

---

_Comment by @BurntSushi on 2018-02-22 22:25_

TL;DR - Use `-g '!exclude'` instead of `-g '!exclude/*'`.

OK, so I fear this change may have actually been intended. Specifically, this is a result of fixing #761, which was done in #762. Basically, the idea here is that if a glob contains a `/`, then a `*` in the glob *must not match a `/`*. In your case, you have a glob `exclude/*` (which contains a slash), which means it is specifically limited to matching things like `exclude/foo`, but specifically not `a/exclude/foo` or `a/b/exclude/foo` because that would imply an implicit `*` prefix that matches through a `/` character. As #762 notes, the `man gitignore` documentation specifically calls out exactly this example.

Given that the `-g/--glob` flag is documented to follow `gitignore` semantics and given that I believe this behavior is consistent with gitignore behavior, I think that this actually isn't a bug, and you were instead previously relying on the behavior of a bug. You can trivially work around this by using `-g '!exclude'` instead of `-g '!exclude/*'`, which I believe accomplishes the same thing.

It is plausible that we might want to use semantics other than `gitignore` for the `-g/--glob` flags, but that is itself a much larger change.

---

_Comment by @mario-grgic on 2018-02-22 22:51_

Ok, thank you for the clarification.

---

_Closed by @mario-grgic on 2018-02-22 22:51_

---

_Label `question` added by @BurntSushi on 2018-02-22 22:54_

---

_Label `wontfix` added by @BurntSushi on 2018-02-22 22:54_

---

_Comment by @tandrewnichols on 2018-03-14 02:28_

Is this documented anywhere? I just seriously wasted like 3 days trying to figure out why files were not being excluded. The man page only says "Precede a glob with a ! to exclude it." And basically every blog post/explanation I saw online about how to set up ripgrep used the `!somewhere/*` pattern.

---

_Comment by @BurntSushi on 2018-03-14 02:42_

> Is this documented anywhere?

Yes, in `man gitignore`. The man page for ripgrep says, "Globbing rules match .gitignore globs." in the documentation for the `-g/--glob` flag.

---

_Comment by @BurntSushi on 2018-03-14 02:48_

@tandrewnichols If there's a short FAQ entry that could be written about fzf setup, then that might be a good way to combat the existing false info. I don't use fzf though, so I can't/won't write those docs.

---

_Comment by @tandrewnichols on 2018-03-14 03:01_

Well . . . fine, but: a) even with that in the man, I'm not going to think to check some other tool's man page to understand how something works, b) it's an extra thing to run if my first hunch is "this is a problem with ripgrep", c) I don't know that it's common knowledge that gitignore HAS a man page . . . I didn't know that, d) the man for gitignore is dense enough that a tl;dr would be nice, e) most people assume they understand gitignore syntax already, so even with that note they won't think to check man gitignore, f) even if, barring all this, you actually look up the man for gitignore, you still have to understand what it means, and phrases like `Otherwise, Git treats the pattern as a shell glob suitable for consumption by fnmatch(3) with the FNM_PATHNAME flag` make that virtually untenable. I guess what I'm asking is, like, maybe one example of negating a file path would be nice. It could be as simple as:

```bash
# Exclude node_modules from a search
rg -g '!node_modules` banana
```

Per your last, I don't think there's anything specifically about fzf here. I mean, you're correct that the things I was reading online were about integrating ripgrep with fzf, and that definitely muddied the situation for me in terms of debugging, but I couldn't get glob exclusion working even with a `.ripgreprc` just in my shell.

---

_Comment by @tandrewnichols on 2018-03-14 03:05_

Maybe I'm projecting with that list and the average bash user _does_ think to check `man gitignore` in that case, idk. I know I'm no bash expert, but I wouldn't call myself a novice either. And probably the average user of this tool is using it specifically because they don't want to have to understand shell-y things, so a little pointer in this case seems like it could be useful (especially since some other analogous tools like grep have explicit `--exclude` and `--exclude-dir` flags so the concept of negating will be new in that case).

---

_Comment by @tandrewnichols on 2018-03-14 03:07_

And by the way, I mean this all as a positive critique because ripgrep is a great tool. It's the first "grep replacement" I've used that I've actually _used_ and stuck with. It really is excellent work.

---

_Comment by @BurntSushi on 2018-03-14 03:30_

Sure, what you're saying is reasonable, but docs for this sort of thing are hard. Even if I translated the gitignore docs and POSIX globbing into one place in ripgrep's man page, it isn't clear to me that that would have saved you any time. Fundamentally, this particular bug is a subtle corner case. Subtle enough that even I missed it. :-)

That's probably why the existing docs are so terse. But perhaps there is a middle ground. If you file another issue, perhaps with more examples or ideas of what to include, then it has a chance of getting acted upon. :-)

---

_Comment by @tandrewnichols on 2018-03-14 13:22_

I've thought more about it, specifically from a maintainer perspective, because I have some published tools as well, and documentation is always the worst part, and it's especially hard to keep up to date, but definitely the worst kind of documentation is stuff duplicated from elsewhere, so in that way, it makes sense not to spend effort duplicating docs that are elsewhere. A compromise that just came to me this morning is to modify (or add to) the man entry to say something like "See `man gitignore` for more on globstar patterns" which gives you slightly more to go on in terms of debugging without requiring much in the way of changes. If that sounds reasonable (and I have some time), I could look at working at PR for that.

I also just noticed this morning on twitter that you were the one that recently liked my post about ripgrep + fzf. Which explains a lot, because I was like, "I don't think I mentioned fzf, how did this guy know I was using fzf???" Haha, I get it now.

---

_Comment by @BurntSushi on 2018-03-14 13:32_

@tandrewnichols Hah! That sounds reasonable. We might also consider linking or copying this (which are docs for the globbing library that ripgrep uses): https://docs.rs/globset/0.3.0/globset/#syntax

> I also just noticed this morning on twitter that you were the one that recently liked my post about ripgrep + fzf. Which explains a lot, because I was like, "I don't think I mentioned fzf, how did this guy know I was using fzf???" Haha, I get it now.

:P Actually, the tweet didn't give it away, I didn't make the connection until just now when you mentioned it. This issue got linked from https://github.com/junegunn/fzf.vim/issues/315, which shows up in the Github UI for this issue.

---

_Comment by @tandrewnichols on 2018-03-14 13:34_

🤦‍♂️ Yeah, that makes sense.

---

_Comment by @tinmarino on 2021-03-16 12:18_

You can use `**` to match any path including slash <= also from `man gitignore`
```bash
# List all files except in a test directory
rg --files -g '!**/test/**'
```

---
