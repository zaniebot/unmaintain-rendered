```yaml
number: 3058
title: "Enable auto-wrapping of `--help` output"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - cli
assignees: []
merged: true
base: main
head: wrap-help
created_at: 2024-04-16T09:19:04Z
updated_at: 2024-04-16T12:54:46Z
url: https://github.com/astral-sh/uv/pull/3058
synced_at: 2026-01-10T14:43:31Z
```

# Enable auto-wrapping of `--help` output

---

_Pull request opened by @AlexWaygood on 2024-04-16 09:19_

## Summary

Fixes #3057

On `main`, with a narrow terminal:

- <details>

  ![image](https://github.com/astral-sh/uv/assets/66076021/2453dcbc-739c-4174-ba2e-029cff3227a2)

  </details>

- <details>

  ![image](https://github.com/astral-sh/uv/assets/66076021/02553e90-fe35-4828-b50f-71f926a1e347)

  </details>

- <details>

  ![image](https://github.com/astral-sh/uv/assets/66076021/b33eac26-f2fe-4328-8aa0-c51235b7c4c3)

  </details>

- <details>

  ![image](https://github.com/astral-sh/uv/assets/66076021/e731f647-3519-4c54-ab33-b42500faf544)

  </details>

With PR:

- <details>

  ![image](https://github.com/astral-sh/uv/assets/66076021/0f1aaec0-960a-4060-95ba-f49bec2f6995)

  </details>

- <details>

  ![image](https://github.com/astral-sh/uv/assets/66076021/b8078125-bd57-41a9-9c09-1966c8971c92)

  </details>

- <details>

  ![image](https://github.com/astral-sh/uv/assets/66076021/c2f38eb0-5f67-46ee-8a09-47da9e9ce0a5)

  </details>

- <details>

  ![image](https://github.com/astral-sh/uv/assets/66076021/31b9fdca-938a-47ca-96ba-751d987c269e)

  </details>

## Test Plan

See screenshots in the summary


---

_Label `cli` added by @AlexWaygood on 2024-04-16 09:19_

---

_Comment by @AlexWaygood on 2024-04-16 09:33_

(Basically an identical PR to the change we made at Ruff in https://github.com/astral-sh/ruff/pull/9633)

---

_Merged by @charliermarsh on 2024-04-16 12:54_

---

_Closed by @charliermarsh on 2024-04-16 12:54_

---

_Branch deleted on 2024-04-16 12:54_

---

_Comment by @charliermarsh on 2024-04-16 12:54_

Thx!

---
