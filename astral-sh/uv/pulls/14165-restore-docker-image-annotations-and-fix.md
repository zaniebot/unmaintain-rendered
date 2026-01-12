```yaml
number: 14165
title: Restore Docker image annotations and fix attestations
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/annotate-attest
created_at: 2025-06-20T17:40:28Z
updated_at: 2025-06-21T14:01:22Z
url: https://github.com/astral-sh/uv/pull/14165
synced_at: 2026-01-12T16:11:03Z
```

# Restore Docker image annotations and fix attestations

---

_@zanieb_

More follow-up to #13459 

- Depot doesn't support annotations, so we push those manually
- Docker push for the re-tag was breaking the manifest, since we need to annotate manually, we just do that instead
- We attest after the annotation

A bit of an aside

- We test building the extra images, it's very fast and I don't see why it's better to gate it

I tested this on my fork then cleaned it up a bit for a commit here. You can see the images at

- https://github.com/zanieb/uv/pkgs/container/uv 
- https://hub.docker.com/r/astral/uv/tags

---

_Label `internal` added by @zanieb on 2025-06-20 17:40_

---

_Marked ready for review by @zanieb on 2025-06-20 18:05_

---

_@zanieb reviewed on 2025-06-20 18:05_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:307 on 2025-06-20 18:05_

This feels clunky, I presume there's a cleaner way to do this?

---

_@zanieb reviewed on 2025-06-20 18:06_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:326 on 2025-06-20 18:06_

These are just copied from the pre-Depot workflow

---

_@zanieb reviewed on 2025-06-20 18:06_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:416 on 2025-06-20 18:06_

See https://github.com/astral-sh/uv/pull/14165/files#r2159466956

---

_@zanieb reviewed on 2025-06-20 18:10_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:275 on 2025-06-20 18:10_

Hoping this is a brief problem https://github.com/depot/build-push-action/pull/38

---

_@zanieb reviewed on 2025-06-20 18:11_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:282 on 2025-06-20 18:11_

I'm not sure if this early attestation is needed or not

---

_Review requested from @samypr100 by @samypr100 on 2025-06-21 02:38_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:306 on 2025-06-21 04:05_

```suggestion
            readarray -t lines < <(grep "^${image}:" <<< "$TAGS"); tags=(); for line in "${lines[@]}"; do tags+=(-t "$line"); done
            docker buildx imagetools create \
              "${annotations[@]}" \
              "${tags[@]}" \
              "${image}@${DIGEST}"
```

Similar to the readarray, but we filter by image. This makes it a single command per registry making sure we don't do multiple writes. We could've also used the previous jq command, but we don't have $DOCKER_METADATA_OUTPUT_JSON in the last job so we use readarray to make it the same on both.

e.g.

```bash
          for image in $IMAGES; do
            docker buildx imagetools create \
            $(jq -cr --arg img "${image}" '.tags | map(select(startswith($img)) | "-t " + .) | join(" ")' <<< "$DOCKER_METADATA_OUTPUT_JSON") \
            "${image}@${DIGEST}"
          done
```

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:354 on 2025-06-21 04:07_

nit: For consistency with the rest of this file/jobs (we login to dockerhub first, then ghcr), I think its worth moving this above `docker/login-action` for ghcr in this job.

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:352 on 2025-06-21 04:08_

```suggestion
      - uses: docker/login-action@74a5d142397b4f367a81961eba4e8cd7edddf772 # v3.4.0
        if: ${{ github.event.pull_request.head.repo.full_name == 'astral-sh/uv' }}
        with:
```

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:384 on 2025-06-21 04:09_

```suggestion
            readarray -t lines < <(grep "^${image}:" <<< "$TAGS"); tags=(); for line in "${lines[@]}"; do tags+=(-t "$line"); done
            docker buildx imagetools create \
              "${annotations[@]}" \
              "${tags[@]}" \
              "${image}@${DIGEST}"
```

Same as https://github.com/astral-sh/uv/pull/14165#discussion_r2159855379

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:416 on 2025-06-21 04:09_

```suggestion
```

Old doc from old workflow

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:369 on 2025-06-21 04:10_

```suggestion
```

Old doc from old workflow

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:370 on 2025-06-21 04:12_

```suggestion
        # The final command becomes `docker buildx imagetools create --annotation 'index:foo=1' --annotation 'index:bar=2' ... -t tag1 -t tag2 ... <IMG>@sha256:<sha256>`
```

---

_@samypr100 reviewed on 2025-06-21 04:15_

---

_@zanieb reviewed on 2025-06-21 12:41_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:306 on 2025-06-21 12:41_

We could grab the JSON output, I think — but the `readarray` reads clearer to me.

---

_@zanieb reviewed on 2025-06-21 12:42_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:352 on 2025-06-21 12:42_

Does this make sense? Won't we fail to push downstream anyway?

---

_@samypr100 reviewed on 2025-06-21 13:24_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:352 on 2025-06-21 13:24_

I only suggested for consistency with others and because it helped with running it on my fork. I agree though, you'd still need to set docker hub en var to empty string or remove it from the fork.

---

_@samypr100 reviewed on 2025-06-21 13:58_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:282 on 2025-06-21 13:58_

It doesn't hurt as we're now pushing `tagged` images very early on. The previous workflow didn't push the tags until the last job.

---

_@samypr100 approved on 2025-06-21 13:59_

---

_Merged by @zanieb on 2025-06-21 14:01_

---

_Closed by @zanieb on 2025-06-21 14:01_

---

_Branch deleted on 2025-06-21 14:01_

---
