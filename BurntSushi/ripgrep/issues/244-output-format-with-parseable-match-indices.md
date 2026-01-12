```yaml
number: 244
title: Output format with parseable match indices?
type: issue
state: closed
author: birkenfeld
labels:
  - enhancement
  - question
  - libripgrep
assignees: []
created_at: 2016-11-21T19:17:11Z
updated_at: 2018-08-20T11:10:20Z
url: https://github.com/BurntSushi/ripgrep/issues/244
synced_at: 2026-01-12T16:13:21Z
```

# Output format with parseable match indices?

---

_@birkenfeld_

For integration in other tools (editors...) it would be great to have a parseable output format that includes the start and end index for each match in a line. You can do it with colors (but see #242) but it's not really what colors are meant for.

ag has --ackmate, which outputs something like this:
```
$ ag --ackmate host
:src/util.rs
85;43 4,55 4:                                       ext_host: "localhost".into(),
```

---

_Comment by @BurntSushi on 2016-11-21 19:22_

Is `ackmate` a sufficiently popular format that we should just provide that and call it done? Is it convenient to use? Is there a specification for it?

---

_Label `enhancement` added by @BurntSushi on 2016-11-21 19:22_

---

_Label `question` added by @BurntSushi on 2016-11-21 19:22_

---

_Comment by @BurntSushi on 2016-11-21 19:23_

Note that `--vimgrep` exists, but I think it's missing the end position of a match (it give you line number and starting column number).

---

_Comment by @birkenfeld on 2016-11-21 19:28_

> Is ackmate a sufficiently popular format that we should just provide that and call it done? Is it convenient to use? Is there a specification for it?

It seems to be what ack clones have. Convenience is probably not that important here - although I like that it avoids text volume because file names are not repeated. There is no specification that I know of.

---

_Comment by @BurntSushi on 2016-11-22 01:43_

The format for `--ackmate` appears to be:

```
:{file-path}
{line};{column} {length}:{match}
```

Note that the `--vimgrep --heading` format is:

```
{file-path}
{line}:{column}:{match}
```

They are pretty close, including the reduction in volume by avoiding repeated file paths. Alas, `--vimgrep` is missing the length of the match. I don't think changing `--vimgrep` is an option, and supporting `--ackmate` seems nice if only because it's probably a format that people have written ad hoc parsers for.

---

_Comment by @BurntSushi on 2016-11-22 01:46_

One thing that grates me a little bit is that I managed to implement the `--vimgrep` option in the `Printer` without actually mentioning or relying on the concept "vimgrep." (Instead, it's a combination of `line_per_match`, `column` and `line_number`.) With `--ackmate`, its format is so weird that I'd probably need to add an option just for it. Blech.

---

_Comment by @BurntSushi on 2016-11-22 01:47_

@birkenfeld Could you say more about why you specifically need this? I've never done an editor integration, so I don't really know. However, I do know that lots of folks have already integrated ripgrep into emacs and vim, and this is the first I'm hearing that `--vimgrep` is insufficient.

---

_Comment by @BurntSushi on 2016-11-22 01:50_

The "orthogonal" way to do this I guess is to add a `--length` option that includes the length of the match (in bytes). And if we're going to make `--column` imply `--line-number`, then `--length` probably needs to in turn imply `--column`. That way, you could use `rg --length --heading foo` to get:

```
{file-path}
{line}:{column}:{length}:{match}
```

I think I'd prefer this over hacking `--ackmate` into the printer. (Which I have plans of factoring out into a library.)

---

_Comment by @birkenfeld on 2016-11-22 07:34_

The goal is highlighting matched parts of a line in an editor-specific way (e.g. faces in Emacs). The ripgrep Emacs package once had this feature, but relied on parsing color escapes, which changed in 0.3. 

A vimgrep-like format including match length is possible, but harder to handle because usually you'd want to display a matching line only once, but highlight multiple matches if appropriate. With an ackmate-like format, which specifies all matches in a single line (see initial post), that requires much less postprocessing of the output.

cc @nlamirault (the ripgrep.el author)

---

_Comment by @BurntSushi on 2016-11-22 12:10_

> but relied on parsing color escapes

Wow. I didn't even consider that as a breaking change. And probably still don't. Relying on color escapes sounds like really bad juju! But I understand why someone might do it if there's no other way!

> A vimgrep-like format including match length is possible, but harder to handle because usually you'd want to display a matching line only once, but highlight multiple matches if appropriate.

OK, this is a key thing I was missing. I guess in vim, the opposite is true. This also means that my description of the `ackmate` format above was wrong. It looks like this instead:

```
:{file-path}
({line};{column} {length}, ...):{match}
```

Blech. I guess I begrudgingly accept that we should do this. I will attempt to put this into the current printer, but it's not going to be nice, and hopefully I can rethink it for libripgrep. The key problem with adding stuff to the printer is that you need to consider its interaction with *every other* output flag. For example, does `--no-heading/--heading` have an effect with `--ackmate`? (Yes?) What about `--no-filename`? (Yes?) Some others: `--vimgrep`, `--null`, `--context/--before-context/--after-context`.

---

_Label `question` removed by @BurntSushi on 2016-11-22 12:11_

---

_Assigned to @BurntSushi by @BurntSushi on 2016-11-22 12:11_

---

_Label `question` added by @BurntSushi on 2016-11-28 22:43_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Label `libripgrep` added by @BurntSushi on 2017-01-11 03:04_

---

_Comment by @garygreen on 2018-05-28 15:52_

@BurntSushi 

> For example, does --no-heading/--heading have an effect with --ackmate? (Yes?) What about --no-filename? (Yes?) Some others: --vimgrep, --null, --context/--before-context/--after-context.

Should any flags have an effect on the *structured* output format? No. Absolutely not. That's the whole point of having a format that is consistent and won't change in it's format. Some flags *may* add data as it'll perform additional search, but the format itself should remain unchanged and be consistent no matter what flags you add.

I would also suggest this should be done in JSON format rather than the way ackmate does, that way you can have an output structure like this:

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

Which allows you to include all sorts of meta information and other stuff in a meaningful and consistent way. At it's core, it's not about *displaying*, this is to do with being able to *consume* the data in an easy way, for example in text editors/extensions/packages/tools.

---

_Comment by @BurntSushi on 2018-05-28 15:58_

> Some flags may add data

Yes, that's what I meant. This implies doing one of the following:

1. Duplicating the case analysis in the current printer for each output format.
2. Abstracting the case analysis so that it is done in one place that works for all formats. Performance of the printer is important, so this abstraction must be either zero overhead or so close to zero overhead that it does not have a measurable impact on the speed of ripgrep.

---

_Comment by @garygreen on 2018-05-28 16:08_

> Abstracting the case analysis so that it is done in one place that works for all formats

That's kind of what I was thinking - when ripgrep is coming across matches it should just add it to some kind of `ResultsList` class and have an event where as matches come through if your outputting to console then output them as they come through, or pipe to JSON.

So is that the issue then, that there is no intermediary part of ripgrep that holds a consistent data format? It's all output on the fly to console without  structuring the data internally first?


---

_Comment by @BurntSushi on 2018-05-28 16:11_

The printer isn't that big; it's just subtle. You can skim it and get an idea of the type of case analysis involved here: https://github.com/BurntSushi/ripgrep/blob/master/src/printer.rs

It is used directly from the searcher. Do a search for `self.printer`: https://github.com/BurntSushi/ripgrep/blob/master/src/search_stream.rs

---

_Comment by @BurntSushi on 2018-06-05 11:37_

Another thing to consider with respect to JSON is that I was thinking about a "JSON lines" format. But I see the proposal from @garygreen above is not. The benefit of a JSON lines approach is that each match can be emitted as it appears. If we instead emit one big JSON blob, then all of the matches must be buffered into memory before we get any output at all.

---

_Comment by @garygreen on 2018-06-05 12:28_

I was having the same thoughts, I think a "JSON lines" is an excellent idea and would allow you to stream the results in a consistent format.

Maybe this could be like:

Start of query:
```json
{
    "type": "begin_search",
    "token": "<some unique token string>",
    "query": "ipaddress",
    "paths": ["path1", "path2"]
}
```

A line match:
```json
{
    "type": "line_match",
    "token": "<token>",
    "file": "E:\\Sites\\cc-new\\app\\Member\\BannedMember.php",
    "line": 34,
    "content": "'ipaddress' => Request::getClientIp(),",
    "ranges": [
        [1, 9]
    ]
}
```

End of query:
```json
{
    "type": "end_search",
    "token": "<token>",
    "stats": {
        "total_matches": 2,
        "files_searched": 102
    }
}
```

The `token` is just an idea to make sure you are consuming results from the same stream, in case you have parallel things running? Not sure if that would be a concern or not but maybe something worth considering.


---

_Comment by @garygreen on 2018-06-05 12:37_

Interesting wiki on JSON streaming: https://en.wikipedia.org/wiki/JSON_streaming

---

_Comment by @alphapapa on 2018-06-22 01:59_

JSON output would be very useful indeed for editor integration.  e.g. in Emacs one could use the built-in JSON-parsing functions, which are written in C, rather than doing regexp-parsing of ripgrep's output.

Does JSON output deserve a separate issue, or should this one be commandeered for it?  :)

---

_Comment by @BurntSushi on 2018-07-24 11:11_

For the people who desire JSON output, how would you handle the fact that ripgrep's output is not necessarily guaranteed to be valid UTF-8? This is a problem for JSON because JSON requires its strings to be valid UTF-8 (or UTF-16 or UTF-32, but we can ignore those for the purposes of ripgrep and this problem). At a high level, I think there are three approaches ripgrep could take:

1. Silently drop any matches containing invalid UTF-8.
2. Lossily encode any invalid UTF-8 using the Unicode replacement codepoint.
3. Come up with a tagged union representation that represents matches that are valid UTF-8 as standard JSON strings and matches that are invalid UTF-8 as base64 encoded JSON strings.

(1) kind of stinks, for obvious reasons.

(2) sounds nice on the surface, but I also envision the JSON output containing match offsets for individual matches found within the line. It seems doable to update those offsets such that they are correct for the lossily encoded match string. (e.g., A single `\xFF` byte would be replaced by `\xEF\xBF\xBD`, which is the UTF-8 encoding of `U+FFFD`.) This seems annoying to me. This also seems less than ideal since I could imagine that consumers might want to use those matches offsets to go find something in the original file for example, which wouldn't work unless we yielded two sets of matches offsets: one for the string provided in the JSON and another for the original string found in the file. Again, that's annoying.

(3) feels like it might be the best solution. It preserves, byte-for-byte, the original match and also makes it easy for callers to choose their own behavior. e.g., They could ignore matches that aren't valid UTF-8 for example. Here's a straw man JSON representation for matches that are valid UTF-8:

```json
{
    "text": "the match",
    "bytes": null
}
```

and now for matches that are not valid UTF-8:

```json
{
    "text": null,
    "bytes": "dGhl/21hdGNo"
}
```

where `dGhl/21hdGNo` is base64 for `the\xFFmatch`.

---

_Comment by @fluffysquirrels on 2018-07-24 13:08_

I like (2) and (3). Consumers writing a simple script may want to display
_something_ useful in all cases (this is my typical use case). More
sophisticated consumers may want something completely correct and accurate
(e.g. IDE integration or editing the file).

How about providing both UTF-8 encoded and raw bytes, one for each
consumer? Or a command line flag for one or the other.

On Tue, 24 Jul 2018, 12:11 Andrew Gallant, <notifications@github.com> wrote:

> For the people who desire JSON output, how would you handle the fact that
> ripgrep's output is not necessarily guaranteed to be valid UTF-8? This is a
> problem for JSON because JSON requires its strings to be valid UTF-8 (or
> UTF-16 or UTF-32, but we can ignore those for the purposes of ripgrep and
> this problem). At a high level, I think there are three approaches ripgrep
> could take:
>
>    1. Silently drop any matches containing invalid UTF-8.
>    2. Lossily encode any invalid UTF-8 using the Unicode replacement
>    codepoint.
>    3. Come up with a tagged union representation that represents matches
>    that are valid UTF-8 as standard JSON strings and matches that are invalid
>    UTF-8 as base64 encoded JSON strings.
>
> (1) kind of stinks, for obvious reasons.
>
> (2) sounds nice on the surface, but I also envision the JSON output
> containing match offsets for individual matches found within the line. It
> seems doable to update those offsets such that they are correct for the
> lossily encoded match string. (e.g., A single \xFF byte would be replaced
> by \xEF\xBF\xBD, which is the UTF-8 encoding of U+FFFD.) This seems
> annoying to me. This also seems less than ideal since I could imagine that
> consumers might want to use those matches offsets to go find something in
> the original file for example, which wouldn't work unless we yielded two
> sets of matches offsets: one for the string provided in the JSON and
> another for the original string found in the file. Again, that's annoying.
>
> (3) feels like it might be the best solution. It preserves, byte-for-byte,
> the original match and also makes it easy for callers to choose their own
> behavior. e.g., They could ignore matches that aren't valid UTF-8 for
> example. Here's a straw man JSON representation for matches that are valid
> UTF-8:
>
> {
>     "text": "the match",
>     "bytes": null
> }
>
> and now for matches that are not valid UTF-8:
>
> {
>     "text": null,
>     "bytes": "dGhl/21hdGNo"
> }
>
> where dGhl/21hdGNo is base64 for the\xFFmatch.
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/244#issuecomment-407369034>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAm2krEEQVVCgIC8SfYiTFVZJSrjdRz6ks5uJwDhgaJpZM4K4lcS>
> .
>


---

_Comment by @BurntSushi on 2018-07-24 13:11_

I doubt I'll do both. Doing both would require computing match indices for both.

In general, "just do everything" isn't a tenable strategy moving forward because of the maintenance costs it implies. A benefit of option (3) is that it is both very simple to implement and very simple to get correct and very hard to use incorrectly. I'm inclined to say that "writing a simple script that handles all cases" ceases to be simple, and that you should just base64 decode the raw bytes and do your own lossy decode if that's what you want.

---

_Comment by @fluffysquirrels on 2018-07-24 13:16_

Fair points. Then (3) is easy to implement and imposes only a small cost
for the simplr script consumer; sounds like a great option.

On Tue, 24 Jul 2018, 14:11 Andrew Gallant, <notifications@github.com> wrote:

> I doubt I'll do both. Doing both would require computing match indices for
> both.
>
> In general, "just do everything" isn't a tenable strategy moving forward
> because of the maintenance costs it implies. A benefit of option (3) is
> that it is both very simple to implement and very simple to get correct and
> very hard to use incorrectly.
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/244#issuecomment-407400964>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAm2ksqyoha14sFI7ShXlQ3wsdfdx2s9ks5uJx0EgaJpZM4K4lcS>
> .
>


---

_Comment by @BurntSushi on 2018-07-29 20:11_

For folks interested extracting machine readable descriptions of search results from ripgrep, would you want to take a quick look [my proposed JSON wire format](https://burntsushi.net/stuff/libripgrep/doc/grep_printer/struct.JSON.html) and see whether it looks reasonable to you?

cc @roblourens I'm especially interested in your opinion, since I think you're are my biggest machine consumer, and I suspect you'd be happy to stop parsing ANSI escape sequences. :-)

---

_Comment by @garygreen on 2018-07-29 21:12_

@BurntSushi

That is some awesome work Andrew. Thank you so much for all your time and effort on this it's much appreciated.

I'm also interested in seeing what Rob makes of it as I'm sure vscode would love to consume the JSON results rather than doing some manual parsing.

Anyway, I've had a look over the proposed spec if your interested in hearing my thoughts and it looks perfect, though I have made a few observations as below.

---

* Any reason why submatches need to be keyed by "match"?

```json
"submatches": [
  {"match": {"text": "Watson"}, "start": 15, "end": 21}
]
```

Would this be simpler?

```json
"submatches": [
  {"text": "Watson", "start": 15, "end": 21}
]
```

* Will the submatches always be in order, e.g. start of 15 would be always before start 30? I'm sure it wouldn't make much difference to consumers but might be worth noting.

* Would it make sense rather than having `path` in every JSON-line to instead have some kind of code that uniquely represents the search being performed?

For example:

```json
{
  "type": "begin",
  "token": "nBeqhQgeljaMnkq2J1h6",
  "data": {
    "path": {"text": "/home/andrew/sherlock"}},
  }
}
{
  "type": "context",
  "token": "nBeqhQgeljaMnkq2J1h6",
  "data": {
    "lines": {"text": "can extract a clew from a wisp of straw or a flake of cigar ash;\n"},
    "line_number": 4,
    "absolute_offset": 193,
    "submatches": []
  }
}
```

This would ensure that you are consuming results for the right search - for example, if your running search in parallel on the same terminal but for different queries, simply saying "path" won't allow you to group the results easily.

With a unique token approach (which can just be a random string of characters to make collisions extremely unlikely), you can group those results and know exactly what search they are for. I'm sure this would also be useful for vscode and other consumers as well that handle a `Cancellation Token` of some kind - they could use the token for that search.


---

_Comment by @BurntSushi on 2018-07-29 21:27_

@garygreen Awesome, thanks for taking a look and giving feedback!

> Any reason why submatches need to be keyed by "match"?

Yes. A `match` is an [arbitrary data object](https://burntsushi.net/stuff/libripgrep/doc/grep_printer/struct.JSON.html#object-arbitrary-data), which means it is itself an object that contains either a `text` or `bytes` field. The `text`/`bytes` fields could be "inlined" into the submatch itself, but I'd rather not do that and keep things a bit more explicit. This is also consistent with how file paths are handled.

> Will the submatches always be in order, e.g. start of 15 would be always before start 30? I'm sure it wouldn't make much difference to consumers but might be worth noting.

Hmm, good question. It _seems_ reasonable to guarantee an ordering here. I will add that to the docs.

> Would it make sense rather than having path in every JSON-line to instead have some kind of code that uniquely represents the search being performed?

I think I _at least_ want to have a path in every message, which should be much more convenient for consumers. If you don't include a path, then it makes it necessary for even simple use cases like "do something with a match and its file path" to track state.

I've thought about your identifier idea. I am not opposed to it. But I would like to see how far we can get without it and just use file paths, which are something I think should be in every message anyway.

If you're worried about file paths adding too much redundant to the output, then it is plausible that we could make ripgrep's `--no-filename` suppress `path` in match/context messages (which is consistent with its current documented behavior). In which case, we'd probably want to add an identifier to each message. But this is a backwards compatible addition and is more complex, so I'd like to see how far we can get with a simpler approach.

> This would also ensure that you are consuming results for the same search - for example, if your running search in parallel on the same terminal but for different queries, simply saying "path" won't allow you to group the results easily. With a unique token approach, you can group those results and know exactly what search they are for. I'm sure this would also be useful for vscode and other consumers as well that handle a Cancellation Token of some kind - they could use the token for that search.

I'm not sure I'm convinced of the prevelance of this use case. Generally speaking, you don't exploit parallelism by running multiple ripgrep processes. Instead, you should let ripgrep handle parallelism for you.

Also, how is this token generated? Is the onus on ripgrep to ensure that every token it generates is unique? Probably a uuidv4 is good enough for that.

---

_Comment by @roblourens on 2018-07-30 21:13_

I think this looks great, and I am very excited to delete the current parsing code 😁 

What's the purpose of the `begin` message? I'm not sure what that would be used for. Will begin/end be sent even when a file doesn't contain any matches?

In developing vscode's search provider API, I've thought about matches in very long lines, or very long matches. The [current version of the API](https://github.com/Microsoft/vscode/blob/master/src/vs/vscode.proposed.d.ts#L139) has result objects that include a "preview" of the match, not necessarily the whole line. Then the client will be able to request a preview of a certain size in some TBD format, such as number of characters before and after the match, or just total number of characters.

I wonder whether something like that would be useful for ripgrep too. If you have a short match in a very long line, the consumer doesn't necessarily need the full text of the line.

On the other hand, if you have several matches in a long line, you won't be duplicating the full line text, which isn't true in vscode's model.

If the full text of the line is present, then technically you don't need to include the match text in 'submatches', since it can be easily derived from `lines`, right? I suppose it's convenient to have it already there.

I like the approach for non-utf8 encodings, I think that's a good idea.

---

_Comment by @BurntSushi on 2018-07-30 21:24_

@roblourens That's great feedback, thank you!

> What's the purpose of the begin message? I'm not sure what that would be used for. Will begin/end be sent even when a file doesn't contain any matches?

Yeah, the intent is that both `begin` and `end` would be sent even if the file didn't contain any matches. Writing that out though, I wonder if that will actually adversely impact performance. It probably will. I could change it such that `begin`/`end` are only emitted when there is at least one match.

The purpose of `begin` isn't entirely clear, other than to serve as an indicator that a new set of matches is being shown. If we adopt a token approach as described above, then `begin` would also serve to introduce a token for each new search. I could also imagine `begin` potentially growing new fields as time progresses. If we make it so `begin`/`end` are only shown when there's a match, then `begin` itself probably has very little downside.

> In developing vscode's search provider API, I've thought about matches in very long lines, or very long matches. The current version of the API has result objects that include a "preview" of the match, not necessarily the whole line. Then the client will be able to request a preview of a certain size in some TBD format, such as number of characters before and after the match, or just total number of characters.

> I wonder whether something like that would be useful for ripgrep too. If you have a short match in a very long line, the consumer doesn't necessarily need the full text of the line.

> On the other hand, if you have several matches in a long line, you won't be duplicating the full line text, which isn't true in vscode's model.

Ah this is interesting! My current thinking is to just let consumers such as yourself sort this out, but if the volume of data turns out to be a performance bottleneck then we can certain add some backwards compatible knobs for this going forward.

> If the full text of the line is present, then technically you don't need to include the match text in 'submatches', since it can be easily derived from `lines`, right? I suppose it's convenient to have it already there.

That's correct, yeah. The text in each submatch is strictly superfluous. It's mostly there as a convenience. We can expose knobs for this too if performance is a problem.

@roblourens @garygreen If you'll allow me to summarize to make sure we're on the same page:

1. Short of actually using JSON wire format (which might reveal problems, as such things always do), it appears reasonable enough to emit search results in a structured format.
2. There may be a desire for adding tokens to the output so that the consumer can properly associate inputs with outputs. But this can be done in a backwards compatible way.
3. The current JSON wire format is perhaps convenient, but may have print too much data such that it becomes a performance bottleneck. We can expose new knobs (as ripgrep flags) that reduce the data volume in various ways if that would help performance, but we can cross that bridge when we get there and tune as needed.

---

_Comment by @garygreen on 2018-07-30 22:24_

Interesting observation matching against a huge line. I can see it being a concern for people who match against minified files (god knows why though). I'm unfamiliar with how ripgrep currently handles those matches, I would assume it just outputs a massive line showing the match.

It's not really a problem specifically related to the JSON formatter, though. If it does become an issue, then I'm sure we could consider adding some kind of "--match-byes-around=100" type option which would give you 100 bytes around the matches. Again, not sure how ripgrep handles that currently so it might already be available.

### `begin` and `end` when no matches

I would agree that it's probably not worth sending these for files with no matching results. Just skip sending both, that would also prevent sending JSON lines for all files that don't have any matches (those files the consumer probably isn't interested in at). If consumers did want it, then it could probably be added as an option to always send them, even when no matches with a caveat that it may impact performance.

### `progress` idea

Following on from the above, in order to avoid sending lots of redundant data like stats for `end` matches for each file - instead have a new `progress` type which periodically outputs the overall progress of the search.

How often this is output could be configured `--json-progress-timer=1000` which would output progress every second. Alternatively, it could output progress after searching every X files.

This would also serve as a way of providing stats for what's been searched so far periodically - especially in cases where there are no matches. Say your searching 100,000 files that takes a few minutes and expect no matches, if you didn't send `begin` and `end` you would have no progress updates or no way of displaying how many files have been searched so far until the search completes.

### `finish` idea

At the moment there is currently no way of knowing when the search is totally finished, so you may be left in limbo and can't for instance have a UI that says "Search complete"? You could possibly listen for when the ripgrep process exits, but maybe it would be easier to be explicit and have a `finish` type that would let you know when the search has finished in it's entirety. It would contain a `stats` object detailing the information of the overall search.

### Order of `context` and `match` guaranteed?

I'm just thinking from the point of view from a consumer how easy it would be to re-match the contexts up against the matches to show them in a UI in the correct order.

If the order is guaranteed, it would be easier, for example, a match on line 12 (with 2 context matches enabled)

* You would first get JSON lines for `context` lines 10 and 11
* Followed by the `match` on line 12
* Two more `context` for lines 13 and 14.


---

_Comment by @BurntSushi on 2018-07-30 22:33_

@garygreen 

> I would agree that it's probably not worth sending these for files with no matching results. Just skip sending both, that would also prevent sending JSON lines for all files that don't have any matches (those files the consumer probably isn't interested in at). If consumers did want it, then it could probably be added as an option to always send them, even when no matches with a caveat that it may impact performance.

Yup, this is done. `begin`/`end` won't be shown by default in the non-match case, but I have an option now that enables this.

> progress idea

Definitely going to punt on this until a specific use case for it can be evaluated. I'm trying to focus on shipping the (nearly) minimal subset of useful things here that will let folks migrate off of parsing ANSI escapes without losing or regressing on any features.

> At the moment there is currently no way of knowing when the search is totally finished, so you may be left in limbo and can't for instance have a UI that says "Search complete"? You could possibly listen for when the ripgrep process exits, but maybe it would be easier to be explicit and have a finish type that would let you know when the search has finished in it's entirety. It would contain a stats object detailing the information of the overall search.

You generally know it's done because the process exits and the stdout pipe closes, at which point the consumer reads EOF. But yes, the wire format presented thus far is the library-ized format for a single search. ripgrep will add at least one message, probably called `summary`, that is emitted once the search is complete. And yes, it will have a `stats` object representing the accumulation of statistics.

> Order of context and match guaranteed?

Oh yes, absolutely. Matches are printed in the order in which they appear. To do otherwise would be crazytown. :-)

---

_Comment by @roblourens on 2018-07-30 23:59_

> If we make it so begin/end are only shown when there's a match, then begin itself probably has very little downside.

Sounds good, I think `begin` could get very noisy when searching in large directories.

And I agree with what you say about large lines - in vscode's case at least, it's the same as what we're doing already, and getting large lines from ripgrep definitely isn't the bottleneck.

---

_Comment by @jessegrosjean on 2018-07-31 16:13_

>> On the other hand, if you have several matches in a long line, you won't be duplicating the full line text, which isn't true in vscode's model.

> Ah this is interesting! My current thinking is to just let consumers such as yourself sort this out, but if the volume of data turns out to be a performance bottleneck then we can certain add some backwards compatible knobs for this going forward.

I'm just starting with integrating this project into my own editor, and this is the first problem I ran into. Maybe just change the behavior of `--max-columns` flag? Instead of reporting that the line was over max column, just return the portion of the line around the match that fits into max columns?

Great project, thanks for making it. I was 2 days in to reinventing the wheel badly when I can across it.

---

_Comment by @BurntSushi on 2018-07-31 17:02_

@jessegrosjean Could you elaborate more on the problem? I'm having trouble understanding it. You're saying that you ran into performance problems? Is it possible to reproduce them outside the editor?

What you're asking for really sounds like a new "preview" type feature, which is similar to, but different from, what `--max-columns` achieves. `--max-columns` is really about limiting the length of lines reported, where as a hypothetical preview feature is quite a bit more complex than that and will need to account for windowing every match, of which there may be multiple.

It might be smart to create a new issue for this. I doubt it will wind up in the initial support for JSON.

---

_Comment by @jessegrosjean on 2018-07-31 20:13_

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

_Comment by @BurntSushi on 2018-07-31 22:55_

@jessegrosjean Like I said, I think you should open a new ticket, since we're getting way off the topic of JSON. I did this for you in #999 and gave you a response. TL;DR - I'm not convinced any specific action should be taken on the JSON API at this time. Let's tackle performance problems---if they exist---as they arise.

---

_Comment by @BurntSushi on 2018-08-07 22:46_

@roblourens The [`ag/libripgrep-freeze-2`](https://github.com/BurntSushi/ripgrep/tree/ag/libripgrep-freeze-2) branch has JSON Lines support. You can enable JSON with the `--json` flag.

---

_Comment by @roblourens on 2018-08-07 23:30_

The output looks great, I am very excited for this.

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---
