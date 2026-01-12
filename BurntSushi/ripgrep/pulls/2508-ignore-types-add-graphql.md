```yaml
number: 2508
title: "ignore/types: add 'graphql'"
type: pull_request
state: closed
author: jparise
labels:
  - rollup
assignees: []
base: master
head: types-graphql
created_at: 2023-05-06T22:00:08Z
updated_at: 2023-11-28T18:55:04Z
url: https://github.com/BurntSushi/ripgrep/pull/2508
synced_at: 2026-01-12T18:23:14Z
```

# ignore/types: add 'graphql'

---

_@jparise_

GraphQL file extensions: .graphql and .graphqls (schema)

---

_Comment by @jparise on 2023-05-06 22:19_

Whoops, I hadn't noticed #2439 was already proposing a `graphql` type. I think the file extensions I chose (`.graphql`, `.graphqls`) are slightly more correct, however:

- `.graphql` is the canonical GraphQL file extension (graphql/graphql-spec#203)
- `.graphqls` is the recommended GraphQL SDL file extension (https://www.apollographql.com/docs/kotlin/essentials/file-types/)

I excluded `.gql`, a commonly used abbreviated extension. It could conflict with other existing types (e.g. https://filext.com/file-extension/GQL), although I doubt many people are going to use `-tgraphql` in contexts where non-GraphQL `.gql` files appear.

---

_Label `rollup` added by @BurntSushi on 2023-07-05 19:04_

---

_Comment by @BurntSushi on 2023-07-05 19:04_

I'll go with this PR for now, since it's more conservative.

We can add `.gql` later if there's demand. It's inevitable that some extensions will conflict. That's OK.

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---

_Branch deleted on 2023-11-28 18:55_

---
