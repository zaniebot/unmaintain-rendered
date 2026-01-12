```yaml
number: 930
title: Add json output format
type: issue
state: closed
author: garygreen
labels:
  - duplicate
assignees: []
created_at: 2018-05-28T15:28:49Z
updated_at: 2018-05-28T16:05:46Z
url: https://github.com/BurntSushi/ripgrep/issues/930
synced_at: 2026-01-12T16:13:22Z
```

# Add json output format

---

_@garygreen_

It would be awesome if ripgrep could output the search results in JSON format, something like:

```json
{
    "search_query": "ipaddress",
    "results": [
        {
            "file": "E:\\Sites\\cc-new\\app\\Member\\BannedMember.php",
            "line": 34,
            "content": "'ipaddress' => Request::getClientIp(),",
            "ranges": [
                [1, 9]
            ]
        },
        {
            "file": "E:\\Sites\\cc-new\\app\\Http\\Controllers\\Admin\\MemberController.php",
            "line": 177,
            "content": "$create_data['ipaddress'] = $request->get('ipaddress');",
            "ranges": [
                [14, 24],
                [44, 53]
            ]
        },
    ],
    "stats": {
        "total_matches": 2,
        "files_searched": 102
    }
}
```

This would mean you can create custom looking interfaces by consuming the search results much easier.

---

_Comment by @garygreen on 2018-05-28 15:42_

Another use case for this - currently vscode uses ripgrep and [manually parses](https://github.com/Microsoft/vscode/blob/4eaf58348b51e601877c360f59c3ca82f45691ad/extensions/search-rg/src/ripgrepTextSearch.ts#L141) the output in order to make sense of the output format which is a major undertaking in itself, but to display it in in vscode. If there was a consistent output format, then it would be MUCH easier to consume.

I realise this relates to other issues like: #244, #359, #848 but those haven't provided an example format. I honestly believe this can be relatively easily achieved if a bit of thought went into the output format as in the example provided. It may not cater to everyone's needs - but the main thing is to provide the raw data / context for each of the matches and then people can consume that data however they like.


---

_Comment by @BurntSushi on 2018-05-28 15:43_

Dupe of #244

---

_Closed by @BurntSushi on 2018-05-28 15:43_

---

_Label `duplicate` added by @BurntSushi on 2018-05-28 15:43_

---

_Comment by @BurntSushi on 2018-05-28 15:45_

@garygreen I'm not going to have multiple issues tracking structured output format. It's something I'd like to have for ripgrep some day. A hard blocker on doing this is getting [libripgrep done](https://github.com/BurntSushi/ripgrep/milestone/1).

---

_Comment by @garygreen on 2018-05-28 15:46_

@BurntSushi Understood. Do you have any preference on if it should be JSON output or something? That issue you linked to is something way back in 2016 and seems focussed on the file / range matches whereas if you had a JSON output you could also print all kinds of meta information related to the matches (like stats, how many matches in total, how many files searched, how many files ignored, etc). It maybe tricky having that information in a basic CSV/ackmate string fashion.


---

_Comment by @BurntSushi on 2018-05-28 15:48_

@garygreen #244 documents the "ackmate" format, which is a format specifically supported by other tools AIUI.

It's not clear to me whether we should do more than this. Supporting additional output formats is a maintenance hazard, and it might be better if folks just wrote their own programs with libripgrep (when it exists) that outputs the data they want. But this is a weakly held opinion at this point.

---

_Comment by @garygreen on 2018-05-28 15:59_

> and it might be better if folks just wrote their own programs with libripgrep

But that's the catch 22 problem - to do that, you need to regex the search results and output then into JSON, or whatever format you like then it becomes pointless - at that point you don't need an intermediary format you might as well just used the parsed regex parts. Look at what vscode is doing.

In it's core, ripgrep knows that what the file matches are, the lines, the ranges, the stats, can't it just output to something that can be easily parsed by tools? That doesn't seem particularity challenging problem to solve, IMO. Maybe I'm taking a simplistic view on this as you have a better understanding of the internals though

---

_Comment by @BurntSushi on 2018-05-28 16:05_

> But that's the catch 22 problem - to do that, you need to regex the search results and output then into JSON, or whatever format you like then it becomes pointless - at that point you don't need an intermediary format you might as well just used the parsed regex parts.

Yes, exactly.

> Look at what vscode is doing.

I know what VS Code is doing. It is not the only tool doing it.

> In it's core, ripgrep knows that what the file matches are, the lines, the ranges, the stats, can't it just output to something that can be easily parsed by tools? That doesn't seem particularity challenging problem to solve, IMO. Maybe I'm taking a simplistic view on this as you have a better understanding of the internals though

"just" and "challenging" are very relative terms. They are hand wavy. In principle, I agree with you. Please be content with the fact that my mind is not made up on this point. I'm trying to give you an honest assessment of where I'm sitting. I am skeptical because I don't perceive a huge demand for this. I am skeptical because I perceive this to add a maintenance burden. I am skeptical because it adds more dependencies. I am skeptical because it could increase code complexity. I am skeptical because, as the maintainer of ripgrep, I fear touching the printer.

My hope is that libripgrep will help reframe my skepticism and make this a more manageable task. Please do not try to force the issue. Please be patient.

---
