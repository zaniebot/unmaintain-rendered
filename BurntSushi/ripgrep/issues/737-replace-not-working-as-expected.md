```yaml
number: 737
title: "--replace not working as expected"
type: issue
state: closed
author: leeoniya
labels: []
assignees: []
created_at: 2018-01-08T17:59:55Z
updated_at: 2018-01-08T18:11:26Z
url: https://github.com/BurntSushi/ripgrep/issues/737
synced_at: 2026-01-12T16:13:22Z
```

# --replace not working as expected

---

_@leeoniya_

Hi,

I'm trying to process some webserver access logs with the following format:

```
8.8.8.8 - - [07/Jan/2018:06:25:10 -0600] "GET /some/url HTTP/1.1" 200 25861 "-" "Mozilla/5.0 (Linux; Android 6.0.1; Nexus 5X Build/MMB29P) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.96 Mobile Safari/537.36 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
```

what i want as output is:

```
8.8.8.8, "Mozilla/5.0 (Linux; Android 6.0.1; Nexus 5X Build/MMB29P) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.96 Mobile Safari/537.36 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
```

this does not seem to be working:

```
rg -N --no-filename "^([0-9\.]+).+(\"Mozilla.*)$" --replace '$1, $2' ./logs
```

what i get as outputs is:

```
'8.8.8.8,
```

(Windows 10)

any help would be appreciated.

---

_Comment by @okdana on 2018-01-08 18:02_

Works for me on UNIX. Are you using `cmd` or PowerShell? AFAIK, `cmd` has some bizarre escaping rules, like it wants `^` instead of `\`. Not sure about PS. Try single-quotes maybe?

---

_Comment by @leeoniya on 2018-01-08 18:09_

ok, thx. it worked in powershell:

```
.\rg -o -N --no-filename "^([0-9\.]+).+(\"Mozilla.*)$" --replace '$1, $2' ./logs
```

---

_Closed by @leeoniya on 2018-01-08 18:09_

---
