```yaml
number: 715
title: Add armhf build to Travis CI
type: pull_request
state: merged
author: lilianmoraru
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2017-12-17T14:16:10Z
updated_at: 2017-12-18T21:56:53Z
url: https://github.com/BurntSushi/ripgrep/pull/715
synced_at: 2026-01-12T18:23:13Z
```

# Add armhf build to Travis CI

---

_@lilianmoraru_

The purpose of this is so that `ripgrep` will create the "release" binary for the most popular ARM target - but I did not manage to test if the binary is correctly pushed to the "releases" page...

---

_Comment by @lilianmoraru on 2017-12-17 16:07_

While trying to make sure that ARM tests are not run in the CI, I observed that `cargo clean` was being run only on `ripgrep` and not on all the subprojects(in `run_test_suite()`) - while analyzing if this is ok, I observed that every subproject gets its own `target` directory which means that these don't share dependencies.

Using cargo workspaces here should improve the build speed.

---

_@okdana reviewed on 2017-12-17 19:36_

---

_Review comment by @okdana on `ci/utils.sh`:58 on 2017-12-17 19:36_

Having a shell function return `1` on success/true and `0` on failure/false is pretty unorthodox. Also there's a lot of unnecessary forking here. It doesn't really matter, it's not like it's performance-critical or anything, but (in the spirit of some of your other changes) it would probably be more idiomatic to do something like this:

```sh
case "${TARGET}" in
  i686-unknown-netbsd) return 1 ;;
  i686*|x86_64*)       return 0 ;;
esac
return 1
```

Then you can use it like:

```sh
if is_ssse3_target; then
  # do SSSE3 stuff
else
  # do not-SSSE3 stuff
fi
```

---

_@okdana reviewed on 2017-12-17 19:38_

---

_Review comment by @okdana on `ci/before_deploy.sh`:10 on 2017-12-17 19:38_

Related to my other comment: I don't believe this construction will work reliably anyway — if `is_ssse3_target` returns `1` here, it will cause the script to bail because of `set -e`. The effect of `set -e` needs to be suppressed by putting it in the `if` condition (or using `||` or `&&`)

---

_@lilianmoraru reviewed on 2017-12-17 21:53_

---

_Review comment by @lilianmoraru on `ci/utils.sh`:58 on 2017-12-17 21:53_

> Having a shell function return 1 on success/true and 0 on failure/false is pretty unorthodox.

That's what I thought too but then decided to make it more traditional programming languages-like... :)
Good point though and I will change it.

Btw, I used `$?` because I tried to do:
```sh
is_ssse3_target() { return 0; }
test is_ssse3_target && echo "TRUE" || echo "FALSE"
```
And this would not work, but I see that this works:
```sh
if is_ssse3_target; then echo "TRUE"; else echo "FALSE"; fi
```
The alternative was to not use early-return and just `echo true/false`.

---

_Review comment by @BurntSushi on `ci/script.sh`:31 on 2017-12-18 13:21_

Does this mean we aren't running tests on arm? Why?

---

_Review comment by @BurntSushi on `ci/utils.sh`:59 on 2017-12-18 13:22_

I think you can just remove this comment. As @okdana said, `1` for `false` is standard in shell scripts.

---

_@BurntSushi reviewed on 2017-12-18 13:23_

Thanks for doing this! I'm willing to give this a go, but as I mentioned in #676, my ability to maintain this is not set in stone. If it causes headaches, then I'll like just remove it.

Also, could you add `Fixes #676` to the bottom of your commit message on its own line? Thanks.

---

_Comment by @BurntSushi on 2017-12-18 13:24_

Also, thanks for the tip about workspaces. I will do it soon.

---

_@lilianmoraru reviewed on 2017-12-18 19:37_

---

_Review comment by @lilianmoraru on `ci/script.sh`:31 on 2017-12-18 19:37_

In order to run the tests on ARM, you would need to setup `qemu` inside the Travis CI.
This is doable(I've seen it done somewhere) but I don't currently have the knowledge(nor would I like to hold this PR for that) to easily add this feature.

It would definitely add overhead - you would probably need to limit it to run only when preparing a release or something like this.

---

_Comment by @lilianmoraru on 2017-12-18 19:52_

> Also, thanks for the tip about workspaces. I will do it soon.

The thing is almost a one-liner(works on `1.17`):
```
[workspace]
members = [ "grep", "globset", "ignore", "termcolor", "wincolor" ]
```

But you need to remove `[profile.release]` from `ignore`.

For the full "Win", if all the packages would have the same versions of the dependencies than you would have full re-use.

At some point the CI should also be switched to use `cargo build --all`.

---

_Comment by @lilianmoraru on 2017-12-18 20:02_

Does not seem complicated: https://github.com/PJK/libcbor/blob/master/.travis-qemu.sh
Edit, or this: http://blog.kragniz.eu/raspbian-on-travis-ci/

---

_Comment by @lilianmoraru on 2017-12-18 20:39_

I pushed another change:
Installing specifically `gcc-4.8-arm-linux-gnueabihf` because if Travis moves to `Ubuntu 16.04` at some point, `gcc-arm-linux-gnueabihf` would become `GCC 5.3`, which would be incompatible with RPI's `GCC 4.9`(glibc API/ABI).

---

_@BurntSushi reviewed on 2017-12-18 21:25_

---

_Review comment by @BurntSushi on `ci/script.sh`:31 on 2017-12-18 21:25_

Oh I see, I understand. Thanks for explaining that to me!

---

_@BurntSushi approved on 2017-12-18 21:26_

All right, let's do this. I'll try to keep a close eye on this during the next release.

---

_Merged by @BurntSushi on 2017-12-18 21:26_

---

_Closed by @BurntSushi on 2017-12-18 21:26_

---

_Comment by @lilianmoraru on 2017-12-18 21:45_

@BurntSushi Speaking of overhead. What's your opinion on removing the `cargo clean` step?
I have not yet encountered problems with incremental compilation unless you are building a C/C++ application through `build.rs`(which `ripgrep` doesn't).
That would allow to ask Travis to cache the `target` directory and have considerably better build speeds.

---

_Comment by @BurntSushi on 2017-12-18 21:56_

@lilianmoraru It sounds fine to me. I doubt there is any strong reason for it to be there now. It's probably there because I copied it from somewhere else, and it was probably there because I was debugging something and just didn't remove it. :)

---
