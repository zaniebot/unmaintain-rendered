```yaml
number: 1866
title: "--vimgrep doesn't satisfy vim's spec for multi-line searches"
type: issue
state: closed
author: poetaman
labels:
  - bug
  - rollup
assignees: []
created_at: 2021-05-12T20:19:43Z
updated_at: 2021-06-01T01:51:26Z
url: https://github.com/BurntSushi/ripgrep/issues/1866
synced_at: 2026-01-12T16:13:24Z
```

# --vimgrep doesn't satisfy vim's spec for multi-line searches

---

_@poetaman_

NOTE from BurntSushi: This issue has been re-classified as a bug that was discovered in the course of discussion. There's too much context here to split it apart, which GitHub doesn't really make easy to do anyway. So I've re-purposed this issue for the bug described in the title. The main details of the bug are described here: https://github.com/BurntSushi/ripgrep/issues/1866#issuecomment-843020533

Below is the original issue.

-----

#### Describe your feature request

Vim recently added a new feature called "text properties" that can be used to highlight text. All it needs to know to highlight is: 

* lnum : line number where match begins
* col : column (counted in bytes) where match begins
* end_lnum : line number where match ends
* end_col : column (counted in bytes) where match ends

Ideally there would be an option like ``--json-pos`` that  just prints positions of matches for each file, perhaps in json format like following. If that is not possible or two cumbersome, is there a way I can already do this in a performance efficient way using ripgrep?

```json
{
  {
    "file": "fileA.txt",
    "positions": [
      {
        "lnum": 10,
        "col": 4,
        "end_lnum": 12,
        "end_col": 7
      },
      {
        "lnum": 15,
        "col": 3,
        "end_lnum": 15,
        "end_col": 5
      },
      {
        "lnum": 17,
        "col": 12,
        "end_lnum": 20,
        "end_col": 10
      }
    ]
  },
  {
    "file": "fileB.txt",
    "positions": [
      {
        "lnum": 1,
        "col": 4,
        "end_lnum": 1,
        "end_col": 7
      },
      {
        "lnum": 30,
        "col": 3,
        "end_lnum": 31,
        "end_col": 5
      }
    ]
  }
}
```

---

_Comment by @BurntSushi on 2021-05-12 20:23_

> Ideally there would be an option like `--json-pos` that just prints positions of matches for each file, perhaps in json format like following.

Could you please explain why the existing `--json` flag is insufficient?

---

_Comment by @poetaman on 2021-05-12 20:30_

@BurntSushi Hmm, not sure if I understand the format of existing json then... Is there a way to find the information I need for multi-line matches already? Could you provide an example.

---

_Comment by @poetaman on 2021-05-12 20:31_

@BurntSushi Also, for large number big multiline matches, it would be better not to print the text of matches. That I believe would be faster than the existing format that prints a lot of information. 

---

_Comment by @BurntSushi on 2021-05-12 20:46_

The format is documented here: https://docs.rs/grep-printer/0.1.5/grep_printer/struct.JSON.html

I'm not changing the format unless there's a well motivated benchmark of a real use case in a real environment. If you get that, then we can evaluate options for improving performance like adding `--json-no-text` or something.

---

_Comment by @poetaman on 2021-05-12 21:08_

@BurntSushi Ok, here's one problem: Even if I use ``"line_number"`` to get lnum, and  ``"lines" -> "text"`` to count the number of lines to get end_lnum. The ``"lines" -> "text"`` prints entire last line of match, not just till the column where the match was hit. How to find the end/start column of match? It seems like one could find that from the ``"submatches"``. But is it guaranteed that the 1st sub match always corresponds to the match? (like even when there are capture groups)?

*Regarding benchmarks*: Let's look at the usecase: I am trying to integrate this in pure vimscript. To be able to find end line number in the multiline example below, I will need to process the string ``"text"`` in vim script to find the number of ``\n``'s and length of text after last ``\n`` to get the end_col. If one were to assume that such string processing is being done in a fast language like rust, then there is no problem. But that assumes such flexibility on user side, given my target it to integrate some fast search engine within vim in pure vimscript, it is preferable that the search engine (which is written generally in rust/c/c++) which might already have such information in its internal state to print that information. In short I think a benchmark would be useful if one assumes that user applications are written in fast languages, I am afraid thats not true for something like vimcript. The following table compares the performance of incrementing a counter in vimscript to languages like python.

<img width="510" alt="Screen Shot 2021-05-12 at 2 04 52 PM" src="https://user-images.githubusercontent.com/71736629/118044224-3d661d00-b32b-11eb-9843-a6096517a28d.png">

Source: https://github.com/vim/vim/blob/master/README_VIM9.md. Note: New vim-9 (Vim new) is still in the works, and many setups lag behind in vim versions anyway.

Here's one example from my latex file ``decoding.tex`` for pattern ``rg --json -U  "(?s)usepackage.*?newcommand" decoding.tex``. 

```json
{
  "type": "match",
  "data": {
    "path": {
      "text": "decoding.tex"
    },
    "lines": {
      "text": "\\usepackage{blindtext}\n\\usepackage[a4paper,left=0in,right=0in,top=0in,bottom=0in]{geometry}\n\\usepackage{pdfpages}\n\n\\defaultfontfeatures{\n    Color=000000,\n    Ligatures=TeX,\n    WordSpace={1.0,1.0,1.0},\n    PunctuationSpace=0,\n    LetterSpace=0\n}%Opacity=0.3,\n\\newcommand{\\llakkuratstyle}{\\addfontfeatures{Opacity=1.0}} \n"
    },
    "line_number": 9,
    "absolute_offset": 147,
    "submatches": [
      {
        "match": {
          "text": "usepackage{blindtext}\n\\usepackage[a4paper,left=0in,right=0in,top=0in,bottom=0in]{geometry}\n\\usepackage{pdfpages}\n\n\\defaultfontfeatures{\n    Color=000000,\n    Ligatures=TeX,\n    WordSpace={1.0,1.0,1.0},\n    PunctuationSpace=0,\n    LetterSpace=0\n}%Opacity=0.3,\n\\newcommand"
        },
        "start": 1,
        "end": 271
      }
    ]
  }
}
```



---

_Comment by @BurntSushi on 2021-05-12 22:33_

> Ok, here's one problem: Even if I use `"line_number"` to get lnum, and `"lines" -> "text"` to count the number of lines to get end_lnum. The `"lines" -> "text"` prints entire last line of match, not just till the column where the match was hit. How to find the end/start column of match?

The output includes the byte offsets of the match. You can use those to compute column. And I note that "column" is an ill-defined concept in general. I'm sure vim has it more rigorously defined, but I don't know what that definition is and whether it's stable.

> It seems like one could find that from the `"submatches"`. But is it guaranteed that the 1st sub match always corresponds to the match? (like even when there are capture groups)?

Capture groups aren't reported in the JSON output. Each `submatch` corresponds to a match of the input pattern(s). The "sub" here refers to the fact that the overall match is the line (or lines) itself.

> _Regarding benchmarks_: Let's look at the usecase: I am trying to integrate this in pure vimscript. To be able to find end line number in the multiline example below, I will need to process the string `"text"` in vim script to find the number of `\n`'s and length of text after last `\n` to get the end_col. If one were to assume that such string processing is being done in a fast language like rust, then there is no problem. But that assumes such flexibility on user side, given my target it to integrate some fast search engine within vim in pure vimscript, it is preferable that the search engine (which is written generally in rust/c/c++) which might already have such information in its internal state to print that information.

I see why you think this, and you might very well be right. But it's still just a guess. I was serious. I'm _not_ adding flags to ripgrep based on guesses. This is my mantra: make it work, make it right, make it fast. Before adding flags, we should have real repeatable benchmarks based on real use cases in real environments that end users are having problems with. That's what I would like my standard to be.

---

_Comment by @poetaman on 2021-05-13 04:53_

@BurntSushi Some good news: vim has a command ``:goto [count]``. The ``[count]`` is the byte offset from the beginning of file. This is what I did to find the end_lnum and end_col for the example above, run ``:goto 418`` (418 comes from "absolute_offset" (147) + "end" (271)), then I can query the cursor position from vim ``getpos('.')``. Also vim's cursor position on line (misnamed as "col") is actually byte offset from the beginning of the line. To find starting "col", it is simply"start"+1 from the "submatch" entry. We are all set at least for simple matches...

Though I need to understand submatches a bit more... I have a bunch of questions in mind:

1) for instance you mention it can be empty for inverted matches, so where would I get the values for end_col, end_lnum, and col?

2) so far in my testing, all the submatches's beginning byte is on the same line number. Is that right? Will any of the submatches -- for instance in some multiline pattern -- ever begin on a line other than that specified in "line_number"??






---

_Comment by @BurntSushi on 2021-05-13 11:13_

>     1. for instance you mention it can be empty for inverted matches, so where would I get the values for end_col, end_lnum, and col?

An inverted match is _a line in which none of the input patterns match_. Thus, the match is the line itself.

>     2\. so far in my testing, all the submatches's beginning byte is on the same line number. Is that right? Will any of the submatches -- for instance in some multiline pattern -- ever begin on a line other than that specified in "line_number"??

I _believe_ it's true that the first match will begin on the first line emitted from ripgrep. I am not 100% sure that it's a rock solid guarantee, but I cannot immediately think of any cases where it wouldn't be true. (It would likely be a strange corner case.)

---

_Comment by @poetaman on 2021-05-13 11:57_

@BurntSushi Ok thanks for the information. So in that case the meaning of "submatch" is the list of all matches starting on "line_number". If that is not the case then there is ambiguity in the definition+implementation. I mean if a submatch begins on some other line, then why would it not be in the submatch entry of that line... Did you implement the json emitter? Anyway when you have time, please clarify the definition of submatch (with confidence/expected behavior) & perhaps add it to the documentation.

---

_Comment by @BurntSushi on 2021-05-13 12:28_

I don't see what's ambiguous about this:

> An array of submatch objects corresponding to matches in lines. The offsets included in each submatch correspond to byte offsets into lines. (If lines is base64 encoded, then the byte offsets correspond to the data after base64 decoding.) The submatch objects are guaranteed to be sorted by their starting offsets. Note that it is possible for this array to be empty, for example, when searching reports inverted matches.

The wording here looks precisely correct to me.

>  I mean if a submatch begins on some other line, then why would it not be in the submatch entry of that line...

I am having a hard time understanding your confusion. To be clear, we're both looking at [the "match" message](https://docs.rs/grep-printer/0.1.5/grep_printer/struct.JSON.html#message-match), right? It is not a "line" message. It's a "match" message. ripgrep does not emit JSON objects for every line. It emits JSON objects for _matches_. In the default non-multiline mode, yes, every match will be limited to precisely one line and every submatch of that match will start and end on that line. That's what it means to be line oriented and it is standard grep behavior.

But when multiline mode is enabled, then a match may span _multiple_ lines. In that case, submatches may start on one line and end on another. But, yes, the first submatch should have a starting position on the first line in the "match" message. Once a line is included in a "match" message, it will not be included in any subsequent messages. That's the point of submatches: it enumerates all possible matches within the region emitted in the "match" message. (And in multiline mode, a "match" message could be the entire input!)

> Did you implement the json emitter?

Yes.

---

_Comment by @poetaman on 2021-05-13 12:47_

@BurntSushi What about the other sub matches (other than the first sub-match) for multi line matches. That’s the question I have. Can any of such sub-matches begin on lines other than the first line of match? So far in my simple non-multi-line testing, sub matches array just consists of all matches on the same line. The reason I ask this is: in my current implementation, I assume lnum (line number) of all submatches is “line_number” of match. If that’s not true for all cases, it would be good to have an example. 

---

_Comment by @BurntSushi on 2021-05-13 12:51_

> Can any of such sub-matches begin on lines other than the first line of match?

Of course!

```
$ echo 'foobar\nfoobar\nfoo quux' | rg -U 'foobar\nfoobar\nfoo|quux' --json
... snip ...
{
  "type": "match",
  "data": {
    "path": {
      "text": "<stdin>"
    },
    "lines": {
      "text": "foobar\nfoobar\nfoo quux\n"
    },
    "line_number": 1,
    "absolute_offset": 0,
    "submatches": [
      {
        "match": {
          "text": "foobar\nfoobar\nfoo"
        },
        "start": 0,
        "end": 17
      },
      {
        "match": {
          "text": "quux"
        },
        "start": 18,
        "end": 22
      }
    ]
  }
}
... snip ...
```

---

_Comment by @BurntSushi on 2021-05-13 12:52_

Or perhaps a bit simpler to read, try `echo 'abc\nxyz 123' | rg -U '[\w\n]+' --json`.

---

_Comment by @poetaman on 2021-05-13 13:07_

@BurntSushi Ok now it’s clear what sub match means. It could be a regex sub-pattern-match, or one of the multiple matches on the same line for entire regex. You did save lines in json by putting multiple independent full regex matches as “submatch” of a “match”. Hope you now get what is the source of confusion. 

I will implement a fully functional version of ripgrep in vim & compare it to something like ag (I guess it provides exact information I asked in this ticket). I suspect ripgrep will be slow for such multi line patterns because the start line number for multi line sub matches is not available (& one needs to find with splits on newline character (ouch)).

Do you have benchmark files and patterns for such cases?

---

_Comment by @BurntSushi on 2021-05-13 13:34_

> I guess it provides exact information I asked in this ticket

Does it? Could you show me which command is being run to get such information? `ag` certainly has no JSON output. `ag` has a `--vimgrep` flag, but ripgrep has that too. The only difference is that ripgrep's version isn't riddled with bugs:

```
$ echo 'abc\nxyz 123' | rg -U '[\w\n]+' --vimgrep --no-filename
1:1:abc
2:1:xyz 123
2:9:xyz 123

$ echo 'foobar\nfoobar\nfoo quux' | rg -U 'foobar\nfoobar\nfoo|quux' --vimgrep --no-filename
1:1:foobar
2:1:foobar
3:1:foo quux
3:19:foo quux

$ echo 'abc\nxyz 123' | ag '[\w\n]+' --vimgrep --no-filename
... uuhhh this doesn't search stdin, instead it searches CWD
$ echo 'abc\nxyz 123' | ag '[\w\n]+' --vimgrep --no-filename -
ERR: Error stat()ing: -
ERR: Error opening directory -: No such file or directory
... lol really? okay okay, i give up, you win

$ echo 'abc\nxyz 123' > /tmp/test-ag-input
$ ag '[\w\n]+' --vimgrep --no-filename /tmp/test-ag-input
/tmp/test-ag-input:0:xyz 123
/tmp/test-ag-input:0:
... wat?

$ echo 'foobar\nfoobar\nfoo quux' > /tmp/test-ag-input-2
$ ag 'foobar\nfoobar\nfoo|quux' --vimgrep --no-filename /tmp/test-ag-input-2
/tmp/test-ag-input-2:0:foo quux
/tmp/test-ag-input-2:5:foo quux
... again, wat? it dropped the first lines from the match
```

The `--vimgrep` flag does seem to give you the line number you want, but it otherwise provides very little info. You don't know where one match begins and the other ends.

I think it's important to note that I didn't just push out the JSON emitter on a lark. I wrote it specifically for editor integration, and in particular, with VS Code in mind. I got feedback from them before publishing it. Any time you do "search in files" in VS Code, it's using `rg --json`. I've never heard about any performance problems with parsing the output.

> (& one needs to find with splits on newline character (ouch))

Personally, I think you're over-estimating the importance of the performance of this. I'm sure you'll be able to find places where this matters, but I suspect it will only occur when either the match frequency is extremely high or when the match itself is extremely large. And in those cases, performance is likely going to suffer for other reasons (latency for high frequency matches or allocating a huge chunk of memory for a large match).

And _that's_ why I ask for benchmarks. It has to be a real and meaningful case. Guesses aren't good enough. A big part of the reason for building ripgrep is speed. That's what I do. It's what I've been doing for a long time. I have some instincts about things, but it's never a substitute for actually measuring things in an environment that matches a real end user's environment as closely as possible.

If you go off and implement this, get it working and you come back to me with "here's a case a real user hit (or is very likely to hit), and there's a user visible performance impact resulting specifically from having to count line numbers," then I'd be very very very happy to work with you on fixing that. But that's _not_ what we have here. What we have here are guesses.

> Do you have benchmark files and patterns for such cases?

No, i don't. I don't work on editor integrations.

---

_Comment by @poetaman on 2021-05-14 02:08_

@BurntSushi Sorry I realized it wasn't ``ag`` that produced that. In the communication I had with someone regarding this on vim, they had actually mentioned ``rg`` produces this information. The communication was a month ago, and it seems my ``ag``/``rg`` neurons had merged. Now there is enough differential potential there.

Thanks for the examples. I think I get it how to bend the current information to implement something efficient in vim. Perhaps it will require adding a function in vim to work more efficiently with byte offsets, which I will ask Bram (the creator of Vim).

---

_Closed by @poetaman on 2021-05-14 02:08_

---

_Comment by @bfrg on 2021-05-15 10:21_

@BurntSushi Your example from above doesn't print the correct numbers with the `--vimgrep` option:
```bash
printf 'foobar\nfoobar\nfoo quux' | rg -U 'foobar\nfoobar\nfoo|quux' --vimgrep
```
Output:
```
<stdin>:1:1:foobar
<stdin>:2:1:foobar
<stdin>:3:1:foo quux
<stdin>:3:19:foo quux
```

I would have expected something like:
```
<stdin>:1:1:foobar\nfoobar\nfoo
<stdin>:3:5:quux
```
Or am I misunderstanding something here?

---

_Comment by @poetaman on 2021-05-15 10:30_

@BurntSushi I agree with @bfrg, its a bug! You are printing the byte offset relative to beginning of "match region" instead of printing "character column" from beginning of line of "submatches". @bfrg good catch, I ignored looking at it as I was only interested in ``--json`` printer.

---

_Comment by @BurntSushi on 2021-05-15 12:29_

@bfrg Excellent catch! It's now fixed on master. :-) Interestingly, I had several tests confirming the current buggy behavior. In any case, the output on master is now:

```
1:1:foobar
2:1:foobar
3:1:foo quux
3:5:foo quux
```

---

_Comment by @bfrg on 2021-05-15 12:56_

Thank you for the quick fix. But shouldn't there be only two matches just like with the `--json` output? Since the regex `'foobar\nfoobar\nfoo|quux'` is matched only for the first `foobar` in the first line _or_ the `quux` in the third line. Hence:
```
1:1:foobar
3:5:quux
```
or, to be consistent with the `text` entry of the json output:
```
1:1:foobar\nfoobar\nfoo
3:5:quux
```

For comparison:
```bash
printf 'foobar\nfoobar\nfoo quux' | rg -U 'foobar\nfoobar\nfoo|quux' --json | jq 'select(.type == "match")'
```
Output:
```json
{
  "type": "match",
  "data": {
    "path": {
      "text": "<stdin>"
    },
    "lines": {
      "text": "foobar\nfoobar\nfoo quux"
    },
    "line_number": 1,
    "absolute_offset": 0,
    "submatches": [
      {
        "match": {
          "text": "foobar\nfoobar\nfoo"
        },
        "start": 0,
        "end": 17
      },
      {
        "match": {
          "text": "quux"
        },
        "start": 18,
        "end": 22
      }
    ]
  }
}
```

---

_Comment by @BurntSushi on 2021-05-15 13:03_

No. That part is correct. `--vimgrep` prints a line for each match:

```
$ echo 'foofoo' | rg foo --vimgrep
<stdin>:1:1:foofoo
<stdin>:1:4:foofoo

$ echo 'foofoo' > /tmp/ag-input
$ ag foo --vimgrep /tmp/ag-input
/tmp/ag-input:1:1:foofoo
/tmp/ag-input:1:4:foofoo
```

So in multi-line mode, it does the same: it prints the _entire_ match, even if it spans multiple lines, just like it would if you ran your query without `--vimgrep`.

To be honest, `--vimgrep` and multi-line mode don't really interact well. The only way to answer questions like this is to look at concrete use cases where folks are actually using `--vimgrep` and multi-line mode somewhere.

> or, to be consistent with the text entry of the json output:

There is no real desired consistency between output modes at this level.

---

_Comment by @bfrg on 2021-05-15 18:30_

> --vimgrep prints a line for each match.

But there are only two matches in the multi-line example. Hence, there should be only two lines in the `--vimgrep` output (and not four). I would have expected to print only the line+column numbers where the match starts. Currently, every line which is part of the match is printed.

In my opinion, `--vimgrep` and multi-line mode can interact together. With Vim's internal `:vimgrep` command, I get two matches (just like with ripgrep's `--json` option):

Example file:
```
foobar
foobar
foo quux
```
Open the file in Vim and run:
```vim
:vimgrep /foobar\nfoobar\nfoo\|quux/ %
```
Vim will find the following two matches (open Vim's `quickfix` window with `:copen`):
```
test|1 col 1| foobar
test|3 col 5| foo quux
```
This is the same result that I also get with `--json`, after transforming the byte offsets to line+column numbers.

I tried to compare the behavior with GNU/grep and git-grep using their PCRE regex engines but neither one seems to work correctly.

```bash
printf 'foobar\nfoobar\nfoo quux' | grep -znP 'foobar\nfoobar\nfoo|quux'
```
Output:
```
1:foobar
foobar
foo quux
```
Unfortunately, grep doesn't provide a `--column` option, and multi-line regex patterns work only with the `-z` option which will treat the entire input as one line making it completely useless for Vim.

---

_Comment by @BurntSushi on 2021-05-15 20:32_

> But there are only two matches in the multi-line example. Hence, there should be only two lines in the `--vimgrep` output (and not four). I would have expected to print only the line+column numbers where the match starts. Currently, every line which is part of the match is printed.

Yes, as I said, that is the intended behavior. I don't really know why you would expect the complete matches to not be printed. I don't think that's ever true for any ripgrep output mode. (Other than aggregate views or things that limit the maximum line length printed.) I don't see why it should be true for `--vimgrep`.

It's hard for me to say whether `:vimgrep`'s output should be what ripgrep does. Vim is a text editor and it might think about matches differently. ripgrep is a line oriented search tool.

> I tried to compare the behavior with GNU/grep and git-grep using their PCRE regex engines but neither one seems to work correctly.

Right. "grep" is a line oriented tool. Multi-line searches on work in things like GNU grep with weird hacks.

---

_Comment by @poetaman on 2021-05-15 22:38_

@BurntSushi Regarding your "I don't really know why you would expect the complete matches to not be printed.". You are not printing complete matches, you are giving ``\n`` some special treatment and printing lines that don't really "match" in rest of the industry's terminology. In ``:vimgrep`` format only starting lines of matches are printed, this also helps in counting the number of matches if someone uses the same output in other scripts/tools. If someone were to use use ripgrep to feed its ``--vimgrep`` output for 3 different consumers: 1) feed to a vim, 2) feed to  another program that expects ``:vimgrep`` format, 3) count the number of matches based on line count, in all the cases the results will be inconsistent because you aren't really complying to ``:vimgrep``'s format. Its never a good idea to just go by opinion ("...I don't really know why you..."), because every format is a contract, something that ripgrep's ``--vimgrep`` option breaks by using its name, and describing it as such.

Regarding ``:vimgrep``'s format, the ``col`` there actually refers to byteoffet/byteindex from start of line. Just FYI, so you don't print "column number" as "character number".

---

_Comment by @BurntSushi on 2021-05-15 23:08_

> You are not printing complete matches, you are giving \n some special treatment and printing lines that don't really "match" in rest of the industry's terminology.

Huh?
![vimgrep-match](https://user-images.githubusercontent.com/456674/118380298-a27d7500-b5ae-11eb-88c4-906fa150331e.png)

Breaking it down really carefully:

```
$ echo 'foobar\nfoobar\nfoo quux' | rg -U 'foobar\nfoobar\nfoo|quux' --vimgrep --color always
<stdin>:1:1:foobar
<stdin>:2:1:foobar
<stdin>:3:1:foo quux
<stdin>:3:5:foo quux
```

There are two overall matches in the input. The first is `foobar\nfoobar\nfoo`. The second is `quux`. There is no "special" interpretation given to `\n` here. The first three lines are printed because it corresponds to the first match. The fourth line is printed because it corresponds to the second match. Now let's look at the docs for `--vimgrep`:

```
        --vimgrep
            Show results with every match on its own line, including line numbers and
            column numbers. With this option, a line with more than one match will be
            printed more than once.
```

This doesn't call out multi-line specifically (because `--vimgrep` was added before ripgrep got multi-line support). The wording is tied to non-multi-line search, but a re-phrasing of that would be, "Print the lines containing matches for each match, including line numbers and column numbers. With this option, lines containing more than one match will be printed more than once."

That is exactly the behavior that ripgrep has.

There is no special treatment given to `\n`.

> In :vimgrep format only starting lines of matches are printed

This seems like the crux of the confusion. I did not know this. This is a critical piece of information.

> because every format is a contract, something that ripgrep's --vimgrep option breaks by using its name, and describing it as such.

Sadly, I got the name from `ag`. I didn't even know there was a specification for a format called "vimgrep." (I've used vim for over a decade, but I never use its searching facilities because I've always found it sub-optimal.) Could you please link me to the specification? I just don't understand why it took so long to get to this point in the conversation. Like, if there is a disagreement with a spec, then just say that and link to the relevant section of the spec. ripgrep's `--vimgrep` flag is indeed _meant_ to be used for easy vim integration, and if it is violating assumptions that vim makes, then yes, absolutely, we should fix that. If that means multi-line mode only prints the first line of multi-line matches, then so be it.

What happened was likely this:

1. I added `--vimgrep` based on the idea from `ag`.
2. In single line mode, its behavior doesn't have a lot of choices. The main interesting one is that some lines may be printed more than once, which is unique to the `--vimgrep` flag.
3. When I added multi-line search, it wasn't obvious to me how `--vimgrep` should evolve its behavior. I did not know there was a spec for the format at the time (including for multi-line matches), and so I evolved it in a way that made sense to me: I generalized the property that lines in their entirety are printed for each match, including the _entire_ match, and may even result in printing duplicate lines. That's where its behavior today comes from.

> Regarding :vimgrep's format, the col there actually refers to byteoffet/byteindex from start of line. Just FYI, so you don't print "column number" as "character number".

Yes, ripgrep's column numbers are byte offsets. And "column number" is just as ambiguous as "character number." :-) I suspect the space of possible concrete definitions is at least:

1. Each byte is a column.
2. Each Unicode codepoint is a column.
3. Each grapheme cluster is a column.
4. Each column corresponds to a "width" of 1 according to [UAX #11](http://www.unicode.org/reports/tr11/).

These are all precise definitions with specific meanings.

My _guess_ is that by "character number" you mean "codepoint number."

---

_Comment by @poetaman on 2021-05-18 09:41_

@BurntSushi 

> I just don't understand why it took so long to get to this point in the conversation. Like, if there is a disagreement with a spec, then just say that and link to the relevant section of the spec.

You make it sound like we should know what you don't know. There was nothing in your prior comments to come to that conclusion. Also, given you added the ``--vimgrep`` option, any user would assume you have read what the spec is.

There are two to four sections worth reading to understand how Vim processes the lines returned by a regex engine. But I will summarize the story for you here in short.

* Vim can parse lines from various sources (make, grep, etc) using some default inbuilt patterns. These patterns are scanf like strings. The complete syntax is described in section [error-file-format](https://vimhelp.org/quickfix.txt.html#errorformat). Don't worry about the name, the same syntax can be used to make vim parse other information of interest (like searches, see for instance option [grepformat](https://vimhelp.org/options.txt.html#%27grepformat%27)).

* After processing the the output of external programs with scanf like patterns, the results are populated in a special list  that is used for navigating from one error (or search or etc) to another. This is important to know because otherwise one would not be able to understand why in [error-file-format](https://vimhelp.org/quickfix.txt.html#errorformat) Bram provided for patterns that get ignored or used for multiline output.

* The pattern used for vim's internal regex engine (``vimgrep``) (which is invoked by commands like ``:vimgrep``, ``:lvimgrep``, etc) is not specified, as I assume Vim doesn't need to output lines to itself & then parse them. But it behaves equivalent of ``"%f:%l:%c:%m"``†. This can be observed by performing ``:vimgrep /somepattern/g`` followed by ``:copen``. It will open a window with formatted search results, one per line (even for multi-line matches).‡

> † ``%f``: file, ``%l``: line number (for ``:vimgrep`` its start line number), ``%c``: "column" (byte offset), ``%m``: message (for vimgrep it's match). 
> ‡ From [error-file-format](https://vimhelp.org/quickfix.txt.html#errorformat):  "Keep in mind that in the :make and :grep output all NUL characters are replaced with SOH (0x01)." So if there is a ``\n`` in match string, it gets shown as something like ``^@`` in the flattened line derived from ``%m``.

* Though there is another command in vim that runs external regex engine: ``:grep``, it defaults to running ``>>grep -n``. That command can be changed, using option [grepprg](https://vimhelp.org/options.txt.html#%27grepprg%27). This is what most Vim users would use to direct external regex program's (like ``ag``, ``rg``, etc) result to Vim's internal lists. The default pattern ``%f:%l:%m`` for [grepformat](https://vimhelp.org/options.txt.html#%27grepformat%27) is meant for default ``>>grep -n``. It lacks field for byteoffset ``%c``, as the ``>>grep -n`` does not provide byteoffset of characters on line. So if someone redefines ``grepprg`` to ``rg --vimgrep``, they are better served to also redefine ``grepformat`` to ``%f:%l:%c:%m`` (which is essentially vimgrep's format); else all the cols will be 0. So one needs these two lines to make ``:grep`` use ``rg --vimgrep``, and populate internal lists in Vim.

> ```let &g:grepformat="%f:%l:%c:%m"```
> ```let &g:grepprg="rg --vimgrep"```

> As of now, your ``rg --vimgrep`` output will be interpreted with as "%f:%l:%m", all cols will be 0, and when user would want to move to next or previous search hit (using commands ``:cnext`` or ``:cprevious``), the cursor will progress to another line within a multiline match (instead of jumping to next/previous match).

* Vim does give you more freedoms, like if you are hellbent to print all lines of multi-line match, then that is possible. Check for string ``errorformat-multi-line`` in section [error-file-format](https://vimhelp.org/quickfix.txt.html#error-file-format) for that. It presents a sensible Python example of multiple-line error message, where only 1 line number gets added to vim's internal list to jump to.

* For the last point on printing multiline messages on separate lines, I would recommend against using that for multiple  reasons: a) the default ``:vimgrep`` behaves as if it has parsed a file which is formatted as ``"%f:%l:%c:%m"``, b) its a plugin creator's nightmare to process more special formats, c) you will have to provide the format of your output & link people to [error-file-format](https://vimhelp.org/quickfix.txt.html#errorformat) so they can study, understand, and write correctly parse your complicated vim error format.

Finally below is a mp4 that shows result of my integration of ``rg --json`` in vim. As can be seen, the multi-line match results are not printed on separate lines in match list window at the bottom of screen. Rather I rely on highlighting in file itself for user to see what lines multi-line matches contain, so there is no point of repeating the lines in match list. 

> Btw, Bram concedes that current Vimscript way of getting the information I asked for in this ticket would be a "bit slow": https://github.com/vim/vim/issues/8206#issuecomment-841629191.

https://user-images.githubusercontent.com/71736629/118628390-69e1c500-b781-11eb-925a-dd244c5e0ecc.mp4





---

_Comment by @BurntSushi on 2021-05-18 11:12_

> You make it sound like we should know what you don't know. There was nothing in your prior comments to come to that conclusion. Also, given you added the `--vimgrep` option, any user would assume you have read what the spec is.

Yes, you're right, I'm sorry. I spoke out of frustration.

Thank you for the follow up details. If you don't mind, I'd like to classify this issue as a bug and re-purpose it to "`--vimgrep` doesn't satisfy vim's spec for multi-line searches."

If and when you get to a point where it's clear that the `--json` output needs more information, please open a new issue.

---

_Label `bug` added by @BurntSushi on 2021-05-18 11:12_

---

_Renamed from "Print only beginning and ending linenumber and column of matches" to "--vimgrep doesn't satisfy vim's spec for multi-line searches" by @BurntSushi on 2021-05-18 11:12_

---

_Reopened by @BurntSushi on 2021-05-18 11:14_

---

_Comment by @BurntSushi on 2021-05-18 11:14_

(I will dig into your links when I get some time, and I'll plan to have this fixed for the next release if it's feasible to do so. I expect it will be.)

---

_Label `rollup` added by @BurntSushi on 2021-05-31 01:38_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
