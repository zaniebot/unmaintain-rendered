```yaml
number: 1476
title: Add flags for searching only in test code and excluding test code
type: issue
state: closed
author: thanasis2028
labels:
  - question
assignees: []
created_at: 2020-02-02T12:42:09Z
updated_at: 2020-02-02T13:12:42Z
url: https://github.com/BurntSushi/ripgrep/issues/1476
synced_at: 2026-01-12T16:13:23Z
```

# Add flags for searching only in test code and excluding test code

---

_@thanasis2028_

#### What version of ripgrep are you using?

ripgrep 11.0.2 (rev 32057b3883)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Compiled from latest master

#### What operating system are you using ripgrep on?

Debian 9 (stretch)

#### Describe your question, feature request, or bug.

I was thinking that it would be useful to be able to search for text in rust code files, while being able to distinguish whether the found text is in a `#[test]` section, and filter it out.

For example, one might want to search for uses of the `println!` macro and replace it with something else in production, but doesn't care about println uses in tests.

Similarly, one might want to find and eliminate assertions in non-test sections.

So, I implemented 2 new flags `--excludetests` and `--testsonly` that do that (https://github.com/thanasis2028/ripgrep). If there is interest in this feature, I could create a PR.

4 new test cases have been implemented and pass, along with all current test cases.


---

_Comment by @BurntSushi on 2020-02-02 13:12_

It's a cool feature, but I've rejected features like this in the past. For the most part, ripgrep just isn't quite the right tool for this. I grant that it works in your specific case, but if the implementation isn't correct, then this becomes a vector for bug reports and potentially significant maintenance burden going forwards.

Examples of concrete problems with your current implementation:

* It is primarily implemented inside of `grep-regex`, which means users of PCRE2 will not get this feature.
* Test detection is based on a simple textual test, but 100% correct test detection needs more than that. There could be other cfg knobs, as a simple example.
* The requirement that multiline mode be enabled is fairly severe. It either requires memory maps to be used or requires reading the entire file into memory. I'd rather not add new features to ripgrep that require this mode.

I would encourage you to maintain your patch though, if you find it useful!

---

_Closed by @BurntSushi on 2020-02-02 13:12_

---

_Label `question` added by @BurntSushi on 2020-02-02 13:12_

---
