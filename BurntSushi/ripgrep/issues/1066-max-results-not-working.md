```yaml
number: 1066
title: "--max-results not working"
type: issue
state: closed
author: janwirth
labels: []
assignees: []
created_at: 2018-09-25T09:26:37Z
updated_at: 2018-09-25T13:34:57Z
url: https://github.com/BurntSushi/ripgrep/issues/1066
synced_at: 2026-01-12T16:13:22Z
```

# --max-results not working

---

_@janwirth_

actual
```
❯ rg -l -m 3 'hi'  | wc -l
     541
```

expected
```
❯ rg -l -m 3 'hi'  | wc -l
     3
```
version
```                                                                                                                   
❯ rg --version            
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```
platform
```
OSX High Sierra 10.13.6
```
installed with `brew`

---

_Comment by @BurntSushi on 2018-09-25 09:42_

There is no --max-results flag. The -m flag is short for --max-count. As the documentation states, it limits the *number of lines matched per file*. Therefore, it has no impact on the behavior of the -l flag.

---

_Closed by @BurntSushi on 2018-09-25 09:42_

---

_Comment by @janwirth on 2018-09-25 11:49_

ah okey so that means if I want to truncate the number of results I will need `head`

---

_Comment by @BurntSushi on 2018-09-25 11:58_

@FranzSkuffka Yes, but note that by default, the order of results on subsequent runs isn't necessarily stable. So the results that `head` shows can change from run to run. You can make results stable with `--sort path` at the expense of parallelism.

---

_Comment by @janwirth on 2018-09-25 13:34_

That's fine :) I was using this to create an interactive file selection.

https://github.com/FranzSkuffka/dotfiles/blob/master/rgki.sh#L11

thanks.

---
