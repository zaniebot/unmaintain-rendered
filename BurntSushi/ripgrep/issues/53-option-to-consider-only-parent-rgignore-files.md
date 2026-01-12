```yaml
number: 53
title: Option to consider only parent .rgignore files?
type: issue
state: closed
author: kaushalmodi
labels: []
assignees: []
created_at: 2016-09-24T05:17:36Z
updated_at: 2016-09-26T12:47:52Z
url: https://github.com/BurntSushi/ripgrep/issues/53
synced_at: 2026-01-12T18:23:11Z
```

# Option to consider only parent .rgignore files?

---

_@kaushalmodi_

I have a `~/.gitignore` and that is considered as an ignore file for everything in my `$HOME`. In my use case the contents of `~/.gitignore` do not apply as ignore criteria for searches! I have `~/.gitignore` set up just to prevent committing sensitive text files to git (and then push) by mistake. So that file contains something like

```
a*
!a_ok_to_commit
```

That would prevent search of all `a*` files everywhere in my `$HOME`.

So it would be great to allow parent `.rgignore` (only) file consideration.

In addition a `.gitignore` specific `-u` would also be needed.

In summary, there is very little overlap of the contents of `.gitignore` and search ignore files like `.agignore` and `.rgignore`.

For `.gitignore`:
- Never use that for search ignores
- Do not use parent `.gitignore`

For `.rgignore`:
- Use that for search ignores
- Use parent `.rgignore` files

From what I understand, right now `-u` and `--no-ignore-parent` applies to both of these files and you cannot set those options separately for those 2 files.


---

_Comment by @BurntSushi on 2016-09-24 05:21_

Why not just whitelist the files you want to search in your `~/.rgignore`? e.g., `!a*`.

(I think I had the same problem as you, so I just put `!*` in my `$HOME/.rgignore`, which whitelists anything that would otherwise be ignored by `$HOME/.gitignore`.)


---

_Comment by @kaushalmodi on 2016-09-24 05:37_

Before whitelisting:

```
km²~/temp/:proj> rg foo --debug                                                                                                                                                                      09/24 1:26am
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'f',
        'o',
        'o'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(foo)], limit_size: 250, limit_class: 10 }
DEBUG:rg::ignore: ./SOMEPRJ/abc ignored by Pattern { from: "/home/kmodi/.gitignore", original: "a*", pat: "**/a*", whitelist: false, only_dir: false }
DEBUG:rg::ignore: ./SOMEPRJ/ghi ignored by Pattern { from: "/home/kmodi/.gitignore", original: "g*", pat: "**/g*", whitelist: false, only_dir: false }
DEBUG:rg::ignore: ./SOMEPRJ/.rgignore ignored because it is hidden
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

After whitelisting by add `!*` to the very top of `~/.rgignore`:

```
km²~/temp/:proj> rg foo --debug                                                                                                                                                                      09/24 1:31am
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'f',
        'o',
        'o'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(foo)], limit_size: 250, limit_class: 10 }
DEBUG:rg::ignore: ./SOMEPRJ whitelisted by Pattern { from: "/home/kmodi/.rgignore", original: "!*", pat: "**/*", whitelist: true, only_dir: false }
DEBUG:rg::ignore: ./SOMEPRJ/abc whitelisted by Pattern { from: "/home/kmodi/.rgignore", original: "!*", pat: "**/*", whitelist: true, only_dir: false }
DEBUG:rg::ignore: ./SOMEPRJ/abc/def whitelisted by Pattern { from: "/home/kmodi/.rgignore", original: "!*", pat: "**/*", whitelist: true, only_dir: false }
DEBUG:rg::ignore: ./SOMEPRJ/abc/def/XXX whitelisted by Pattern { from: "/home/kmodi/.rgignore", original: "!*", pat: "**/*", whitelist: true, only_dir: false }
DEBUG:rg::ignore: ./SOMEPRJ/abc/def/XXX/YYY whitelisted by Pattern { from: "/home/kmodi/.rgignore", original: "!*", pat: "**/*", whitelist: true, only_dir: false }
DEBUG:rg::ignore: ./SOMEPRJ/abc/def/XXX/YYY/bar whitelisted by Pattern { from: "/home/kmodi/.rgignore", original: "!*", pat: "**/*", whitelist: true, only_dir: false }
DEBUG:rg::ignore: ./SOMEPRJ/ghi whitelisted by Pattern { from: "/home/kmodi/.rgignore", original: "!*", pat: "**/*", whitelist: true, only_dir: false }
DEBUG:rg::ignore: ./SOMEPRJ/ghi/XXX whitelisted by Pattern { from: "/home/kmodi/.rgignore", original: "!*", pat: "**/*", whitelist: true, only_dir: false }
DEBUG:rg::ignore: ./SOMEPRJ/ghi/XXX/YYY whitelisted by Pattern { from: "/home/kmodi/.rgignore", original: "!*", pat: "**/*", whitelist: true, only_dir: false }
DEBUG:rg::ignore: ./SOMEPRJ/ghi/XXX/YYY/bar whitelisted by Pattern { from: "/home/kmodi/.rgignore", original: "!*", pat: "**/*", whitelist: true, only_dir: false }
DEBUG:rg::ignore: ./SOMEPRJ/.rgignore ignored because it is hidden
SOMEPRJ/abc/def/XXX/YYY/bar
1:foo

SOMEPRJ/ghi/XXX/YYY/bar
1:foo
```

I believe this works (just that this repeats the bug in https://github.com/BurntSushi/ripgrep/issues/50).

Thanks, 

I only concern is that if you implement a global ignore file in `$HOME/ripgrep/ignore` ( https://github.com/BurntSushi/ripgrep/issues/45 ), then how would the priority work?

Then I would need to put `!*` in `$HOME/ripgrep/ignore` that would override the ignores in `~/.gitignore`? But wouldn't that be the wrong order? 

On the other hand, if I put `!*` in `~/.rgignore`, then wouldn't that override all the ignores in `$HOME/ripgrep/ignore`? :)


---

_Comment by @BurntSushi on 2016-09-24 05:50_

It seems like the priority of any "global" ignores file should come strictly after any ignores from inside parent directories are considered. If you want to override `~/.gitignore` then it makes sense that you should use a `~/.rgignore` to do it.


---

_Comment by @BurntSushi on 2016-09-25 01:15_

It sounds like this use case should be solved either by a global ignore file (#45) or by using explicit per-directory overrides.


---

_Closed by @BurntSushi on 2016-09-25 01:15_

---

_Comment by @kaushalmodi on 2016-09-26 12:47_

This issue was more related to https://github.com/BurntSushi/ripgrep/issues/68.

The fix for that issue ( https://github.com/BurntSushi/ripgrep/commit/8eeb0c0b60da59828a48d995e969bdba5816ea31 ) fixes this issue too.

Thanks!


---
