---
name: fix-vulns
description: Check for vulnerabilities with an audit command and raise a PR to fix them. Use when the user asks to "fix vulnerabilities", "fix vulns", "audit dependencies", or "fix security issues".
disable-model-invocation: true
allowed-tools: Bash, Read, Edit
---

# fix-vulns

Check for dependency vulnerabilities and raise a PR that addresses all of them.

## Instructions

1. Detect the package manager by checking for lock files in the current working directory:

   - `pnpm-lock.yaml` → `pnpm`
   - `yarn.lock` → `yarn`
   - `package-lock.json` → `npm`

   If none is found, report that no recognised lock file was found and stop.

2. Run the audit:

```bash
$SHELL -i -c "<package-manager> audit"
```

3. If no vulnerabilities are found, report that and stop.

4. If vulnerabilities are found, fetch the latest `main` and create a branch:

```bash
git fetch origin main
git checkout -b fix/security-vulnerabilities origin/main
```

5. For each vulnerable package identified in the audit output, update it to the minimum safe version:

```bash
$SHELL -i -c "<package-manager> <update-command> <package-name>"
```

Where `<update-command>` is:

| Package manager | Update command |
| --------------- | -------------- |
| `pnpm`          | `update`       |
| `yarn`          | `upgrade`      |
| `npm`           | `update`       |

If the required safe version exceeds the current semver range in `package.json`, update the range in `package.json` first, then re-run the update command.

For transitive (indirect) dependencies that cannot be updated directly, add an overrides entry in `package.json` to force the minimum safe version across the entire dependency tree:

| Package manager | Key in `package.json`  |
| --------------- | ---------------------- |
| `pnpm`          | `pnpm.overrides`       |
| `yarn`          | `resolutions`          |
| `npm`           | `overrides`            |

Example for `pnpm`:

```json
{
  "pnpm": {
    "overrides": {
      "<package-name>": "<minimum-safe-version>"
    }
  }
}
```

Then run install to apply the override:

```bash
$SHELL -i -c "<package-manager> install"
```

6. Re-run the audit to confirm all vulnerabilities are resolved:

```bash
$SHELL -i -c "<package-manager> audit"
```

7. Commit the changes, staging only the lock file and `package.json`:

```bash
git add <lock-file> package.json
git commit -m "fix(deps): resolve audit vulnerabilities"
```

8. Push the branch and raise a draft PR. The PR body must list every vulnerability that was fixed, including package name, severity, and the resolution applied.
