```yaml
number: 1806
title: Slash-rooted glob pattern matches directories, not just files
type: issue
state: open
author: bryceschober
labels: []
assignees: []
created_at: 2021-02-25T04:41:29Z
updated_at: 2021-03-01T18:45:27Z
url: https://github.com/BurntSushi/ripgrep/issues/1806
synced_at: 2026-01-12T16:13:24Z
```

# Slash-rooted glob pattern matches directories, not just files

---

_@bryceschober_

#### What version of ripgrep are you using?

ripgrep 12.1.0 (rev 1980630f17)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

`sudo snap install --classic`, no permissions issues

#### What operating system are you using ripgrep on?

Ubuntu 20.04 x86-64

#### Describe your bug.

My context is <https://github.com/microsoft/vscode/issues/117619>, but I'm reproducing with `rg` independently.

I can't find a glob pattern to exclude matches from `*.d` files without excluding `*.d` directories. I thought that `/**/*.d` should work, but it doesn't.

#### What are the steps to reproduce the behavior?

The [github glob pattern docs](https://git-scm.com/docs/gitignore#_pattern_format) say:

> A slash followed by two consecutive asterisks then a slash matches zero or more directories.

Which leads me to believe that maybe my pattern needs to be rooted by a `/` so that the `*.d` part doesn't match directories, only files, but no such luck. So here's a minimal experiment that I've done to reproduce my situation. I can't find a pattern that will exclude only `*.d` files as opposed to also including `*.d` directories. What am I missing?

#### What is the actual behavior?

```shell
$ rg --version
ripgrep 12.1.0 (rev 1980630f17)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
$ mkdir -p /tmp/example/foo/bar.d
$ echo "hello, cruel world" > /tmp/example/foo/bar.d/baz
$ cd /tmp/example
$ rg 'cruel'
foo/bar.d/baz
1:hello, cruel world
$ rg 'cruel' -g '**/*.d'
$ rg 'cruel' -g '!**/*.d'
$ rg 'cruel' -g '/**/*.d'
$ rg 'cruel' -g '!/**/*.d'
$
```

#### What is the expected behavior?

I should be able to search files under `*.d` directory, while still excluding `*.d` files.

---

_Comment by @BurntSushi on 2021-02-25 08:09_

Could you please provide the expected output for each of your 4 rg commands?

---

_Comment by @bryceschober on 2021-02-25 21:51_

Well,  I don't know what to think anymore. If I set up a test environment to try actual git and its globs:

```shell
# In my test git dir:
$ mkdir -p foo/bar.d foo/baz
$ touch foo/bar.d/foo.d
$ touch foo/bar.d/baz
$ touch foo/baz/bar.d
$ touch foo/baz.d
```

The goal being to ignore/exclude all but `foo/bar.d/baz`.

Then I tried all kinds of globs as first posted, but only when I used the following glob combination in my `.gitignore` do I get the desired behavior:
```gitignore
*.d # Ignore all things ending with `.d`
!*.d/ # But stop ignoring directories ending with `.d`
```

Is there any other single glob that can have the desired effect? The above will work for `rg`, but requires more options, and won't end up working in the VS Code usage, but that's a different issue...

---

_Comment by @BurntSushi on 2021-03-01 16:00_

@bryceschober Sorry, I'm still having a bit of trouble following. In your initial comment, you included 4 rg commands. Could you say what your expected/desired output is for each?

---

_Comment by @bryceschober on 2021-03-01 18:45_

@BurntSushi Sorry, here is what I was thinking with those tests. I'll expand with more comments. Also, what I'm really looking for is a file path match, so I took out the text search, which isn't core to my use case:
```shell
$ mkdir /tmp/glob_test
$ cd /tmp/glob_test
$ git init
$ mkdir -p foo/bar.d foo/baz
$ touch > foo/bar.d/foo.d
$ touch > foo/bar.d/baz
$ touch > foo/baz/bar.d
$ touch > foo/baz.d
# List all of the *.d files in any directory
$ rg --files -g '**/*.d'
foo/baz/bar.d
foo/baz.d
foo/bar.d/foo.d
# Does this list non-*.d files?
$ rg --files -g '!**/*.d'
# Nope. I was hoping for:
foo/bar.d/baz
# What if I add a leading slash? Does the positive match work the same?
$ rg --files -g '/**/*.d'
foo/baz/bar.d
foo/bar.d/foo.d
foo/baz.d
# What about for a negative match?
$ rg --files -g '!/**/*.d'
# Nope. I was hoping the leading slash might keep the *.d from excluding a dir and yield:
foo/bar.d/baz
```
How do I get a search to exclude all `*.d` files, but not exclude non-`*.d` files beneath `*.d` directories? It seems pretty much not possible. In `.gitignore`'s sequential application of globs in the negative sense, a later glob can be used to re-include only `*.d` directories:
```shell
$ echo -e '*.d\n!*.d/' > .gitignore
$ git ls-files --exclude-standard --other
.gitignore
foo/bar.d/baz
```
But I can't find a way to make the same idea work with `rg`'s file-inclusion/exclusion globs.

---
