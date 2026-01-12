```yaml
number: 804
title: "cmd isn't waiting for all threads to return before exiting"
type: issue
state: closed
author: rodrigobahiense
labels:
  - invalid
assignees: []
created_at: 2018-02-14T16:40:25Z
updated_at: 2018-02-21T00:41:36Z
url: https://github.com/BurntSushi/ripgrep/issues/804
synced_at: 2026-01-12T16:13:22Z
```

# cmd isn't waiting for all threads to return before exiting

---

_@rodrigobahiense_

#### What version of ripgrep are you using?

ripgrep 0.8.0 (rev 23d1b91ead)
-SIMD -AVX

#### What operating system are you using ripgrep on?

**Edition:** Windows 10 Pro
**Version:** 1709
**OS Build:** 16299.248

#### Describe your question, feature request, or bug.

I was getting incomplete results from rg (without flags) and tried using `--sort-files` and `-j1` (in separate runs). Both returned the expected results.

#### If this is a bug, what are the steps to reproduce the behavior?

Search some string in a somewhat "deep" directory. My case has 17094 files in 1092 folders.

#### If this is a bug, what is the actual behavior?

I can't put the actual output here, but goes as something like:

Normal search:
```
# rg "whatever" .

path/to/file1
1: this file has whatever on it
```

Singlethreaded search:
```
# rg -j1 "whatever" .

path/to/file1
1: this file has whatever on it

path/to/file2
1: this other file also has whatever on it but don't appear on the normal search
```

#### If this is a bug, what is the expected behavior?

That the normal search return all results without the need to use `--sort-files` or `-j1`.

---

_Comment by @BurntSushi on 2018-02-14 16:52_

Thanks for the bug report! Unfortunately, without the ability to reproduce this, it's unlikely to get fixed. In particular, the issue title is jumping to conclusions a bit. The bug here may not necessarily be that threads aren't waiting until proper termination, but rather, there may just be a bug somewhere that's causing the search routines themselves to behave differently between the single threaded and multithreaded implementations.

---

_Comment by @BurntSushi on 2018-02-14 16:52_

Is there any way you can find a corpus of code on which to reproduce this bug? Perhaps a large open source repository? Chromium?

---

_Comment by @rodrigobahiense on 2018-02-14 17:16_

I'm finding it very hard to reproduce outside of the directory it happened, apparently I've found some unusual case.

I've also just found out that if I use `rg -c "string" .` it counts correctly the number of results in each file.

Can I send the debug output to you directly, privately?

---

_Comment by @BurntSushi on 2018-02-14 17:25_

> Can I send the debug output to you directly, privately?

@zugl Yes, but I doubt that will help. You can send it to jamslam@gmail.com --- I really need to be able to reproduce a bug like this myself unfortunately.

I guess we can try debug-by-proxy? What are the outputs of the following commands?

```
$ rg whatever | wc -l
$ rg -j1 whatever | wc -l
$ rg -j2 whatever | wc -l
$ rg . | wc -l
$ rg -j1 . | wc -l
$ rg -j2 . | wc -l
$ rg whatever -c | cut -d':' -f2 | python -c 'import sys; print(sum(int(x) for x in sys.stdin))'
$ rg whatever -c -j1 | cut -d':' -f2 | python -c 'import sys; print(sum(int(x) for x in sys.stdin))'
$ rg whatever -c -j2 | cut -d':' -f2 | python -c 'import sys; print(sum(int(x) for x in sys.stdin))'
```

I'm not sure any of this will lead to answer for you, but let's see what happens.

---

_Comment by @rodrigobahiense on 2018-02-14 17:33_

I'll convert this commands to PowerShell equivalents because I've also just found out that this only occurs on rg for Windows (using PowerShell or Cmd).

Using rg (same version) on linux through WSL (bash) it returns correct results.

---

_Comment by @rodrigobahiense on 2018-02-14 17:38_

Another test and apparently rg.exe works correctly through WSL (it supports running windows binaries in bash).

The issue seems related to rg.exe through PowerShell or Cmd.

---

_Comment by @BurntSushi on 2018-02-14 17:39_

@zugl Hmm, that is a helpful data point! It _could_ be a red herring, but I'll noodle on it.

---

_Comment by @rodrigobahiense on 2018-02-14 18:09_

I guess I've found the core issue and it might not have anything to do with rg itself. Here is the real problem:

One of the first results contained a latin1 character in the line, something like:

```
# rg -E latin1 "erro" .

path/to/file
1: erro improvável
```

Using just `rg "erro" .` resulted in:
```
path/to/file
1: erro improv
```

The cmd hangs after the **v** in "improvável" and after some time exits.

Running `rg -j1 "erro" .` or `rg --sort-files "erro" .` resulted in:
```
path/to/file
1: erro improv�vel
```

Here the full text is returned but with the **replacement character** in the string.

Outputting to a file in all cases returned the correct results, so maybe this is a console buffer issue? I for one have no idea.

However, now the question is: why rg didn't output the **replacement character** in all cases?

Hope this helps somehow and you may close this issue.

Thank you very much for your time on this project! =)

---

_Comment by @BurntSushi on 2018-02-14 18:13_

That is... interesting. I am pretty sure there are things going on with the console and encoding, but I honestly don't know the precise details. @retep998 might know something though!

---

_Comment by @rodrigobahiense on 2018-02-14 18:14_

By the way, on WSL the latin1 characters were cut out of the result:

```
path/to/file
1: erro improvvel
```

---

_Label `invalid` added by @BurntSushi on 2018-02-21 00:40_

---

_Comment by @BurntSushi on 2018-02-21 00:41_

Closing this since it seems like this isn't ripgrep's problem.

---

_Closed by @BurntSushi on 2018-02-21 00:41_

---
