```yaml
number: 2097
title: rg uses all CPU for an extremely long time
type: issue
state: closed
author: dclong
labels:
  - invalid
assignees: []
created_at: 2021-12-06T20:03:23Z
updated_at: 2023-11-22T00:25:50Z
url: https://github.com/BurntSushi/ripgrep/issues/2097
synced_at: 2026-01-12T16:13:24Z
```

# rg uses all CPU for an extremely long time

---

_@dclong_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Installed using cargo.

#### What operating system are you using ripgrep on?

Ubuntu 20.04 via Docker. 

#### Describe your bug.

`rg` uses all CPU for an extremely long time.

#### What are the steps to reproduce the behavior?
Not sure how to reproduce it. I used `rg` to search for texts in my project for a few times. Later the machine was reported to be very slow. Running `top` shows that `rg` uses all CPU for an extremely long time. 
<img width="478" alt="Screen Shot 2021-12-06 at 11 42 55 AM" src="https://user-images.githubusercontent.com/824507/144914199-5f927bf6-d02e-4596-8780-c0ac978edb78.png">


#### What is the actual behavior?

Show the command you ran and the actual output. Include the `--debug` flag in
your invocation of ripgrep.

If the output is large, put it in a gist: https://gist.github.com/

If the output is small, put it in code fences:

```
your
output
goes
here
```

#### What is the expected behavior?

`rg` should use any resources after it finishes running. 

---

_Renamed from "rg consumes " to "rg uses all CPU for an extremely long time" by @dclong on 2021-12-06 20:03_

---

_Comment by @BurntSushi on 2021-12-06 20:13_

If it's using CPU then it's still running.

Otherwise, this isn't actionable unfortunately. If there is a bug here, I need a reproduction. Otherwise, you would need to debug it on your own.

---

_Comment by @BurntSushi on 2023-11-22 00:25_

Closing due to inactivity.

---

_Closed by @BurntSushi on 2023-11-22 00:25_

---

_Label `invalid` added by @BurntSushi on 2023-11-22 00:25_

---
