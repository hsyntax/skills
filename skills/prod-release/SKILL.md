---
name: prod-release
disable-model-invocation: true
description:
  Summarise changes between origin/main and origin/dev as short, user-focused
  release notes for Slack
---

# Prod Release

Run `git log origin/main..origin/dev --oneline`

Summarise the changes as short release notes that I can post in a slack channel.
Use short bullet-points if possible and base this on how they impact the
end-user. i.e, `web`, `cli` or `mcp`.

For changes that don't impact the user, like `backend` changes, include them in
a separate section with high-level detail but keep them short.

Write the message in slack markdown format.