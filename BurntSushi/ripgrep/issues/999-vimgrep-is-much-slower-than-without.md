```yaml
number: 999
title: "--vimgrep is much slower than without"
type: issue
state: closed
author: BurntSushi
labels:
  - question
assignees: []
created_at: 2018-07-31T22:53:48Z
updated_at: 2018-08-01T11:27:02Z
url: https://github.com/BurntSushi/ripgrep/issues/999
synced_at: 2026-01-12T16:13:22Z
```

# --vimgrep is much slower than without

---

_@BurntSushi_

@jessegrosjean wrote this from https://github.com/BurntSushi/ripgrep/issues/244#issuecomment-409352528

---

@BurntSushi I'm blown away at the speed of ripgrep in terminal. I'm trying not to loose that speed when running from my app. I think the JSON format (as I understand it) makes large searches that `rg` handles easily from the terminal near impossible to perform using the JSON API.

I think including a full line of context with each match is just too much bandwidth for some cases.

For example:

My test case is pathological–search for `e` in my home directory. It generates 4,655,585 results. Crazy, but this is just the kind of thing that a user might try to see if an app works and isn't buggy. And in fact it's what made me so impressed with ripgrep. When I run ripgrep on my home directory I see:

```
time rg e > NULL

real	0m2.381s
user	0m2.226s
sys	0m1.615s
```

Wow! Fast. But then if I do:

```
 time rg --vimgrep e > NULL
```

My computer starts to die. I kill the process after 20 seconds and starting to run out of memory. I "think" the problem is that unlike the default command `--vimgrep` returns a full line for each result. It’s just to much bandwidth when you have many results.

> What you're asking for really sounds like a new "preview" type feature, which is similar to, but different from, what --max-columns achieves. --max-columns is really about limiting the length of lines reported, where as a hypothetical preview feature is quite a bit more complex than that and will need to account for windowing every match, of which there may be multiple.

I was asking for this, but I think you are correct, it’s to complicated, and would still require to much overlapping bandwidth in some cases. For example imagine the case where the user searches for `e` in a giant minified.js file.

Better I think is to just provide some options for what data is included in “match” (from the JSON API). What about an option to just omit the “match.lines” value?

My app could then generate the initial list of results quickly (by omitting the “lines” values). And then lazily (as matches are scrolled through the view) load and highlight the actually matched text.

---

---

_Comment by @BurntSushi on 2018-07-31 22:53_

Could we please try to reproduce this problem on a repository of code that we both have access to? I propose [this commit of the Linux kernel](https://github.com/BurntSushi/linux). In general, I do observe `rg --vimgrep` to be slower, but this is actually expected. Here are my tests:

Without `--vimgrep`:

```
$ time rg e | wc -l
12702460
real    0m0.641s
user    0m3.547s
sys     0m1.387s

$ time rg e > /dev/null
real    0m0.307s
user    0m2.886s
sys     0m0.585s
```

With `--vimgrep`:

```
$ time rg e --vimgrep | wc -l
34972117
real    0m2.172s
user    0m20.189s
sys     0m2.599s
$ time rg e --vimgrep > /dev/null
real    0m1.508s
user    0m17.199s
sys     0m0.505s
```

We can also see that memory usage doesn't change too much, with `--vimgrep` using slightly more, probably because it needs bigger output buffers on average:

```
$ /usr/bin/time -v rg e > /dev/null
...
        Maximum resident set size (kbytes): 59864
...

$ /usr/bin/time -v rg e --vimgrep > /dev/null
...
        Maximum resident set size (kbytes): 82184
...
```

To test the hypothesis that the output buffers are to blame for the increased memory usage, we can forcefully run ripgrep in single threaded mode, which removes the use of output buffers and instead streams matches to stdout as they are found:

```
$ /usr/bin/time -v rg e -j1 > /dev/null
...
        Maximum resident set size (kbytes): 6548
...

$ /usr/bin/time -v rg e -j1 --vimgrep > /dev/null
...
        Maximum resident set size (kbytes): 6336
...
```

In this case, we get identical memory usage.

To get an idea of the bandwidth involved here, let's measure the size of the output:

```
$ time rg e > /tmp/rg-without-vimgrep
real    0m0.768s
user    0m3.360s
sys     0m1.390s

$ time rg e --vimgrep > /tmp/rg-with-vimgrep
real    0m1.881s
user    0m17.951s
sys     0m1.751s

$ ls -lh /tmp/rg-with*
-rw-rw-r-- 1 andrew users 895M Jul 31 18:41 /tmp/rg-without-vimgrep
-rw-rw-r-- 1 andrew users 2.9G Jul 31 18:41 /tmp/rg-with-vimgrep
```

So here's what happening here. First and foremost, `rg e` will **emit a full line of output** for each matching line in the corpus. Conversely, `rg e --vimgrep` will emit a full line of output **for every match**. That means `--vimgrep` can duplicate some lines when `e` matches multiple times on the same line. This is the reason why `rg e` produces ~900MB of output where as `rg e --vimgrep` produces ~2.9GB of output.

On top of all of this, `rg e > /dev/null` should also be faster because it never needs to re-run the regex engine to detect the precise location of a match, where as `--vimgrep` needs to find the locations of every match.

In my view, `--vimgrep` is slower here mostly be necessity. By using the flag, you're specifically asking for a lot more output. The flag exists for compatibility with how vim reads matches from grep, but note that `--vimgrep` is not necessarily the optimal way to read match data.

For example, #244 discusses a JSON output format. The JSON output format **does not duplicate matching lines for every single match**. Instead, it shows the matching lines exactly once (equivalent to `rg e`), and then provides the locations of each individual match along with the matched text itself. In this case, you might get a lot of additional data because `e` matches a lot, but you will not get the entire line repeated, which should be much less data overall.

Finally, I'd like to note that if you're invoking ripgrep in a way that causes it to produce GBs of data, then there is no reasonable expectation that it should do so instantly. The right thing to do here is to change how you invoke ripgrep or put some sort of cap of how much data you actually read from ripgrep. You can in fact stop reading data at any time you like and stop ripgrep in its tracks.


---

_Label `question` added by @BurntSushi on 2018-07-31 23:15_

---

_Comment by @jessegrosjean on 2018-08-01 01:12_

> For example, #244 discusses a JSON output format. The JSON output format does not duplicate matching lines for every single match.

Grrr... sorry for all the distraction. If that's the case then I see no problem. I was under the impression that "message: match" happened once per match and included full line content each time. I see now that's not the case.

It seemed like a crazy way to do things, but since --vimgrep included full line text with each match  I though the spec just copied that approach. I might help future readers to change that documentation to plural:

> ### Message: match*es*
> This message indicates that a match*es* has been found. Match*es* generally corresponds to a single line of text, although it may correspond to multiple lines if the search can emit matches over multiple lines.

---

_Comment by @jessegrosjean on 2018-08-01 01:19_

I think this issue can be closed. I understand why --vimgrep is so much slower (in particular on minified javascript files). And because of the way that --vimgrep output is structured there's no way to really make it faster. My whole point was to just make sure not to design the JSON format with that same weekness. And of course you hadn't, I just got confused reading the singular/plurals in the spec.

---

_Comment by @BurntSushi on 2018-08-01 11:27_

Aye. Thanks for the feedback!

---

_Closed by @BurntSushi on 2018-08-01 11:27_

---
