```yaml
number: 576
title: Somehow using rg in an alias breaks it
type: issue
state: closed
author: lexicalunit
labels: []
assignees: []
created_at: 2017-08-17T15:33:48Z
updated_at: 2017-08-17T16:50:58Z
url: https://github.com/BurntSushi/ripgrep/issues/576
synced_at: 2026-01-12T16:13:22Z
```

# Somehow using rg in an alias breaks it

---

_@lexicalunit_

When I run the exact same command as an alias, it works as expected. The intent is to search all files, disregarding any vcs or ignore files, and only exclude `.yml` files from the search.

```shell
$ cd /tmp
$ git clone git@github.com:lexicalunit/atom-notes.git
$ cd atom-notes
$ alias foo='rg --no-ignore -g "\!.yml" -c '
$ foo this
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
$ rg --no-ignore -g "\!.yml" -c this
LICENSE.md:2
README.md:4
lib/atom-notes.js:27
lib/notes-view-list-item.js:6
lib/notes-view-list.js:44
lib/notes-view.js:18
lib/utility.js:12
notebook/Welcome.md:1
$ foo --debug this
DEBUG:globset: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        't',
        'h',
        'i',
        's'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(this)], limit_size: 250, limit_class: 10 }
DEBUG:ignore::walk: ignoring ./.all-contributorsrc: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./.eslintrc.js: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG:ignore::walk: ignoring ./.gitignore: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./.nvatom-all-contributorsrc: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./.travis.yml: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./appveyor.yml: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./circle.yml: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./LICENSE.md: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./package-lock.json: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./package.json: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./README.md: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./grammars/notes.cson: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./lib/atom-notes.js: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./lib/config.coffee: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./lib/interlink.js: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./lib/notes-view-list-item.js: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./lib/notes-view-list.js: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./lib/notes-view.js: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./lib/utility.js: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./menus/atom-notes.cson: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./notebook/How to use.md: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./notebook/Welcome.md: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./spec/.eslintrc.js: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./spec/atom-notes-spec.js: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./spec/interlink-spec.js: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./spec/utility-spec.js: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./styles/atom-notes.less: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

# Versions

macOS 10.12.5

```
$ eval "$SHELL --version"
zsh 5.2 (x86_64-apple-darwin16.0)
$ rg --version
ripgrep 0.5.2
```


---

_Comment by @BurntSushi on 2017-08-17 15:47_

My guess is that you're experiencing a problem with your shell, and in particular, its interaction with the `!`. For example, neither of your commands output results in my shell, but if I surround the `!` in single quotes, it works:

```
[andrew@Serval ~] cd /tmp/
[andrew@Serval tmp] g clone git://github.com/lexicalunit/atom-notes
Cloning into 'atom-notes'...
remote: Counting objects: 1007, done.
remote: Compressing objects: 100% (51/51), done.
remote: Total 1007 (delta 33), reused 47 (delta 19), pack-reused 937
Receiving objects: 100% (1007/1007), 212.45 KiB | 4.25 MiB/s, done.
Resolving deltas: 100% (592/592), done.
[andrew@Serval tmp] cd atom-notes/
[andrew@Serval atom-notes] alias foo='rg --no-ignore -g "\!.yml" -c '
[andrew@Serval atom-notes] foo this
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
[andrew@Serval atom-notes] rg --no-ignore -g "\!.yml" -c this
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
[andrew@Serval atom-notes] rg --no-ignore -g '!.yml' -c this
README.md:4
LICENSE.md:2
notebook/Welcome.md:1
lib/utility.js:12
lib/notes-view.js:18
lib/notes-view-list-item.js:6
lib/notes-view-list.js:44
lib/atom-notes.js:27
```

---

_Comment by @okdana on 2017-08-17 16:12_

Yeah, your `\` is being interpreted literally. I *think* that's because `!` begins a history expansion, which happens to be the only thing that is processed *before* alias expansion in zsh. Since the `!` ceases to be a special character after history expansion is performed, the shell leaves the quoted `\` alone instead of consuming it.

You can either remove the `\` or remove the double-quotes around it.

Also, you have a space at the end of your alias, which tells the shell to expand the following word as an alias too. This will cause problems if you ever try to search for a pattern that matches an alias name (for example, if you alias `bar=baz`, then use your `foo` alias like `foo bar`, it will search for `baz`). So you should probably remove the space too.

Lastly, `-g '!.yml'` will exclude files named exactly `.yml`. I assume that's not what you want? Maybe `-g '!*.yml'` instead?

---

_Comment by @lexicalunit on 2017-08-17 16:50_

Yep! My bad, y'all. Using `'!...'` worked as I had intended. Also yeah I needed `!*.yml`.

---

_Closed by @lexicalunit on 2017-08-17 16:50_

---
