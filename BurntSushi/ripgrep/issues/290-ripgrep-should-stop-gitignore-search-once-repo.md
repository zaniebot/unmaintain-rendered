```yaml
number: 290
title: ripgrep should stop .gitignore search once repo root directory is found
type: issue
state: closed
author: stephenh
labels:
  - question
assignees: []
created_at: 2016-12-23T19:10:49Z
updated_at: 2016-12-27T18:20:01Z
url: https://github.com/BurntSushi/ripgrep/issues/290
synced_at: 2026-01-12T16:13:21Z
```

# ripgrep should stop .gitignore search once repo root directory is found

---

_@stephenh_

It looks like when rg searches for `.gitignore` files, it keeps recursing up the directory tree, even outside of the git repository itself.

I admittedly have a somewhat-non-kosher git setup, where both my home directory is a git repository, as well as my regular projects, e.g. something like:

```
~/.gitignore
~/.git <-- my homedir.git repo
~/code/projecta/.git <-- project repo
~/code/projecta/.gitignore
```

My `~/.gitignore` has `*` in it, which means ignore all of the myriad things except those that I explicitly `git add -f .bashrc` or what not.

However, if I'm in `~/code/projecta` and run `rg`, it finds both `.gitignore`s, applies `*`, and then says "nope, didn't find anything", so I end up always having to use `rg -u`.

So, long explanation, but ideally `rg` would stop searching for `.gitignore` files once it hit a directory with `.git` in it, e.g. signaling we've reached the top-level directory of this repo, and so don't need to continue looking for more `.gitignore` files up the tree.



---

_Comment by @BurntSushi on 2016-12-23 20:27_

> It looks like when rg searches for .gitignore files, it keeps recursing up the directory tree, even outside of the git repository itself.

This is a known case and there is actually a test for it: https://github.com/BurntSushi/ripgrep/blob/bb70f967437279e49fb87df23ae567c276e4cc33/tests/tests.rs#L481

> I admittedly have a somewhat-non-kosher git setup, where both my home directory is a git repository, as well as my regular projects

I have the same setup. :-)

> My ~/.gitignore has * in it

Same!

```
$ cat $HOME/.gitignore | head -n1
*
```

> However, if I'm in ~/code/projecta and run rg, it finds both .gitignores, applies *, and then says "nope, didn't find anything", so I end up always having to use rg -u.

Hmm, if that's true, then that's definitely a bug. However, given that we have the same exact setup, I would notice this particular bug almost immediately. :-) That leads me to believe something else is going on. Can we try to reproduce this bug in a more controlled environment? Let's try this:

```
$ cd /tmp
$ mkdir -p code/projecta/.git
$ mkdir .git
$ echo '*' > .gitignore
$ echo 'test' > code/projecta/foo
$ rg test
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
$ cd code/projecta/
$ rg test
foo
1:test
```

And now with the `--debug` flag:

```
$ cd /tmp/ripgrep-290
$ rg test --debug
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
DEBUG:globset: glob converted to regex: Glob { glob: "**/*", re: "(?-u)^(?:/?|.*/).*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore]) }
DEBUG:globset: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG:ignore::walk: ignoring ./.gitignore: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring ./.git: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG:ignore::walk: ignoring ./code: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
$ cd code/projecta/
$ rg test --debug
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
DEBUG:globset: glob converted to regex: Glob { glob: "**/*", re: "(?-u)^(?:/?|.*/).*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore]) }
DEBUG:globset: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
foo
1:test
DEBUG:ignore::walk: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
```

I wonder if you are perhaps running `rg` from your `~` or `~/code` directories? If so, _that_ will indeed respect your `~/.gitignore` rules. If you don't want that behavior, then you can do:

```
$ echo '!*' > ~/.ignore
```

Which basically says, "tell ripgrep to whitelist everything in `~` without telling `git` to whitelist the same stuff."

---

_Label `question` added by @BurntSushi on 2016-12-23 20:28_

---

_Comment by @stephenh on 2016-12-27 18:10_

Thanks for the quick response! Apologies for the delay; got sidetracked with the holidays.

...and you are exactly right, this already works great/as expected.

After seeing it work (in my setup, on the first try), I had to stop for a few minutes to think why I had thought this was broken...it turns out I use a dual laptop/tower setup, e.g. edit files on the laptop, run the server on the tower, and forgot that when I mirror files between the two machines, I don't copy .git directories around.

So, on my laptop, rg (or any other tool) can't tell my "projecta" is a git repo. But it works great on the tower. Of course. Sorry about that. :-)

Thanks again for the response and your work on rg in general; it's great!


---

_Closed by @stephenh on 2016-12-27 18:10_

---

_Comment by @BurntSushi on 2016-12-27 18:20_

@stephenh Awesome! The best bugs are the ones that fix themselves. :-)

---
