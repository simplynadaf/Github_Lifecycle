# Branching and Merging Strategy

## Table of Contents
- [Branching and Merging Strategy](#branching-and-merging-strategy)
  - [Table of Contents](#table-of-contents)
  - [Repository Management](#repository-management)
  - [Git Flow](#git-flow)
    - [Branch Types](#branch-types)
      - [Main Branches](#main-branches)
      - [Supporting Branches](#supporting-branches)
  - [Workflow](#workflow)
    - [Feature Development](#feature-development)
    - [Release Preparation](#release-preparation)
    - [Hotfixes](#hotfixes)
  - [Advantages](#advantages)
  - [Branch Summary](#branch-summary)
  - [Merge Strategy](#merge-strategy)
  - [Permissions](#permissions)
  - [Example Workflow](#example-workflow)
    - [Developing a Feature or Fixing a Minor Bug](#developing-a-feature-or-fixing-a-minor-bug)
    - [Preparing for a Release](#preparing-for-a-release)
    - [Fixing a Critical Bug in Production](#fixing-a-critical-bug-in-production)
  - [Commonly Used Git Commands Without Using Git Flow](#commonly-used-git-commands-without-using-git-flow)
    - [Step-by-Step Guide](#step-by-step-guide)
      - [1. Creating a Feature Branch](#1-creating-a-feature-branch)
      - [2. Working on the Feature](#2-working-on-the-feature)
      - [3. Merging the Feature into `develop`](#3-merging-the-feature-into-develop)
      - [4. Creating a Release Branch](#4-creating-a-release-branch)
      - [5. Finalizing and Merging the Release](#5-finalizing-and-merging-the-release)
      - [6. Creating a Hotfix Branch](#6-creating-a-hotfix-branch)
      - [7. Applying the Fix and Merging](#7-applying-the-fix-and-merging)
  - [Commonly Used Git Commands Using Git Flow](#commonly-used-git-commands-using-git-flow)
  - [1. Creating a Feature Branch](#1-creating-a-feature-branch-1)
  - [2. Working on the Feature](#2-working-on-the-feature-1)
  - [3.Finishing the Feature and Merging into develop](#3finishing-the-feature-and-merging-into-develop)
  - [4.Creating a Release Branch](#4creating-a-release-branch)
  - [5.Finalizing and Merging the Release](#5finalizing-and-merging-the-release)
  - [6.Creating a Hotfix Branch](#6creating-a-hotfix-branch)
  - [7.Applying the Fix and Merging](#7applying-the-fix-and-merging)
---

## Repository Management
- **Platform**: GitLab Community Edition
- **Version Control**: Git
- **Branching Model**: Git Flow

---

## Git Flow
Git Flow provides a structured approach to managing branches for software development. Below are its key components:

### Branch Types
#### Main Branches
- **`main`**: The production-ready branch that reflects the latest stable release.
- **`develop`**: The integration branch containing the latest development changes for the next release.

#### Supporting Branches
- **Feature Branches**:
  - **Purpose**: For developing new features.
  - **Base Branch**: `develop`
  - **Naming Convention**: `feature/*` (e.g., `feature/user-login`)

- **Release Branches**:
  - **Purpose**: For final tweaks and fixes before a new release.
  - **Base Branch**: `develop`
  - **Naming Convention**: `release/*` (e.g., `release/1.0.0`)

- **Hotfix Branches**:
  - **Purpose**: For addressing critical issues in production.
  - **Base Branch**: `main`
  - **Naming Convention**: `hotfix/*` (e.g., `hotfix/1.0.0`)

---

## Workflow

### Feature Development
1. Create a feature branch from `develop`.
2. Work on the feature and commit changes.
3. Merge the branch back into `develop`.

### Release Preparation
1. Create a release branch from `develop`.
2. Perform final adjustments and testing.
3. Merge the release branch into both `main` and `develop`.

### Hotfixes
1. Create a hotfix branch from `main`.
2. Apply the fix and test changes.
3. Merge the branch back into both `main` and `develop`.

---

## Advantages
- Clear structure for collaboration.
- Easier management of releases and hotfixes.

---

## Branch Summary

| Branch       | Purpose                                  |
|--------------|------------------------------------------|
| **`main`**   | The stable, production-ready branch. Contains only released code. |
| **`develop`**| The integration branch. Contains code ready for testing and release. |
| **`feature/*`** | Temporary branches for developing new features or minor bug fixes. |
| **`release/*`** | Temporary branches for preparing a release (bug fixes, version bump, final testing). |
| **`hotfix/*`**  | Temporary branches for fixing critical production issues. |

---

## Merge Strategy

| Target Branch  | Source Branch     | Merge Strategy       | Notes                                           |
|----------------|-------------------|----------------------|------------------------------------------------|
| **`main`**     | `release/*`, `hotfix/*` | Merge Commit (No Squash) | Preserves history for releases and hotfixes. Triggers production deployment. |
| **`develop`**  | `feature/*`, `release/*`, `hotfix/*` | Merge Commit or Squash | Integrates features and fixes. Squashing can simplify history. |
| **`feature/*`**| Local branches    | Squash and Merge     | Local commits are squashed for a cleaner history. |
| **`release/*`**| `develop`         | Merge Commit         | Stabilizes the release process. |
| **`hotfix/*`** | `main`            | Merge Commit         | Fixes are immediately deployed to production and integrated into `develop`. |

---

## Permissions
To maintain workflow integrity, roles and permissions are configured as follows:

| Branch         | Allowed Roles     | Permissions                                        |
|----------------|-------------------|---------------------------------------------------|
| **`main`**     | Maintainers only  | - Push and merge restricted to Maintainers.<br>- Protected branch.<br>- Requires approvals. |
| **`develop`**  | Developers and above | - Push allowed after code reviews.<br>- Requires 1-2 approvals. |
| **`feature/*`**| Developers and above | - Created and managed by Developers.<br>- Merged into `develop` after review. |
| **`release/*`**| Maintainers only  | - Push restricted to Maintainers.<br>- Merged into `main` and `develop`. |
| **`hotfix/*`** | Maintainers only  | - Push restricted to Maintainers.<br>- Merged into `main` and `develop`. |

---

## Example Workflow

### Developing a Feature or Fixing a Minor Bug
1. Developer creates a branch: `feature/user-login`.
2. Completes work, tests locally, and creates a merge request targeting `develop`.
3. After approvals and CI/CD checks, the branch is merged into `develop`.

### Preparing for a Release
1. Maintainer creates a branch: `release/1.0.0` from `develop`.
2. Performs final testing and minor bug fixes.
3. Merges the branch into `main` for production and back into `develop`.

### Fixing a Critical Bug in Production
1. Maintainer creates a branch: `hotfix/critical-bug` from `main`.
2. Applies and tests the fix.
3. Merges the branch into both `main` and `develop`.

---

## Commonly Used Git Commands Without Using Git Flow

### Step-by-Step Guide
#### 1. Creating a Feature Branch
```bash
git checkout develop
git pull origin develop
git checkout -b feature/user-login
```
#### 2. Working on the Feature
Make changes, then stage and commit them:
```bash
git add .
git commit -m "Add user login functionality"
```
#### 3. Merging the Feature into `develop`
```bash
git checkout develop
git pull origin develop
git merge --no-ff feature/user-login
git push origin develop
```

#### 4. Creating a Release Branch
```bash
git checkout develop
git pull origin develop
git checkout -b release/1.0.0
git push origin release/1.0.0
```
#### 5. Finalizing and Merging the Release
```bash
git checkout main
git pull origin main
git merge --no-ff release/1.0.0
git push origin main
git checkout develop
git merge --no-ff release/1.0.0
git push origin develop
```

#### 6. Creating a Hotfix Branch
```bash
git checkout main
git pull origin main
git checkout -b hotfix/critical-bug
git push origin hotfix/critical-bug
```
#### 7. Applying the Fix and Merging
Apply the fix, stage, and commit changes:
```bash
git add .
git commit -m "Fix critical bug"
git checkout main
git merge --no-ff hotfix/critical-bug
git push origin main
git checkout develop
git merge --no-ff hotfix/critical-bug
git push origin develop
```
## Commonly Used Git Commands Using Git Flow
## 1. Creating a Feature Branch
To start a new feature, use:
```bash
git flow feature start user-login
```
## 2. Working on the Feature
Make your changes, then stage and commit:
```bash
git add .
git commit -m "Add user login functionality"
```
## 3.Finishing the Feature and Merging into develop
Once you have completed your feature, finish and merge it:
```bash
git flow feature finish user-login
```
## 4.Creating a Release Branch
To prepare for a new release, create a release branch:
```bash
git flow release start 1.0.0
```
## 5.Finalizing and Merging the Release
After completing the release, finalize and merge it back into develop and main:
```bash
git flow release finish 1.0.0
```
## 6.Creating a Hotfix Branch
If a critical bug needs to be addressed, start a hotfix branch:
```bash
git flow hotfix start critical-bug
```
## 7.Applying the Fix and Merging
After applying the fix, stage and commit your changes:
```bash
git add .
git commit -m "Fix critical bug"
git flow hotfix finish critical-bug
```

