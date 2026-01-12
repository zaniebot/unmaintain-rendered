```yaml
number: 3139
title: Inconsistent behaviour when using --files-with-matches vs without
type: issue
state: closed
author: F8enjoyer
labels:
  - bug
assignees: []
created_at: 2025-09-05T18:39:12Z
updated_at: 2025-09-22T13:12:17Z
url: https://github.com/BurntSushi/ripgrep/issues/3139
synced_at: 2026-01-12T16:13:25Z
```

# Inconsistent behaviour when using --files-with-matches vs without

---

_@F8enjoyer_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)

### How did you install ripgrep?

cargo install ripgrep --features pcre2



### What operating system are you using ripgrep on?

Windows 10 & 11

### Describe your bug.

Output when using switch `--files-with-matches` appears to leave out files, that otherwise show matches when the switch is not present.

### What are the steps to reproduce the behavior?

Compare the output of these two commands on the attached files:

`rg --pcre2 --iglob *.txt --multiline "(?s)Start(?!.*thing1)(?=.*thing2)(?!.*thing3)"`
`rg --pcre2 --iglob *.txt --multiline "(?s)Start(?!.*thing1)(?=.*thing2)(?!.*thing3)" --files-with-matches`

[issue.txt](https://github.com/user-attachments/files/22177751/issue.txt)
[normal.txt](https://github.com/user-attachments/files/22177755/normal.txt)

Base64-encoded version of issue.txt (just in case attachment mangles it):
> U3RhcnQgCiAgIAoKICAgWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFgKICAgWVlZWVlZWVlZWVlZ
> WVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZ
> WVlZWVkKICAgCiAgICAgIHRoaW5nMiAKCgo=

Base64 normal.txt:
> U3RhcnQKCiAgdGhpbmcyIAoK

### What is the actual behavior?

Without `--files-with-matches`:
> normal.txt
> 1:Start
> 
> issue.txt
> 1:Start

(Note: When run in a console, the match for the match for normal.txt is colored red, but the match in issue.txt is not)

With `--files-with-matches`:
> normal.txt

### What is the expected behavior?

I would expect the `--files-with-matches` version of the command to list both normal.txt and issue.txt

---

_Comment by @F8enjoyer on 2025-09-05 19:17_

I can simplify the steps to reproduce - the negative lookaheads are not necessary to trigger the issue:

`rg --pcre2 --iglob *.txt --multiline "(?s)Start(?=.*thing2)"`
`rg --pcre2 --iglob *.txt --multiline "(?s)Start(?=.*thing2)" --files-with-matches`

However, the positive lookahead is necessary; the following does _not_ trigger the issue:

`rg --pcre2 --iglob *.txt --multiline "(?s)Start.*thing2" --files-with-matches`


---

_Comment by @F8enjoyer on 2025-09-06 23:08_

I think I have bisected the issue down to a specific commit.

This version _does not_ have the issue:
https://github.com/BurntSushi/ripgrep/commit/656aa126492b04f1ac8b0406f589b6de4c4b7d60

The next version _does_ have the issue:
https://github.com/BurntSushi/ripgrep/commit/efd9cfb2fc1f0233de9eda4c03416d32ef2c3ce8

(This commit message mentions both multiline and PCRE2, so it seems plausible?)
**Edit:** This would make the first affected build v13.0.0

---

_Comment by @BurntSushi on 2025-09-06 23:12_

If that is indeed it, there is definitely an open issue for this somewhere. This sort of bug was known when I hacked around a different bug.

Anyway, this bug is unlikely to be fixed any time soon.

---

_Comment by @F8enjoyer on 2025-09-06 23:47_

Ok, understood.

Meanwhile, I've tried to express this as a regression test:
```
rgtest!(r3139_multiline_lookahead_files_with_matches, |dir: Dir, mut cmd: TestCommand| {

    // Only PCRE2 supports look-around.
    if !dir.is_pcre2() {
        return;
    }
    dir.create("test", "Start \n   \n\n   XXXXXXXXXXXXXXXXXXXXXXXXXX\n   YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY\n   \n      thing2 \n\n");
    
    cmd
        .args(&["--multiline", "--pcre2", "--files-with-matches", r"(?s)Start(?=.*thing2)", "test"])
        .assert_exit_code(0);
        
    eqnice!("test\n", cmd.stdout());
});
```



---

_Label `bug` added by @BurntSushi on 2025-09-21 18:02_

---

_Comment by @BurntSushi on 2025-09-21 18:03_

The issue I was referring to above was #2528.

I managed to find a hacky fix for this _specific_ issue in #3154 without resolving the underlying problem identified in #2528.

Thank you for the nice regression test!

---

_Closed by @BurntSushi on 2025-09-22 13:12_

---
