```yaml
number: 2166
title: Flag for dedicated file output on searches
type: issue
state: closed
author: 0xjmux
labels:
  - duplicate
assignees: []
created_at: 2022-03-20T00:13:57Z
updated_at: 2023-11-22T21:59:29Z
url: https://github.com/BurntSushi/ripgrep/issues/2166
synced_at: 2026-01-12T16:13:24Z
```

# Flag for dedicated file output on searches

---

_@0xjmux_

#### Describe your feature request
I use rg to search extremely large directories (>1TB), and the output can get a bit long (several thousand lines, in some cases). I would like a way to see the colored, pretty output in the terminal, and also save the search results to a file in case an issue or really long line is run into that completely takes up the viewable terminal history. 

Something like 
```
rg --output-file outputFile.txt searchText
```
Would output the usual, pretty output to stdout, but also save the results of the search in a readable format to out.txt. 

I am well aware of things like `tee -a` that allow for output to stdout and to a file, but using it results in either A) bash color formatting in the file and the ruining of viewability in vim or B) difficult to read output in the terminal. 

The features present in programs like youtube-dl and nmap are close to what i'm looking for. 

Thanks, and thanks for creating such an awesome search tool!

---

_Comment by @BurntSushi on 2022-03-21 12:58_

In terms of implementation, in theory, it should not be too difficult to implement. I think the strategy I would take is to provide a tee-like implementation in ripgrep core of the [`termcolor::WriteColor`](https://docs.rs/termcolor/latest/termcolor/trait.WriteColor.html) trait. It would do the standard stuff for writing with color support, but would also provide a way to be built with a file that it would also write to (but without color support).

But I'm not quite convinced that this is worth adding. This seems like a very niche issue and could be solved relatively easily on your end with a tool that strips color formatting from the file you've written with `tee`. See: https://superuser.com/questions/380772/removing-ansi-color-codes-from-text-stream

---

_Comment by @rwese on 2023-10-25 09:06_

relates to #2371

---

_Comment by @BurntSushi on 2023-11-22 21:59_

Duplicate of #2371. (I know this issue was created first, but I think #2371 ended up containing more info.)

---

_Closed by @BurntSushi on 2023-11-22 21:59_

---

_Label `duplicate` added by @BurntSushi on 2023-11-22 21:59_

---
