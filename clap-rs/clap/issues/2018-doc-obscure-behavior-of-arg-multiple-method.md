---
number: 2018
title: "[Doc] Obscure behavior of Arg.multiple method"
type: issue
state: closed
author: marcospb19
labels:
  - A-docs
assignees: []
created_at: 2020-07-15T23:05:43Z
updated_at: 2021-01-30T22:11:54Z
url: https://github.com/clap-rs/clap/issues/2018
synced_at: 2026-01-10T01:27:11Z
---

# [Doc] Obscure behavior of Arg.multiple method

---

_Issue opened by @marcospb19 on 2020-07-15 23:05_

I'm trying to learn how to use Clap but I'm having a **very hard time** with the documentation, here I will try to explain my struggle only for the sake of improvement.

I might just be dumb for the time I wasted stuck on my task xD, but then let's try to understand how a dumb person deals with the documentation and how can we make it better.



### Where

https://docs.rs/clap/2.33.1/clap/

---

I have now realized, a long time being confused how to simply read all args from the CLI with this code.

![image](https://user-images.githubusercontent.com/38900226/87584043-4490b000-c6b3-11ea-9d04-7298140ab516.png)

Where is this teached on [the docs]( https://docs.rs/clap/2.33.1/clap/)?

I was very intrigued this is not clearly shown, I see almost every linux CLI tool accepting multiple arguments as a list. but `clap`'s documentation don't teach this at all.

As a former python developer I also found this weird because the first [example](https://docs.python.org/3/library/argparse.html) in `python argparse`'s documentation shows how to receive a variable number of integers and sum them.

### What's wrong

Examples containing `.multiple(true)` show how to accumulate levels of verbosity (`-v`/`--verbose`) but don't explain what happen if `.multiple(true)` is added to an `Arg` without `.short()` applied.

This is very obscure, if a short option is not added, then `multiple` has a totally different behavior/meaning, should `clap` users be supposed to just figure it out by trial and error? I don't think so.

### How to fix

_Please consider adding this to the documentation and not throwing it at the `examples/` folder, this is to grant the solution visibility._

Adding an example at the start of the documentation that debunks this confusion, here are some suggestions:

A program that receives a list of words and prints them out again, the program accept optional `--upper` and `--lower` flags that conflict and transform the output to upper case or lower case.

**Or**

A calculator that receives numbers and accepts `--sum` or `--mul`, and prints out the result of the chained operation (maybe similar to argparse's example?).

Although I have suggested two solutions, I would like to hear external opinions

EDIT: I should also mention that I'm sorry if I sound cocky or offensive on this issue (I'm not a native speaker), but this was **obviously** not intended by me.

---

_Label `C: docs` added by @marcospb19 on 2020-07-15 23:05_

---

_Comment by @marcospb19 on 2020-07-22 19:44_

I'm willing try helping with the documentation and create a PR, but I don't know how to build the documentation for this project.

---

_Added to milestone `3.0` by @pksunkara on 2020-11-26 07:15_

---

_Closed by @marcospb19 on 2021-01-30 22:11_

---
