```yaml
number: 1115
title: String + int
type: issue
state: closed
author: yoni386
labels: []
assignees: []
created_at: 2017-11-25T21:35:32Z
updated_at: 2018-08-02T03:30:15Z
url: https://github.com/clap-rs/clap/issues/1115
synced_at: 2026-01-12T16:14:10Z
```

# String + int

---

_@yoni386_

Hello,

How to work with “combo type” (unit metric) values such as; 100KB, 512GB and etc? 


---

_Comment by @kbknapp on 2017-11-26 13:19_

I need some more context about what you're trying to do and how you'd like it work. Can you give me an example?

---

_Comment by @yoni386 on 2017-11-27 21:21_

Thank you for the library and for this mail.

I would like to set flags similar to FIO application which allows to get
int and string for example 1M or 1MB 1G or 1g. Maybe this similar to
“subcommand” match library implementation.

How do you match string? With multiple if?

I find it difficult to use match outside main function. How to get value in
a function? Should I pass matches as argument or use macro get value inside
main and pass it?

On Sun, 26 Nov 2017 at 15:19 Kevin K. <notifications@github.com> wrote:

> I need some more context about what you're trying to do and how you'd like
> it work. Can you give me an example?
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/kbknapp/clap-rs/issues/1115#issuecomment-347008161>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/ALrs2s9aDHJLIDckeofkxvG0tmFxC-7Nks5s6WVVgaJpZM4QqkP9>
> .
>
-- 

Best Regards,

Yoni Shperling


---

_Comment by @kbknapp on 2018-01-09 19:19_

I'm sorry, I still don't know exactly what you're trying to do. I understand the 1MB 5GB, etc. but I don't see what you're trying to do with CLIs.

---

_Closed by @kbknapp on 2018-01-09 19:19_

---
