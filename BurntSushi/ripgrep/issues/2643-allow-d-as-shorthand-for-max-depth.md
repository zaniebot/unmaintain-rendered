```yaml
number: 2643
title: Allow -d as shorthand for --max-depth
type: issue
state: closed
author: 3tilley
labels: []
assignees: []
created_at: 2023-11-02T14:19:57Z
updated_at: 2023-11-21T23:39:37Z
url: https://github.com/BurntSushi/ripgrep/issues/2643
synced_at: 2026-01-12T16:13:24Z
```

# Allow -d as shorthand for --max-depth

---

_@3tilley_

`--max-depth` is one of my most used flags, I love it. It's a bit clunky to enter regularly though. Would you allow the adding of a `-d` shorthand for it? It would also have the advantage that it would match how `fd` as `--max-depth / -d` does it. I don't think you should have to alter your software for someone else's software, but I would suspect there are a group of people who use both who might appreciate it, on top of the group that only use `rg`. It also appears to be used by `du` and a few other tools, which might be why I keep subconsciously reaching for it, though of course there are lots of tools that use `-d` for something else.

It seems like this may have been asked before, but I can't see any record of the discussion with a brief search, but worst-case this can be that discussion.

The only reason I can see that it might not have been implemented is that there is a [quirky](https://man7.org/linux/man-pages/man1/grep.1.html) grep flag with the same name.
```
       -d ACTION, --directories=ACTION
              If an input file is a directory, use ACTION to process it.
              By default, ACTION is read, i.e., read directories just as
              if they were ordinary files.  If ACTION is skip, silently
              skip directories.  If ACTION is recurse, read all files
              under each directory, recursively, following symbolic
              links only if they are on the command line.  This is
              equivalent to the -r option.
```

I understand that breaking behaviour with `grep` is undesirable, but perhaps this is a sufficiently awkward and underused flag that it might be ok? After all current the behaviour isn't compatible as `-d` does nothing.

Otherwise there is an argument for paucity of shortcodes for future use, in reply to which I can only make the case that I think this would be useful to lots of people, perhaps more than future yet-to-be-implemented flags? I guess you know much better than me about that.

A final option would be a config flag to enable it, is there prior art for that?

Happy to contribute the PR if you agree,

Max

---

_Comment by @3tilley on 2023-11-02 20:19_

You'll see that I've given this a go in #2644. I also had a quick look at what other similar tools did. A bit of a mix seems to be the answer.

Comparison with other greppers:
## [ugrep](https://github.com/Genivia/ugrep):
`-d` works as grep does it, `--depth` operates slightly unusually
```
--depth=[MIN,][MAX], -1, -2, -3, ... -9, -10, -11, -12, ...
        Restrict recursive searches from MIN to MAX directory levels deep,
        where -1 (--depth=1) searches the specified path without recursing
        into subdirectories.  Note that -3 -5, -3-5, and -35 search 3 to 5
        levels deep.  Enables -r if -R or -r is not specified.
...
ugrep option -d, --directories=ACTION is skip by default, instead of read. By default, directories specified on the command line are searched, but not recursively deeper into subdirectories.
```
## [git grep](https://mirrors.edge.kernel.org/pub/software/scm/git/docs/git-grep.html)
`-d` doesn't do anything and `--max-depth` as per rg

## [The Silver Searcher](https://github.com/ggreer/the_silver_searcher)
`-d` doesn't do anything, and `--depth` is used for limiting search depth

## [ack](https://linux.die.net/man/1/ack)
`-d` doesn't do anything, and I can't see a way of limiting depth of search



---

_Closed by @BurntSushi on 2023-11-21 23:39_

---
