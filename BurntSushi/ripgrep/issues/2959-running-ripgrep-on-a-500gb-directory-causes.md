```yaml
number: 2959
title: running ripgrep on a 500GB directory causes ripgrep to use a lot of memory
type: issue
state: closed
author: bananafirestorm
labels: []
assignees: []
created_at: 2025-01-02T14:57:18Z
updated_at: 2025-01-02T16:20:10Z
url: https://github.com/BurntSushi/ripgrep/issues/2959
synced_at: 2026-01-12T16:13:25Z
```

# running ripgrep on a 500GB directory causes ripgrep to use a lot of memory

---

_@bananafirestorm_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

14.1.1

### How did you install ripgrep?

Uploaded from releases

### What operating system are you using ripgrep on?

Windows 10

### Describe your bug.

When the search result exceeds the memory capacity, the program breaks.

### What are the steps to reproduce the behavior?

Open the 500GB folder find (find a large number of lines, more than the amount of memory) for example: all non-null lines. Redirect the output to write to a file.

### What is the actual behavior?

destruction of the Windows machine

### What is the expected behavior?

Make an option to write to a file in parts and disable output to the screen (so that you can select the size of the parts). Then there will be writing to the file in parts, while freeing up memory. And the ability to work with any volume.

---

_Comment by @BurntSushi on 2025-01-02 16:19_

From the man page:

```
       ripgrep may use a large amount of memory depending on a few fac‐
       tors.  Firstly,  if ripgrep uses parallelism for search (the de‐
       fault), then the entire  output  for  each  individual  file  is
       buffered into memory in order to prevent interleaving matches in
       the  output. To avoid this, you can disable parallelism with the
       -j1 flag. Secondly, ripgrep always needs to have at least a sin‐
       gle line in memory in order to execute a search. A file  with  a
       very  long  line  can thus cause ripgrep to use a lot of memory.
       Generally, this only occurs when searching binary data with  the
       -a/--text  flag enabled. (When the -a/--text flag isn't enabled,
       ripgrep will replace all NUL bytes with line terminators,  which
       typically  prevents exorbitant memory usage.) Thirdly, when rip‐
       grep searches a large file using a memory map, the process  will
       likely report its resident memory usage as the size of the file.
       However,  this does not mean ripgrep actually needed to use that
       much heap memory; the operating  system  will  generally  handle
       this for you.
```

It is _not_ a foregone conclusion that merely running ripgrep on 500GB of data will cause it to use a ton of memory.

The main way for a huge memory spike to happen is if you're running with multiple threads (the default) _and_ your regex pattern matches nearly everything. Because of the use of parallelism, ripgrep has to buffer the output _per file_ in memory. So if you have five 100GB files in that directory, ripgrep could conceivably try to use 500GB of memory. The only way around this is to disable parallelism. Then ripgrep will _stream_ the output to stdout without buffering the entire contents into memory first.

The somewhat less common way for a huge memory spike to happen is if you have files with very long lines. Like, GBs long. This is very unlikely to happen unless `-a/--text` is given, but is theoretically possible. (The `-a/--text` flag matters, because, without it, NUL bytes are turned into line terminators by ripgrep. This eliminates the largest source of long lines: long runs of NUL terminators in binary data. But when `-a/--text` is given, ripgrep searches the data as-is and no NUL replacement is done. Which increases the chances of seeing a very long line.)

> Make an option to write to a file in parts and disable output to the screen (so that you can select the size of the parts). Then there will be writing to the file in parts, while freeing up memory. And the ability to work with any volume.

I don't know what you mean by "disable output to the screen."

ripgrep is not going to grow an option for "writing to a file in parts." I'm not even sure what that means, and it doesn't even necessarily solve the problem. Indeed, it's impossible to say _why_ ripgrep is using up a ton of memory in your use case because there is no MRE. For that reason, this issue isn't actually actionable.

---

_Closed by @BurntSushi on 2025-01-02 16:19_

---

_Renamed from "Inability to work with large amounts of data." to "running ripgrep on a 500GB directory causes ripgrep to use a lot of memory" by @BurntSushi on 2025-01-02 16:20_

---
