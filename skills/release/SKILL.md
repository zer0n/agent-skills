---
name: release
description: Release on gitlab by creating a new tag
activation:
  - "release"
  - "create tag"
  - "deploy tag"
---

Release on gitlab by creating a new tag. If there are multiple arguments, then release them one by one: release one first, wait for the pipeline to pass, then continue with the next, and so on.
