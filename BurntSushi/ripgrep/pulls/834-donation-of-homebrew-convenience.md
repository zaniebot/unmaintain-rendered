```yaml
number: 834
title: Donation of homebrew-convenience
type: pull_request
state: closed
author: Byron
labels: []
assignees: []
base: master
head: master
created_at: 2018-02-24T12:07:51Z
updated_at: 2018-03-10T13:17:36Z
url: https://github.com/BurntSushi/ripgrep/pull/834
synced_at: 2026-01-12T18:23:13Z
```

# Donation of homebrew-convenience

---

_@Byron_

Hi @BurntSushi ,

first of all let me say that I use `rg` every day, it replaced everything else and it's good to know to use the best of the best! Thanks you so much for making it, and even more for maintaining it!

Homebrew for quite some time was a black box to me! I was using it, but had no idea how to bring my own programs in. A random perusal of the `ripgrep` repository finally gave me the hints I needed to implement a tap for [`termbook`][termbook].

After a few releases I realized that I was quite annoyed by the additional maintenance burden - now I had to wait for travis to build the binaries, download the archives, and update hashes.
Judging from you commit-history on `pkg/brew`, it's a manual process for you, too.

So I thought: let's fix this, and make it as easy as a single program invocation, fire and forget.
For `termbook` it's just `make update-homebrew`, and for this repository, and due to a lack of a better place, it is the following:

```bash
./ci/update-homebrew-formula.sh 0.8.1
```

The script is to be executed manually for now, and can certainly serve as basis for even more automation.

Please let me know if you find it useful, or if you need some changes to make it more suitable for `ripgrep`.

*Maybe I can also ask you how it's possible to get a program into homebrew core? Is it as easy as a PR?*

[termbook]: https://github.com/Byron/termbook

---

_Renamed from "Donation of convenience" to "Donation of homebrew-convenience" by @Byron on 2018-02-24 12:08_

---

_Comment by @okdana on 2018-02-24 22:04_

>Maybe I can also ask you how it's possible to get a program into homebrew core? Is it as easy as a PR?

Yes, but they do have certain criteria. They won't add a formula unless the software is stable, widely used, and well maintained; they often judge that based on things like GitHub star/fork counts and commit frequency. They also discourage submitting your own project, or at least they consider it a red flag.

* [Acceptable formulae](https://github.com/Homebrew/brew/blob/master/docs/Acceptable-Formulae.md)
* [Formula contribution guide lines](https://github.com/Homebrew/homebrew-core/blob/master/CONTRIBUTING.md)
* [Previous new-formula PRs for reference](https://github.com/Homebrew/homebrew-core/pulls?q=is%3Apr+label%3A%22new+formula%22&utf8=%E2%9C%93)

---

_Comment by @Byron on 2018-02-25 06:43_

Thanks a lot, as tap will do (*for now* :D).

---

_Review comment by @BurntSushi on `ci/update-homebrew-formula.sh`:36 on 2018-02-26 11:59_

What is the purpose of this loop? Why not just `curl` the two tar archives and call it a day?

---

_@BurntSushi reviewed on 2018-02-26 12:00_

Thanks! I left a comment, but yeah, I might take this! I haven't been overly burdened by updating them manually, and there is a script in the `ci` directory that gives me the sha256 sums.

---

_@Byron reviewed on 2018-02-26 19:14_

---

_Review comment by @Byron on `ci/update-homebrew-formula.sh`:36 on 2018-02-26 19:14_

I think it's more easily understood when it's clear how I am using it.

My goal is to not have the feeling to have to wait for something.

I `git tag X` and push tags. Now travis will eventually produce the binaries, in any order. Instead of waiting around for the right time, I run `./ci/update-homebrew-formula.sh X` and just let it wait for me.

Eventually, after a long enough time, I go back and commit and push the updated brew file.
Something that might be useful here is to also `commit && push` in the script itself, so that it is truly fire & forget.



---

_Comment by @BurntSushi on 2018-03-10 13:17_

@Byron Thanks for submitting this PR, I really appreciate it. :-) I think I'm just going to stick with what I have for now, but I may make improvements inspired by your work. I do admittedly like the idea of running the script and just letting it do its own thing.

---

_Closed by @BurntSushi on 2018-03-10 13:17_

---

_Reopened by @BurntSushi on 2018-03-10 13:17_

---

_Closed by @BurntSushi on 2018-03-10 13:17_

---
