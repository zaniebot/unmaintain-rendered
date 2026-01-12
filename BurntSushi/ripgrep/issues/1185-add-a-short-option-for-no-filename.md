```yaml
number: 1185
title: Add a short option for --no-filename
type: issue
state: closed
author: sbraz
labels:
  - question
assignees: []
created_at: 2019-02-02T15:53:03Z
updated_at: 2024-05-20T14:07:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1185
synced_at: 2026-01-12T16:13:23Z
```

# Add a short option for --no-filename

---

_@sbraz_

Hello,
Would it be possible to add a short option for `--no-filename`?
I can understand that you may not want to use `-h` like grep to avoid confusion but I think that any short option would be better than none :)

---

_Comment by @BurntSushi on 2019-02-02 16:32_

Please suggest a short flag that hasn't been used yet.

---

_Comment by @sbraz on 2019-02-03 15:35_

The only letters which aren't used are:
```
d
h
k
y
D
G
I
J
K
O
Q
R
V
W
X
Y
Z
```
Out of which only `d`, `k` and `y` are neither used in lower-case or upper-case.
I think using `-h` would be nice for consistency with grep (and I guess that we could still use `--help` to print the help). But if you don't want to use this one, I have no preference.

---

_Comment by @BurntSushi on 2019-02-03 15:42_

I'm not sold on adding a short option for this flag. With that said, I don't have a good feel for how often it's used, so it's pretty hard to make a decision. However, I do know that short flags are in short supply, and I'd like to be very careful in how we spend our budget there.

To make one thing clear, there is absolutely no way that I'll be changing the behavior of `-h`. Prescriptively, I think it is a mistake that grep doesn't print help output when the `-h` flag is used. Descriptively, the behavior of `-h` and `--help` in ripgrep is **not the same**, so we cannot just use `--help`.

---

_Label `question` added by @BurntSushi on 2019-02-03 15:42_

---

_Comment by @sbraz on 2019-02-03 15:53_

> I don't have a good feel for how often it's used

Then I guess we should wait and see if anybody else feels like this is needed. All I can say is that I will often do stuff like `zgrep pattern somelog-*.gz` and I will add `-h` because the time recorded in the log file is enough and I don't need the filenames.

> I think it is a mistake that grep doesn't print help output when the -h flag is used

I totally agree, it's just that I've gotten so used to it…

---

_Comment by @lydell on 2019-02-10 14:10_

Just to provide a usage data point:

I sometimes run stuff like this:

```
rg 'font-family:\s*([^;]+)' -r '$1' -o --no-filename -tsass | sort | uniq -c | sort -hr
```

... to find the most common values for the CSS `font-family` property in a project (or for `font-weight`, `z-index`, you name it). From time to time I do similar aggregations for JavaScript and Python code ("extract all _such and such_ from _that_ part of the code base").

So I wouldn't mind a shortcut.

---

_Comment by @BurntSushi on 2019-04-03 13:09_

I do kind of feel like this might deserve a short flag. But I don't know which to pick.

`-h` is the obvious choice, since that's what grep uses and is likely what everyone is familiar with. Unfortunately, I am 100% unwilling to change the behavior of `-h` today.

I'd also like to avoid using a short flag that has both lowercase and uppercase variants available. For example, both `-d` and `-D` are unused, which means they could be a convenient way to enable or disable some other feature in the future. Since we already have `-H` to enable file names and can't use `-h`, we should pick a flag that has no chance of being paired with something else.

For example, the `-J` flag is available, and its lowercase variant, `-j`, controls parallelism with the number of threads. So `-j` will never need a `-J` to "negate" it, making it available. Using this logic and the list above, I think that narrows our choices down to the following:

```
G
I
J
O
Q
R
V
W
X
Z
```

I don't think I see any obvious mnemonic device here between the flags above and `--no-filename`. The only one I can think of is `-I`, which "comes after `-H` in the alphabet" and is also the second letter in the word `filename`. grep does have an `-I` flag, but I don't see ripgrep ever needing to add a similar flag.

---

_Comment by @sbraz on 2019-04-03 13:51_

Both `-I` and `-J` seem fine :)

---

_Closed by @BurntSushi on 2019-04-14 23:29_

---

_Comment by @kastiglione on 2019-04-16 02:31_

Thanks for adding this, I use it a lot

---

_Comment by @banool on 2024-05-20 14:07_

For anyone reading this looking for a super quick answer to what the flags are:

- `-H, --with-filename` Print the file path with each matching line.
- `-I, --no-filename` Never print the path with each matching line.


---
