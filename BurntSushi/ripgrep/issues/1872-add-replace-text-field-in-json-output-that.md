```yaml
number: 1872
title: "Add \"replace\" text field in --json output that contains replace string for respective matches"
type: issue
state: closed
author: poetaman
labels:
  - enhancement
assignees: []
created_at: 2021-05-26T06:56:11Z
updated_at: 2025-09-20T01:08:29Z
url: https://github.com/BurntSushi/ripgrep/issues/1872
synced_at: 2026-01-12T16:13:24Z
```

# Add "replace" text field in --json output that contains replace string for respective matches

---

_@poetaman_

Currently ``rg --json`` provides full text of every match in submatch array item's ``match.text`` field. This ticket is to request addition of ``replace.text`` field in submatch array items. The ``replace.text`` fields will only get added if user passed the replace flag ``-r`` and the replace string. For usual searches without replace (``-r``), replace.text will not get added & the json output will be same as it is today.

Motivation: This will allow using ``rg --json`` to become a real alternative to Vim's internal search-and-replace functionality ``:%s/<match_pattern>/<replace_pattern>/g`` that is based on legacy vim regex format. Given the start-end position of submatches are known, developers would just need replacement text information to delete ``match.text`` from text files, and replace it with ``replace.text``. Given its in a clean json format, it would be easier to allow batch processing, replace an occurrence of interest, and even selectively rollback* easily. The same information can be used in other tools/scripts as well to do search-and-replace. Moreover, currently there is no way of doing this using ``rg`` with Vim & just having it integrated as a search tool makes it less preferred over vim's built in regex engine because: users will have to learn new regex syntax that they cannot use for capture group based complex text replace. This will force a user to either learn both syntaxes (vim regex, and rust regex), or just stick with vim regex (thus limiting adoption of ``rg``). Having this feature will also bring the power of pcre2 search and replace to Vim.

\* Selective rollback is different from undo. Undo by its very nature is LIFO, whereas selective rollback can be in any order the user wishes (without having to undo all steps after it). It's easy to build something like that in Vim with information asked in this ticket.

> NOTE: If you decide to add this, then please cover those corners where entire line is a match, and no submatches are printed.

---

_Renamed from "Add "replace" text field in --json output that contains the result if replace were to be performed" to "Add "replace" text field in --json output that contains replace string for respective matches" by @poetaman on 2021-05-26 06:58_

---

_Comment by @BurntSushi on 2021-05-26 10:40_

Just to make sure we are in the same page here, could you give an example input with desired output?

Thank you for detailed use case. It is a little hard for me to follow, since I'm perhaps not as familiar with the inner workings of vim's search and replace system. It might help to describe it from the perspective of data flow. That is, how does the data flow from ripgrep's JSON output through vim?

---

_Comment by @poetaman on 2021-05-27 22:28_

@BurntSushi Let's say for ``temp.tex``, we match ``\<macroname>{<optiontext>}`` or ``\<macroname>[<optiontext>]``, and want to replace it with a string ``\macro<macroname>{option:<optiontext>}``. The ``rg`` command will look like this:

Command:
```regex
rg -N --passthrough '^.*\\(.*)\{(.*)\}.*?$|^.*\\(.*)\[(.*)\].*?$' -r '\macro$1{option:$2}' temp.tex
```

Input file:
```tex
% temp.tex
\documentclass{article}

\usepackage{blindtext}
\usepackage{fancyhdr}
\usepackage{pdftexcmds}
\pagestyle{fancy}

\begin{document}
\blindtext[5]
\end{document}
```

This will produce output:
```tex
\macrodocumentclass{option:article}

\macrousepackage{option:blindtext}
\macrousepackage{option:fancyhdr}
\macrousepackage{option:pdftexcmds}
\macropagestyle{option:fancy}

\macrobegin{option:document}
\macro{option:}
\macroend{option:document}
```

> As a sidenote, why does the second pattern (``^.*\\(.*)\[(.*)\].*?$``) not match \blindtext[5]?

As of today if ``--json`` option is passed, one of the ``"type":"match"`` json will look like this:

```json
{
    "type": "match",
    "data": {
        "path": {
            "text": "temp.tex"
        },
        "lines": {
            "text": "\\documentclass{article}\n"
        },
        "line_number": null,
        "absolute_offset": 0,
        "submatches": [
            {
                "match": {
                    "text": "\\documentclass{article}"
                },
                "start": 0,
                "end": 23
            }
        ]
    }
}
```

There are multiple ways in which you could decide to add replace text information. It could be either 1) in existing list  ``data.submatches[<num>].match.replacetext`` (or pick a shorter  name ``rtext`` or just ``replace``); or 2) in a new list like ``data.submatches[<num>].replace.text``. The advantage of former approach is three pronged: a) less text to print, b) consistency: for the case where you mention that entire line can be a match and submatches are empty, you could add replace text information in ``data.lines.replacetext``, or you will have to create something like ``data.replace.text``, c) fields like ``data.submatches[<num>].start`` & ``data.submatches[<num>].end`` won't look orphaned from ``data.submatches[<num>].match.text``.

1) json for possibility-1:
```json
{
    "type": "match",
    "data": {
        "path": {
            "text": "temp.tex"
        },
        "lines": {
            "text": "\\documentclass{article}\n"
        },
        "line_number": null,
        "absolute_offset": 0,
        "submatches": [
            {
                "match": {
                    "text": "\\documentclass{article}",
                    "replacetext": "\\macrodocumentclass{option:article}"
                },
                "start": 0,
                "end": 23
            }
        ]
    }
}
```

2) json for possibility-2 (with a typo mentioned in comment https://github.com/BurntSushi/ripgrep/issues/1872#issuecomment-849994553 corrected)

```json
{
    "type": "match",
    "data": {
        "path": {
            "text": "temp.tex"
        },
        "lines": {
            "text": "\\documentclass{article}\n"
        },
        "line_number": null,
        "absolute_offset": 0,
        "submatches": [
            {
                "match": {
                    "text": "\\documentclass{article}"
                },
                "replace": {
                    "text": "\\macrodocumentclass{option:article}"
                },
                "start": 0,
                "end": 23
            }
        ]
    }
}
```

--------------------------------------------------

Regarding flow of data from ``rg`` into vim: vimscript provides a functions to run external programs ``:h system()``, and to parse json strings: ``:h json_decode()``. Then user has a choice based on what they intend to do with the data. For the feature I intend to implement, I would use vim's inbuilt data structure called quickfixlist ``:h quickfix.txt``. Its a logic-less data structure by itself, but it comes packaged with commands that make life easier to traverse the list (in our case the matches). The selective rollback, etc logic, I will code myself as there is no pre-baked function to replace bytes in a file (afaik).

---

_Comment by @BurntSushi on 2021-05-27 22:56_

The examples help a lot, thanks.

With respect to `"match": {"text": "..."}`, the `{"text": "..."}` is an [arbitrary data object](https://docs.rs/grep-printer/0.1.5/grep_printer/struct.JSON.html#object-arbitrary-data). The value pointed to by `match` is either valid UTF-8 and thus represented as `{"text": "..."}`, or it is not valid UTF-8 and is thus represented as `{"bytes": "<base64 of data>"}`. So having `"match": {"text": "...", "replacetext": "..."}` doesn't make much sense. At that level, your second option makes more sense. (I assume it is a typo that `replacetext` is present twice in your second possibility?)

> Regarding flow of data from rg into vim: vimscript provides a functions to run external programs :h system(), and to parse json strings: :h json_decode(). Then user has a choice based on what they intend to do with the data. For the feature I intend to implement, I would use vim's inbuilt data structure called quickfixlist :h quickfix.txt. Its a logic-less data structure by itself, but it comes packaged with commands that make life easier to traverse the list (in our case the matches). The selective rollback, etc logic, I will code myself as there is no pre-baked function to replace bytes in a file (afaik).

Hmm so this wasn't quite what I was hoping for. I'm not particularly interested in Vim features. While I've used Vim for a long time, certain corners of it remain mysterious to me and I do not have the background knowledge necessary to write plugin. So talking about this in terms of Vim features doesn't aide understanding unfortunately. Let me take a guess at what I think you want to do:

* You are adding a text replacement feature to Vim.
* You want to implement this with ripgrep. When text replacement is requested, you shell out to ripgrep. Since you want structured data, you use the `--json` flag.
* ripgrep already provides a text replacement option, so you're hoping to use that instead of re-implementing it yourself. (And re-implementing it yourself would be a fair amount of work I imagine, since I assume you'd want to use the same regex engine as ripgrep.)
* Thus, you accept some user input that is intended to be used with the `--replace` flag, possibly after some transformation. But as it turns out, ripgrep ignores the `--replace` flag in JSON output mode. So you'd like ripgrep to support it.
* ripgrep already provides the byte offsets of each match. So if it also provides the replacement for each match, then you could use those byte offsets to splice the replacement into the user's data.
* As far as I can tell, the _only_ relevant pieces of data in the JSON output for your use case are `absolute_offset`, `submatches[].start`, `submatches[].end` and a hypothetical `submatches[].replacement.{text|bytes}`.

If that's right, then I believe this should be doable and I agree that it sounds like a good thing to support.

It might also be appropriate to provide a `lines_with_replacement` field as well, although I'm less sure about that. Probably best to punt. AIUI, that isn't something you need here. 

> As a sidenote, why does the second pattern `(^.*\\(.*)\[(.*)\].*?$)` not match \blindtext[5]?

It does, but ripgrep's regex engine's capturing groups don't work in the way you're trying to use them. For the regex `(\w+)|(\d+)`, there are three capturing groups: `$0` (the whole match), `$1` (the first alternate) and `$2` (the second alternate. Only one of `$1` or `$2` will be non-empty.

ripgrep has very limited facilities for text replacements. The `-r/--replace` flag solves the 80% use case, but it is not maximally flexible.

---

_Comment by @poetaman on 2021-05-27 23:26_

@BurntSushi

> (I assume it is a typo that replacetext is present twice in your second possibility?)

Yep thats a typo. I have updated the comment https://github.com/BurntSushi/ripgrep/issues/1872#issuecomment-849982920

Yep your analysis is correct.

> (the second alternate. Only one of $1 or $2 will be non-empty. ripgrep has very limited facilities for text replacements. The -r/--replace flag solves the 80% use case, but it is not maximally flexible.

What about replacement with ``--pcre2`` flag? Does that also have this limitation? Is there a way to do this with ripgrep without having to run the search-and-replace multiple times (with different sub patterns?) 

Not sure what you meant by ``lines_with_replacement``, if its the entire set of lines after replacements are done, then no I won't need that. Yes the set of information you mention that I will need is correct, I don't need more information.

---

_Comment by @BurntSushi on 2021-05-27 23:43_

> What about replacement with `--pcre2` flag? Does that also have this limitation?

Yes. It's inherent to how capture groups are indexed. With that said, I am not a PCRE2 expert and PCRE2 has many options. For example, the `PCRE2_DUPNAMES` option looks applicable here, but ripgrep does not set it. For the default regex engine, this issue is related: https://github.com/rust-lang/regex/issues/492

(There are competing concerns here. ripgrep's default regex engine is the main support for regexes for a full programming language. In that context, not being able to use duplicate capture group names is usually a minor annoyance. But in contexts such as yours, where your expressiveness is severely restricted, every little bit of extra juice you can squeeze out of the regex itself helps.)

---

_Comment by @poetaman on 2021-05-28 03:44_

@BurntSushi I think what I need can be done with ``PCRE2_SUBSTITUTE_EXTENDED``. Given ``--pcre2`` engine is a fallback engine for ripgrep, setting this would not harm. It would be nice to have a config file option to provide default ``PCRE2`` flags.

---

_Comment by @BurntSushi on 2021-05-28 12:38_

@reportaman It wouldn't work. ripgrep doesn't use PCRE2's replacement routines.

---

_Comment by @poetaman on 2021-05-28 16:18_

@BurntSushi Ok sure, running batch replacement from within vim multiple times (to cover different sub pattern cases) on files of a project wouldn't hurt. If users want to do batch replacement on thousands of files they should anyway do it from command line.

I look forward to playing with the replace strings you add to ``--json`` printer output.

Curious why that decision though? Couldn't ripgrep just link to libpcre* and use that for fallback?

---

_Label `enhancement` added by @BurntSushi on 2021-05-29 12:30_

---

_Comment by @manikantag on 2023-02-27 04:52_

@BurntSushi, Thanks for the excellent tool.

I also need to replace matched strings in the files. Having a replacement string will make it a lot easier and most of all the replacement strings will be consistent with the ripgrep matching engine.

Do you have any plans for this enhancement?

Also, I'm curious that is there a specific reason you chose not to implement the file replacement functionality as, like you said, the tool is already doing 80% heavy lifting? 

---

_Comment by @BurntSushi on 2023-02-27 10:45_

@manikantag This issue isn't about replacing stuff in files. This issue is about the `--json` output. See #74. 

> Do you have any plans for this enhancement?

What enhancement are you talking about? If you're talking about the one represented by this specific issue, then yes, it is open and marked as `enhancement`.

> the tool is already doing 80% heavy lifting?

That's not really what I said. I was alluding to the [Pareto principle](https://en.wikipedia.org/wiki/Pareto_principle). That is, it hits the 80% use case with _not much effort_, but going the rest of the way takes quite a bit more work and complexity. So it is the opposite of "if you're already 80% of the way there, then why not just do a little bit more to get to 100%."

---

_Comment by @manikantag on 2023-03-01 03:12_

@BurntSushi yes, I meant the JSON changes only as you clearly mention no option in ripgrep will change the actual file content (even `-r`). 

My thought is if the tool includes the replacement string in the JSON output, then by consuming the JSON, one can built tool to the actual file replacement. 

May be I'm not clear enough, but I said 80% because the tool is already giving matching file, line, column & highlighting the word match too. Using the JSON output, I'm able to generate HTML markup like how vscode (or any other editor) is doing. Now with addition of replacement string in JSON output, my end objective to build 'File search & replace' feature will be 100% complete. 

I see you considered this as an enhancement. Looking forward for it. And hopefully the JSON output schema will not change in future which would break my logic.

Thanks

---

_Comment by @MagicDuck on 2024-08-08 19:26_

My plugin could also use this feature. Currently I am working around it by setting colors for matches like rgb(0,0,1) and parsing those from the output in order to figure out what to highlight. Having the replacement would allow me to use the json output instead and show a diff as well.

---

_Comment by @MagicDuck on 2024-08-30 07:18_

@BurntSushi my rust skills are very tiny, but am I correct in thinking that this work would mainly involve doing the same "replacer" thing as in the standard printer:
https://github.com/BurntSushi/ripgrep/blob/e0f1000df67f82ab0e735bad40e9b45b2d774ef0/crates/printer/src/standard.rs#L576
(and all it's usages in that file)
but in the json printer?
https://github.com/BurntSushi/ripgrep/blob/e0f1000df67f82ab0e735bad40e9b45b2d774ef0/crates/printer/src/json.rs

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
