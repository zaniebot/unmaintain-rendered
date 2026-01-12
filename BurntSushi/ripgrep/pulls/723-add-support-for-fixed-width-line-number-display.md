```yaml
number: 723
title: "Add support for fixed width line number display (fixes #544)"
type: pull_request
state: merged
author: balajisivaraman
labels: []
assignees: []
merged: true
base: master
head: fix_544
created_at: 2017-12-30T15:35:13Z
updated_at: 2018-01-01T14:10:21Z
url: https://github.com/BurntSushi/ripgrep/pull/723
synced_at: 2026-01-12T18:23:13Z
```

# Add support for fixed width line number display (fixes #544)

---

_@balajisivaraman_

As per the discussion on #544, this is my initial attempt at adding support for fixed width line numbers. Please review and let me know if things can be improved upon.

A few thoughts I had as I worked on this:

- Rust's `format!` macro currently has no way of providing a custom padding character as a parameter. You have to provide it directly in the format string itself. (As per [this](https://doc.rust-lang.org/std/fmt/#syntax), only the width can be provided as a parameter.)
- This meant that adding support for format strings as suggested in the issue discussion proved to be difficult. Right now, the argument only accepts a number. It seems that we'll have to write a custom code for left padding or rely on a library to achieve this. I'm open to suggestions on this one.
- Should we handle possible combinations of CLI args? In the long help message for `line-number-width`, I've mentioned that it has no effect if `--no-line-number` is enabled, which is very obvious. Should I handle any other cases specific to this newly introduced argument to ensure nothing breaks?

Thanks in advance!

---

_Comment by @okdana on 2017-12-30 19:37_

In order to handle it as i suggested, i think you would want to have it check the argument string before conversion to `usize` and set a boolean in the `Args` struct to `arg.starts_with("0")`. Then you would pass that boolean to the printer and just have an `if` condition determine the format string to use

But it doesn't really matter either way obv. Would be a cute feature but certainly not important

Will add further comments on the diff

---

_@okdana reviewed on 2017-12-30 19:43_

---

_Review comment by @okdana on `complete/_rg`:48 on 2017-12-30 19:43_

The 'exclusive-or' list at the beginning should exclude all options that this one makes irrelevant. So we would probably want something like this at the very least:

```
(-l -N --line-number-width --no-line-number)
```

Then the definition for `-N` should be updated similarly to exclude `--line-number-width`

(I think we should also add a few more, like `-c`, but it seems i haven't accounted for them in the past)

---

_@okdana reviewed on 2017-12-30 19:46_

---

_Review comment by @okdana on `complete/_rg`:48 on 2017-12-30 19:46_

Oh, i was going to comment that i don't think a short option is necessary, but it seems you reached that conclusion too, since you didn't actually add one. You should remove the `-l+` (which is used by something else anyway) then. So it'd be just one string, `(...)--line-number-width[...]:...`

ETA: And if you remove the `-l` you don't need to reference `--line-number-width` in the list any more; it's included by default

---

_@BurntSushi requested changes on 2017-12-30 21:33_

This looks great to me! Nice PR. :-)

My main concern here is supporting arguments like `06` to mean "left pad, using zeroes." Right now, in this PR, I believe `06` will be parsed as `6`, which we will left pad with spaces. I am fine with starting there, but I very much expect someone to come along and request zero padding as well. At that point, if we change `06` to left pad zeroes, then it would technically be a breaking change, which I'd like to avoid.

As a simple work-around to this, I'd suggest return an error if the number starts with a `0`.

If and when we add a custom format character, we shouldn't use some other library to do left padding for us. Re-implementing this specific part of the `format!` macro should be a very short helper function.

---

_@balajisivaraman reviewed on 2017-12-31 04:55_

---

_Review comment by @balajisivaraman on `complete/_rg`:48 on 2017-12-31 04:55_

Ah, it is with some embarrassment that I have to admit I was very excited by raising what would be my first PR in an OSS project that I failed to get this part of the change absolutely right. 😞  I'm sorry about that. Thanks for clarifying. I will make the changes you suggested.

And yes, I also felt like we didn't need a short option for this. I briefly considered using `-w` but decided against it.

---

_Comment by @balajisivaraman on 2017-12-31 04:59_

@BurntSushi, I understand your concern. When I was making the change, I thought we wanted it to be very general and support any padding character, which is why I looked into the `format!` macro.

However, if it is always going to be a choice between 0 or space, I could do what @okdana suggested. Then we can fail if the first character is anything other than a 0.

Let me know if this works for you and I can update the PR accordingly.

---

_Comment by @BurntSushi on 2017-12-31 05:05_

I would suggest just starting with supporting spaces. We can add more support as needed, but we will need to return an error if the padding width starts with a zero. In fact, we should return an error if the width starts with anything other than 1-9, but that is already true in this PR since it would fail to parse as an integer.

---

_Comment by @balajisivaraman on 2017-12-31 05:06_

@BurntSushi, Okay, I understand. Will update the PR accordingly. Thanks for the comments.

---

_Comment by @BurntSushi on 2017-12-31 17:11_

@balajisivaraman This also needs to update the man page, which is done in this file: https://github.com/BurntSushi/ripgrep/blob/162e085b98f8f2c627a92402d2e38dda04fc3e48/doc/rg.1.md --- If you have pandoc on your system, then run the [`convert-to-man`](https://github.com/BurntSushi/ripgrep/blob/162e085b98f8f2c627a92402d2e38dda04fc3e48/doc/convert-to-man) script and commit the changes to `rg.1`. Thanks!

---

_Comment by @balajisivaraman on 2017-12-31 20:13_

@BurntSushi, Oops! For some reason, I thought that was something automatically provided by `clap`. I've now updated the PR for the man pages as well as you suggested.

ETA: Do we need to add the issue number in the commit message, or is it enough to add it to the PR only? I see that it ends up creating a lot of noise in the actual issue thread. (Apologies if I did it wrong and it created noise in this issue's thread.)

---

_Review comment by @BurntSushi on `src/app.rs`:586 on 2017-12-31 22:28_

Sorry to drag this out, but can you add a test for this error? There should be other examples of tests in `tests/tests.rs` that check that ripgrep errors out.

---

_@BurntSushi reviewed on 2017-12-31 22:28_

---

_Comment by @balajisivaraman on 2018-01-01 04:25_

Sorry, that is something I should've known better and added when I made that change itself. It is done now.

---

_Merged by @BurntSushi on 2018-01-01 14:00_

---

_Closed by @BurntSushi on 2018-01-01 14:00_

---

_Comment by @BurntSushi on 2018-01-01 14:00_

@balajisivaraman Awesome! Thanks so much. Great work!

---

_Branch deleted on 2018-01-01 14:10_

---
