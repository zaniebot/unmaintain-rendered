```yaml
number: 433
title: Add dfa-size-limit and regex-size-limit arguments
type: pull_request
state: closed
author: tiehuis
labels: []
assignees: []
base: master
head: issue-362
created_at: 2017-04-01T22:15:30Z
updated_at: 2017-04-15T10:51:03Z
url: https://github.com/BurntSushi/ripgrep/pull/433
synced_at: 2026-01-12T18:23:13Z
```

# Add dfa-size-limit and regex-size-limit arguments

---

_@tiehuis_

See #362.

 - The macro is not really to my taste but I wanted to see if you were happy with it first or had any ideas otherwise. I did try creating a generic function with the required trait bounds but the generic multiplication was the stopping point here for me. Also, if using a `saturating_mul` this is further pronounced since there is currently no trait in std which these are bound to. The `Saturating` trait on the [num](https://github.com/rust-num/num) also only applies for addition to.

- Removed the straight-forward multiplications and replaced with saturating versions. This avoids the potential overflow. Added a test case for this also to ensure it is handled properly. Since these arguments are always some memory bound (as per the suffix) a saturating limit makes the most sense. That being said, it is very (very) unlikely for the current width types.

---

_@kpp reviewed on 2017-04-03 16:45_

---

_Review comment by @kpp on `src/args.rs`:322 on 2017-04-03 16:45_

You can split this macro into 2 functions: validator and value converter. See [an example of validator](https://github.com/kbknapp/clap-validators/blob/master/src/num/mod.rs)

---

_Review comment by @BurntSushi on `src/app.rs`:350 on 2017-04-09 12:26_

I think this should say `10M` instead of `10Mb`. (Even so, the `Mb` suffix means "megabit", which is otherwise wrong. :-))

---

_Review comment by @BurntSushi on `src/app.rs`:471 on 2017-04-09 12:27_

Same here. `10M` instead of `10Mb`.

---

_Review comment by @BurntSushi on `src/args.rs`:322 on 2017-04-09 12:29_

I'm not sure this macro is necessary, and I'd really like to find a way to get rid of it. Can it be written to specifically return a `u64` and then just convert the result to a `usize`? (That conversion should return an error if the `u64` is greater than [`std::usize::MAX`](https://doc.rust-lang.org/std/usize/constant.MAX.html).)

---

_@BurntSushi requested changes on 2017-04-09 12:31_

Thanks for this! This looks great. My only request is that we get rid of the macro. See the comments. :-)

I'd also argue that if overflow occurs, it should return an error to the user. But that doesn't need to be solved in this PR. Saturating is better than what we were doing. :-) Thanks for that!

---

_Comment by @BurntSushi on 2017-04-09 12:55_

Could you also change the commit message to say, `Fixes #362` so that github properly connects the PR with the issue? Thanks.

---

_Comment by @tiehuis on 2017-04-10 07:08_

Made fixes as per your suggestions and squashed into a single commit.

I've also added overflow detection instead of the saturation as you argued. Figured that it was better to get it as intended right now rather than leaving it for later.

Added an extra test specifically for 32-bit systems for when `usize < u64`.

---

_Comment by @BurntSushi on 2017-04-12 22:17_

@tiehuis Thanks again for working on this! I merged this PR in https://github.com/BurntSushi/ripgrep/commit/66efbad871620fe2dbe675438fdc8d9922e72826. I ended up making a few cosmetic changes to make the code more consistent with the style of the rest of the code. Nice work! :-)

---

_Closed by @BurntSushi on 2017-04-12 22:17_

---

_Branch deleted on 2017-04-13 07:11_

---

_Comment by @kpp on 2017-04-15 09:55_

@BurntSushi stop closing pull requests. There is a button to merge the code with any message you like.

---

_Comment by @BurntSushi on 2017-04-15 10:51_

@kpp If you have a problem with the way I'm running the project and can suggest a better alternative, then please file an issue.

---
