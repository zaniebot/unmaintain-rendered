```yaml
number: 2446
title: Support searching structured data
type: issue
state: closed
author: orf
labels:
  - wontfix
assignees: []
created_at: 2023-03-09T16:30:32Z
updated_at: 2023-03-09T22:46:37Z
url: https://github.com/BurntSushi/ripgrep/issues/2446
synced_at: 2026-01-12T16:13:24Z
```

# Support searching structured data

---

_@orf_

Ripgrep is a very versatile tool, but there is one limitation that I frequently run into that I would love to be able to use Ripgrep with.

Imagine you have a tool that outputs structured content in JSON:

```jsonl
{"name": "foo.py", "text": "foobar"}
{"name": "baz.py", "text": "baz\nsome new line here"}
```

I would love to be able to feed this into `rg` and have it match only on the `text`  but treat it as if it had been read from a file called `foo.py`. For example

```
$ stream_input | rg "foo" --key="name" --content="text"
./foo.py
1:foobar
```

The use of JSON isn't the only option, using something like tar (as discussed in #1112) would work as well - it's a structured stream of "file name" and "content":

```
$ stream_input  | convert_into_tar_stream | rg "foo"
./foo.py
1: foobar
```

The reason this would be useful would be to combine it with `--json` for output:

```
$ stream_input | rg "foo" --structured --json | jq "filter(.type = 'match') | .data.path.text"
"foo.py"
```

The `name` might not even be a full key, it could be any form of identifier. Imagine you where streaming a big set of comments from a database table and you wanted to use `rg` to match against the content but output the comment primary key if there is a match:

```
$ stream_input | rg "foo" --structured --json | jq "filter(.type = 'match') | .data.path.text"
"comment_primary_key=1234"
```

Right now ripgrep is able to associate a key with input content if it comes from a file (or something within a compressed file), but if you are streaming data from another process via pipes it becomes impossible to keep some form of key to match to the output.

You could of course spawn lots of `rg` processes with `parallel`:

```
$ stream_input | parallel 'rg "foo" --structured && somehow echo the input ID'
```

But this seems inefficient especially if the inputs are of varying length, where for some the startup time of `rg` may dominate the execution time.

---

_Comment by @BurntSushi on 2023-03-09 16:35_

Thanks for the thoughtful request, but this one is very easy to say no to. It is well beyond the scope of ripgrep, as you might imagine, teaching ripgrep about the hundreds (thousands?) of different structured formats in the world is quite obviously a non-starter.

ripgrep does push back against the notion of composability via shell pipelines, but this is a bridge way too far. If you want ripgrep to search only parts of a structured representation, then _you_ should convert the structured representation into something that ripgrep understands. (For example, you might use `gron` for JSON.) This isn't going to get you the nicest UX because the output is invariably going to be in terms of your transformation instead of the original structured format. But improving that UX is just really well beyond the scope of ripgrep.

---

_Closed by @BurntSushi on 2023-03-09 16:35_

---

_Label `wontfix` added by @BurntSushi on 2023-03-09 16:35_

---

_Comment by @orf on 2023-03-09 16:48_

I totally agree that it's not a good idea to teach ripgrep about lots of structured formats but it may be OK to teach it about a single one? In this case JSON came to mind, but this would also be fixed by only teaching ripgrep about the `tar` format which may be on the cards as #1112 is still open?

That would enable searching very large `git archive`'s without needing to unpack them, which is actually exactly the use case I have:

```
$ curl -Ls https://github.com/BurntSushi/ripgrep/archive/refs/heads/master.tar.gz | tar -xOzf - | rg "foobar"
foobar
1:foobar
    const ROOT: &'static str = "/home/foobar/rust/rg";
            "combo:foobar:html,rust",
```


---

_Comment by @BurntSushi on 2023-03-09 16:56_

#1112 is, to me, about teaching ripgrep how to a very specific thing that is a high value add, but to otherwise do it in a way that is _transparent_. That is, it shouldn't (ideally) require any new flags. It should just be the case that passing `-z/--search-zip` will make ripgrep automatically do it. The challenge in that sort of task is not really about reading the `tar` format itself, but layering it into ripgrep's filtering rules in a way that makes sense. For example, a `tar` is often conceptually a directory tree, so it likely makes sense to actually treat it like one.

The feature you're asking for here looks like it's not just about making ripgrep transparently aware of a structured format, but actively giving it options to interact with.

I really do think that once you get to this point, it's time to start building your own bespoke tool. ripgrep is split up into libraries, so this is actually a conceivable (although not trivial) amount of work. You don't need to specialize in how to make the grep searching part of it fast. That's done. You "just" need to hook everything up. And indeed, that is likely difficult to do because there is no real high level docs for ripgrep's suite of `grep` libraries. But, if you were interested in that path, I'd be happy to help by answering questions via Discussions.

---

_Comment by @orf on 2023-03-09 17:12_

I see your point and it does make sense (even if that is inconvenient for me!). Ripgrep is a fantastic tool and I'll try out creating something like `ripgrep-json` as an experiment using the library interface.

> The feature you're asking for here looks like it's not just about making ripgrep transparently aware of a structured format, but actively giving it options to interact with.

That's not the total idea. The core of it is for ripgrep to define a way to provide a "key" in-line _somehow_ with stdin input and to have that outputted with matches instead of just `-`. It's a specialised use case but it has come up for me a surprising number of times, and I don't imagine I'm the only one.

Ripgrep doesn't need to provide options to interact with a structured format, it could just define something really simple as a best-effort. Something as simple as

```
[KEY] [base64 encoded contents]\n
```

Or something like 

```
[KEY] [SIZE] [contents]
```

Where size is the length of the content to read from stdin.

I wasn't imagining something super flexible that allowed ripgrep to search arbitrary JSON, CSV or other formats via stdin, but just _some_ way to nail the "i have a stream that represents many files and I'd like to stream them into a ripgrep process" use case without having to build out custom tooling. 

Then you can just say "If you can massage your input into that format then great, otherwise go flex your rust muscles and do whatever you want" without worrying about an explosion of "please support format XYZ" requests.

---

_Comment by @BurntSushi on 2023-03-09 17:55_

I think it might help if you could talk about a real use case here. I think I am somewhat confused. The issue opened with running ripgrep with a bunch of new flags in a way that made it _look_ like ripgrep needed to understand JSON. I now see that you're saying that's just an implementation path... but an implementation path for what? I see that you're talking about "getting a key" and associating things, but I don't really know how to connect the dots here.

Also, have you looked at the `--pre` flag? Does it help your use case?

---

_Comment by @orf on 2023-03-09 20:15_

Apologies for the confusion - I was definitely mixing the potential implementation with the issue.

### This specific use case

The concrete use case I have right now is running `ripgrep` over a very large number files in a large collection of git repositories. It's unfeasible to unpack each file to disk, and the use case here is grepping over them as fast as possible.

Here's some bechmarks running `git grep` and `rg` over 2,644,401 files (563,469 unique blobs) to find python functions that have no arguments and are indented by 4 spaces:

```
❯ time git grep --threads=1 -E "^    def [a-zA-Z]+\(self\):\$" b058acc0f3a51ea22d3b059120924c772c1b1bf4 | wc -l
 1263124

________________________________________________________
Executed in  170.51 secs    fish           external
   usr time  165.51 secs    0.15 millis  165.51 secs
   sys time    5.10 secs    3.43 millis    5.09 secs
```

Compared to `rg`:

```
❯ time cat blobs | git cat-file --batch-command --unordered --buffer | rg "^    def [a-zA-Z]+\(self\):\$" | wc -l
 1263171

________________________________________________________
Executed in   60.04 secs    fish           external
   usr time   71.30 secs    0.10 millis   71.30 secs
   sys time   12.06 secs    1.14 millis   12.06 secs
```

Reading objects directly from the git object database (the cat-file portion) and piping the contents into `rg` is much, much faster than `git grep`. However we lose the ability to know _which_ specific objects matched because it's all one stream.

In this specific case I would like `rg` to be able to output the git object ID that matched, then "do stuff" with it later.


### More generally

I use ripgrep a lot, it's muscle-memory bound to whenever I need to "search stuff". It is fast, simple and just works. However it falls down when I need to search streams of content.

For example, imagine I have a file containing a number of tweets:

```json
{"user": "tom", "id": "1", "tweet": "hello\nworld!"}
{"user": "james", "id": "2", "tweet": "foobar"}
```

I'd like to find all tweets that match the multiline expression `^he.*$\nworld$`, and I'd like to know what specific tweet ID matched.

Ok, so right now I could create 1 file for each tweet ID and run `rg` over those files. The output would contain the file name, and I could say "file 1 matched" which means "tweet ID 1 matched". But this isn't great and requires a single file per tweet. 

Running `rg` over the whole file isn't useful for two reasons:

1. multiline matching won't work
2. it won't really tell me which ID matches. It would output the whole line, which I would need to re-parse to extract the tweet ID. The line could be very big and this could add overhead.

What I would like to be able to do is quickly and simply convert this file into a format that `rg` accepts, pipe it into stdin and get back matches with a _given key_. Keeping it simple, I could pipe in:

```
1 "hello\nworld!"
2 "foobar"
```

and `rg --json` would output:

```
{
  "type": "match",
  "data": {
    "path": {
      "text": "1"
    },
    "lines": {
      "text": "hello\nworld"
    },
    "line_number": 1,
    "absolute_offset": 0,
    "submatches": [
      {
        "match": {
          "text": "hello\nworld"
        },
        "start": 0,
        "end": 11
      }
    ]
  }
}

```

Instead of the `path` being a file path, it's the first column of the input (the "key"). In the case above it means `tweet ID 1 matched`.

I can then quickly compose complex pipelines to search over streams:

```
cat tweets | rg -U '^he.*$\nworld$' --some-flag | jq 'select(.type = "match") | .data.path.text' | parallel -I@ "delete-tweet --tweet-id=@"
```

Which would find all tweets that match the expression, extract the tweet ID from the output and then call `delete-tweet` on each ID in parallel. This is a contrived example but the general pattern comes up quite often.

I hope this clarifies my use case for this feature, which I really think would be a useful addition to ripgrep and could be done in a way that doesn't add a huge maintenance burden.

---

_Comment by @BurntSushi on 2023-03-09 20:20_

Before I dig in, have you looked at `--pre`?

---

_Comment by @orf on 2023-03-09 20:31_

I've used `--pre` before but I'm not sure how this would help here. Please tell me if I'm wrong, but the pre-processor is called once per input file (or once for stdin), and the raw output is then searched as usual.

This is a great feature, but it doesn't allow me to tell ripgrep that only a certain part of the input should be searched.

If we think about how `--pre` could be a solution it might be clearer: we could invoke the pre-processor once per input line which would let me write  a script like:

```sh
#!/bin/sh

jq '.tweet' -
```

This would extract the tweet text for each line, which `rg` would then search over and ignore any other content. This would solve part of the issue but we have lost other parts of the input - `rg` would only output the matching line number and not anything really useful.

---

_Comment by @orf on 2023-03-09 22:39_

Perhaps some code might make this clearer. I've made this as an example, and to try using the `grep` library: https://github.com/orf/ripgrep-stream

If the input file contains this:

```
id_one "hell! no!\nworld"
id_two "hello\nworld"
id_three "no hello world\nhere"
```

Then we can run multi-line matching over each input line, and the output "paths" are part of the user-supplied input: 

```
$ cat example_json.txt | cargo run -- '^he.*$\nworld$'
id_one:hell! no!
id_one:world
id_two:hello
id_two:world
```

I'm not proposing this input format specifically - it's certainly not the most efficient format, but it's the quickest POC I could come up with.

If the rationale behind this request still doesn't satisfy you then that's OK, I can have a go at building this into it's own tool. But I still think it's useful to have _something_ like this available in ripgrep itself.

---
