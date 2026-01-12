```yaml
number: 706
title: Support ignore files with custom names
type: pull_request
state: merged
author: ptzz
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2017-12-04T09:07:26Z
updated_at: 2018-03-14T04:42:39Z
url: https://github.com/BurntSushi/ripgrep/pull/706
synced_at: 2026-01-12T18:23:13Z
```

# Support ignore files with custom names

---

_@ptzz_

This allows for application specific ignorefile names, e.g. using
`.fdignore` for `fd`.

- Moved the hardcoded `.rgignore` from the ignore crate to ripgrep

- `.ignore` is always respected and has the highest precedence

---

_Comment by @ptzz on 2017-12-04 09:12_

See discussion in https://github.com/sharkdp/fd/issues/156, specifically https://github.com/sharkdp/fd/issues/156#issuecomment-347371863

---

_@ptzz reviewed on 2017-12-04 09:15_

---

_Review comment by @ptzz on `ignore/src/dir.rs`:531 on 2017-12-04 09:15_

`ignorefile()` vs existing `add_ignore()` (for global ignorefiles) might be a bit confusing. Happy to change the name into something better. Or maybe change `add_ignore` to `add_global_ignore`?

---

_Comment by @ptzz on 2017-12-04 11:09_

Should probably add tests that uses multiple ignore files and validates precedence.

---

_Review comment by @BurntSushi on `ignore/src/dir.rs`:115 on 2017-12-04 11:36_

The names here could be better I think. I'd probably go with `custom_ignore_filenames` here.

---

_Review comment by @BurntSushi on `ignore/src/dir.rs`:531 on 2017-12-04 11:38_

This method's type signature should be:

```rust
pub fn ignorefile<S: AsRef<OsStr>>(&mut self, file_name: S) -> &mut IgnoreBuilder {
```

And yes, this means your ignore files should not be `Vec<String>` but rather `Vec<OsString>`.

---

_Review comment by @BurntSushi on `ignore/src/dir.rs`:531 on 2017-12-04 11:39_

I think this should be named `add_custom_ignore_filename`.

---

_@BurntSushi requested changes on 2017-12-04 12:11_

Thanks for working on this! Unfortunately, I think there was a miscommunication or I didn't make myself sufficiently clear, and as a result, this PR isn't quite what I was expecting.

First and foremost, these docs need to be our reference point: https://docs.rs/ignore/0.3.1/ignore/struct.WalkBuilder.html#ignore-rules --- I'll speak in terms of those.

A critical aspect of ignore file checking is that there are layers of ignore rules. Notice that in the second bullet item, each class of ignore file has its own precedence rules. This is necessary not only for predictable UX, but it also matches how `git` does things. (`git` has its own layers: `.gitignore`, `.git/info/exclude` and global gitignores.) In particular, *any* `.gitignore` file---regardless of where it lives---will override *all* `.git/info/exclude` files.

This precedence hierarchy continues with `.ignore`. This is best shown with an example:

```
$ mkdir /tmp/demorules
$ cd /tmp/demorules
$ git init
$ mkdir foo
$ echo test > baz
$ echo test > foo/quux
$ echo test > foo/bar
$ echo -e 'baz\nquux' > .gitignore
$ echo '!quux' > foo/.gitignore
```

If you stop here, you'll notice that `git` won't ignore `quux` since the `.gitignore` inside the `foo` directory overrides the rules found in the `.gitignore` in the root of the repository. If you run `rg --files`, then ripgrep will come up with the same results.

However, if you run

```
$ echo quux > .ignore
```

in the root of the repository and run `rg --files`, you'll notice that `quux` is actually ignored, even though a `.gitignore` file exists that is "closer" to `quux` in the `foo` directory. This is because *any* `.ignore` overrides *all* `.gitignore` files.

So where am I going with this? Well, when I said this:

> I think the way to solve your problem is to add an additional layer of ignore files that has higher precedence than .ignore but uses a custom (probably application specific) name.

I meant that the application specific ignore files should actually be another layer, in addition to `.ignore`. That means *any* application specific ignore file (like `.fdignore`) will override *all* `.ignore` rules, just like *any* `.ignore` file will override *all* `.gitignore` rules.

In this PR, you've implemented such that `.ignore` is inter-mingled with the application specific ignores. That is, a `foo/.fdignore` won't override a `foo/bar/.ignore`, even though I think it should in order to be consistent with how the ignore rules are processed today.

Doing this is a little harder, but you should still mostly be able to work off of what's there. Please ping me with questions!

---

_Comment by @ptzz on 2017-12-04 13:01_

@BurntSushi Thanks for the comments!

> I think the way to solve your problem is to add an additional layer of ignore files that has higher precedence than .ignore but uses a custom (probably application specific) name.

Indeed I didn't read this carefully and assumed `.ignore` would have the highest precedence. I understand that just correcting the order won't solve the hierarchy issues that you talk about though.

I will try to find time to fix this properly.

---

_Comment by @ptzz on 2017-12-06 20:36_

@BurntSushi I *think* it should be working as expected now. Also addressed your comments on naming and method signatures.

---

_Review comment by @BurntSushi on `ignore/src/dir.rs`:116 on 2017-12-30 21:12_

This should have a doc string.

---

_Review comment by @BurntSushi on `ignore/src/dir.rs`:218 on 2017-12-30 21:14_

I suspect we don't want this. The caller needs to supply an application specific name, I assume, in order for a matcher to be created at all. So by passing a name, they are implicitly enabling it.

---

_Review comment by @BurntSushi on `ignore/src/dir.rs`:115 on 2017-12-30 21:15_

This should be in a `Arc<Vec<OsString>>`. Otherwise you're going to wind up copying it everywhere.

---

_@BurntSushi requested changes on 2017-12-30 21:21_

Thanks for continuing to work on this! I took another review and this is looking much better. There are still a few fixes I'd like to see though.

One major thing missing from this PR is tests, and we really need those before merging this. The tests should cover at least the following:

1. Basic functionality. i.e., Can I specify a custom ignore file and have it be respected?
2. Precedence. Precedence should be checked between different ignore file types and also between different custom ignore names.
3. Integration tests for ripgrep itself. These tests don't need to have as much coverage as 1+2, but something that checks that it actually works is fine.

I think 1+2 should be in the `ignore` crate while 3 should be in `tests/tests.rs`.

---

_Comment by @ptzz on 2018-01-01 20:55_

@BurntSushi Tests have been added for 1+2 above. As for precedence tests, I added one that verifies that a custom ignore file have higher precedence than `.ignore`, and one that verifies that earlier custom ignore files have lower precedence than later.

Let me know if you want to see more extensive tests. I wasn’t sure if it would make sense to add precedence tests between e.g. custom ignore and `.gitignore`, as there already tests for `.ignore` over `.gitignore` and precedence should apply transitively.

As for 3, there are already tests that uses `.rgignore`, maybe that's sufficient? It's not hardcoded in the ignore crate anymore, but passed as a (hardcoded) parameter from ripgrep.
  

---

_Comment by @ptzz on 2018-01-27 21:31_

@BurntSushi Do you have concerns about merging this and/or want more time for review? Let me know if there is anything I can address.

---

_@BurntSushi approved on 2018-01-29 19:18_

This looks great! Thanks for pinging me. Apologies for the tardiness. Also, thanks for pointing out existing tests for `.rgignore`, I forgot about those. :-) I will get this merged once CI passes (I just kicked it).

---

_Merged by @BurntSushi on 2018-01-29 21:06_

---

_Closed by @BurntSushi on 2018-01-29 21:06_

---

_Comment by @ptzz on 2018-01-29 22:52_

Thanks, it was a great learning experience doing a bit of Rust!

---
