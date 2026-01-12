```yaml
number: 42
title: Files with matches
type: pull_request
state: merged
author: andyleejordan
labels: []
assignees: []
merged: true
base: master
head: files-with-matches
created_at: 2016-09-24T03:19:48Z
updated_at: 2016-09-25T19:31:00Z
url: https://github.com/BurntSushi/ripgrep/pull/42
synced_at: 2026-01-12T18:23:12Z
```

# Files with matches

---

_@andyleejordan_

Resolves #26 (or at least starts to).
- [x] Needs tests
- [x] Needs docs
- [x] Needs to terminate the search at the first match
- [ ] Needs to be refactored after a review


---

_Comment by @andyleejordan on 2016-09-24 03:26_

@BurntSushi So I _think_, if I'm understanding this code correctly, to terminate early for mmap searching I could just break out of

``` rust
        for m in self.grep.iter(self.buf) {
            if self.opts.invert_match {
                self.print_inverted_matches(last_end, m.start());
            } else {
                self.print_match(m.start(), m.end());
            }
            last_end = m.end();
        }
```

t the first match. Is this loop "for matches in lazy iter"? I could use some pointers for sure 😉 (pun totally intended).

For now I'm going to eat some dinner and get to those video games, but this was fun!


---

_Comment by @BurntSushi on 2016-09-24 03:50_

>  Is this loop "for matches in lazy iter"? I could use some pointers for sure :wink: (pun totally intended).

You got it, that's right. Breaking out of that loop is perfect.

Note that you'll also need to do this in `search_stream.rs`, which is a little less nice.


---

_Comment by @BurntSushi on 2016-09-24 03:50_

@andschwa Thanks for working on this by the way! Enjoy the video games. :-)


---

_Review comment by @andyleejordan on `src/search_stream.rs` on 2016-09-25 00:29_

@BurntSushi in my experimentation, this seemed to work correctly to terminate after one match. However, I expected to be able to just break at the first match further down in the loop... but when I tried that I found that the match_count was > 1 at the top of the loop, which was unexpected. I'm missing something.


---

_@andyleejordan reviewed on 2016-09-25 00:29_

---

_Review comment by @andyleejordan on `src/search_stream.rs` on 2016-09-25 00:31_

@BurntSushi I think it might not be a bad refactor to add an `opts.just_file = self.opts.count || self.opts.files_with_matches`, which can replace four instances of this or statement; eh?


---

_@andyleejordan reviewed on 2016-09-25 00:31_

---

_@BurntSushi reviewed on 2016-09-25 00:35_

---

_Review comment by @BurntSushi on `src/search_stream.rs` on 2016-09-25 00:35_

`match_count` could increase by more than than you might expect if you're doing `inverted` matching. For regular searching, `match_count` should only be incremented by `1` at most on each iteration.

I would probably move this check to right after the call to `read_match`. i.e., `if self.opts.files_with_matches && matched { break; }`.


---

_@BurntSushi reviewed on 2016-09-25 00:37_

---

_Review comment by @BurntSushi on `src/search_stream.rs` on 2016-09-25 00:37_

That's fine, but I wouldn't add a new field. I'd just add a method on the `Options` struct. :-)


---

_@andyleejordan reviewed on 2016-09-25 00:46_

---

_Review comment by @andyleejordan on `src/search_stream.rs` on 2016-09-25 00:46_

It can't be _right_ after it because the first match needs to be handled. But what's odd is that if I put it at the end of the while loop, I get this:

``` rust
loop {
            println!("{:?} {}", self.path, self.match_count);
...
while ... {
...
        if self.opts.files_with_matches && matched {
            break;
        }
    }
}
```

```
"src/args.rs" 1
"src/args.rs" 2
"src/args.rs" 3
"src/args.rs" 3
```

I think I have to break out of both loops maybe.


---

_@BurntSushi reviewed on 2016-09-25 01:02_

---

_Review comment by @BurntSushi on `src/search_stream.rs` on 2016-09-25 01:02_

Oh, yes, indeed you do. The outer loop corresponds to refilling the buffer with data from the file and the inner loop corresponds to iterating over all matches within that buffer. Breaking out of the inner loop won't cause the outer loop to stop.

You might consider changing `loop` to `while !self.opts.files_with_matches || self.match_count == 0` or something similar. (With the case unfortunately repeated inside the `while` loop as well.)


---

_@andyleejordan reviewed on 2016-09-25 01:20_

---

_Review comment by @andyleejordan on `src/search_stream.rs` on 2016-09-25 01:20_

Added a small `terminate()` function; it's working just grand.


---

_@andyleejordan reviewed on 2016-09-25 01:20_

---

_Review comment by @andyleejordan on `src/search_stream.rs` on 2016-09-25 01:20_

Added `opts.skip_matches()`, definitely feels better.


---

_Comment by @andyleejordan on 2016-09-25 01:21_

Just FYI I'm already using this; I wanted to rename a function I'd plumbed through:

``` sh
./target/debug/rg -l just_file | xargs gsed -i 's/just_file/skip_matches/g'
```

🎉 


---

_Comment by @andyleejordan on 2016-09-25 02:27_

So either `--invert-match` or there's a bug: when I combine it with `--count` (i.e. without these changes), I still get files that have matches listed (is this expected?). So I've removed `--files-without-match` as I was using the same logic and so had the same bug.


---

_Comment by @andyleejordan on 2016-09-25 02:49_

@BurntSushi okay this is ready for review!


---

_Comment by @andyleejordan on 2016-09-25 03:03_

Hm my test failures are due to a few runners swapping the order in which the output was written.


---

_Comment by @BurntSushi on 2016-09-25 03:04_

Run with -j1.

On Sep 24, 2016 11:03 PM, "Andrew Schwartzmeyer" notifications@github.com
wrote:

> Hm my test failures are due to a few runners swapping the order in which
> the output was written.
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/pull/42#issuecomment-249399707,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34gPWaSfGYAvmqG4HeVpcGLS2ZWeCks5qteSbgaJpZM4KFiaJ
> .


---

_Comment by @andyleejordan on 2016-09-25 03:23_

Sadly, while `-j1` does make it deterministic, it does not account for ordering being different across platforms, which caused most of the failures in [build #168](https://travis-ci.org/BurntSushi/ripgrep/builds/162517709) (although a couple failed with very odd panics on tests these changes didn't touch).

Since I don't actually need to test with multiple files... I'm going the simpler route and just testing with `sherlock`. Slightly less robust; much less prone to pointless failure.


---

_Comment by @BurntSushi on 2016-09-25 03:25_

Sounds good to me!

On Sep 24, 2016 11:23 PM, "Andrew Schwartzmeyer" notifications@github.com
wrote:

> Sadly, while -j1 does make it deterministic, it does not account for
> ordering being different across platforms, which caused most of the
> failures in build #168
> https://travis-ci.org/BurntSushi/ripgrep/builds/162517709 (although a
> couple failed with very odd panics on tests these changes didn't touch).
> 
> Since I don't actually need to test with multiple files... I'm going the
> simpler route and just testing with sherlock. Slightly less robust; much
> less prone to pointless failure.
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/pull/42#issuecomment-249400293,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34jSjzwepbCSJGJynrRbxvbC6diNOks5qtelCgaJpZM4KFiaJ
> .


---

_Comment by @andyleejordan on 2016-09-25 03:48_

So, this is admittedly strange, but Travis CI appears to be caching the built targets (and so left around my created test file `file` from [build #168](https://travis-ci.org/BurntSushi/ripgrep/builds/162517709) on the next run [build #169](https://travis-ci.org/BurntSushi/ripgrep/builds/162518787), which didn't expect it), causing failures. This appears to be what's happening though because when I added a `cargo clean` to the CI scripts, they're all now passing [build #170](https://travis-ci.org/BurntSushi/ripgrep/builds/162519607). I can squash that into this commit if you agree.


---

_Comment by @BurntSushi on 2016-09-25 04:21_

That's fine sure, although I haven't seen that happen before!

On Sep 24, 2016 11:48 PM, "Andrew Schwartzmeyer" notifications@github.com
wrote:

> So, this is admittedly strange, but Travis CI appears to be caching the
> built targets (and so left around my created test file file from build
> #168 https://travis-ci.org/BurntSushi/ripgrep/builds/162517709 on the
> next run build #169
> https://travis-ci.org/BurntSushi/ripgrep/builds/162518787, which didn't
> expect it), causing failures. This appears to be what's happening though
> because when I added a cargo clean to the CI scripts, they're all now
> passing build #170
> https://travis-ci.org/BurntSushi/ripgrep/builds/162519607. I can squash
> that into this commit if you agree.
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/pull/42#issuecomment-249401017,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34ncj9yfag16hKbsOupFKj9EEQO0Qks5qte8dgaJpZM4KFiaJ
> .


---

_Merged by @BurntSushi on 2016-09-25 18:53_

---

_Closed by @BurntSushi on 2016-09-25 18:53_

---

_Comment by @BurntSushi on 2016-09-25 18:53_

I just reviewed it and this is high quality work. Thank you so much. :-)

Your code refactoring is also going to help fix #77. Yay!


---

_Branch deleted on 2016-09-25 19:30_

---

_Comment by @andyleejordan on 2016-09-25 19:31_

Thank you! I really wanted this feature, and missed programming in Rust a lot. Cheers!


---
