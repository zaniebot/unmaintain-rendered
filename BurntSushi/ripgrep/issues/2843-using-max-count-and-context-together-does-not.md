```yaml
number: 2843
title: "Using `--max-count` and `--context` together does not correctly display syntax highlighting for matched characters"
type: issue
state: closed
author: 5HT2
labels: []
assignees: []
created_at: 2024-06-24T22:26:57Z
updated_at: 2025-09-23T02:15:55Z
url: https://github.com/BurntSushi/ripgrep/issues/2843
synced_at: 2026-01-12T16:13:25Z
```

# Using `--max-count` and `--context` together does not correctly display syntax highlighting for matched characters

---

_@5HT2_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+NEON
simd(runtime):+NEON

PCRE2 10.42 is available (JIT is available)
```

### How did you install ripgrep?

Homebrew

### What operating system are you using ripgrep on?

```
$ sw_vers
ProductName:		macOS
ProductVersion:		14.2.1
BuildVersion:		23C71
$ uname -r
23.2.0
```

### Describe your bug.

Using both of these flags together produces the expected output, for example, 2 lines of context with 1 match, but still highlights other matches that were not included due to `--max-count`.

_Because_ these matches are only being included because of `--context`, it's unintuitive and confusing to the user to also highlight the text for the matches that weren't actually included.

Related: https://github.com/BurntSushi/ripgrep/issues/1380

### What are the steps to reproduce the behavior?

Run `rg Pro -m 1 -C 2 therapeutic.txt`, you can find this file here:
https://github.com/5HT2C/doses-logger/blob/master/therapeutic.txt

It's also fairly easy to produce a less specific test case yourself on any type of sorted data (where you are just matching the beginning and have context enabled).

### What is the actual behavior?

Notice how the lines are also suffixed with `:` before the line number instead of `-`.

<img width="794" alt="image" src="https://github.com/BurntSushi/ripgrep/assets/17222512/1c0bd437-4fe9-4193-87ac-8f2d11382f7f">


### What is the expected behavior?

I feel like this would be much more intuitive:

![image](https://github.com/BurntSushi/ripgrep/assets/17222512/9895108f-38c1-4c4b-bf7d-ca08ba0d887a)

I do understand that there is an argument against the behavior I am proposing, wherein a user might be confused by non-highlighted matches, but I feel like that would be encountered less often than the current behavior, and in the proposed case the issue would lie in "user has forgotten they set the `--max-context` flag" rather than the program's behavior being inconsistent / confusing.

---

_Closed by @BurntSushi on 2025-09-23 02:13_

---

_Comment by @BurntSushi on 2025-09-23 02:15_

I appreciate the request here, but I think the existing behavior is the most reasonable. If I abided your request, there would now be cases where ripgrep could emit a match that isn't actually treated it as a match. I find that stranger than emitting a larger number of matches than `--max-count` specifies.

The tipping point for me here is that extra contextual lines are specifically requested. Those contextual lines could contain additional matches beyond the limit, regardless of whether ripgrep recognizes them as such. For ripgrep to _not_ recognize them as a match is, IMO, being misleading about what ripgrep actually found.

I added a note about this to the docs of `-m/--max-count`.

---
