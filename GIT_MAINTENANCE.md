# Git Branch Maintenance Guide

This document explains the repository branching structure for this fork of [SysReptor](https://github.com/Syslifters/sysreptor) and provides step-by-step instructions for staying up to date with the original repository.

---

## Branch Overview

* **`main`**: Tracks the original upstream repository (`Syslifters/sysreptor:main`) directly. No custom commits should be committed directly to `main`.
* **`custom-changes`**: Contains custom modifications (such as GitHub Actions Docker build workflows, `.gitignore` entries, etc.) applied on top of the latest `main`.

---

## How to Sync with Upstream (`Syslifters/sysreptor`)

Whenever the original `Syslifters/sysreptor:main` receives updates, run the following commands to update your repository while preserving your custom modifications:

### 1. Update your local `main` branch
```bash
git checkout main
git pull upstream main
git push origin main
```

### 2. Rebase `custom-changes` onto the updated `main`
```bash
git checkout custom-changes
git rebase main
git push origin custom-changes --force-with-lease
```

---

## Conflict Resolution (If Needed)

If an upstream update modifies a file that was also modified on `custom-changes` (e.g., `.gitignore`), Git will pause during `git rebase main` and report a conflict.

To resolve conflicts:
1. Open the conflicting file(s) and resolve the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
2. Stage the resolved files:
   ```bash
   git add <resolved-file>
   ```
3. Continue the rebase:
   ```bash
   git rebase --continue
   ```
4. Force push the rebased branch:
   ```bash
   git push origin custom-changes --force-with-lease
   ```
