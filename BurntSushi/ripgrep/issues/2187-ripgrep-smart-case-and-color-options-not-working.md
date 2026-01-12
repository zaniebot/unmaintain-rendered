```yaml
number: 2187
title: ripgrep  smart-case and color options not working on macOS
type: issue
state: closed
author: ciscohack
labels:
  - invalid
assignees: []
created_at: 2022-04-18T18:36:39Z
updated_at: 2022-04-18T21:32:14Z
url: https://github.com/BurntSushi/ripgrep/issues/2187
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep  smart-case and color options not working on macOS

---

_@ciscohack_

RG Version - 13.0.0
OS - macOS 12.3
Installed - Brew installed

Hello Expert, I am facing some issues with ripgrep options available. I want to enable --smart-case to enable my search case insensitive but this option not working and the search result does not produce any output. same I  tried for the color option to highlight the searched keyword but that also failed.

Here is my search query and it failed 

rg --colors 'match:fg:magenta' --colors 'line:bg:yellow' -S "Show Blocks" 20220407_142710_shtec.txt

but if I add -i instead of -S option then below is the output but here color option is ignored by ripgrep.

![image](https://user-images.githubusercontent.com/11479557/163857167-49d740cf-75c6-4578-8cd7-0629ee10e1b5.png)



---

_Comment by @BurntSushi on 2022-04-18 19:11_

Why did you delete the issue template? Please fill out the template. You're missing the `--debug` output and you haven't given me a command I can run on my own machine because I don't have `20220407_142710_shtec.txt`. If you can't share `20220407_142710_shtec.txt`, then please find another file you can provide me and a command that reproduces the problem.

---

_Comment by @ciscohack on 2022-04-18 19:21_

[shtech.log](https://github.com/BurntSushi/ripgrep/files/8507635/shtech.log)

here is the file and below is the command 

this will fail
rg --colors 'match:fg:magenta' --colors 'line:bg:yellow' -S "Show Blocks" shtech.log

this will work

rg --colors 'match:fg:magenta' --colors 'line:bg:yellow' -i "Show Blocks" shtech.log

---

_Comment by @BurntSushi on 2022-04-18 19:22_

Why haven't you shown your `--debug` output?

---

_Comment by @ciscohack on 2022-04-18 19:23_

how to use it... sorry don't know haven't run before

---

_Comment by @ciscohack on 2022-04-18 19:24_

I have also created .ripgreprc file in my root and added export RIPGREP_CONFIG_PATH=~/.ripgreprc in my zshrc this also not working 

---

_Comment by @BurntSushi on 2022-04-18 19:25_

Pass the `--debug` flag and copy the output to here......... There isn't anything else to do.

And your first command failing is expected. Please consider reading the docs for the `--smart-case` flag. The first sentence is:

> Searches case insensitively if the pattern is all lowercase. Search case sensitively otherwise.

Your pattern isn't all lowercase. So ripgrep treats it as a case sensitive search.

I'm sorry, but I think the issue here isn't ripgrep. I think you would benefit from some synchronous one-on-one help. Maybe you have a friend or something that you can do some pair programming with.

---

_Comment by @ciscohack on 2022-04-18 19:30_

@BurntSushi  sorry but the smart case in the doc mentioned that it's like a flag " -i  " second what the point of the smart case if it does not automatically treat the upper and lower word search like the "-i" flag. --smart-case is not smart in this case then. this --debug also not showing anything 

![image](https://user-images.githubusercontent.com/11479557/163865214-67db7475-4c8c-452e-acea-fa1693bf98d9.png)


---

_Comment by @ciscohack on 2022-04-18 19:32_

if I type everything as it is in the file then there is no point in having ignored case sensitive and smart-case. vim also has similar stuff which works fine and smartly. What about the color option I used in the search that also not produce the output as per the color I defined

here I mentioned magenta and yellow and result is red 

![image](https://user-images.githubusercontent.com/11479557/163865814-903df4bb-c02c-4ccb-9baa-40e5f587d2a9.png)


---

_Comment by @BurntSushi on 2022-04-18 19:40_

`--smart-case` is "smart" because it detects whether everything in your pattern is lowercase. _Only then_ will it enable case insensitive search. Otherwise, it becomes a normal case sensitive search. Smart case helps when you want your search to be by default case insensitive _unless_ there is something that isn't lowercase in your pattern. So, e.g., `rg -S 'foo'` behaves like `rg -i 'foo'` and `rg -S 'Foo'` behaves like `rg 'Foo'`. That's the "smartness."

If `--debug` doesn't show anything at all, then there is something very fundamentally wrong with your setup and I don't know how to help you with it.

Colors work fine for me.

I'm sorry, but I don't have the capacity to help you further. I don't mean to be mean, but I think you need more basic help than what I feasibly offer through GitHub Issues. I recommend you get synchronous one-on-one help from a friend. Either that, or there is a language barrier here that I don't know how to overcome.

---

_Closed by @BurntSushi on 2022-04-18 19:40_

---

_Label `invalid` added by @BurntSushi on 2022-04-18 19:41_

---

_Comment by @ciscohack on 2022-04-18 19:44_

@BurntSushi Thank you sir for your time. if not you then no one help at least in my circle any way thanks. i just did brew install ripgrep and copied your .ripgreprc config and added in the root. now don't know what's wrong with causing a functional break is strange to me. anyway thanks for your help

---

_Comment by @ciscohack on 2022-04-18 20:41_

@BurntSushi I found the problem, --debug and colors options were not working because I pipe the output of rg with the bat. but similar think I have tested with silver_search and pipe the output with the bat not break the functionality. anyway, I found the issue. thought to document here so someone like me come get some help 

---

_Comment by @BurntSushi on 2022-04-18 21:31_

In the future, when reporting issues, it is imperative you include the full command you're *actually* running and its entire output. Otherwise it wastes a lot of time.

---

_Comment by @BurntSushi on 2022-04-18 21:32_

But thank you for sharing the answer. I appreciate it.

---
