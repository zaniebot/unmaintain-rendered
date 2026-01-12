```yaml
number: 2932
title: "Inconsistency with empty lines output and `--only-matching`"
type: issue
state: closed
author: alvin55531
labels:
  - duplicate
assignees: []
created_at: 2024-11-13T04:11:27Z
updated_at: 2024-11-14T01:59:25Z
url: https://github.com/BurntSushi/ripgrep/issues/2932
synced_at: 2026-01-12T16:13:25Z
```

# Inconsistency with empty lines output and `--only-matching`

---

_@alvin55531_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 13.0.0 

### How did you install ripgrep?

apt repository

### What operating system are you using ripgrep on?

Windows 10 with WSL 2.3.24.0 
Android with Termux 

### Describe your bug.

I have a regex which searches whether a certain set of keywords exist within a single block (delimited by `END`)

When I put the regex and file contents into <regex101.com> (with PCRE2 engine), it would match from the very end of line 1 to the end of line 3. With `--only-matching`, there should be zero characters matched for line 1, thus it should output nothing. Line 2 only matches line boundaries, so it should also output nothing. The only line that should be outputted should be line 3 (unless I misunderstood ripgrep's behavior). 

When I use `--count --include-zero`, it would show no matches (`test.md:0`), which I believe means ripgrep is not matching anything at all, yet it is still outputting lines. But it's not outputting *every line* in the file, it's outputting the precise lines that are involved in the match (if ripgrep was in "always printed whole lines" mode).  

I have using the following flags (none of these had any effect):
* `--mmap` and `--no-mmap`
* `--line-buffered` and `block-buffered`
* Using `(?ims)` and not using it
* `--regex-size-limit 1G` 
* `-uuu`

### What are the steps to reproduce the behavior?

I have a test markdown file (called `test.md`) with the following contents:
```
Testword4 END

Testword5 Testword6 
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
Testword4 END
```

This is the search command: 
```
rg --type md --ignore-case --pcre2 --no-heading --with-filename --line-number --multiline --multiline-dotall --only-matching '(?ims)(?:\A|(?<=END))(?=(?:(?!END).)*?testword5)(?=(?:(?!END).)*?testword6)(?=(?:(?!END).)*?testword4)[\s]*?[\S][^\n]*?$'
```

### What is the actual behavior?

```
test.md:1:Testword4 END
test.md:2:
test.md:3:Testword5 Testword6
```

### What is the expected behavior?

```
test.md:3:Testword5 Testword6
```

The thing is, if I make one of the following adjustments as shown below, now it suddenly works as expected (I have no clue why). 

Shortening contents in the block being matched
```
Testword4 END

Testword5 Testword6 

This is a test sentence.
This is a test sentence.
This is a test sentence.
Testword4 END
```

Group the keywords together
```
Testword4 END

Testword5 Testword6 
Testword4 
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
END
```

---

_Comment by @BurntSushi on 2024-11-13 12:08_

Please use the latest release of ripgrep.

Please minimize the `rg` command. 

Please minimize the regex.

(This looks more like a PCRE2 regex question than a ripgrep question. You might have better luck asking in a regex help forum.)

---

_Comment by @alvin55531 on 2024-11-13 21:55_

I have made changes which I discussed further down the comment, but the following paragraph is the central part of the issue: 
The regex works as expected, what ripgrep outputs isn't necessarily wrong per say, I'm just wondering why the results are *inconsistent* with the way ripgrep treats empty lines based on *changes in the input file*. Why does it seem like the `--only-matching` flag gets dropped when I change the input file? Is it a memory problem or the way ripgrep reads files? 

### Changes 

I updated to ripgrep 14.1.1. I simplified the rg command to the following:
```
rg --pcre2 --multiline --multiline-dotall --only-matching '(?=.*Testword4)(?=.*Testword5)[\s]*?[\S][^\n]*?$'
```
It is seeing whether `Testword4` and `Testword5` both exists anywhere in the string, and then matching up to the first non-empty line. 

I also simplified the test file:
```


Testword5  
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
This is a test sentence.
Testword4 
```

Actual results:
```
test.md
1:
2:
3:Testword5
```
Expected:
```
test.md
3:Testword5
```

If I change the test file like this:
```


Testword5
This is a test sentence.
This is a test sentence.
This is a test sentence.
Testword4 
```

Now it outputs the expected results:
```
test.md
3:Testword5
```

---

_Renamed from "Issue with multiline matching with lengthy regex " to "Inconsistency with empty lines output and `--only-matching`" by @alvin55531 on 2024-11-13 21:57_

---

_Comment by @BurntSushi on 2024-11-13 22:23_

This is probably a bug due to

https://github.com/BurntSushi/ripgrep/blob/4b0e5a11b9c5a35fc4553c527051489c5f97ece0/crates/printer/src/lib.rs#L78-L87

and

https://github.com/BurntSushi/ripgrep/blob/4b0e5a11b9c5a35fc4553c527051489c5f97ece0/crates/printer/src/util.rs#L478-L500

I don't think this will be fixed any time soon. I believe there are other extant issues already open with the same underlying bug. So I'm going to call this a duplicate of #2528.

---

_Label `duplicate` added by @BurntSushi on 2024-11-13 22:23_

---

_Closed by @BurntSushi on 2024-11-13 22:24_

---

_Comment by @BurntSushi on 2024-11-13 22:24_

Thanks for the minimization! It will make for a nice test case for whenever this bug gets fixed.

---

_Comment by @alvin55531 on 2024-11-13 23:47_

Thank you for the explanation! 

I am not familiar with Rust nor search engine implementations, so I have a few more questions. 

1. Does the 128 bytes for `MAX_LOOK_AHEAD` mean ripgrep will give up look-ahead matching if the current character is 128 bytes away from the beginning of the match? 
2. I saw the terms "searching" and "printing" in the other issue you linked. In my case, it still produces the correct match, just with an unexpected formatting, so I'm assuming it's a "printing" bug. Does this bug only impact the "printing" stage? Or will ripgrep sometimes produce outright incorrect matches?
3. If I modify the end of the regex to match more lines (rather than up till the first non-empty line), it'll output/print no problem for whatever reason. The lookahead portions are left unchanged. Is that also because of the way ripgrep treats lookahead? 
```
rg --pcre2 --multiline --multiline-dotall --only-matching '(?=.*Testword4)(?=.*Testword5).*?Testword4' 
```
Output (expected behavior where empty lines are not printed, following the `--only-matching` flag)
```
test.md
3:Testword5
4:This is a test sentence.
5:This is a test sentence.
6:This is a test sentence.
7:This is a test sentence.
8:This is a test sentence.
9:This is a test sentence.
10:This is a test sentence.
11:This is a test sentence.
12:This is a test sentence.
13:This is a test sentence.
14:This is a test sentence.
15:This is a test sentence.
16:This is a test sentence.
17:This is a test sentence.
18:Testword4
```

---
