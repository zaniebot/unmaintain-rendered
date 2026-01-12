```yaml
number: 1981
title: potential missed optimization in extracting a capture group
type: issue
state: open
author: vlmutolo
labels: []
assignees: []
created_at: 2021-08-30T01:57:48Z
updated_at: 2021-08-30T02:21:57Z
url: https://github.com/BurntSushi/ripgrep/issues/1981
synced_at: 2026-01-12T16:13:24Z
```

# potential missed optimization in extracting a capture group

---

_@vlmutolo_

#### What version of ripgrep are you using?
```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?
Arch Linux's `pacman`. 

#### What operating system are you using ripgrep on?
Arch Linux

#### The Missed Optimization (?)

It seems like the following two commands should perform the same task, and that the first should be either faster than or the same speed as the second. 

```bash
… | rg -or '$1' '"value":"([^"]+)","type"'
```

```bash
… | rg -o '"value":"[^"]+","type"' | rg -r '' '(?:^"value":"|","type"$)'
```

Instead, the second, pipelined version is four times slower on my machine.

#### What are the steps to reproduce the behavior?

I don't think I can post the data directly here, but I can link to it. It's an open data set. The following command should download it. (The `-L` is necessary for `curl` to follow the redirects.)

Fair warning, the compressed file is something like 11GB. Also, I believe the decompressed file is over 100GB, so you probably don't want to extract it.

```bash
curl -L https://opendata.rapid7.com/sonar.rdns_v2/2021-07-28-1627430820-rdns.json.gz -o 2021-07-28-1627430820-rdns.json.gz
```

If you'd rather not download a big file of domain names, the data generally take the form of the following.

```json
{"timestamp":"<unix timestamp>","name":"<IPv4 Address>","value":"<oddly long domain name>","type":"<three-letter code>"}
```

Then, I time the following two commands.

```bash
gzip -dc ./2021-07-28-1627430820-rdns.json.gz | head -n 1000000 | rg -or '$1' '"value":"([^"]+)","type"' | wc -l
```

```bash
gzip -dc ./2021-07-28-1627430820-rdns.json.gz | head -n 1000000 | rg -o '"value":"[^"]+","type"' | rg -r '' '(?:^"value":"|","type"$)'
```

The first version takes 2.7s, and the second takes 0.67s.

---

_Comment by @BurntSushi on 2021-08-30 02:05_

Have you rerun the commands to make sure the difference is reproducible?

Without looking into it yet, the slower command uses capturing groups. The faster one doesn't. Dealing with capturing groups takes more work.

---

_Comment by @vlmutolo on 2021-08-30 02:10_

I've re-run the commands several times and the behavior is consistent.

I did think the issue might be the capturing group, but I'm not super familiar with regex engines. I'm not sure if the two commands I posted should be considered "the same" or not, but I wanted to log this here just in case the expected behavior was in fact that they take the same time, or in case anyone else was confused by this.

---

_Comment by @vlmutolo on 2021-08-30 02:21_

I tried the command

```bash
gzip -dc ./2021-07-28-1627430820-rdns.json.gz | head -n 1000000 | rg -o '"value":"[^"]+","type"' | rg -r '' '(^"value":"|","type"$)'
```

and it takes 3.5s, compared to 0.67s without the capture group. So you were right about what's causing the slow down.

---
