```yaml
number: 2519
title: "Typo in the description of the `--field-match-separator` option"
type: issue
state: closed
author: saf-dmitry
labels:
  - doc
  - rollup
assignees: []
created_at: 2023-05-26T14:37:55Z
updated_at: 2023-11-25T20:03:56Z
url: https://github.com/BurntSushi/ripgrep/issues/2519
synced_at: 2026-01-12T16:13:24Z
```

# Typo in the description of the `--field-match-separator` option

---

_@saf-dmitry_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

From Github binary release.

#### What operating system are you using ripgrep on?

macOS v10.15.7

#### Describe your bug.

Typo in the manpage: In the description of the `--field-match-separator` option the default value is incorrectly stated as `-`. The correct value should be `:`.

#### What are the steps to reproduce the behavior?

Issue

```shell
man rg
```

and search for the above-mentioned text.

#### What is the actual behavior?

See above.

#### What is the expected behavior?

In the description of the `--field-match-separator` option the default value should be stated as `:`.


---

_Comment by @AgustinRamiroDiaz on 2023-08-01 19:56_

This seems to be updated [in the code](https://github.com/BurntSushi/ripgrep/blob/341a19e0d05bc6f0cabeb2dc756b37cfc047f8f0/crates/core/app.rs#L1256). I'm not sure how the man pages are generated :thinking: 

---

_Comment by @AgustinRamiroDiaz on 2023-08-01 21:11_

@BurntSushi I see in the files
https://github.com/BurntSushi/ripgrep/blob/341a19e0d05bc6f0cabeb2dc756b37cfc047f8f0/ci/build-deb#L6-L8

I've managed to build the artifacts `_rg, rg.1, rg.bash, rg.fish`. Where am I supposed to upload them in github? Is this something only maintainers can modify in the release artifacts?

---

_Comment by @BurntSushi on 2023-08-01 21:21_

> Where am I supposed to upload them in github? Is this something only maintainers can modify in the release artifacts?

Are you asking how to upload artifacts to someone else's project? No of course you can't do that and of course only maintainers can. Otherwise anyone could insert whatever they want into a release...

Maybe I'm misunderstanding your question?

---

_Comment by @AgustinRamiroDiaz on 2023-08-02 12:11_

@BurntSushi I'll separate this in 2 questions to be clearer

a) In https://github.com/BurntSushi/ripgrep/blob/341a19e0d05bc6f0cabeb2dc756b37cfc047f8f0/ci/build-deb#L6-L8 it mentions that "the resulting dpkg is uploaded to GitHub via the web UI". What Github web UI is this referring to?

b) Not really a question: If the previous answer is "github web UI == github release page", then I guess that this issue can only be done by maintainers due to permissions

---

_Comment by @BurntSushi on 2023-08-02 13:29_

For (A), the UI is this page (which you probably don't have access to): https://github.com/BurntSushi/ripgrep/releases/edit/13.0.0

Here are a couple screenshots. You can see there is a place for me to upload release artifacts:

![release-edit-1](https://github.com/BurntSushi/ripgrep/assets/456674/cdcd9578-29e8-4e5a-9fd7-94e2a14ab0d4)

![release-edit-2](https://github.com/BurntSushi/ripgrep/assets/456674/44fd869e-c208-404e-a440-f2245012d21a)

As for (B), yes. It doesn't get fixed until the next release, just like any other change.

---

_Label `doc` added by @BurntSushi on 2023-09-18 21:04_

---

_Label `rollup` added by @BurntSushi on 2023-11-22 01:09_

---

_Closed by @BurntSushi on 2023-11-25 20:03_

---
