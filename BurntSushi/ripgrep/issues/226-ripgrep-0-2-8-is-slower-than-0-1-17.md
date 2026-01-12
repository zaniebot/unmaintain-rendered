```yaml
number: 226
title: ripgrep 0.2.8 is slower than 0.1.17
type: issue
state: closed
author: danbst
labels:
  - bug
assignees: []
created_at: 2016-11-08T19:09:24Z
updated_at: 2016-11-09T22:19:44Z
url: https://github.com/BurntSushi/ripgrep/issues/226
synced_at: 2026-01-12T18:23:11Z
```

# ripgrep 0.2.8 is slower than 0.1.17

---

_@danbst_

I've built two version of `ripgrep` with Nix and tried to process several large log-files:

```
cat > /tmp/part1 <<EOF
Sep  1 00:00:54 flare poe-wp    actions.log     5       16      id=329968       a=us    sid=7406d&L=6756&nc=354 , US_99fc0
Sep  1 00:00:54 flare poe-wp    actions.log     3       16      id=159925       a=us    sid=4d9f4&L=6658&nc=47  , US_42019
Sep  1 00:00:54 flare poe-wp    actions.log     5       16      id=256192       a=us    sid=97c33&L=9761&nc=825 , US_945fb
Sep  1 00:00:54 flare poe-wp    actions.log     4       16      id=318102       a=us    L=3810&nc=15
Sep  1 00:00:54 flare poe-wp    actions.log     2       16      id=344030       a=us    sid=9db0f&L=4536&nc=1171
Sep  1 00:00:55 flare poe-wp    actions.log     5       1015    id=258449       a=agi   aid=656&L=1981&nc=178
Sep  1 00:00:55 flare poe-wp    actions.log     7       1       id=96809        a=co    bid=416145&dr=169834180,165794832,117081222,685&L=3191&nc=126
Sep  1 00:00:55 flare poe-wp    actions.log     16      25      id=217040       a=us    sid=9ecca&L=2709&nc=172  RF c37443 Resources: gold:5 595  wood: 295   2001:3 , US_5fdfc
Sep  1 00:00:55 flare poe-wp    actions.log     2       95      id=359662       a=us    L=3148&nc=389
Sep  1 00:00:55 flare poe-wp    actions.log     6       1       id=323959       a=co    bid=12ecdd3&dr=159447,1235597,1867293,587&L=0625&nc=6576
EOF

cat /tmp/part1 | perl -0777pe '$_=$_ x 600000' > /tmp/largefile
for n in 1 2 3 4 5; do cp /tmp/largefile /tmp/largefile$n; done

for tool in grep \
            /nix/store/0ckbrgryjgmbzh8132w540nrkxih55kq-ripgrep-0.1.17/bin/rg \
            /nix/store/38ljsyz7y5a9w2bw738jcmymy2zla1sq-ripgrep-0.2.8/bin/rg \
           ; do
    time $tool id=25 /tmp/largefile*  | wc -l
done
```

Here is a result:
```
7200000

real    0m5.020s
user    0m3.800s
sys     0m1.190s
7200000

real    0m2.209s
user    0m4.020s
sys     0m1.270s
7200000

real    0m4.044s
user    0m3.452s
sys     0m0.557s
```

Clearly, the new ripgrep is slower then 0.1.17

---

_Comment by @BurntSushi on 2016-11-09 21:52_

Thanks so much for the easily reproducible bug report. ^_^

This is interesting though. Looks like this happened between `0.2.6` and `0.2.8`, which means it's probably related to the changes I made to the parallel iterator. Some more data points (note that single threaded performance has not regressed):

```
[andrew@Cheetah ripgrep-226] time rg-0.2.6 id=25 ./largefile* | wc -l
7200000

real    0m1.151s
user    0m3.297s
sys     0m2.203s
[andrew@Cheetah ripgrep-226] time rg id=25 ./largefile* | wc -l
7200000

real    0m3.183s
user    0m2.507s
sys     0m1.000s
[andrew@Cheetah ripgrep-226] time rg-0.2.6 -j1 id=25 ./largefile* | wc -l
7200000

real    0m2.822s
user    0m2.520s
sys     0m0.873s
[andrew@Cheetah ripgrep-226] time rg -j1 id=25 ./largefile* | wc -l
7200000

real    0m2.846s
user    0m2.563s
sys     0m0.907s
```


---

_Label `bug` added by @BurntSushi on 2016-11-09 21:52_

---

_Comment by @BurntSushi on 2016-11-09 22:01_

Welp. I'm a dufus. While the iterator is parallel, it doesn't process _explicit_ file paths in parallel.


---

_Closed by @BurntSushi on 2016-11-09 22:19_

---
