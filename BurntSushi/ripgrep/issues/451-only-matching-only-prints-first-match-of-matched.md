```yaml
number: 451
title: "`--only-matching` only prints first match of matched line N times"
type: issue
state: closed
author: s3rb31
labels: []
assignees: []
created_at: 2017-04-17T16:27:05Z
updated_at: 2017-04-20T20:47:24Z
url: https://github.com/BurntSushi/ripgrep/issues/451
synced_at: 2026-01-12T16:13:22Z
```

# `--only-matching` only prints first match of matched line N times

---

_@s3rb31_

Take [this](https://gist.githubusercontent.com/s3rb31/3eea5bb61414f37968ac1d79a554e137/raw/d8768ae3147520581cc84fb23e892fae5795804f/ripgrep%2520test%2520file) file as test data. If I do the following with good old `grep`:

`grep -Po "\/get\/v\/\d*" fgi.txt`

I get the following result:

```
/get/v/19670
/get/v/19637
/get/v/19632
/get/v/19600
/get/v/19598
/get/v/19574
/get/v/19523
/get/v/19521
/get/v/19463
/get/v/19457
/get/v/19433
/get/v/19425
/get/v/19392
/get/v/19390
/get/v/19363
/get/v/19358
/get/v/19337
/get/v/19317
/get/v/19295
/get/v/19278
/get/v/19243
/get/v/19241
/get/v/19208
/get/v/19186
/get/v/19128
/get/v/19126
```

But if I try to achive the same result with rg:

`rg -o "/get/v/\d*" fgi.txt -N`

```
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
/get/v/19670
```

I think that behaviour ist really odd and cannot be intended. If I am not mistaken this also violates the documentation (manpage):

```
-o, --only-matching
    Print only the matched (non-empty) parts of a matching line, with each such part on a separate output line.
```

But it does not print all the parts. It just prints the first match N times, where N _is_ actually the correct number of matched parts. 

I hope this can get fixed. I may create a PR myself if it is not too complicated and someone can lead me the right direction.

Greetings

---

_Comment by @bmalehorn on 2017-04-18 20:37_

Here's a much simpler example:

    $ cat example.txt
    1 2 3

    $ grep -Po "[0-9]+" example.txt
    1
    2
    3

    $ rg -o "[0-9]+" example.txt -N
    1
    1
    1

@s3rb31 if you want to take a stab it, I believe the bug is here: https://github.com/BurntSushi/ripgrep/blob/master/src/printer.rs#L311 Instead of running the regex again, it should use the original match data to get the start & end.

---

_Comment by @kpp on 2017-04-20 14:22_

The column is ok, while data is not:

```
~/ripgrep$ cat tests/digits.txt 
1 2 3
~/ripgrep$ cargo run -- "\d" tests/digits.txt -o -n --column
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/rg '\d' tests/digits.txt -o -n --column`
1:1:1
1:3:1
1:5:1
```

---

_Comment by @kpp on 2017-04-20 15:11_

Sorry for the bug :sob: 

---

_Comment by @s3rb31 on 2017-04-20 20:47_

Oh, this was really quick. Thanks for the fix!

I compiled it and it performs well and solid until now. Will report back if I encounter more bugs.

Thanks again!

---

_Closed by @s3rb31 on 2017-04-20 20:47_

---
