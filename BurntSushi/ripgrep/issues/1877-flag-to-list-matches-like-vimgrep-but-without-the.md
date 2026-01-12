```yaml
number: 1877
title: Flag to list matches like --vimgrep, but without the file content
type: issue
state: closed
author: ad-si
labels: []
assignees: []
created_at: 2021-05-28T14:29:44Z
updated_at: 2021-05-28T19:19:14Z
url: https://github.com/BurntSushi/ripgrep/issues/1877
synced_at: 2026-01-12T16:13:24Z
```

# Flag to list matches like --vimgrep, but without the file content

---

_@ad-si_

This is how `--vimgrep` works:

```shell
$ rg --vimgrep 'version'
package.json:3:4:  "version": "0.1.0",
```

What I want as an extra flag (could be named differently):

```shell
$ rg --location 'version'
package.json:3:4
```

This doesn't work (maybe it should?):

```shell
$ rg --files-with-matches --line-number --column 'version'
package.json
```


---

_Comment by @BurntSushi on 2021-05-28 15:25_

> Flag to list matches like --vimgrep, but without the file content

Why? Please provide a use case.

If this is performance related, please provide a benchmark.

---

_Comment by @ad-si on 2021-05-28 16:46_

To open all occurrences at the correct position in Sublime:

```shell
rg --location 'version' | xargs subl
```

---

_Comment by @BurntSushi on 2021-05-28 17:14_

@ad-si Oh, that should be easy enough without adding a new feature then:

```
$ rg 'version' --vimgrep | cut -d':' -f1-3 | xargs subl
```

---

_Closed by @BurntSushi on 2021-05-28 17:14_

---

_Comment by @ad-si on 2021-05-28 17:40_

Why does rg then have any features at all? I can hack anything together with cut/ack/sed 🙄.

---

_Comment by @ad-si on 2021-05-28 17:41_

Also leaves open the question if this shouldn't actually already do it `rg --files-with-matches --line-number --column`


---

_Comment by @BurntSushi on 2021-05-28 17:56_

> Why does rg then have any features at all? I can hack anything together with cut/ack/sed roll_eyes.

This sort of attitude isn't appreciated. If you continue making unconstructive comments on any of my issue trackers, then I'm going to block you. My issue trackers are not your dumping ground to whine sarcastically.

To respond more directly: while ripgrep subsumes some things that are traditionally done with longer shell pipelines, it does not subsume all of them. In this case, I don't think it's worth adding another flag (or changing how `--files-with-matches` works) just to get rid of a `cut` command.

> Also leaves open the question if this shouldn't actually already do it `rg --files-with-matches --line-number --column`

`--files-with-matches` is an aggregate output format that prints matching file paths exactly once. It doesn't make sense to me personally change that to "write the path for every match but without the match" when certain flags are given.

---

_Comment by @ad-si on 2021-05-28 19:18_

I think a flag which prints the line and column next to filename is quite important, since many editors support opening them nowadays. This would also be helpful across many rg subcommands, since it could be added whenever a filename is printed. Actually I'd rather remove the `--vimgrep` flag (which is very specific) and replace it with something like `rg --location --oneline`, where `--location` ads the line:column and `--oneline` makes it print the match right next to the filename.

---

_Comment by @ad-si on 2021-05-28 19:19_

So what I was looking for would then be `rg --files-with-matches --location 'version'`

---
