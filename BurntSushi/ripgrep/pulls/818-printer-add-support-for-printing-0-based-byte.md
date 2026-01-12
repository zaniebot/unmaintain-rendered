```yaml
number: 818
title: "printer: add support for printing 0-based byte offset before matches"
type: pull_request
state: closed
author: balajisivaraman
labels: []
assignees: []
base: master
head: fix_812
created_at: 2018-02-19T05:27:32Z
updated_at: 2018-03-10T16:07:49Z
url: https://github.com/BurntSushi/ripgrep/pull/818
synced_at: 2026-01-12T18:23:13Z
```

# printer: add support for printing 0-based byte offset before matches

---

_@balajisivaraman_

Closes #812 

As discussed in the issue thread, the only question is the order of line number, column and offset when all three are displayed. I've gone with `line number:column:offset` as the default for now.

---

_Review comment by @BurntSushi on `tests/tests.rs`:407 on 2018-02-20 12:38_

For completeness, could you pass the `--no-mmap` flags to each of the above tests, and then copy the tests and change `--no-mmap` to `--mmap`? I am thinking that your implementation might actually be correct for `--mmap` but not for `--no-mmap`. I'm not sure a global byte offset is even tracked in the `--no-mmap` searcher, so this task might require a bit more work. :)

---

_@BurntSushi requested changes on 2018-02-20 12:39_

Thanks for working on this! We need a little bit more test coverage here, and I suspect it may expose a bug.

---

_@balajisivaraman reviewed on 2018-02-20 15:09_

---

_Review comment by @balajisivaraman on `tests/tests.rs`:407 on 2018-02-20 15:09_

I'm really sorry about overlooking this. When I went into `search_stream`, I noticed that the change was pretty trivial and it made so much sense to me that I missed `search_buffer`. It's on me. I'll have a look at the `mmap` searcher to see what can be done. :+1: 

---

_@balajisivaraman reviewed on 2018-02-20 16:16_

---

_Review comment by @balajisivaraman on `tests/tests.rs`:407 on 2018-02-20 16:16_

Ah, I think I lucked out here. The iterator returned by `self.grep.iter(self.buf)` in `search_buffer.rs` provides us with the starting position of the matching line itself. From then on, we call `printer.matched` with the `start` and `end`, which thankfully works the same way it does for `search_stream`. Both the `--no-mmap` and `--mmap` tests that you asked to write also pass. :+1: 

---

_@BurntSushi reviewed on 2018-02-20 16:18_

---

_Review comment by @BurntSushi on `tests/tests.rs`:407 on 2018-02-20 16:18_

Hmm, awesome! You might actually want to test this also in non-mmap searcher using a smaller capacity, such that the matches reported span multiple buffers. If that test passes, then I'll believe the implementation is correct. :)

---

_@BurntSushi reviewed on 2018-02-20 16:18_

---

_Review comment by @BurntSushi on `tests/tests.rs`:407 on 2018-02-20 16:18_

I believe there's a `smallcap` helper function in the tests somewhere, since there are other tests that proceed in a similar vein. But I can't remember. It's been a while since I looked at that code.

---

_Review comment by @balajisivaraman on `tests/tests.rs`:407 on 2018-02-20 16:54_

Hmm... No, you're right. I tried it with the `smallcaps` test available in `search_stream.rs` and it completely blew apart. (I got 0 as the offset for the next match instead of the offset from the beginning of the file.) The good news is that I can see why this is happening very clearly.

I now understand why you originally thought it will require additional work for the `--no-mmap` case. I'll have a look at it. Sorry. :+1: 

---

_@balajisivaraman reviewed on 2018-02-20 16:54_

---

_Comment by @balajisivaraman on 2018-02-21 16:55_

@BurntSushi, Thanks for the patient explanation and walkthrough yesterday. I've identified the bug and, hopefully, fixed it as well. :-)

One point to note is that within the searchers, I've named the variable that keeps track of the offset as `buffer_offset`. This made sense to me as it tracks the offset of the current buffer from the beginning of the file; and I can't come up with a better name for it.

Also, as I've moved the tests to within the searchers itself, I didn't feel a necessity to have `--mmap` and `--no-mmap` tests on the outside as well. The integration tests now only test for the behaviour of `-b` along with `-o` and `-v`.

EDIT: Moved the `--invert-match` test to the searchers itself. Added a `--before-context` test in `search_stream.rs` to ensure the offset tracking happens properly when bytes from the previous buffer are rolled over.

---

_Comment by @BurntSushi on 2018-03-10 15:16_

@balajisivaraman OK, I finally had a chance to look into merging this. Overall, this looks really great. I merged this PR in b006943c01561a4ae5b928081d1bb4087b599e19 with a few minor tweaks and a bug fix:

1. I renamed `buffer_offset` to `byte_offset`, since the former isn't actually true for the stream searcher. (It is a byte offset for the entire file, not to a buffer.)
2. I changed the type of `byte_offset` to `u64` from `usize`. This is because `usize` is a platform specific type whose size is tied to the size of addressable memory. So on a 32 bit system, `usize`'s max value is `2^32-1`, which means the byte offset would overflow when searching files bigger than 4GB. Note that this is tricky to get right, since there are places in the searcher where `usize` is correct since they correspond to offsets into memory.
3. GNU grep's behavior when one of the `--context` flags is given is to emit a byte offset before each context line. I updated your patch to do that and fixed the test, which was easy to do thanks to your prior work. :-)

Thanks so much for working on this!

---

_Closed by @BurntSushi on 2018-03-10 15:16_

---

_Branch deleted on 2018-03-10 16:07_

---
