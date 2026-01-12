```yaml
number: 320
title: Output only matched patterns (data extraction)
type: issue
state: closed
author: hrxn
labels: []
assignees: []
created_at: 2017-01-14T06:35:48Z
updated_at: 2022-09-30T10:40:38Z
url: https://github.com/BurntSushi/ripgrep/issues/320
synced_at: 2026-01-12T16:13:21Z
```

# Output only matched patterns (data extraction)

---

_@hrxn_

Hi,

just been trying ripgrep for the first time, but I couldn't find an easy and straightforward way to just output the matching pattern.

For example, trying to search in a file like this:
`rg "(\{.*\}\})" example.txt`

The regex matching works as intended, correct result is highlighted in bold red text.

But when I tried to redirect ("pipe") the output to another program I realized that the actual output is still the unchanged input.

Am I missing something here? I couldn't find an option just for this output, seemed pretty straightforward to me, but neither `rg --help` or the front page here seem to mention something..

---

_Comment by @BurntSushi on 2017-01-14 12:50_

It's a missing feature: #34. It's on my list of things to do soon, but it's part of a larger refactoring effort, so it might take a little longer.

There is a work around today though. You can use the `-r/--replace` flag:

    rg "(\{.*\}\})" example.txt -r '$1'

I think that should do it. (On mobile so i haven't checked.)

---

_Comment by @hrxn on 2017-01-15 02:50_

`rg "(\{.*\}\})" example.txt -r '$1'`

Not really. Replacing works as intended apparently, in this case:
* match and take capture group 1
* replace with reference 1, i.e. capture group 1 again

But the thing is, the actual output is still unchanged, because the program always returns the whole line where the match was found.

To help illustrate things better, I made an example file: https://fnpaste.com/YZvd

But I found a way around this: Changing the rx syntax to match the whole line, and then replace the whole line with capture group 1.

For example:
`rg ".*40 (\{.*\}\}).*" example.txt -r "$1"`

This gives me the desired part. But there is a big caveat: I use the `40 ` as an anchor to make the group match the correct part, but this is specific for this example only, I don't actually know the value.

I tried `".*(\{.*\}\}).*"` in my text editor and it actually works there, whereas the same pattern only matches the last `{...}}` part of the target inside the line with rg.

I'm not really familiar with the intricacies of regular expressions in Rust, so this needs a bit of tweaking, I guess, although it should definitely be possible.

---

_Comment by @BurntSushi on 2017-01-15 21:29_

You're right. I forgot to add that you need to make your regex match the entire line in order for `-r/--replace` to work like `-o/--only-matching`.

I think this gives the desired output for you though:

```
$ rg '^.*?(\{.*\}\}).*?$' example.txt -r '$1'
```

The trick is to make the `.*` lazy by using `.*?`. When it's lazy, it will try to match the least possible. Otherwise, it tries to match the most possible.

---

_Comment by @hrxn on 2017-01-15 23:52_

Thanks!

That did the trick just fine, greedy vs. lazy, obviously..

I'll close this issue for now.
We have a good workaround, and you already said that this is on your list of things to do, so I guess all is fine..

---

_Closed by @hrxn on 2017-01-15 23:52_

---

_Comment by @shmerl on 2022-09-30 06:16_

Is there some easy syntax to output only certain but several capture group indexes?

Let's say with pcregrep there is a way to do this:

```
echo 'foo bar baz qux' | pcregrep -o1 -o2 '(bar )baz (qux)'
```
Which will produce:
```
bar qux
```

I suppose that --replace method with capturing the whole line would help, but something easier would be useful.


---

_Comment by @BurntSushi on 2022-09-30 10:39_

Sure:

```
echo 'foo bar baz qux' | rg '(bar )baz (qux)' -or '$1$2'
```

---

_Comment by @BurntSushi on 2022-09-30 10:40_

If you have more questions, please use Discussions instead of burying them in old issues. :)

---
