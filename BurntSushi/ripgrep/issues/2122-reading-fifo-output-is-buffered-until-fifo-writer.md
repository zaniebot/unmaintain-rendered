```yaml
number: 2122
title: "Reading fifo: Output is buffered until fifo writer closes"
type: issue
state: closed
author: spencerwilson
labels:
  - invalid
assignees: []
created_at: 2022-01-11T18:39:11Z
updated_at: 2023-11-24T20:06:51Z
url: https://github.com/BurntSushi/ripgrep/issues/2122
synced_at: 2026-01-12T16:13:24Z
```

# Reading fifo: Output is buffered until fifo writer closes

---

_@spencerwilson_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)

#### How did you install ripgrep?

brew install ripgrep

#### What operating system are you using ripgrep on?

macOS 11.3
Darwin Kernel Version 20.4.0: Fri Mar  5 01:14:02 PST 2021; root:xnu-7195.101.1~3/RELEASE_ARM64_T8101

#### Describe your bug.

When reading a fifo file and instructed to buffer output by line, ripgrep buffers its output neither by line (as expected) nor by block (as it otherwise supports), but rather until the other end of the fifo is closed.

My use case: I have a large stream (probably hundreds of GB; it's `jq --stream -c` output on a 13 GB JSON object) that I'm teeing into multiple fifos, each with a single ripgrep reader. I was alerted to this bug when I noticed ever-growing resident set size on the ripgrep processes and delayed output from the ripgreps (their output appeared only when tee closed its end of the fifo).

#### What are the steps to reproduce the behavior?

```sh
rm -f yesfifo
mkfifo yesfifo
yes > yesfifo &
rg --line-buffered . yesfifo > rgout &

# Let rg process more than a block worth of input (13 MB on my system)
# During this time, rg never writes to stdout.
# Expectation is that it writes once per line of input.
sleep 0.25

# Terminate `yes`, closing the write end of the FIFO.
# Now rg writes its accumulated output to stdout.
kill %1
```

You can increase the sleep duration (but increase disk consumption) to make it easier confirm the lack of stdout writes during the sleep.

I've not verified any issues regarding buffered output when reading regular files.

#### What is the actual behavior?

rg buffers its output until the fifo writer closes.

#### What is the expected behavior?

Behavior described in the manual: rg decides buffering strategy based the terminal environment by default, but that can be overridden with options like `--line-buffered`. In the example above my expectation is that the output is line buffered.

---

_Comment by @BurntSushi on 2022-01-11 19:09_

I notice that you're also including `.` in your search. If you remove that, what happens? Similarly, what happens if you pass `-j1`?

---

_Comment by @spencerwilson on 2022-01-11 20:02_

Oh, I was under the impression that `.` is playing the role of the regex pattern, in this argument format:
```
       rg [OPTIONS] PATTERN [PATH...]
```

If I remove `.` and run `rg --line-buffered yesfifo` it searches $PWD for the pattern `yesfifo`. I believe the argument format it's using is the same as above.

If I add `-j1` I get my "expected behavior".

These sentences in the manual explains everything:
```
       ripgrep may use a large amount of memory depending on a few
       factors. Firstly, if ripgrep uses parallelism for search (the
       default), then the entire output for each individual file is
       buffered into memory in order to prevent interleaving matches in
       the output. To avoid this, you can disable parallelism with the -j1
       flag.
```

🤦‍♂️ I'd read this yesterday in a different context and thought "cool, makes sense". I just failed to recall it when I was working on a subsequent problem.

Thank you!

---

_Closed by @spencerwilson on 2022-01-11 20:02_

---

_Comment by @BurntSushi on 2022-01-11 20:17_

Ah I misread your command, whoops. The thing is, if ripgrep is only given a single file (or stream), then it should behave as if you provided `-j1` automatically. So I think there may actually be a bug here.

---

_Reopened by @BurntSushi on 2022-01-11 20:17_

---

_Comment by @BurntSushi on 2023-11-24 20:06_

As far as I can tell, ripgrep is entering into single threaded mode correctly when only one path is given to it.

---

_Closed by @BurntSushi on 2023-11-24 20:06_

---

_Label `invalid` added by @BurntSushi on 2023-11-24 20:06_

---
