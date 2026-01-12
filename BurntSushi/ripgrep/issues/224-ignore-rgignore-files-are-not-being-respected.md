```yaml
number: 224
title: .ignore / .rgignore files are not being respected
type: issue
state: closed
author: ye
labels: []
assignees: []
created_at: 2016-11-07T19:03:01Z
updated_at: 2016-11-07T21:48:20Z
url: https://github.com/BurntSushi/ripgrep/issues/224
synced_at: 2026-01-12T18:23:11Z
```

# .ignore / .rgignore files are not being respected

---

_@ye_

Not sure if this is a misunderstanding on my part, but I can't seem to get `rg` to respect `.ignore` or `.rgignore` files.

In this example, I can't get `rg` to search the keyword string in the `.hidden-file` with ignore files. But `--hidden` flag does seem to work correctly as expected. Did I miss something with ignore files? 

```bash
$ cat .hidden-file
docker pull python:3.4-wheezy
$ echo \!.hidden-file > .ignore
$ cat .ignore
!.hidden-file
$ rg python:3.4-wheezy
$
$ rg --debug python:3.4-wheezy
DEBUG:grep::search: regex ast:
Concat(
    [
        Literal {
            chars: [
                'p',
                'y',
                't',
                'h',
                'o',
                'n',
                ':',
                '3'
            ],
            casei: false
        },
        AnyCharNoNL,
        Literal {
            chars: [
                '4',
                '-',
                'w',
                'h',
                'e',
                'e',
                'z',
                'y'
            ],
            casei: false
        }
    ]
)
...
DEBUG:rg::ignore: ./.hidden-file ignored because it is hidden
...
$ mv .ignore .rgignore
$
$ rg python:3.4-wheezy
$ 
$ rg --hidden python:3.4-wheezy
.hidden-file
1:docker pull python:3.4-wheezy
```

This is opened as per request by @BurntSushi in #90 

---

_Comment by @BurntSushi on 2016-11-07 20:29_

What version of ripgrep are you using?

On Nov 7, 2016 2:03 PM, "Ye Wang" notifications@github.com wrote:

> Not sure if this is a misunderstanding on my part, but I can't seem to get
> rg to respect .ignore or .rgignore files.
> 
> In this example, I can't get rg to search the keyword string in the
> .hidden-file with ignore files. But --hidden flag does seem to work
> correctly as expected. Did I miss something with ignore files?
> 
> $ echo !.hidden-file > .ignore
> $ cat .ignore!.hidden-file
> $ rg python:3.4-wheezy
> $
> $ rg --debug python:3.4-wheezy
> DEBUG:grep::search: regex ast:
> Concat(
>     [
>         Literal {
>             chars: [
>                 'p',
>                 'y',
>                 't',
>                 'h',
>                 'o',
>                 'n',
>                 ':',
>                 '3'
>             ],
>             casei: false
>         },
>         AnyCharNoNL,
>         Literal {
>             chars: [
>                 '4',
>                 '-',
>                 'w',
>                 'h',
>                 'e',
>                 'e',
>                 'z',
>                 'y'
>             ],
>             casei: false
>         }
>     ]
> )
> ...
> DEBUG:rg::ignore: ./.hidden-file ignored because it is hidden
> ...
> $ mv .ignore .rgignore
> $
> $ rg python:3.4-wheezy
> $
> $ rg --hidden python:3.4-wheezy
> .hidden-file
> 1:docker pull python:3.4-wheezy
> 
> This is opened as per request by @BurntSushi
> https://github.com/BurntSushi in #90
> https://github.com/BurntSushi/ripgrep/issues/90
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/224, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34m9OJiPLFEDiBD7bjZp9i4A6i2EXks5q73XlgaJpZM4KrlkD
> .


---

_Comment by @ye on 2016-11-07 21:37_

@BurntSushi right on! I was having `0.1.15` and after `brew upgrade ripgrep` to `0.2.8`, it worked like a charm! 

Sorry about it, I had a feeling this is human error from the start ...


---

_Closed by @ye on 2016-11-07 21:37_

---

_Comment by @BurntSushi on 2016-11-07 21:48_

No problem! I love bugs that fix themselves. :-)

On Nov 7, 2016 4:37 PM, "Ye Wang" notifications@github.com wrote:

> @BurntSushi https://github.com/BurntSushi right on! I was having 0.1.15
> and after brew upgrade ripgrep to 0.2.8, it worked like a charm!
> 
> Sorry about it, I had a feeling this is human error from the start ...
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/224#issuecomment-258970749,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34nADnhNt1Y_3P4R6BiIF2eTpi_Dnks5q75opgaJpZM4KrlkD
> .


---
