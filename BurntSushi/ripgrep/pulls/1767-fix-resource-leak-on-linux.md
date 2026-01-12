```yaml
number: 1767
title: Fix resource leak on Linux
type: pull_request
state: closed
author: 3point2
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2020-12-22T23:11:20Z
updated_at: 2021-06-06T14:39:15Z
url: https://github.com/BurntSushi/ripgrep/pull/1767
synced_at: 2026-01-12T18:23:14Z
```

# Fix resource leak on Linux

---

_@3point2_

resolves BurntSushi/ripgrep#1766

---

_@BurntSushi requested changes on 2020-12-27 16:54_

Thanks for digging into this! It's a great find.

Your hesitation about the use of `warn!` is spot on. I suspect we need to find a way to at least make it possible for the error to be returned to the caller. Log messages like `warn!` aren't really something I've used much in ripgrep, as I see them more for developer usage. But the kinds of errors reported here are most definitely of interest to the user. Additionally, these are messages that ripgrep needs to be able to squash if requested (i.e., `--no-messages`).

More concretely, I _think_ what's needed here is this:

1. Move the reap logic to its own separate routine and make that routine idempotent.
2. Call that reap logic from within the `read` routine, as was done before.
3. Also call it in `Drop` as a last-ditch-effort. Retaining the `warn!` here in case of an error is fine. (Because of steps 4 and 5.)
4. Add a `close` method to the public API of `CommandReader` that permits callers to stop and reap the process early.
5. Have ripgrep call `close` whenever it uses a `CommandReader`. (This looks doable inside of `search_preprocessor` in `crates/core/search.rs`.)

Since ripgrep will always call `close`, it also follows that the `warn!` messages will never appear. So if they do appear, they can be considered a bug.

And on top of that, this behavior should be documented in the `grep-cli` crate. Probably best to put it on the docs for the new `close` method.

---

_Comment by @3point2 on 2020-12-29 19:52_

> Thanks for digging into this! It's a great find.

My pleasure :blush: Thanks for the feedback! That design sounds sensible to me. I've just pushed those changes, minus point 5.

The issue with point 5 is that `search_preprocessor` and `search_decompress` in `crates/core/search.rs` both [pass](https://github.com/BurntSushi/ripgrep/blob/a6d05475fb353c756e88f605fd5366a67943e591/crates/core/search.rs#L405) the `CommandReader` into `self.search_reader` _by value_. We can't call `close` on the reader afterwards as it ends up being owned by a `encoding_rs_io::DecodeReaderBytes` struct, part of the `ReadByLine` struct in searcher::glue in the grep-searcher crate.

I briefly toyed with refactoring the code to pass the reader around by reference so that we can `close` it afterwards in `crates/core/search.rs` as you suggested. That quickly started feeling like a large enough change that I wanted to check in before going down that route.

I also played with defining and implementing a `Close` trait so that `ReadByLine.run` in searcher/glue.rs could call `close()` which turned out to be a bit of a goose chase as there's no way to get a mutable reference to the CommandReader once it's in DecodeReaderBytes. Part of the reason for my confusion was that Searcher.search_reader takes a read_from argument, then [shadows](https://github.com/BurntSushi/ripgrep/blob/a6d05475fb353c756e88f605fd5366a67943e591/crates/searcher/src/searcher/mod.rs#L699-L710) the variable. [This commit](https://github.com/BurntSushi/ripgrep/pull/1767/commits/32ba75695e3ce79bd542cc3377ee85330fc9375a) is to try make that method easier to read.

I can think of a few ways forward:

* Embark on a non-trivial refactor to pass the CommandReader by reference
* Go back to drop() being the only place where the CommandReader `wait`s on its child, and work on the formatting of the output so that it matches what ripgrep would print if the error had been bubbled up
* Add a method to DecodeReaderBytes in the encoding_rs_io crate which allows us to get a mutable reference to the inner reader so that we can call `close` on it in glue.rs (this implies a Close trait, using a DecodeReaderBytes<T> where T: std::io::Read + Close, and implementing a no-op `close` method on the remaining reader types we use it with).
* Something else? Happy to hear your thoughts on this.


---

_Comment by @BurntSushi on 2021-05-30 01:35_

@3point2 Thanks so much for working on this! I've merged this in a WIP rollup branch. I made a few small tweaks but kept the overall approach. Thanks for digging into this. :-)

One tweak was changing the code to read more linearly, like this:

```rust
        let stdout = match self.child.stdout.take() {
            None => return Ok(()),
            Some(stdout) => stdout,
        };
        drop(stdout);
        ...
```

I also just passed a mutable reference to the reader in the search code. The refactor wasn't too bad at all. The only tricky part I think was returning an error from closing only if the actual operation was successful, but still calling `close` even if the search returned an error.

The most critical change I think was handling an error condition. Namely, when we close stdout before reading its entirety, that provokes a broken pipe error (usually via a signal). I don't know how to detect this portably by inspecting the error code, so instead, I assume it happens when: EOF hasn't been reached yet and when reading stderr produces no data.

After all that, I can confirm that it fixes your reproduction. Thank you for that. :-)

---

_Comment by @BurntSushi on 2021-05-30 01:36_

Here's my updated commit: https://github.com/BurntSushi/ripgrep/commit/feb8dbc6a6e7f3df5a1a2ee8ab7d2825d2210a94

---

_Label `rollup` added by @BurntSushi on 2021-05-30 01:36_

---

_Comment by @3point2 on 2021-05-30 13:39_

Thanks for picking that up @BurntSushi, I'm glad the refactor was easy!

While I'm fine with closing this PR in favor of your ag/work branch, I want to point out that your heuristic approach to deal with the child process receiving a SIGPIPE probably isn't necessary. In my opinion it's the child's responsibility to handle SIGPIPE gracefully and all the common decompression child processes that rg runs by default should be well behaved in this respect. Check out `gzip` for example: I use bash's `pipefail` option to return the error code if any component of the pipe returns with nonzero status. You can see that `gzip` returns 0 even when `head` closes its stdout halfway.
```
[vasili@hal ~]$ false| cat
[vasili@hal ~]$ echo $?
0
[vasili@hal ~]$ set -o pipefail
[vasili@hal ~]$ false| cat
[vasili@hal ~]$ echo $?
1
[vasili@hal ~]$ echo -e 'hello\nworld'| gzip > test.gz
[vasili@hal ~]$ gunzip -c test.gz | head -n1
hello
[vasili@hal ~]$ echo $?
0
```

So my vote would be to leave the responsibility of gracefully handling SIGPIPE to the child process, which all compression utilities should do (I can test the remaining ones used by rg if you'd like). The heuristic you're suggesting says "If you exit due to SIGPIPE and don't print anything to stderr I'm going to assume all was well', but that's not going to be a valid assumption in some cases. An application could exit with an error code without printing to stderr for reasons other than SIGPIPE - in that case rg will silently ignore the error and print misleading output. In my opinion between putting the burden on the child process to behave correctly vs. printing misleading output, I'd choose the former. As always happy to hear your thoughts on this  :)

---

_Comment by @BurntSushi on 2021-05-30 13:45_

> I want to point out that your heuristic approach to deal with the child process receiving a SIGPIPE probably isn't necessary.

Sorry, I probably didn't give full context. It is _empirically_ necessary. I tried without it, and the error branch was triggered every time when using `gzip`. As far as I can tell, this is expected behavior: broken pipe surfaces as a signal by default, and this in turn makes its way into the code returned from reaping the child. This in turn means that `success()` returns false.

If you have a better way of doing this, then I'm all ears. I'd recommend checking out my `ag/work` branch and testing it with and without the broken pipe handling heuristic.

> While I'm fine with closing this PR in favor of your ag/work branch

Yeah once it comes in it will close this PR automatically.

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---

_Comment by @3point2 on 2021-06-06 14:00_

Hi again @BurntSushi, I'm a bit late, but I've taken a closer look at this. You are right, my test in my last comment showing that gzip would exit with code 0 (success) when stdout is closed was incorrect. The test.gz file I created was very small, so gunzip was able to successfully write all of it to the pipe's buffer before exiting. Using a larger test.gz file shows that gzip in fact exits with code 141 (which is 128 plus 13 which is SIGPIPE - that's how bash constructs the exit code when a process terminates from a signal).
```
[vasili@hal ~]$ set -o pipefail
[vasili@hal ~]$ yes | head -n 100000 > test
[vasili@hal ~]$ gzip test 
[vasili@hal ~]$ gunzip -c test.gz | head -n1
y
[vasili@hal ~]$ echo $?
141
```
I took a quick look at the [gzip source code](https://git.savannah.gnu.org/cgit/gzip.git/tree/gzip.c) and interestingly it does handle SIGPIPE under certain circumstances - one way to get it to do that is to run it with -q:
```
[vasili@hal ~]$ gunzip -q -c test.gz | head -n1
y
[vasili@hal ~]$ echo $?
2
```
The exit code is 2 which, although still nonzero, represents a warning not an error, so rg could safely ignore it.

The problem with that approach is that the various decompression utilities that rg runs don't all behave the same way - rg would have to know the acceptable non-zero return codes for each (if any), which is a bit of pain, not to mention not having that information for any command given to --pre. (I suppose an --ignore-precmd-rc option or similar could allow the user to supply that).

After thinking a bit more about the problem, I realized that all the above might be completetly redundant: When ripgrep closes stdout on a child process, that means it has already found what the user looking for. One could make the argument that this is enough of a success to _completely_ ignore the return code of the child process after closing its stdout. I haven't read the source code to check that this holds in all cases (especially inverted matching), but if it did you could just remove the existing heuristic and always return `Ok(())` when `!self.eof` in CommandReader::close.

Having said all that, the existing heuristic will probably work well enough in practice. Thanks for working with me on this PR!

---

_Comment by @BurntSushi on 2021-06-06 14:39_

That's a good insight! I think I like it. I think I'll stick with the existing heuristic since it's a bit more aggressive with reporting failures than the approach of always dropping them. And whenever possible, I'd like to report failures to make debugging easier.

---
