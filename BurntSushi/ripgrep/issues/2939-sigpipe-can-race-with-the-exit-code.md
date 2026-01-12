```yaml
number: 2939
title: SIGPIPE can race with the exit code
type: issue
state: closed
author: DavidArchibald
labels:
  - wontfix
assignees: []
created_at: 2024-11-26T19:21:19Z
updated_at: 2024-12-03T00:23:45Z
url: https://github.com/BurntSushi/ripgrep/issues/2939
synced_at: 2026-01-12T16:13:25Z
```

# SIGPIPE can race with the exit code

---

_@DavidArchibald_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMP -AVX (compiled)
+SIMD +AVX (runtime)

Also tried when building from source with PCRE support. Cloned at SHA 79cbe89deb1151e703f4d91b19af9cdcc128b765.

### How did you install ripgrep?

I believe I installed through APT initially but it also failed after compiling from source. I suspect this would occur in any version.

### What operating system are you using ripgrep on?

Linux 5.15.167.4-microsoft-standard-WSL2

### Describe your bug.

This code actually exhibits a race condition:
```sh
set -o pipefail

if ! echo "$contents" | rg -q -U "$search"; then
   echo "Failure!"
fi
```

Namely `echo` might exit with a status code of 1 because of `SIGPIPE` thus masking rg's status code and and obfuscating the intended meaning of the exit code 1 of being "not matched"

To be honest I'm not sure there's even anything to fix here because it may just be an annoying trivia about Bash rather than anything Ripgrep is doing wrong. I tried rewriting this as `rg -q -U "$search" <<< "$contents"` and it has always worked. However this race-y behaviour really doesn't seem desirable to me and if there's anything Ripgrep could do about it that'd be great.

### What are the steps to reproduce the behavior?

Create a harness named `test.sh`:
```sh
#!/usr/bin/env bash

# Strictly only `-o pipefail` is necessary.
set -ueE -o pipefail

# This bug does NOT seem to occur when the file is passed directly to rg.
contents=$(cat testData)

# This isn't actually necessary for the bug, it just causes it to emits a log when it happens.
# I get something like `echo: write error: Broken pipe`
trap "" SIGPIPE

for _ in $(seq 1 1000); do
    # If you want to fix the issue you can write `rg -q "abc" <<< "$output"` instead which for me has never reproduced the issue.
    if echo "$contents" | rg -q "abc"; then
        echo "Got failure due to SIGPIPE"
        exit 1
    fi
done
```

Create a file named `testData` with contents like so:
```
abc

[...]
```
Fill in the `[...]`s with a lots of unrelated text that won't match. In my case I made it about 5,000 lines of the text "useless garbage". In your case significantly longer or shorter may be necessary. Regardless once the file is large enough this test will consistently fail with the `SIGPIPE` error.

Furthermore you can see that it's racey or at the very least not deterministic if you use a file with a sweet spot size you can observe an exit code of 1 or 0.

### What is the actual behavior?

`SIGPIPE` races with `rg`'s exit code and so can seem to spuriously fail to match in _seemingly_ idiomatic Bash code. As I alluded to maybe what I'm doing is an anti-pattern but I'd never heard of such issues with Bash and I consider myself to be _fairly_ well versed in Bash's footguns.

### What is the expected behavior?

Ripgrep to always succeed as the file always contains `abc` which is what's being searched for.

---

_Comment by @BurntSushi on 2024-11-27 12:57_

As far as I can tell, you've misinterpreted the behavior of your shell program. If you remove the `-q` switch, you'll notice that the SIGPIPE error goes away. This to me reveals what is actually happening here: when you give ripgrep the `-q` switch, you're instructing it to be "quiet." That is, you're only running it for its exit code, not for its output. Because of that, ripgrep can exit early as soon as it finds its first match, since it knows at that point that the exit code is going to be `0`. (Well, actually, it could be a `>1` exit code if it encountered an error later in the search, but the optimization assumes no errors.) It then _stops reading input from stdin and exits_. This closes the pipe that `echo` is writing to and provokes the `SIGPIPE` error you're seeing. This also varies by input size because of buffering. If the input it small enough, `echo` can write all of it into a buffer before ripgrep can find the match and quit.

You'll notice that this same behavior happens with `grep` too.

There really isn't anything ripgrep can do here other than always consume the entire input on `stdin`. But I don't see that happening. This is really just one of the many, _many_, **_many_** subtle footguns you'll find in shell scripting.

---

_Label `wontfix` added by @BurntSushi on 2024-11-27 12:57_

---

_Closed by @BurntSushi on 2024-11-27 12:59_

---

_Comment by @BurntSushi on 2024-11-27 13:01_

I also forgot to mention, crucially, ripgrep's exit code itself is unaffected here. It isn't giving up or not searching or not finding a match that exists. It is precisely because it finds a match early and quits that a SIGPIPE occurs. If you remove `abc` from `testData`, you'll notice that the SIGPIPE error does not occur. This is because ripgrep fully consumes it's entire input and doesn't exit early. (It can only exit early when `-q` is given when a match is found. But no match requires searching the entire input.)

---

_Comment by @DavidArchibald on 2024-12-03 00:23_

Yeah I understood all that. I mentioned it here:
> Namely echo might exit with a status code of 1 because of SIGPIPE thus masking rg's status code and and obfuscating the intended meaning of the exit code 1 of being "not matched"

And as I said I was unsure if there was even anything ripgrep could do but I wanted to raise it in the off chance there was. Thanks for checking anways.

---
