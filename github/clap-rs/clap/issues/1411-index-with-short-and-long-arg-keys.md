---
number: 1411
title: index with short and long arg keys
type: issue
state: closed
author: rerime
labels: []
assignees: []
created_at: 2019-02-11T16:38:45Z
updated_at: 2020-02-01T19:17:44Z
url: https://github.com/clap-rs/clap/issues/1411
synced_at: 2026-01-07T13:12:19-06:00
---

# index with short and long arg keys

---

_Issue opened by @rerime on 2019-02-11 16:38_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

* rustc 1.34.0-nightly (8ae730a44 2019-02-04)

### Affected Version of clap

* 2.32

### Feature Request Summary
Is it possible use index with short and long arg keys same time?

```yml
name: ping
version: "1.0"
args:
    - host:
        index: 1
        short: h
        long: host
        value_name: HOST
        help: Set IP address or FQDN
        takes_value: true
```

### Expected same behavior
```sh
ping 8.8.8.8
ping -h 8.8.8.8
```


---

_Comment by @Dylan-DPC-zz on 2019-04-05 21:45_

hi @rerime. Do you still need help with this? If not we can close it. Thanks :)

---

_Comment by @rerime on 2019-04-06 07:54_

Yep)

---

_Comment by @CreepySkeleton on 2020-02-01 15:10_

I'm pretty sure the code in your comment should work, feel free to reopen if it doesn't

---

_Closed by @CreepySkeleton on 2020-02-01 15:10_

---

_Label `T: RFC / question` added by @pksunkara on 2020-02-01 19:17_

---
