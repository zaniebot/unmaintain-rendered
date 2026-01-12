```yaml
number: 1655
title: Incorrect line numbering when using multiline and replacing by noncontiguous capture groups
type: issue
state: closed
author: dancingbug
labels:
  - question
  - wontfix
assignees: []
created_at: 2020-08-15T12:51:16Z
updated_at: 2020-08-16T11:55:20Z
url: https://github.com/BurntSushi/ripgrep/issues/1655
synced_at: 2026-01-12T16:13:24Z
```

# Incorrect line numbering when using multiline and replacing by noncontiguous capture groups

---

_@dancingbug_

#### What version of ripgrep are you using?
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

`pacman -S ripgrep`
(package version 12.1.1-1 from [community] repo)

#### What operating system are you using ripgrep on?

Arch Linux x86-64 5.7.4

#### Describe your bug.

When using -r with noncontiguous capture groups together with --multiline and -n, the output line numbers are reported incorrectly.

#### What are the steps to reproduce the behavior?

run the following command:
`seq 5 | rg -n --multiline -r '$1$3' '^(2)$(\n^.$)+(\n^5)'`

#### What is the actual behavior?

`seq 5 | rg -n --multiline -r '$1$3' '^(2)$(\n^.$)+(\n^5)'`

gives

```
2:2
3:5
```

#### What is the expected behavior?

output should be one of

```
2:2
4:5
```
```
2:2
5:5
```
```
2:2
4:
5:5
```
depending on the desired semantics (which line should each character in the capture group `(\n^5)` be associated with?)

---

_Comment by @BurntSushi on 2020-08-15 13:44_

This looks correct to me. Or at least, as correct as one might expect. Line numbers and multi-line replacements don't really make a whole lot of sense. The replacement could insert an arbitrary number of new lines or remove an arbitrary number of new lines, and so creating a "correct" mapping seems impossible to me because it's not clear what "correct" would be. Here is perhaps a simpler example:

```
$ seq 8 | rg -n --multiline '(?s)(2).+(\n5)' -r '$1$2'
2:2
3:5
```

Or even:

```
$ seq 8 | rg -n --multiline '(?s)(2).+(\n5)|(7)' -r '$1$2$3'
2:2
3:5
7:7
```

And things get even whackier when the replacement string contains new lines itself:

```
$ seq 8 | rg -n --multiline '(?s)(2).+(\n5)|(7)' -r '$1$2





$3'
2:2
3:5
4:
5:
6:
7:
8:
9:
7:
8:
9:
10:
11:
12:
13:7
```

What exactly should the correct output be here? I don't know. If you count the new lines in the original text as I think you want, then that falls over in the above example because the replacement will throw off the count. (You'd wind up with a non-increasing sequence of line numbers.) ripgrep counts new lines in this sort of case by simply starting from the number of the line in which the match starts and then incrementing the line number for each subsequent line that is _written_ for that particular match.

I'm pretty sure this is probably a `wontfix` bug. I could potentially be swayed to some other sort of behavior if a compelling use case were presented though.

---

_Label `question` added by @BurntSushi on 2020-08-15 13:45_

---

_Comment by @dancingbug on 2020-08-16 06:05_

### Use case
My goal is to inline a range of lines that follow a marker into mentions of that marker, across an entire file hierarchy, using a unix command pipeline.

In my approach, I want to use `rg` and some other commands to generate a list of identifier -> line range associations.

I need the start and end line numbers of the range.

### Further details
I'm using this on files containing an uncommon serialization format that represents GUI objects hierarchically, but I'll use some toy examples to illustrate.

My approach using `rg`,`cut`,`paste`, and `join`  might generate lines like
```
FileA.txt:34:MMM:FileB.txt:1000:1005:2200
FileA.txt:35:NNN:FileB.txt:302:400:440
```

where the fields are
```
<inline-destination-file>:<common match>:<inline-source-file>:<source-match-line>:<range-start-line>:<range-end-line>
```

and where FileA.txt and FileB.txt might look like
```
  FileA.txt   line#     |   FileB.txt           line#
  ---------   -----     |   ---------           -----
  ...                   |   SectionNNN{           302
  var MMM;       34     |   ...     
  var NNN;       35     |     start_match         400
  ...                   |   ...
                        |     end_match           440
                        |   ...
                        |   }
                        |   ...
                        |   SectionMMM{          1000
                        |   ...
                        |     start_match        1005
                        |   ...
                        |     end_match          2200
                        |   ...
                        |   }
                        |   ...
```

I think this illustrates that this could be quite useful for summarizing salient portions of hierarchical data, giving both line ranges and nesting information from one command while using fancy pcre2 lookaround criteria to include/exclude ranges in delimited contexts, approximating solutions to feature requests like https://github.com/BurntSushi/ripgrep/issues/282  and https://github.com/BurntSushi/ripgrep/issues/236

### A more concrete  (but still contrived) example
`seq -w 0 9999 | sed 'y/0123456789/abcdefghij/' > data.txt`
This fills `data.txt` with the sequence of all strings of length 4 over the characters `{abcdefghij}` in lexicographic ordering.

Now to each match of the form `XYaa` (here X and Y match any word character), I want to associate the first range of lines starting/ending at `XYXY`/ `XYYX`, which is encountered after `XYaa`
```
  rg -e '(?sxx: ((\w)([^a])aa\n) .*? (\2\3\2\3\n) .*? (\2\3\3\2\n) )'\
  -r '@@$1$4$5' --pcre2 -I -n --multiline -- data.txt\
| tail -n 15

output:
6801:@@giaa
6802:gigi
6803:giig
6901:@@gjaa
6902:gjgj
6903:gjjg
7801:@@hiaa
7802:hihi
7803:hiih
7901:@@hjaa
7902:hjhj
7903:hjjh
8901:@@ijaa
8902:ijij
8903:ijji
```
Here, the original line numbers are lost for the desired range in each match.

I can actually already get what I want by naively using two invocations of `rg` in a pipeline:
```
  rg -e '.*' -I -n -- data.txt\
| rg -r '@@$1$4$5' -e '(?sxx: (^\N+?(\w)([^a])aa\n) .*? (^\N+?\2\3\2\3\n) .*? (^\N+?\2\3\3\2\n) )' --pcre2 --multiline\
| tail -n 15

output:
@@6801:giaa
6869:gigi
6887:giig
@@6901:gjaa
6970:gjgj
6997:gjjg
@@7801:hiaa
7879:hihi
7888:hiih
@@7901:hjaa
7980:hjhj
7998:hjjh
@@8901:ijaa
8990:ijij
8999:ijji

```
Which is less convenient and complicates the regex due to the added field in the input.

My understanding of unix pipes is limited, but I wonder about the overhead of pipe related context switching.
While trying to benchmark on an example using longer strings I ran into a PCRE 'match limit exceeded' error. I'll see if I can get that working.

For an example of concatenating the associated lines and selecting parts of the result, pipe the above to 
```
paste -d : - - - | cut -d : -f 1,2,3,5,8
```

### Alternative solution: a separate feature
I can imagine different features that would fit my use case.
Of those, the one I personally find most pleasing would be to provide the user a means to print the line number of the first character that a given capture group captures.

Then I could do something like `rg ... -r ':$@2:$2'`, without `-n`, where `$@2` gives the line number for the first character of capture group 2.

Regarding earlier comments' examples, using this approach it would be very clear what the output is supposed to be since it is being constructed explicitly


---

_Label `wontfix` added by @BurntSushi on 2020-08-16 11:55_

---

_Comment by @BurntSushi on 2020-08-16 11:55_

Thank you for elaborating. I really appreciate it. Unfortunately, I'm not convinced that your use case is really compelling enough to convince me to change how line numbers are handled in cases like this. To be honest, I think you would be better served by writing a dedicated tool to do what you want. That's probably what I would do anyway.

ripgrep provides just the barest replacement support, and I will continue to resist adding features that enhance the replacement syntax. There are other tools more suited to that task.

---

_Closed by @BurntSushi on 2020-08-16 11:55_

---
