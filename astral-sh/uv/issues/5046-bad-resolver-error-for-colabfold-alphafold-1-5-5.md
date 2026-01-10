---
number: 5046
title: "Bad resolver error for `colabfold[alphafold]==1.5.5` on python 3.11"
type: issue
state: closed
author: konstin
labels:
  - error messages
assignees: []
created_at: 2024-07-14T14:42:14Z
updated_at: 2024-08-23T13:42:12Z
url: https://github.com/astral-sh/uv/issues/5046
synced_at: 2026-01-10T01:23:44Z
---

# Bad resolver error for `colabfold[alphafold]==1.5.5` on python 3.11

---

_Issue opened by @konstin on 2024-07-14 14:42_

The resolver error for `echo "colabfold[alphafold]==1.5.5" | cargo run pip compile -p 3.11 --universal -` is verbose and has multiple problems. I'm filing this as one ticket rather than multiple since i expect that some of this is #5045 and others may be the same underlying problem. We should split it into multiple after tracking down the cause of the different problems.

<details>
<summary>Full resolver error</summary>

```
  × No solution found when resolving dependencies:
  ╰─▶ Because jax>=0.4.20 depends on numpy{python_version >= '3.12'}>=1.26.0 and only the following versions of jax are available:
          jax<=0.4.20
          jax==0.4.21
          jax==0.4.22
          jax==0.4.23
          jax==0.4.24
          jax==0.4.25
          jax==0.4.26
          jax==0.4.27
          jax==0.4.28
          jax==0.4.29
          jax==0.4.30
          jax>=0.5.0
      we can conclude that jax>=0.4.20 depends on numpy{python_version >= '3.12'}>=1.26.0.
      And because colabfold[alphafold]==1.5.5 depends on jax>=0.4.20 and only the following versions of numpy{python_version >=
      '3.12'} are available:
          numpy{python_version >= '3.12'}<=1.26.0
          numpy{python_version >= '3.12'}==1.26.1
          numpy{python_version >= '3.12'}==1.26.2
          numpy{python_version >= '3.12'}==1.26.3
          numpy{python_version >= '3.12'}==1.26.4
          numpy{python_version >= '3.12'}==2.0.0
      we can conclude that colabfold[alphafold]==1.5.5, any of:
          numpy<1.26.0
          numpy>1.26.0,<1.26.1
          numpy>1.26.1,<1.26.2
          numpy>1.26.2,<1.26.3
          numpy>1.26.3,<2.0.0
          numpy>2.0.0
      , numpy{python_version >= '3.12'}!=1.26.4 are incompatible. (1)

      Because only the following versions of tensorflow-cpu{sys_platform != 'darwin'} are available:
          tensorflow-cpu{sys_platform != 'darwin'}<=2.12.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.13.0
          tensorflow-cpu{sys_platform != 'darwin'}==2.13.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.14.0
          tensorflow-cpu{sys_platform != 'darwin'}==2.14.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.0
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}==2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>=3.0.0
      and any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.12.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.13.0,<=2.13.1
      depend on numpy>=1.22,<=1.24.3, we can conclude that any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.13.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.13.0,<2.13.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.13.1,<2.14.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.14.0,<2.14.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.14.1,<2.15.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0,<2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0.post1,<2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      depend on numpy>=1.22,<=1.24.3.
      And because tensorflow-cpu{sys_platform != 'darwin'}>=2.13.0,<=2.13.1 depends on numpy>=1.22,<=1.24.3 and
      numpy>=1.22,<=1.24.3, we can conclude that any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.14.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.14.0,<2.14.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.14.1,<2.15.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0,<2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0.post1,<2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      depend on numpy>=1.22,<=1.24.3. (2)

      Because tensorflow-macos{sys_platform == 'darwin'}==2.10.0 depends on tensorflow-macos{sys_platform == 'darwin'}==2.10.0 and
      tensorflow-macos{sys_platform == 'darwin'}>=2.9.1,<=2.11.0 depends on protobuf>=3.9.2,<3.20, we can conclude that any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
      depend on protobuf>=3.9.2,<3.20.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.8.0 depends on keras>=2.8.0rc0,<2.9 and flatbuffers>=1.12,<2, we can
      conclude that any of:
          flatbuffers<1.12
          flatbuffers>=2
      , any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , tensorflow-macos{sys_platform == 'darwin'}!=2.7.0, any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.7.0 depends on flatbuffers>=1.12,<3.0 and any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.14.0,<=2.14.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.15.0,<=2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.16.1,<=2.16.2
      depend on flatbuffers>=23.5.26, we can conclude that any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.14.0
          tensorflow-cpu{sys_platform != 'darwin'}==2.14.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.0
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.2
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.14.0,<=2.14.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.15.0,<=2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.16.1,<=2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>=2.17.0
      depend on one of:
          protobuf>=3.20.3,<4.21.0
          protobuf>4.21.0,<4.21.1
          protobuf>4.21.1,<4.21.2
          protobuf>4.21.2,<4.21.3
          protobuf>4.21.3,<4.21.4
          protobuf>4.21.4,<4.21.5
          protobuf>4.21.5,<5.0.0.dev0
      and keras>=2.14.0,<2.15, we can conclude that any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.14.0
          tensorflow-cpu{sys_platform != 'darwin'}==2.14.1
       and any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because we know from (2) that any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.14.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.14.0,<2.14.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.14.1,<2.15.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0,<2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0.post1,<2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      depend on numpy>=1.22,<=1.24.3, we can conclude that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.14.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.14.1,<2.15.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0,<2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0.post1,<2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible. (3)

      Because tensorflow-macos{sys_platform == 'darwin'}==2.10.0 depends on tensorflow-macos{sys_platform == 'darwin'}==2.10.0 and
      tensorflow-macos{sys_platform == 'darwin'}>=2.9.1,<=2.11.0 depends on protobuf>=3.9.2,<3.20, we can conclude that any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
      depend on protobuf>=3.9.2,<3.20.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.8.0 depends on keras>=2.8.0rc0,<2.9 and flatbuffers>=1.12,<2, we can
      conclude that any of:
          flatbuffers<1.12
          flatbuffers>=2
      , any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , tensorflow-macos{sys_platform == 'darwin'}!=2.7.0, any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.7.0 depends on flatbuffers>=1.12,<3.0 and any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.14.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.15.0,<=2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.16.1,<=2.16.2
      depend on flatbuffers>=23.5.26, we can conclude that any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.14.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.0
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.2
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.14.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.15.0,<=2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.16.1,<=2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>=2.17.0
      depend on one of:
          protobuf>=3.20.3,<4.21.0
          protobuf>4.21.0,<4.21.1
          protobuf>4.21.1,<4.21.2
          protobuf>4.21.2,<4.21.3
          protobuf>4.21.3,<4.21.4
          protobuf>4.21.4,<4.21.5
          protobuf>4.21.5,<5.0.0.dev0
      and keras>=2.14.0,<2.15, we can conclude that tensorflow-cpu{sys_platform != 'darwin'}==2.14.1 and any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because we know from (3) that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.14.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.14.1,<2.15.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0,<2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0.post1,<2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible, we can conclude that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.15.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0,<2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0.post1,<2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible. (4)

      Because tensorflow-macos{sys_platform == 'darwin'}==2.10.0 depends on tensorflow-macos{sys_platform == 'darwin'}==2.10.0 and
      tensorflow-macos{sys_platform == 'darwin'}>=2.9.1,<=2.11.0 depends on protobuf>=3.9.2,<3.20, we can conclude that any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
      depend on protobuf>=3.9.2,<3.20.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.8.0 depends on keras>=2.8.0rc0,<2.9 and flatbuffers>=1.12,<2, we can
      conclude that any of:
          flatbuffers<1.12
          flatbuffers>=2
      , any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , tensorflow-macos{sys_platform == 'darwin'}!=2.7.0, any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.7.0 depends on flatbuffers>=1.12,<3.0 and any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.15.0,<=2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.16.1,<=2.16.2
      depend on flatbuffers>=23.5.26, we can conclude that any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.0
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.2
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.15.0,<=2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.16.1,<=2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>=2.17.0
      depend on one of:
          protobuf>=3.20.3,<4.21.0
          protobuf>4.21.0,<4.21.1
          protobuf>4.21.1,<4.21.2
          protobuf>4.21.2,<4.21.3
          protobuf>4.21.3,<4.21.4
          protobuf>4.21.4,<4.21.5
          protobuf>4.21.5,<5.0.0.dev0
      and keras>=2.15.0,<2.16, we can conclude that any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.0
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.1
       and any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because we know from (4) that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.15.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0,<2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0.post1,<2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible, we can conclude that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0.post1,<2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible. (5)

      Because tensorflow-macos{sys_platform == 'darwin'}==2.10.0 depends on tensorflow-macos{sys_platform == 'darwin'}==2.10.0 and
      tensorflow-macos{sys_platform == 'darwin'}>=2.9.1,<=2.11.0 depends on protobuf>=3.9.2,<3.20, we can conclude that any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
      depend on protobuf>=3.9.2,<3.20.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.8.0 depends on keras>=2.8.0rc0,<2.9 and flatbuffers>=1.12,<2, we can
      conclude that any of:
          flatbuffers<1.12
          flatbuffers>=2
      , any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , tensorflow-macos{sys_platform == 'darwin'}!=2.7.0, any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.7.0 depends on flatbuffers>=1.12,<3.0 and any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.15.0.post1,<=2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.16.1,<=2.16.2
      depend on flatbuffers>=23.5.26, we can conclude that any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.2
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.15.0.post1,<=2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.16.1,<=2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>=2.17.0
      depend on one of:
          protobuf>=3.20.3,<4.21.0
          protobuf>4.21.0,<4.21.1
          protobuf>4.21.1,<4.21.2
          protobuf>4.21.2,<4.21.3
          protobuf>4.21.3,<4.21.4
          protobuf>4.21.4,<4.21.5
          protobuf>4.21.5,<5.0.0.dev0
      and keras>=2.15.0,<2.16, we can conclude that any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.1
       and any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because we know from (5) that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.15.0.post1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.0.post1,<2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible, we can conclude that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible. (6)

      Because tensorflow-macos{sys_platform == 'darwin'}==2.10.0 depends on tensorflow-macos{sys_platform == 'darwin'}==2.10.0 and
      tensorflow-macos{sys_platform == 'darwin'}>=2.9.1,<=2.11.0 depends on protobuf>=3.9.2,<3.20, we can conclude that any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
      depend on protobuf>=3.9.2,<3.20.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.8.0 depends on keras>=2.8.0rc0,<2.9 and flatbuffers>=1.12,<2, we can
      conclude that any of:
          flatbuffers<1.12
          flatbuffers>=2
      , any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , tensorflow-macos{sys_platform == 'darwin'}!=2.7.0, any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.7.0 depends on flatbuffers>=1.12,<3.0 and any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.16.1,<=2.16.2
      depend on flatbuffers>=23.5.26, we can conclude that any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.2
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>=2.16.1,<=2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>=2.17.0
      depend on one of:
          protobuf>=3.20.3,<4.21.0
          protobuf>4.21.0,<4.21.1
          protobuf>4.21.1,<4.21.2
          protobuf>4.21.2,<4.21.3
          protobuf>4.21.3,<4.21.4
          protobuf>4.21.4,<4.21.5
          protobuf>4.21.5,<5.0.0.dev0
      and keras>=2.15.0,<2.16, we can conclude that tensorflow-cpu{sys_platform != 'darwin'}==2.15.1 and any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because we know from (6) that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.15.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.15.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible, we can conclude that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible. (7)

      Because tensorflow-macos{sys_platform == 'darwin'}==2.10.0 depends on tensorflow-macos{sys_platform == 'darwin'}==2.10.0 and
      tensorflow-macos{sys_platform == 'darwin'}>=2.9.1,<=2.11.0 depends on protobuf>=3.9.2,<3.20, we can conclude that any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
      depend on protobuf>=3.9.2,<3.20.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.8.0 depends on keras>=2.8.0rc0,<2.9 and flatbuffers>=1.12,<2, we can
      conclude that any of:
          flatbuffers<1.12
          flatbuffers>=2
      , any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , tensorflow-macos{sys_platform == 'darwin'}!=2.7.0, any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.7.0 depends on flatbuffers>=1.12,<3.0 and
      tensorflow-cpu{sys_platform != 'darwin'}>=2.16.1,<=2.16.2 depends on flatbuffers>=23.5.26, we can conclude that any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.2
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.16.1,<=2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>=2.17.0
      depend on one of:
          protobuf>=3.20.3,<4.21.0
          protobuf>4.21.0,<4.21.1
          protobuf>4.21.1,<4.21.2
          protobuf>4.21.2,<4.21.3
          protobuf>4.21.3,<4.21.4
          protobuf>4.21.4,<4.21.5
          protobuf>4.21.5,<5.0.0.dev0
      and keras>=3.0.0, we can conclude that any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.2
       and any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because we know from (7) that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.16.1
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible, we can conclude that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible. (8)

      Because tensorflow-macos{sys_platform == 'darwin'}==2.10.0 depends on tensorflow-macos{sys_platform == 'darwin'}==2.10.0 and
      tensorflow-macos{sys_platform == 'darwin'}>=2.9.1,<=2.11.0 depends on protobuf>=3.9.2,<3.20, we can conclude that any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
      depend on protobuf>=3.9.2,<3.20.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.8.0 depends on keras>=2.8.0rc0,<2.9 and flatbuffers>=1.12,<2, we can
      conclude that any of:
          flatbuffers<1.12
          flatbuffers>=2
      , any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , tensorflow-macos{sys_platform == 'darwin'}!=2.7.0, any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.7.0 depends on flatbuffers>=1.12,<3.0 and
      tensorflow-cpu{sys_platform != 'darwin'}==2.16.2 depends on flatbuffers>=23.5.26, we can conclude that any of:
          keras<2.8.0rc0
          keras>=2.9
      , any of:
          protobuf<3.9.2
          protobuf>=3.20
      , tensorflow-cpu{sys_platform != 'darwin'}==2.16.2, any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because any of:
          tensorflow-cpu{sys_platform != 'darwin'}==2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>=2.17.0
      depend on one of:
          protobuf>=3.20.3,<4.21.0
          protobuf>4.21.0,<4.21.1
          protobuf>4.21.1,<4.21.2
          protobuf>4.21.2,<4.21.3
          protobuf>4.21.3,<4.21.4
          protobuf>4.21.4,<4.21.5
          protobuf>4.21.5,<5.0.0.dev0
      and keras>=3.0.0, we can conclude that tensorflow-cpu{sys_platform != 'darwin'}==2.16.2 and any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because we know from (8) that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.16.2
          tensorflow-cpu{sys_platform != 'darwin'}>2.16.2,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible, we can conclude that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
      And because only the following versions of tensorflow-macos{sys_platform == 'darwin'} are available:
          tensorflow-macos{sys_platform == 'darwin'}==2.5.0
          tensorflow-macos{sys_platform == 'darwin'}==2.6.0
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
          tensorflow-macos{sys_platform == 'darwin'}==2.12.0
          tensorflow-macos{sys_platform == 'darwin'}==2.13.0
          tensorflow-macos{sys_platform == 'darwin'}==2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>=2.14.0
      and tensorflow-macos{sys_platform == 'darwin'}>=2.13.0,<=2.13.1 depends on numpy>=1.22,<=1.24.3, we can conclude that any of:
          numpy<1.22
          numpy>1.24.3
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}<2.5.0
          tensorflow-macos{sys_platform == 'darwin'}>2.5.0,<2.6.0
          tensorflow-macos{sys_platform == 'darwin'}>2.6.0,<2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>2.12.0
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , tensorflow-macos{sys_platform == 'darwin'}<2.14.0 are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.12.0 depends on numpy>=1.22,<1.24 and numpy>=1.19.2,<1.20.dev0, we
      can conclude that any of:
          numpy<1.19.2
          numpy>=1.20.dev0,<1.22
          numpy>1.24.3
      , any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , tensorflow-macos{sys_platform == 'darwin'}<2.14.0 are incompatible.
      And because we know from (1) that colabfold[alphafold]==1.5.5, any of:
          numpy<1.26.0
          numpy>1.26.0,<1.26.1
          numpy>1.26.1,<1.26.2
          numpy>1.26.2,<1.26.3
          numpy>1.26.3,<2.0.0
          numpy>2.0.0
      , numpy{python_version >= '3.12'}!=1.26.4 are incompatible, we can conclude that colabfold[alphafold]==1.5.5, any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , tensorflow-macos{sys_platform == 'darwin'}<2.14.0 are incompatible. (9)

      Because only the following versions of tensorflow-macos{sys_platform == 'darwin'} are available:
          tensorflow-macos{sys_platform == 'darwin'}==2.5.0
          tensorflow-macos{sys_platform == 'darwin'}==2.6.0
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
          tensorflow-macos{sys_platform == 'darwin'}==2.12.0
          tensorflow-macos{sys_platform == 'darwin'}==2.13.0
          tensorflow-macos{sys_platform == 'darwin'}==2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>=2.14.0
      and tensorflow-macos{sys_platform == 'darwin'}<=2.6.0 depends on h5py>=3.1.0,<3.2.dev0, we can conclude that any of:
          tensorflow-macos{sys_platform == 'darwin'}<2.6.0
          tensorflow-macos{sys_platform == 'darwin'}>2.6.0,<2.7.0
          tensorflow-macos{sys_platform == 'darwin'}>2.7.0,<2.8.0
          tensorflow-macos{sys_platform == 'darwin'}>2.8.0,<2.9.0
          tensorflow-macos{sys_platform == 'darwin'}>2.9.0,<2.9.1
          tensorflow-macos{sys_platform == 'darwin'}>2.9.1,<2.9.2
          tensorflow-macos{sys_platform == 'darwin'}>2.9.2,<2.10.0
          tensorflow-macos{sys_platform == 'darwin'}>2.10.0,<2.11.0
          tensorflow-macos{sys_platform == 'darwin'}>2.11.0,<2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>2.12.0,<2.13.0
          tensorflow-macos{sys_platform == 'darwin'}>2.13.0,<2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>2.13.1,<2.14.0
      depend on h5py>=3.1.0,<3.2.dev0.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.6.0 depends on keras>=2.6,<3.dev0, we can conclude that any of:
          h5py<3.1.0
          h5py>=3.2.dev0
      , any of:
          keras<2.6
          keras>=3.dev0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}<2.7.0
          tensorflow-macos{sys_platform == 'darwin'}>2.7.0,<2.8.0
          tensorflow-macos{sys_platform == 'darwin'}>2.8.0,<2.9.0
          tensorflow-macos{sys_platform == 'darwin'}>2.9.0,<2.9.1
          tensorflow-macos{sys_platform == 'darwin'}>2.9.1,<2.9.2
          tensorflow-macos{sys_platform == 'darwin'}>2.9.2,<2.10.0
          tensorflow-macos{sys_platform == 'darwin'}>2.10.0,<2.11.0
          tensorflow-macos{sys_platform == 'darwin'}>2.11.0,<2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>2.12.0,<2.13.0
          tensorflow-macos{sys_platform == 'darwin'}>2.13.0,<2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>2.13.1,<2.14.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.7.0 depends on keras>=2.7.0rc0,<2.8 and keras>=2.8.0rc0,<2.9, we can
      conclude that any of:
          h5py<3.1.0
          h5py>=3.2.dev0
      , any of:
          keras<2.6
          keras>=3.dev0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}<2.9.0
          tensorflow-macos{sys_platform == 'darwin'}>2.9.0,<2.9.1
          tensorflow-macos{sys_platform == 'darwin'}>2.9.1,<2.9.2
          tensorflow-macos{sys_platform == 'darwin'}>2.9.2,<2.10.0
          tensorflow-macos{sys_platform == 'darwin'}>2.10.0,<2.11.0
          tensorflow-macos{sys_platform == 'darwin'}>2.11.0,<2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>2.12.0,<2.13.0
          tensorflow-macos{sys_platform == 'darwin'}>2.13.0,<2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>2.13.1,<2.14.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}>=2.9.0,<=2.9.2 depends on keras>=2.9.0rc0,<2.10.0 and
      keras>=2.9.0rc0,<2.10.0, we can conclude that any of:
          h5py<3.1.0
          h5py>=3.2.dev0
      , any of:
          keras<2.6
          keras>=3.dev0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}<2.9.2
          tensorflow-macos{sys_platform == 'darwin'}>2.9.2,<2.10.0
          tensorflow-macos{sys_platform == 'darwin'}>2.10.0,<2.11.0
          tensorflow-macos{sys_platform == 'darwin'}>2.11.0,<2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>2.12.0,<2.13.0
          tensorflow-macos{sys_platform == 'darwin'}>2.13.0,<2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>2.13.1,<2.14.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.9.2 depends on keras>=2.9.0rc0,<2.10.0 and keras>=2.10.0,<2.11, we
      can conclude that any of:
          h5py<3.1.0
          h5py>=3.2.dev0
      , any of:
          keras<2.6
          keras>=3.dev0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}<2.11.0
          tensorflow-macos{sys_platform == 'darwin'}>2.11.0,<2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>2.12.0,<2.13.0
          tensorflow-macos{sys_platform == 'darwin'}>2.13.0,<2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>2.13.1,<2.14.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.11.0 depends on keras>=2.11.0,<2.12 and keras>=2.12.0,<2.13, we can
      conclude that any of:
          h5py<3.1.0
          h5py>=3.2.dev0
      , any of:
          keras<2.6
          keras>=3.dev0
      , any of:
          tensorflow-macos{sys_platform == 'darwin'}<2.13.0
          tensorflow-macos{sys_platform == 'darwin'}>2.13.0,<2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>2.13.1,<2.14.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}>=2.13.0,<=2.13.1 depends on keras>=2.13.1,<2.14 and
      keras>=2.13.1,<2.14, we can conclude that any of:
          h5py<3.1.0
          h5py>=3.2.dev0
      , any of:
          keras<2.6
          keras>=3.dev0
      , tensorflow-macos{sys_platform == 'darwin'}<2.14.0 are incompatible.
      And because tensorflow-cpu{sys_platform != 'darwin'}==2.17.0 depends on keras>=3.2.0 and h5py>=3.10.0, we can conclude that
      tensorflow-cpu{sys_platform != 'darwin'}==2.17.0 and tensorflow-macos{sys_platform == 'darwin'}<2.14.0 are incompatible.
      And because we know from (9) that colabfold[alphafold]==1.5.5, any of:
          tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<2.17.0
          tensorflow-cpu{sys_platform != 'darwin'}>2.17.0,<3.0.0
      , tensorflow-macos{sys_platform == 'darwin'}<2.14.0 are incompatible, we can conclude that colabfold[alphafold]==1.5.5,
      tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<3.0.0, tensorflow-macos{sys_platform == 'darwin'}<2.14.0 are incompatible.
      And because colabfold==1.5.5 depends on tensorflow-cpu{sys_platform != 'darwin'}>=2.12.1,<3.0.0 and
      tensorflow-macos{sys_platform == 'darwin'}<2.14.0, we can conclude that colabfold==1.5.5 and colabfold[alphafold]==1.5.5 are
      incompatible.
      And because you require colabfold==1.5.5 and colabfold[alphafold]==1.5.5, we can conclude that the requirements are
      unsatisfiable.
```

</details>

- [x] The following part says that due to `jax>=0.4.20 depends on numpy{python_version >= '3.12'}>=1.26.0`, we know that
`jax>=0.4.20 depends on numpy{python_version >= '3.12'}>=1.26.0.`. We can skip this entire section with the jax listing and only tell the user about the jax -> numpy dependency.
```
  ╰─▶ Because jax>=0.4.20 depends on numpy{python_version >= '3.12'}>=1.26.0 and only the following versions of jax are available:
          jax<=0.4.20
          jax==0.4.21
          jax==0.4.22
          jax==0.4.23
          jax==0.4.24
          jax==0.4.25
          jax==0.4.26
          jax==0.4.27
          jax==0.4.28
          jax==0.4.29
          jax==0.4.30
          jax>=0.5.0
      we can conclude that jax>=0.4.20 depends on numpy{python_version >= '3.12'}>=1.26.0.
```

- [x] In the section below, we list numpy sometimes with a requires-python marker, and sometimes without. It is not
clear why, and if we need this information, we shouldn't repeat it for each package.
- [x] Also in the section below, we jump from `numpy{python_version >= '3.12'}>=1.26.0` to a list of incompatibilities and a conclusion about `numpy{python_version >= '3.12'}!=1.26.4` for a reason not explained.
```
      And because colabfold[alphafold]==1.5.5 depends on jax>=0.4.20 and only the following versions of numpy{python_version >=
      '3.12'} are available:
          numpy{python_version >= '3.12'}<=1.26.0
          numpy{python_version >= '3.12'}==1.26.1
          numpy{python_version >= '3.12'}==1.26.2
          numpy{python_version >= '3.12'}==1.26.3
          numpy{python_version >= '3.12'}==1.26.4
          numpy{python_version >= '3.12'}==2.0.0
      we can conclude that colabfold[alphafold]==1.5.5, any of:
          numpy<1.26.0
          numpy>1.26.0,<1.26.1
          numpy>1.26.1,<1.26.2
          numpy>1.26.2,<1.26.3
          numpy>1.26.3,<2.0.0
          numpy>2.0.0
      , numpy{python_version >= '3.12'}!=1.26.4 are incompatible. (1)
```

- [x] We also don't collapse packages with markers, there are no prerelease for <2.13.0rc0:
```
      , tensorflow-macos{sys_platform == 'darwin'}!=2.7.0, any of:
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
       are incompatible.
```


---

_Label `error messages` added by @konstin on 2024-07-14 14:42_

---

_Label `preview` added by @konstin on 2024-07-14 14:42_

---

_Comment by @zanieb on 2024-07-14 16:10_

Related

- https://github.com/astral-sh/uv/issues/4075
- https://github.com/pubgrub-rs/pubgrub/issues/232

---

_Referenced in [astral-sh/uv#4922](../../astral-sh/uv/issues/4922.md) on 2024-07-30 15:25_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-30 15:27_

---

_Assigned to @zanieb by @charliermarsh on 2024-08-16 13:34_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-08-16 13:34_

---

_Comment by @zanieb on 2024-08-16 14:24_

As of f2d67180388101ed24e70eb71e04fef1e3ddfc8e the error is

```
  × No solution found when resolving dependencies for split (python_full_version >= '3.12'):
  ╰─▶ Because colabfold[alphafold]==1.5.5 depends on jax>=0.4.20 and only the following versions of jax are available:
          jax<=0.4.20
          jax==0.4.21
          jax==0.4.22
          jax==0.4.23
          jax==0.4.24
          jax==0.4.25
          jax==0.4.26
          jax==0.4.27
          jax==0.4.28
          jax==0.4.29
          jax==0.4.30
          jax==0.4.31
      we can conclude that colabfold[alphafold]==1.5.5 depends on jax>=0.4.20.
      And because jax>=0.4.20 depends on numpy>=1.26.0, we can conclude that colabfold[alphafold]==1.5.5 depends on numpy>=1.26.0.
      (1)

      Because only the following versions of tensorflow-macos{sys_platform == 'darwin'} are available:
          tensorflow-macos{sys_platform == 'darwin'}==2.5.0
          tensorflow-macos{sys_platform == 'darwin'}==2.6.0
          tensorflow-macos{sys_platform == 'darwin'}==2.7.0
          tensorflow-macos{sys_platform == 'darwin'}==2.8.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.0
          tensorflow-macos{sys_platform == 'darwin'}==2.9.1
          tensorflow-macos{sys_platform == 'darwin'}==2.9.2
          tensorflow-macos{sys_platform == 'darwin'}==2.10.0
          tensorflow-macos{sys_platform == 'darwin'}==2.11.0
          tensorflow-macos{sys_platform == 'darwin'}==2.12.0
          tensorflow-macos{sys_platform == 'darwin'}==2.13.0
          tensorflow-macos{sys_platform == 'darwin'}==2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>=2.14.0
      and tensorflow-macos{sys_platform == 'darwin'}==2.5.0 has no wheels with a matching Python ABI tag, we can conclude that any
      of:
          tensorflow-macos{sys_platform == 'darwin'}<2.6.0
          tensorflow-macos{sys_platform == 'darwin'}>2.6.0,<2.7.0
          tensorflow-macos{sys_platform == 'darwin'}>2.7.0,<2.8.0
          tensorflow-macos{sys_platform == 'darwin'}>2.8.0,<2.9.0
          tensorflow-macos{sys_platform == 'darwin'}>2.9.0,<2.9.1
          tensorflow-macos{sys_platform == 'darwin'}>2.9.1,<2.9.2
          tensorflow-macos{sys_platform == 'darwin'}>2.9.2,<2.10.0
          tensorflow-macos{sys_platform == 'darwin'}>2.10.0,<2.11.0
          tensorflow-macos{sys_platform == 'darwin'}>2.11.0,<2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>2.12.0,<2.13.0
          tensorflow-macos{sys_platform == 'darwin'}>2.13.0,<2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>2.13.1,<2.14.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.6.0 has no wheels with a matching Python ABI tag and
      tensorflow-macos{sys_platform == 'darwin'}==2.7.0 has no wheels with a matching Python ABI tag, we can conclude that any of:
          tensorflow-macos{sys_platform == 'darwin'}<2.8.0
          tensorflow-macos{sys_platform == 'darwin'}>2.8.0,<2.9.0
          tensorflow-macos{sys_platform == 'darwin'}>2.9.0,<2.9.1
          tensorflow-macos{sys_platform == 'darwin'}>2.9.1,<2.9.2
          tensorflow-macos{sys_platform == 'darwin'}>2.9.2,<2.10.0
          tensorflow-macos{sys_platform == 'darwin'}>2.10.0,<2.11.0
          tensorflow-macos{sys_platform == 'darwin'}>2.11.0,<2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>2.12.0,<2.13.0
          tensorflow-macos{sys_platform == 'darwin'}>2.13.0,<2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>2.13.1,<2.14.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.8.0 has no wheels with a matching Python ABI tag and
      tensorflow-macos{sys_platform == 'darwin'}==2.9.0 has no wheels with a matching Python ABI tag, we can conclude that any of:
          tensorflow-macos{sys_platform == 'darwin'}<2.9.1
          tensorflow-macos{sys_platform == 'darwin'}>2.9.1,<2.9.2
          tensorflow-macos{sys_platform == 'darwin'}>2.9.2,<2.10.0
          tensorflow-macos{sys_platform == 'darwin'}>2.10.0,<2.11.0
          tensorflow-macos{sys_platform == 'darwin'}>2.11.0,<2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>2.12.0,<2.13.0
          tensorflow-macos{sys_platform == 'darwin'}>2.13.0,<2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>2.13.1,<2.14.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.9.1 has no wheels with a matching Python ABI tag and
      tensorflow-macos{sys_platform == 'darwin'}==2.9.2 has no wheels with a matching Python ABI tag, we can conclude that any of:
          tensorflow-macos{sys_platform == 'darwin'}<2.10.0
          tensorflow-macos{sys_platform == 'darwin'}>2.10.0,<2.11.0
          tensorflow-macos{sys_platform == 'darwin'}>2.11.0,<2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>2.12.0,<2.13.0
          tensorflow-macos{sys_platform == 'darwin'}>2.13.0,<2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>2.13.1,<2.14.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}==2.10.0 has no wheels with a matching Python ABI tag and
      tensorflow-macos{sys_platform == 'darwin'}==2.11.0 has no wheels with a matching Python ABI tag, we can conclude that any of:
          tensorflow-macos{sys_platform == 'darwin'}<2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>2.12.0,<2.13.0
          tensorflow-macos{sys_platform == 'darwin'}>2.13.0,<2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>2.13.1,<2.14.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}>=2.13.0,<=2.13.1 depends on numpy>=1.22,<=1.24.3 and numpy>=1.22,<1.24,
      we can conclude that tensorflow-macos{sys_platform == 'darwin'}<2.14.0 depends on numpy>=1.22,<=1.24.3.
      And because we know from (1) that colabfold[alphafold]==1.5.5 depends on numpy>=1.26.0, we can conclude that
      colabfold[alphafold]==1.5.5 and tensorflow-macos{sys_platform == 'darwin'}<2.14.0 are incompatible.
      And because colabfold==1.5.5 depends on tensorflow-macos{sys_platform == 'darwin'}<2.14.0, we can conclude that
      colabfold==1.5.5 and colabfold[alphafold]==1.5.5 are incompatible.
      And because you require colabfold==1.5.5 and colabfold[alphafold]==1.5.5, we can conclude that your requirements are
      unsatisfiable.
```

---

_Referenced in [astral-sh/uv#6154](../../astral-sh/uv/pulls/6154.md) on 2024-08-16 16:55_

---

_Referenced in [astral-sh/uv#6160](../../astral-sh/uv/pulls/6160.md) on 2024-08-16 18:19_

---

_Referenced in [astral-sh/uv#6162](../../astral-sh/uv/pulls/6162.md) on 2024-08-16 20:14_

---

_Comment by @dimbleby on 2024-08-17 20:13_

error message aside: is it right that there is no solution?  https://github.com/sokrypton/ColabFold/blob/v1.5.5/poetry.lock says there is.

---

_Comment by @charliermarsh on 2024-08-17 21:52_

Not sure. Worth understanding, I'll take a look. The part of the error that I don't see in the dependency graph is: `because jax>=0.4.20 depends on numpy>=1.26.0`.

---

_Comment by @charliermarsh on 2024-08-17 21:54_

Jax does have `Requires-Dist: numpy >=1.26.0 ; python_version >= "3.12"`, so that's actually correct. I think there isn't a solution to this for Python 3.12, and that's why we fail. We try to solve for Python 3.11 and later.

---

_Comment by @charliermarsh on 2024-08-17 21:54_

It would be fixed by https://github.com/astral-sh/uv/issues/6150, which we should probably do.

---

_Comment by @charliermarsh on 2024-08-17 21:55_

I actually think the right fix is... have the user specify a range of Python versions to resolve for, separate from `requires-python`. I don't think users should be putting upper bounds on their `requires-python`. What you want to convey is: I don't care about having a valid resolution for Python 3.12 (as opposed to: my code only supports Python 3.11).

---

_Comment by @charliermarsh on 2024-08-17 21:56_

So, I'd say it should actually be solved by https://github.com/astral-sh/uv/issues/4087.

---

_Comment by @dimbleby on 2024-08-17 21:59_

Ah, the `-p 3.11` bit is ignored then?

---

_Comment by @charliermarsh on 2024-08-17 22:00_

Not quite -- it's not that it's ignored. It's that we solve for Python 3.11 as a minimum supported version.

---

_Comment by @charliermarsh on 2024-08-17 22:00_

It's like: `requires-python = ">=3.11"`.

---

_Comment by @dimbleby on 2024-08-17 22:02_

That'd  do it.  Then there's definitely no solution, right from the start: https://github.com/sokrypton/ColabFold/blob/675f93a44eee6589a003164b047e7d4183073d1e/pyproject.toml#L22

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---

_Comment by @konstin on 2024-08-23 13:15_

The error with current main is:

```
  × No solution found when resolving dependencies for split (python_full_version >= '3.12'):
  ╰─▶ Because colabfold[alphafold]==1.5.5 depends on jax>=0.4.20 and jax>=0.4.20 depends on numpy>=1.26.0, we
      can conclude that colabfold[alphafold]==1.5.5 depends on numpy>=1.26.0. (1)

      Because only the following versions of tensorflow-macos{sys_platform == 'darwin'} are available:
          tensorflow-macos{sys_platform == 'darwin'}<=2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>=2.13.0
      and tensorflow-macos{sys_platform == 'darwin'}<=2.11.0 has no wheels with a matching Python ABI tag, we
      can conclude that all of:
          tensorflow-macos{sys_platform == 'darwin'}<2.6.0
          tensorflow-macos{sys_platform == 'darwin'}>2.6.0,<2.7.0
          tensorflow-macos{sys_platform == 'darwin'}>2.7.0,<2.8.0
          tensorflow-macos{sys_platform == 'darwin'}>2.8.0,<2.9.0
          tensorflow-macos{sys_platform == 'darwin'}>2.9.0,<2.9.1
          tensorflow-macos{sys_platform == 'darwin'}>2.9.1,<2.9.2
          tensorflow-macos{sys_platform == 'darwin'}>2.9.2,<2.10.0
          tensorflow-macos{sys_platform == 'darwin'}>2.10.0,<2.11.0
          tensorflow-macos{sys_platform == 'darwin'}>2.11.0,<2.12.0
          tensorflow-macos{sys_platform == 'darwin'}>2.12.0,<2.13.0
          tensorflow-macos{sys_platform == 'darwin'}>2.13.0,<2.13.1
          tensorflow-macos{sys_platform == 'darwin'}>2.13.1,<2.14.0
       are incompatible.
      And because tensorflow-macos{sys_platform == 'darwin'}>=2.13.0,<=2.13.1 depends on numpy>=1.22,<=1.24.3
      and numpy>=1.22,<1.24, we can conclude that tensorflow-macos{sys_platform == 'darwin'}<2.14.0 depends
      on numpy>=1.22,<=1.24.3.
      And because we know from (1) that colabfold[alphafold]==1.5.5 depends on numpy>=1.26.0, we can conclude
      that colabfold[alphafold]==1.5.5 and tensorflow-macos{sys_platform == 'darwin'}<2.14.0 are incompatible.
      And because colabfold==1.5.5 depends on tensorflow-macos{sys_platform == 'darwin'}<2.14.0, we can conclude
      that colabfold==1.5.5 and colabfold[alphafold]==1.5.5 are incompatible.
      And because you require colabfold==1.5.5 and colabfold[alphafold]==1.5.5, we can conclude that your
      requirements are unsatisfiable.
```

This does not look bad anymore. We could collapse `tensorflow-macos{sys_platform == 'darwin'` some more and it's unhelpful that we're complaining about a lack of ABI when we're meaning specifically the python version in the build tag, but the error message overall is correct and explains the problem.

---

_Closed by @konstin on 2024-08-23 13:15_

---

_Comment by @dimbleby on 2024-08-23 13:33_

if uv is trying to solve for python >= "3.12", it would have been even simpler to notice directly that colabfold 1.5.5 requires python ">=3.9,<3.12"

---

_Comment by @konstin on 2024-08-23 13:35_

We're ignoring upper bounds on `requires-python` in the resolver, we've made the experience that this leads to undesirable behavior (https://github.com/astral-sh/uv/issues/4022)

---

_Comment by @dimbleby on 2024-08-23 13:42_

interesting choice!  (I see I was in that issue already but either missed or forgot this outcome)

well I guess this issue is an example of that decision having downside

Even with the relatively improved error message: if the fact is that this project does not install on python 3.12 and had declared that it didn't install on python 3.12 - I guess users would rather have seen that message than this one.

(of course there is upside too, no doubt some projects will succeed in installing even though they declared that they would fail)

---

_Referenced in [astral-sh/uv#4022](../../astral-sh/uv/issues/4022.md) on 2025-09-18 09:11_

---
