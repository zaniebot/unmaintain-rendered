```yaml
number: 1448
title: rg --no-heading -N faults on multiple large files unless multithreading disabled
type: issue
state: closed
author: drekbot
labels:
  - wontfix
assignees: []
created_at: 2019-12-16T20:49:08Z
updated_at: 2019-12-17T01:23:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1448
synced_at: 2026-01-12T16:13:23Z
```

# rg --no-heading -N faults on multiple large files unless multithreading disabled

---

_@drekbot_

#### What version of ripgrep are you using?

11.0.2 (also observed with 0.8.1)

#### How did you install ripgrep?

Cargo and debian package method, both showed the same behavior.

#### What operating system are you using ripgrep on?

Ubuntu 16.04 and Redhat Server 7.6

#### Describe your question, feature request, or bug.

When ripgrep is run with the --no-heading and -N flags and multiple large files are searched. The sample files tested against were hundreds of gigabytes, larger than available system memory. A large percentage of the data in the files would match the search term.
-N and --no-heading appear to function when used separately, but together they cause this error.

The behavior does not occur when -j1 is used, or when used against a single large file (even if the same size as the multiple files in the failing example). 

#### If this is a bug, what are the steps to reproduce the behavior?

Fails
rg --no-heading -N 'asearch' largeFile1 largeFile2 

Works as expected
rg --no-heading -N -j1 'asearch' largeFile1 largeFile2

#### If this is a bug, what is the actual behavior?

rg --no-heading -N 'asearch' largeFile1 largeFile2 

On rg 11.0.2 the command fails with a memory allocation error similar to the following.

```memory allocation of 206158430208 bytes failedAborted (core dumped)```

On rg 0.8.1 the command fails silently (no visible error message) or has this error message.
```
fatal runtime error: allocator memory exhausted
Illegal instruction
```

#### If this is a bug, what is the expected behavior?

Returned normal search results and not crashed.

---

_Comment by @BurntSushi on 2019-12-16 21:03_

This is expected. From the CAVEATS section of the man page:

> ripgrep may use a large amount of memory depending on a few factors.
> Firstly, if ripgrep uses parallelism for search (the default), then the
> entire output for each individual file is buffered into memory in order to
> prevent interleaving matches in the output. To avoid this, you can disable
> parallelism with the -j1 flag. Secondly, ripgrep always needs to have at
> least a single line in memory in order to execute a search. A file with a
> very long line can thus cause ripgrep to use a lot of memory. Generally,
> this only occurs when searching binary data with the -a flag enabled. (When
> the -a flag isn’t enabled, ripgrep will replace all NUL bytes with line
> terminators, which typically prevents exorbitant memory usage.) Thirdly, when
> ripgrep searches a large file using a memory map, the process will report its
> resident memory usage as the size of the file. However, this does not mean
> ripgrep actually needed to use that much memory; the operating system will
> generally handle this for you.

This _normally_ isn't a problem because this is only an issue with the size of the _output_, not the input.

---

_Closed by @BurntSushi on 2019-12-16 21:03_

---

_Label `wontfix` added by @BurntSushi on 2019-12-16 21:04_

---

_Comment by @drekbot on 2019-12-16 21:28_

Thank you for the quick response.

---

_Comment by @drekbot on 2019-12-17 00:58_

The main reason I wanted to document is that this only occurred when we were using both --no-heading and -N. When run by themselves everything got allocated and there was output.

---

_Comment by @BurntSushi on 2019-12-17 01:23_

Yeah that makes sense. `--no-heading -N` increase the output size, potentially by a lot, since the file path is printed for every matching line. So that's going to use more memory.

---
