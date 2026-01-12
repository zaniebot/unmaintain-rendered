```yaml
number: 829
title: "Passing a path argument doesn't respect .gitignore"
type: issue
state: closed
author: tmccombs
labels:
  - bug
  - rollup
  - gitignore
assignees: []
created_at: 2018-02-22T07:15:23Z
updated_at: 2025-09-20T01:08:21Z
url: https://github.com/BurntSushi/ripgrep/issues/829
synced_at: 2026-01-12T16:13:22Z
```

# Passing a path argument doesn't respect .gitignore

---

_@tmccombs_

#### What version of ripgrep are you using?
ripgrep 0.8.1
-SIMD -AVX


#### What operating system are you using ripgrep on?

Archlinux (4.15.3-2-ARCH), also Ubuntu 16.04

#### Describe your question, feature request, or bug.

If I am in a git repository and run `rg` with a directory as a PATH argument, then all files under that directory are searched, even if they would normally be ignored because of .gitignore rule. If I `cd` into the directory and run `rg` without a PATH argument, the files are excluded as I expect. changing into a subdirectory is a workaround but is sometimes less convenient than using a path argument.

The man page does say

> Paths specified expicitly on the command line override glob and ignore rules.

Which could be interpreted to mean that the if such an argument is specified, the ignore rules are ignored altogether. But that behaviour is unexpected to me, and the wording of the documentation isn't entirely clear on whether it overrides _all_ rules, or only rules that specifically ignore the specified path (but not children of that path).

If overriding all ignore rules is the intended behaviour there should at least be a way to turn the ignore rules back on, but `--ignore-vcs` does nothing when supplied with a PATH argument.

Interestingly, if I specify a glob such as `-g "*.c"`, that is respected for files under the path.

#### Steps to reproduce:

.gitignore:
```
/a/b/
```

a/b/test.txt:
```
Sample text
```

In root of repository:

`rg Sample` produces no output

```
$ rg Sample a
a/b/test.txt
1:Sample text
```

Adding `--ignore-vcs` produces the same output.

In `a` directory `rg Sample` also produces no output.

#### If this is a bug, what is the actual behavior?

`rg --debug Sample a`

```
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Literal {
    chars: [
        'S',
        'a',
        'm',
        'p',
        'l',
        'e'
    ],
    casei: false
}
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(Sample)], limit_size: 250, limit_class: 10 }
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "**/.*~", re: "(?-u)^(?:/?|.*/)\\..*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), ZeroOrMore, Literal('~')]) }
DEBUG/globset/globset/src/lib.rs:401: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG/globset/globset/src/lib.rs:401: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
a/b/test.txt
1:Sample text
```

#### If this is a bug, what is the expected behavior?

Produced no output, since a/b/test.txt is ignored by the .gitignore file.


---

_Comment by @BurntSushi on 2018-02-22 12:00_

OK, so this issue is really tangled and I'm going to do best untangle it. The core of the problem here is that you've seemed to derive a generic failure from a very specific one, namely, that all ignore rules are overridden for a directory that is specified on the command line. But this is not true, as you noted with the glob rules. But your glob rules and your ignore rules are different, so you haven't accounted for the fact that there may be a bug with the specific pattern in your ignore file that doesn't impact the pattern in your `-g` glob. For example, if you change your rule in your ignore file from `/a/b` to `*.txt`, then `rg Sample` behaves the same as `rg Sample a`. The heart of the matter seems to be that `/a/b` does not match `a/b` when `a` is given as an explicit path, but it does indeed seem like it should match and therefore be ignored.

Note though that if you specified `a/b` on the command line, then that path shouldn't be ignored. In particular, the documentation provided by ripgrep is exactly correct:

> Paths specified expicitly on the command line override glob and ignore rules.

That is, if you give a path to ripgrep, then that path itself is immune to any other ignore rules inherited from the configuration, including those specified explicitly on the command line via the `-g` globs. But this says nothing about children of those paths. The ignore rules still apply to them because they weren't specified explicitly on the command line.

---

_Label `bug` added by @BurntSushi on 2018-02-22 12:01_

---

_Comment by @BurntSushi on 2018-02-22 12:04_

Interestingly, `a/b` also doesn't work, but `**/a/b` behaves as expected. So hopefully there's just a bug somewhere and this can be fixed, but it requires more debugging to understand exactly what's going on.

Thanks for the bug report!

---

_Comment by @BurntSushi on 2018-02-22 12:05_

Also, if you have suggestions on how to improve the documentation (while maintaining some level of succinctness), I'd be happy to try and fix that too, although it does seem like it might have been this bug that really threw you off. :)

---

_Comment by @tmccombs on 2018-02-22 17:40_

That makes sense. If I have some time I can try debugging some more.

---

_Comment by @BurntSushi on 2018-02-22 17:44_

@tmccombs Thanks! The kind of debugging to be done here is along the lines of "insert printf statements in strategic places inside the `ignore` (and possibly `globset`) crates." :-)

---

_Comment by @tmccombs on 2018-02-23 09:23_

I'm not entirely sure why yet, but the problem seems to be that in` Ignore::matched_ignore`, we join the `path` with `self.absolute_base`. And in the example directory above, `self.absolute_base` is `<repo_path>/a` and path is `a/b`. so the joined path is `<repo_path>/a/a/b`, when it should be `<repo_path>/a/b`.

---

_Comment by @tmccombs on 2018-02-25 07:25_

I think this might be the same as #278. Or at least caused by the same thing.

```rust
         if let Some(abs_parent_path) = self.absolute_base() {
             let path = abs_parent_path.join(path);
```

What's happening is when `add_parents` is called on the root ignore, the absolute base is set to the path that is passed (<repo_path>/a) and then that path is used as the next step. In fact the _only_ top level path this actually works for is "./" because "./" joined to any path is the original path.

Why is path joined to absolute_base rather than the current directory? I don't entirely understand what the purpose of absolute_base is. 

I'd be happy to help implement a fix for this, but I may need some help understanding what exactly absolute_base is used for and any gotchas I should watch out for.

---

_Comment by @tmccombs on 2018-06-12 06:11_

https://github.com/tmccombs/ripgrep/commit/9a42a12ad108352fc41888cd874ed4f1a8bb73cb fixes this problem, but I'm not sure if it introduces another problem, like not handling symlinks properly.

---

_Comment by @tmccombs on 2018-06-12 08:01_

yeah it breaks current behaviour with symlinks.

If you have a layout like this:
```
.
├── a
│   ├── b
│   │   ├── foogit
│   │   └── test.txt
│   ├── e -> ../c
│   ├── test.c
│   └── test.txt
├── c
    └── blah
```
And .gitignore contians a rule excluding "/c/blah", then currently `rg Sample a` would give a match at "a/c/blah" whereas with my fix the path would be canonicalized, and then matches "/c/blah", which is probably not what we want (I think). 

---

_Comment by @mitsuhiko on 2018-11-13 09:19_

I am running into the same issue. Our `.gitignore` file has `/src/sentry/static/sentry/dist/` in the root and when I cd into `src/sentry` and grep for something that is also in a dist file I get what I expect. However when I'm in the root and provide `src/sentry` as path it gives me a lot of matches I want to ignore.

The repo in question is here: https://github.com/getsentry/sentry

---

_Comment by @NikhilVerma on 2019-11-13 13:58_

Since VSCode uses ripgrep I have similar issues, it makes the search very slow + return results which are not desirable. I need to manually specify excludes to make it work.

---

_Comment by @bodinsamuel on 2019-12-30 11:22_

> Interestingly, a/b also doesn't work, but **/a/b behaves as expected

Thanks for this, it worked. Big impact in monorepo. 
Changing **.gitignore**: `packages/*/dist` -> `**/dist` (or simply `dist/`)

It's more a misunderstanding than a bug, but it clearly bothered me for month ahah


---

_Comment by @rattrayalex-stripe on 2020-01-09 19:45_

FWIW the `**/` workaround does not seem to work for me, at least via VS Code. 

---

_Label `gitignore` added by @BurntSushi on 2020-05-08 11:43_

---

_Comment by @leonheess on 2022-09-02 10:44_

Will this issue be resolved at some point?

---

_Label `rollup` added by @BurntSushi on 2025-09-10 11:48_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
