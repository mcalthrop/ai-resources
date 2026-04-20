---
name: update-ai-resources
description: Update the ai-resources snapshot in the current repo to the latest version from GitHub. Use when the user asks to "update ai-resources", "pull latest ai-resources", or "refresh ai-resources".
disable-model-invocation: true
allowed-tools: Bash
---

# update-ai-resources

Download the latest snapshot of ai-resources from GitHub and commit the update on a new branch.

## Instructions

1. Confirm that an `ai-resources/` directory exists in the current working directory. If not, abort and tell the user to add ai-resources first (see the ai-resources README).

2. Create a branch and download the latest snapshot:

```bash
git checkout -b chore/update-ai-resources
curl -L https://github.com/mcalthrop/ai-resources/archive/refs/heads/main.tar.gz \
  | tar -xz --strip-components=1 --directory ai-resources
```

3. Check whether anything changed:

```bash
git diff --stat ai-resources
```

If there are no changes, report that ai-resources is already up to date, delete the branch, and stop.

4. Stage and commit the changes:

```bash
git add ai-resources
git commit -m "chore: update ai-resources snapshot"
```

5. Report which files changed and remind the user to raise a PR.
