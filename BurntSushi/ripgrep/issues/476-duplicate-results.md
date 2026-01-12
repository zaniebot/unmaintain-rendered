```yaml
number: 476
title: Duplicate results
type: issue
state: closed
author: CamJN
labels:
  - duplicate
assignees: []
created_at: 2017-05-09T02:32:33Z
updated_at: 2017-05-09T10:55:19Z
url: https://github.com/BurntSushi/ripgrep/issues/476
synced_at: 2026-01-12T16:13:22Z
```

# Duplicate results

---

_@CamJN_

Both these commands are run against the same file, with the same contents.
```
camdennarzt@SECUR-T:~/Documents/japanese $ rg -o '.{10}たいです.{85}' words.json 
1:{"kana":"〜たいです","furigana":"〜たいです","english":["(I","we) want to ...","(I","we) would like to ..."]}
1:{"kana":"〜たいです","furigana":"〜たいです","english":["(I","we) want to ...","(I","we) would like to ..."]}
1:{"kana":"〜たいです","furigana":"〜たいです","english":["(I","we) want to ...","(I","we) would like to ..."]}
1:{"kana":"〜たいです","furigana":"〜たいです","english":["(I","we) want to ...","(I","we) would like to ..."]}
1:{"kana":"〜たいです","furigana":"〜たいです","english":["(I","we) want to ...","(I","we) would like to ..."]}
1:{"kana":"〜たいです","furigana":"〜たいです","english":["(I","we) want to ...","(I","we) would like to ..."]}
1:{"kana":"〜たいです","furigana":"〜たいです","english":["(I","we) want to ...","(I","we) would like to ..."]}
1:{"kana":"〜たいです","furigana":"〜たいです","english":["(I","we) want to ...","(I","we) would like to ..."]}
1:{"kana":"〜たいです","furigana":"〜たいです","english":["(I","we) want to ...","(I","we) would like to ..."]}
1:{"kana":"〜たいです","furigana":"〜たいです","english":["(I","we) want to ...","(I","we) would like to ..."]}
camdennarzt@SECUR-T:~/Documents/japanese $ egrep -o '.{10}たいです.{85}' words.json 
{"kana":"〜たいです","furigana":"〜たいです","english":["(I","we) want to ...","(I","we) would like to ..."]}
さんのために何か買いたいです","furigana":"わたしはりほさんのためになにかかいたいです","english":["I want to buy something for Ms. Riho
なたのために何か買いたいです","furigana":"あなたのためになにかかいたいです","english":["I want to buy something for you"]},{"kana
"鈴木さんは何を買いたいですか？","furigana":"すずきさんはなにをかいたいですか？","english":["what does Mr. Suzuki want to buy?"]},{
ana":"何を買いたいですか？","furigana":"なにをかいたいですか？","english":["what do you want to buy?"]},{"kana":"私は緑のスカー
a":"私は服を買いたいです","furigana":"わたしはふくをかいたいです","english":["I want to buy some clothes"]},{"kana":"銀行は開い
{"kana":"冷たいです","furigana":"つめたいです","english":["is cold"]},{"kana":"寒いです","furigana":"さむいです","engli
:"私は劇場を見つけたいです","furigana":"わたしはげきじょうをみつけたいです","english":["I want to find the theatre"]},{"kana":"現
"これはきんきゅうじたいです","kana":"これは緊急事態です","english":["this is an emergency"]},{"furigana":"わたしはぐあいがわるいです",
rigana":"いたいです","kana":"痛いです","english":["to hurt","it hurts"]},{"furigana":"おなかがいたいです","kana":"お腹が
```
Not sure if this is a known limitation or what. File contents at: https://gist.github.com/CamJN/71c16779bce5e8bb38c24eba8d13b1ff

---

_Comment by @BurntSushi on 2017-05-09 10:55_

I think this is a dupe of #451, which is fixed on master.

---

_Label `duplicate` added by @BurntSushi on 2017-05-09 10:55_

---

_Closed by @BurntSushi on 2017-05-09 10:55_

---
