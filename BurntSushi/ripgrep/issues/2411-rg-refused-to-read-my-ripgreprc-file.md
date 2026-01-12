```yaml
number: 2411
title: "Rg refused! To read my .ripgreprc file."
type: issue
state: closed
author: McUsr
labels: []
assignees: []
created_at: 2023-02-08T12:09:19Z
updated_at: 2023-02-08T13:38:48Z
url: https://github.com/BurntSushi/ripgrep/issues/2411
synced_at: 2026-01-12T16:13:24Z
```

# Rg refused! To read my .ripgreprc file.

---

_@McUsr_

#### What version of ripgrep are you using?
12.1..1
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
#### How did you install ripgrep?
Can't remember, but from apt, if that was possible

If you installed ripgrep with snap and are getting strange file permission or
file not found errors, then please do not file a bug. Instead, use one of the
Github binary releases.

#### What operating system are you using ripgrep on?

Debian (11+)  Bullseye
Replace this text with your operating system and version.

#### Describe your bug.

IDK if it's a bug. I was really STRUGGLING to make ripgrep read the .ripgreprc file, and in the end, when all hope was ABANDONED, I changed the permissions to u+x, in pure DESPERATION.

That WORKED!  which is befuddling, flummoxing and flabbergasting.

Having read the above, about snap and permissions, I don't think I used snap, but still, sounds like it is kind of the same issue.
(I'm not sure if I used cargo either, so, it is open, how I got it.)

Great piece of software!

Thank you.



---

_Comment by @BurntSushi on 2023-02-08 13:38_

I don't personally understand how `u+x` would have transitioned you from "a config file that wasn't being read" to "a config file that is being read." The config file is not executed. It is just read. To make the config file work, you do indeed need sufficient permissions to read it, but that _should_ occur by default by virtue of creating it with the same user that you're running ripgrep with. At that point, it's just a simple matter of `RIPGREP_CONFIG_PATH=path/to/.ripgreprc rg foobar` to test that it works. You can run ripgrep with the `--debug` flag to see whether it's reading the config file.

Other things to consider are whether you have an `rg` alias or wrapper script somewhere. If either of those has `--no-config`, that would prevent the config file from working.

But again, I don't know how `u+x` could have fixed things for you. 

Anyway, I'm glad it's working and thank you for sharing the solution that worked for you. :-)

---

_Closed by @BurntSushi on 2023-02-08 13:38_

---
