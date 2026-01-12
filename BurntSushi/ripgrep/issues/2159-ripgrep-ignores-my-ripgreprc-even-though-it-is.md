```yaml
number: 2159
title: ripgrep ignores my .ripgreprc, even though it is set as as an enviromental variable
type: issue
state: closed
author: emilBeBri
labels: []
assignees: []
created_at: 2022-03-10T15:45:00Z
updated_at: 2022-03-12T07:25:17Z
url: https://github.com/BurntSushi/ripgrep/issues/2159
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep ignores my .ripgreprc, even though it is set as as an enviromental variable

---

_@emilBeBri_

#### What version of ripgrep are you using?


Replace this text with the output of `rg --version`.

  ripgrep 13.0.0 (rev 0b36942f68)
  -SIMD -AVX (compiled)
  +SIMD +AVX (runtime)



#### How did you install ripgrep?


installed from source on github


#### What operating system are you using ripgrep on?

Lubuntu 20.04 with i3wm

#### Describe your bug.

When trying to use a .ripgreprc config-file, it does not set the flags. In my .bashrc I have set, as instructed in the repo readme: 

  RIPGREP_CONFIG_PATH='/home/emil/Dropbox/share/.ripgreprc'


#### What are the steps to reproduce the behavior?

for example, in the .ripgreprc I have:


  --smart-case

but it is does not change rg behaviour - this goes for all the flags I try to include.



#### What is the actual behavior?


  rg monkey


only finds text with lowercase monkey.


#### What is the expected behavior?

the expected output would be rg to find any case of monkey, like:

  Monkey 
  monKey

and so on.





---

_Comment by @BurntSushi on 2022-03-10 15:49_

Please show the entire output of `echo MONKEY | rg --debug monkey`.

---

_Comment by @BurntSushi on 2022-03-10 15:50_

This is very likely a problem on your end. e.g., Maybe you aren't setting the env var correctly. Maybe you aren't `export`ing it. etc etc.

You can test this by running `echo MONKEY | RIPGREP_CONFIG_PATH=/home/emil/Dropbox/share/.ripgreprc rg monkey`. If that works, then there's nothing wrong with ripgrep here, and the problem is definitively with how you're setting the environment variable. Consider running `env` to debug whether the env var is set or not.

---

_Comment by @emilBeBri on 2022-03-12 07:25_

Oh okay, when running `echo MONKEY | RIPGREP_CONFIG_PATH=/home/emil/Dropbox/share/.ripgreprc rg monkey` the output is captured correctly. Hmm, how strange, I would've thought adding it to .bashrc could not go wrong. Sorry for wasting your time!
 

---

_Closed by @emilBeBri on 2022-03-12 07:25_

---
